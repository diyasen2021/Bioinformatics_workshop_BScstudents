# Finding Mutations from DNA Sequencing Data
### NGS Variant Calling Tutorial — *E. coli* Evolution Dataset

---

## The biological story

This dataset comes from one of the most famous experiments in evolutionary biology: the **Lenski Long-Term Evolution Experiment (LTEE)**.

In 1988, Richard Lenski took a single *Escherichia coli* strain (REL606) and split it into 12 independent populations. He has grown them every day since — over **60,000 generations** so far — watching evolution happen in real time. Each day he freezes a sample, creating a living fossil record of evolution.

We are going to sequence one of these evolved strains and find the mutations that accumulated compared to the original ancestor. These are real adaptive mutations that helped *E. coli* survive and grow faster.

This is the same dataset used in the **Data Carpentry Genomics curriculum**, a globally recognised bioinformatics training programme.

---

## How Illumina sequencing works

1. DNA is extracted from the bacterial culture
2. DNA is broken into fragments ~150–300 bp long
3. Each fragment is sequenced independently — producing a **read**
4. Millions of short reads are stored in a **FASTQ** file
5. Each base has a **Phred quality score** indicating sequencing confidence

```
@SRR2584863.1                    <- read name (starts with @)
ATCGATCGATCGATCGATCG             <- the DNA sequence
+                                <- separator
IIIIIIIIIIIIIIIIIIIII            <- Phred quality scores (ASCII-encoded)
```

The pipeline then figures out where in the genome each read came from (alignment) and identifies positions where the sequenced strain differs from the reference (variant calling).

```
Reference:  ATCG[C]TTGCCA
Read 1:     ATCG[T]TTGCCA   <- T instead of C
Read 2:     ATCG[T]TTGCCA   <- T instead of C
Read 3:     ATCG[C]TTGCCA   <- matches reference
Read 4:     ATCG[T]TTGCCA   <- T instead of C
                 ^
     Variant: C->T  (3 of 4 reads = 75% allele frequency)
```

---

## The pipeline

```
FASTQ reads
    |
    v
FastQC          -- are the reads good quality?
    |
    v
BWA-MEM         -- align reads to the reference genome
    |
    v
SAMtools        -- sort and index the alignment
    |
    v
samtools depth  -- how many reads cover each position?
    |
    v
freebayes       -- find positions that differ from the reference
    |
    v
VCF file        -- the final list of variants
    |
    v
NCBI search     -- what do these variants mean biologically?
```

---

## Step 0 — Install Tools

We install four bioinformatics tools. `bwa`, `samtools`, and `fastqc` come from the standard Linux package manager. `freebayes` is downloaded as a pre-compiled binary from GitHub because it is not available via `apt` on Colab.

| Tool | What it does |
|------|-------------|
| **FastQC** | Quality control reports for sequencing reads |
| **BWA** | Aligns short reads to a reference genome |
| **SAMtools** | Manipulates alignment files — sort, index, coverage |
| **freebayes** | Bayesian variant caller — finds positions differing from reference |

```bash
%%bash
apt-get update -q 2>/dev/null
apt-get install -y -q bwa samtools fastqc 2>/dev/null

wget -q -O freebayes.gz \
    https://github.com/freebayes/freebayes/releases/download/v1.3.10/freebayes-1.3.10-linux-amd64-static.gz
gunzip freebayes.gz
chmod +x freebayes

./freebayes --version 2>&1 | head -1
bwa 2>&1 | grep -i version | head -1
samtools --version | head -1
fastqc --version
```

---

## Step 1 — Download the Data

We need two things:

1. **The reference genome** — *E. coli* REL606, the ancestral strain from the start of the Lenski experiment. This is what we compare our evolved sample against.

2. **The sequencing reads** — paired-end Illumina reads from an evolved strain (SRR2584863), downloaded directly from the **European Nucleotide Archive (ENA)**.

**What is paired-end sequencing?** Paired-end sequencing reads each DNA fragment from both ends, giving two reads per fragment stored in two files: `_1.fastq` and `_2.fastq`. Having both ends helps the aligner place reads more accurately, especially near repetitive regions.

```
Fragment:   ----[READ 1]----------[READ 2]----
                 ->                    <-
```

### Download the reference genome

```bash
%%bash
wget -q -O ecoli_rel606.fasta.gz \
    https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/000/017/985/GCA_000017985.1_ASM1798v1/GCA_000017985.1_ASM1798v1_genomic.fna.gz

gunzip -f ecoli_rel606.fasta.gz

head -1 ecoli_rel606.fasta
grep -v '>' ecoli_rel606.fasta | tr -d '\n' | wc -c
```

### Download the sequencing reads

```bash
%%bash
wget https://ftp.sra.ebi.ac.uk/vol1/fastq/SRR258/003/SRR2584863/SRR2584863_1.fastq.gz
wget https://ftp.sra.ebi.ac.uk/vol1/fastq/SRR258/003/SRR2584863/SRR2584863_2.fastq.gz

gunzip -f SRR2584863_1.fastq.gz
gunzip -f SRR2584863_2.fastq.gz

ls -lh SRR2584863_*.fastq
```

---

## Step 2 — Quality Control with FastQC

Before any analysis we inspect read quality. **FastQC** reads the FASTQ files and produces an HTML report with a series of quality metrics. We run it on both paired-end files.

| Module | What it tells you |
|--------|------------------|
| Per-base sequence quality | Is quality dropping at read ends? |
| Per-sequence quality scores | Are most reads high quality overall? |
| Adapter content | Are adapter sequences present? |
| Sequence duplication | Over-duplication suggests a library problem |

**Phred quality scores:**

| Q score | Error probability | Accuracy |
|---------|------------------|----------|
| Q20 | 1 in 100 | 99% |
| Q30 | 1 in 1,000 | 99.9% |
| Q40 | 1 in 10,000 | 99.99% |

For variant calling we want Q ≥ 20 for most bases. If quality drops at read ends we would trim using Trimmomatic or fastp before alignment.

```bash
%%bash
mkdir -p fastqc_output

fastqc SRR2584863_1.fastq SRR2584863_2.fastq \
    --outdir fastqc_output \
    --quiet

ls -lh fastqc_output/*.html
```

Print the text summary for each file:

```bash
%%bash
unzip -p fastqc_output/*_1*fastqc.zip '*/summary.txt' 2>/dev/null
```

```bash
%%bash
unzip -p fastqc_output/*_2*fastqc.zip '*/summary.txt' 2>/dev/null
```

Open the HTML reports in Colab using the Python cell below:

```python
from IPython.display import IFrame, display
import glob

for report in sorted(glob.glob('fastqc_output/*.html')):
    print(f'Showing: {report}')
    display(IFrame(report, width=950, height=650))
```

---

## Step 3 — Index the Reference and Align Reads with BWA-MEM

**Why index first?** Before BWA can align reads it builds an index of the reference genome — a data structure (the Burrows-Wheeler Transform) that allows it to very quickly locate any short sequence in the 4.6 million bp *E. coli* genome. You only need to do this once per reference.

We pass both FASTQ files to BWA at the same time. BWA aligns each pair together, which improves placement accuracy.

**SAM format** — the alignment output is a SAM file (Sequence Alignment/Map). One line per read, containing the chromosome, position, CIGAR string, sequence, and quality scores.

```
CIGAR string:
  250M       = 250 bp match/mismatch
  10M2I238M  = 10 matches, 2-base insertion, 238 matches
  10M2D240M  = 10 matches, 2-base deletion, 240 matches
```

### Build the index

```bash
%%bash
bwa index ecoli_rel606.fasta 2>&1
ls -lh ecoli_rel606.fasta.*
```

### Align the reads

```bash
%%bash
R1=$(ls SRR2584863*_1*.fastq* | head -1)
R2=$(ls SRR2584863*_2*.fastq* | head -1)

bwa mem -t 2 ecoli_rel606.fasta $R1 $R2 \
    > aligned.sam 2>bwa.log

cat bwa.log
```

### Preview the SAM file

```bash
%%bash
grep '^@' aligned.sam | head -5

grep -v '^@' aligned.sam | head -1 | \
    awk '{printf "Read : %s\nChr  : %s\nPos  : %s\nMapQ : %s\nCIGAR: %s\nSeq  : %s...\n",$1,$3,$4,$5,$6,substr($10,1,60)}'
```

---

## Step 4 — Sort, Convert and Index the BAM

After alignment reads are in sequencing order — essentially random across the genome. Downstream tools need reads sorted by genomic position.

**BAM** is the binary compressed version of SAM — typically 3–5x smaller and much faster to work with. The `.bai` index file lets tools jump directly to any genomic region without reading the whole file.

`samtools flagstat` gives a quick summary of alignment rates — always check this after alignment.

```bash
%%bash
samtools view -bS aligned.sam -o aligned.bam
samtools sort aligned.bam -o aligned_sorted.bam
samtools index aligned_sorted.bam

samtools flagstat aligned_sorted.bam
```

---

## Step 5 — Check Coverage

**Coverage** at a position = number of reads overlapping that base. For bacterial variant calling ~20–50x is usually sufficient. Higher coverage gives more confidence, especially for distinguishing real variants from sequencing errors.

`samtools depth` outputs three columns: chromosome, position, depth.

```bash
%%bash
samtools depth -a aligned_sorted.bam > coverage.txt

awk '
BEGIN{min=999999;max=0;sum=0;n=0;zeros=0}
{ n++; sum+=$3
  if($3<min) min=$3
  if($3>max) max=$3
  if($3==0)  zeros++ }
END{
  printf "Total positions  : %d\n", n
  printf "Mean coverage    : %.1fx\n", sum/n
  printf "Max coverage     : %dx\n", max
  printf "Zero-coverage bp : %d\n", zeros
}' coverage.txt
```

Plot coverage using Python (bash has no plotting capability):

```python
import matplotlib.pyplot as plt
import numpy as np

positions, depths = [], []
with open('coverage.txt') as f:
    for line in f:
        parts = line.strip().split('\t')
        if len(parts) == 3:
            positions.append(int(parts[1]))
            depths.append(int(parts[2]))

step = 100
fig, ax = plt.subplots(figsize=(14, 4))
ax.fill_between(positions[::step], depths[::step], alpha=0.5, color='steelblue')
ax.axhline(np.mean(depths), color='red', linestyle='--', linewidth=1,
           label=f'Mean: {np.mean(depths):.1f}x')
ax.axhline(20, color='orange', linestyle=':', linewidth=1, label='20x minimum')
ax.set_xlabel('Position in E. coli REL606 genome (bp)')
ax.set_ylabel('Coverage depth')
ax.set_title('Read Coverage Across the E. coli Genome', fontweight='bold')
ax.legend()
plt.tight_layout()
plt.savefig('coverage_plot.png', dpi=150, bbox_inches='tight')
plt.show()
```

---

## Step 6 — Call Variants with freebayes

At every position the variant caller counts reads showing each base, compares to the reference, and uses a statistical model to decide: **real variant** or **sequencing error**?

**freebayes** is a Bayesian variant caller. For a haploid organism like *E. coli* we use `--ploidy 1` — there is only one copy of the genome, so every real variant should appear in nearly 100% of reads at that position.

**VCF format:**

```
#CHROM      POS    ID  REF  ALT  QUAL  FILTER  INFO
CP000819.1  12345  .   C    T    850   .       DP=45;AF=0.98
```

- **REF** = reference base
- **ALT** = base found in our sample
- **QUAL** = confidence score (higher = better)
- **DP** = depth at this position
- **AF** = allele frequency (close to 1.0 for a haploid fixed mutation)

```bash
%%bash
./freebayes \
    -f ecoli_rel606.fasta \
    --ploidy 1 \
    aligned_sorted.bam \
    > variants.vcf

grep -c '^#' variants.vcf
grep -vc '^#' variants.vcf
grep -v '^#' variants.vcf | head -5
```

---

## Step 7 — Filter Variants

Raw calls include low-confidence positions. We filter using `awk`.

| Filter | Threshold | Reason |
|--------|-----------|--------|
| Minimum depth (DP) | >= 10 | Too few reads = unreliable |
| Minimum allele frequency (AF) | >= 0.95 | Haploid: real mutations should be in nearly all reads |
| Minimum quality (QUAL) | >= 20 | Low QUAL = low confidence |

Note the high AF threshold (0.95). Unlike cancer — where somatic mutations may be in 30–70% of reads — *E. coli* is haploid so a real fixed mutation should be present in essentially all reads at that position.

```bash
%%bash
{
  grep '^#' variants.vcf
  grep -v '^#' variants.vcf | awk '
  {
    qual=$6+0; dp=0; af=0
    n=split($8,info,";")
    for(i=1;i<=n;i++){
      if(info[i]~/^DP=/) dp=substr(info[i],4)+0
      if(info[i]~/^AF=/) af=substr(info[i],4)+0
    }
    if(dp>=10 && af>=0.95 && qual>=20) print
  }'
} > variants_filtered.vcf

grep -vc '^#' variants.vcf
grep -vc '^#' variants_filtered.vcf
```

Print a readable summary of the filtered variants:

```bash
%%bash
grep -v '^#' variants_filtered.vcf | awk '
{
  pos=$2; ref=$4; alt=$5; qual=$6; dp=0; af=0
  n=split($8,info,";")
  for(i=1;i<=n;i++){
    if(info[i]~/^DP=/) dp=substr(info[i],4)+0
    if(info[i]~/^AF=/) af=substr(info[i],4)+0
  }
  printf "Pos %-10s  %s->%-4s  depth=%-5s  AF=%.2f  QUAL=%s\n",
    pos, ref, alt, dp, af, qual
}'
```

Visualise the results using Python:

```python
import matplotlib.pyplot as plt

variants = []
with open('variants_filtered.vcf') as f:
    for line in f:
        if line.startswith('#'):
            continue
        parts = line.strip().split('\t')
        if len(parts) < 8:
            continue
        info = {k: v for k, v in
                (item.split('=') for item in parts[7].split(';') if '=' in item)}
        variants.append({
            'pos':  int(parts[1]),
            'ref':  parts[3],
            'alt':  parts[4],
            'qual': float(parts[5]) if parts[5] != '.' else 0,
            'dp':   int(info.get('DP', 0)),
            'af':   float(info.get('AF', 0))
        })

if variants:
    fig, axes = plt.subplots(1, 2, figsize=(14, 4))

    ax1 = axes[0]
    ax1.bar(range(len(variants)), [v['af'] for v in variants],
            color='steelblue', edgecolor='white')
    ax1.set_xticks(range(len(variants)))
    ax1.set_xticklabels([f"pos {v['pos']}" for v in variants],
                        rotation=45, ha='right', fontsize=7)
    ax1.axhline(1.0, linestyle='--', color='gray', linewidth=1,
                label='AF=1.0 (fixed mutation)')
    ax1.set_ylabel('Allele Frequency')
    ax1.set_title('Allele Frequency per Variant')
    ax1.set_ylim(0, 1.1)
    ax1.legend(fontsize=8)

    ax2 = axes[1]
    ax2.bar(range(len(variants)), [v['dp'] for v in variants],
            color='steelblue', edgecolor='white')
    ax2.set_xticks(range(len(variants)))
    ax2.set_xticklabels([f"pos {v['pos']}" for v in variants],
                        rotation=45, ha='right', fontsize=7)
    ax2.axhline(20, linestyle='--', color='green', linewidth=1, label='20x minimum')
    ax2.set_ylabel('Read Depth')
    ax2.set_title('Coverage Depth at Each Variant')
    ax2.legend(fontsize=8)

    plt.suptitle('Filtered Variant Summary', fontweight='bold')
    plt.tight_layout()
    plt.savefig('variant_summary.png', dpi=150, bbox_inches='tight')
    plt.show()
```

---

## Step 8 — Look Up Variants in NCBI

We have a list of positions where our evolved *E. coli* differs from the REL606 ancestor. We query the **NCBI Entrez API** using `curl` to find which genes these positions fall in — the same NCBI database from your earlier browser sessions, now accessed from the command line.

The *E. coli* REL606 genome is annotated under accession **CP000819.1**.

```bash
%%bash
POSITIONS=$(grep -v '^#' variants_filtered.vcf | awk '{print $2}' | head -3)

for POS in $POSITIONS; do
    echo "--- Position $POS ---"
    curl -s "https://eutils.ncbi.nlm.nih.gov/entrez/eutils/efetch.fcgi?db=nuccore&id=CP000819.1&rettype=ft&retmode=text&seq_start=$((POS-1))&seq_stop=$((POS+1))" \
        | grep -A2 'gene\|CDS\|product' | head -10
    echo ''
    sleep 0.4
done
```

---

## Step 9 — Summary and Discussion

### The complete pipeline

```bash
# Download
wget [reference URL] > ecoli_rel606.fasta.gz && gunzip ecoli_rel606.fasta.gz
wget ftp://ftp.sra.ebi.ac.uk/.../SRR2584863_1.fastq.gz && gunzip
wget ftp://ftp.sra.ebi.ac.uk/.../SRR2584863_2.fastq.gz && gunzip

# QC
fastqc SRR2584863_1.fastq SRR2584863_2.fastq --outdir fastqc_output/

# Index and align
bwa index ecoli_rel606.fasta
bwa mem -t 2 ecoli_rel606.fasta SRR2584863_1.fastq SRR2584863_2.fastq > aligned.sam

# Sort and index
samtools view -bS aligned.sam | samtools sort -o aligned_sorted.bam
samtools index aligned_sorted.bam
samtools flagstat aligned_sorted.bam

# Coverage
samtools depth -a aligned_sorted.bam > coverage.txt

# Variant calling
./freebayes -f ecoli_rel606.fasta --ploidy 1 aligned_sorted.bam > variants.vcf

# Filter
awk 'DP>=10 && AF>=0.95 && QUAL>=20' variants.vcf > variants_filtered.vcf
```

### What we found

| Item | Value |
|------|-------|
| Dataset | SRR2584863 — Lenski LTEE evolved *E. coli* |
| Reference | *E. coli* REL606 ancestor (CP000819.1) |
| Ploidy | Haploid (1 chromosome) |
| Expected AF | ~1.0 for fixed mutations |

These variants are real evolutionary changes that arose over thousands of generations and were fixed in the population because they increased fitness.

---

### Discussion questions

1. **Haploid vs diploid:** We set `--ploidy 1` for *E. coli*. What would change if we were calling variants in a human sample (diploid, ploidy 2)? How would the expected allele frequencies differ?

2. **Allele frequency ~1.0:** Most of our variants have AF close to 1.0. What does this tell you about when in the evolutionary history of this population the mutation occurred?

3. **FastQC:** Look at your FastQC reports. Were there any warnings or failures? What would you do before alignment if quality had dropped significantly at the 3' ends of reads?

4. **Coverage plot:** Was coverage uniform across the genome? What biological or technical reasons could cause some regions to have lower coverage than others?

5. **Connecting to databases:** We used the NCBI Entrez API to find which genes our variants fall in. How is this different from — and complementary to — the browser-based database navigation you did earlier in the course?

6. **Scale and species:** This pipeline worked on a 4.6 Mb bacterial genome. What would need to change to run the same pipeline on a 3 Gb human genome?

---

### Output files

| File | Contents |
|------|----------|
| `ecoli_rel606.fasta` | Reference genome |
| `SRR2584863_*.fastq` | Raw paired-end reads |
| `fastqc_output/` | FastQC HTML reports |
| `aligned_sorted.bam` | Sorted indexed alignment |
| `coverage.txt` | Per-position depth |
| `coverage_plot.png` | Coverage visualisation |
| `variants.vcf` | Raw freebayes variant calls |
| `variants_filtered.vcf` | Filtered high-confidence calls |
| `variant_summary.png` | AF and depth plots |

---

*BSc Bioinformatics — NGS Variant Calling Tutorial*

*Dataset: Data Carpentry Genomics curriculum, Lenski Long-Term Evolution Experiment.*

*Reference: Tenaillon et al. (2016) Science — Tempo and mode of genome evolution in a 50,000-generation experiment.*
