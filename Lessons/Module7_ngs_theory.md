# NGS Variant Calling — Theory Behind the Pipeline

*BSc Bioinformatics — Background Reading*

---

## Overview

Before running any code it is worth understanding **why** each step exists and what problem it solves. Every tool in this pipeline was built to answer a specific question about sequencing data. This document explains the theory behind each step in the order you will encounter it.

---

## 1. Quality Control — FastQC

### What is a Phred quality score?

When an Illumina sequencer reads a base, it does not just report the base — it also reports how confident it is. This confidence is encoded as a **Phred quality score (Q)**:

```
Q = -10 × log₁₀(P)
```

where P is the probability that the base call is **wrong**.

| Q score | Error probability | 1 in... | Accuracy |
|---------|------------------|---------|----------|
| Q10 | 0.1 | 10 | 90% |
| Q20 | 0.01 | 100 | 99% |
| Q30 | 0.001 | 1,000 | 99.9% |
| Q40 | 0.0001 | 10,000 | 99.99% |

A Q30 base call is wrong roughly once every 1,000 reads. For variant calling this matters enormously — if a base has Q20 quality, there is a 1% chance it is wrong. If that position has 50x coverage, you might expect ~0.5 reads to show a false base just from sequencing error alone. A real variant present in all reads will dominate, but a real low-frequency variant (say 2% allele frequency) becomes impossible to distinguish from noise without high coverage and strict quality filtering.

### Why does quality drop at the ends of reads?

Illumina sequencing works by incorporating fluorescently labelled nucleotides one at a time and imaging the fluorescence. After many cycles, the fluorescent signal accumulates **noise and dephasing** — individual molecules in the cluster fall out of sync, making the signal weaker and less distinct. This is why you almost always see quality scores drop at the 3' end of reads. It is a physical limitation of the chemistry, not a sign that something went wrong.

### What FastQC actually checks

FastQC runs a series of modules and flags each as PASS, WARN, or FAIL:

**Per-base sequence quality** — the most important plot. Shows mean Phred score at each position across all reads. You want to see scores consistently above Q28–30. A drop at the 3' end is normal; a drop in the middle suggests a technical problem.

**Per-sequence quality scores** — distribution of the mean quality per read. A well-sequenced library shows a sharp peak at high Q values. A broad distribution or a secondary peak at low Q suggests a mixed-quality run.

**Per-base sequence content** — in random DNA, A:T and G:C should be roughly equal at each position. The first ~10 bases often look biased in RNA-seq due to random hexamer priming, but for DNA sequencing this should be flat across the read.

**Adapter content** — adapter sequences are short synthetic DNA fragments ligated to the ends of fragments during library preparation. If your fragment is shorter than the read length, the sequencer will read into the adapter. These adapter sequences need to be removed before alignment because they are not biological sequence and will cause reads to fail to map.

**Sequence duplication levels** — PCR amplification during library prep can create multiple identical copies of the same original fragment. High duplication is not always a problem (it depends on the application) but it inflates apparent coverage without adding real information.

---

## 2. Alignment — BWA-MEM

### The core problem

You have millions of short reads (~150 bp each) and a reference genome of millions or billions of base pairs. The question is: **where in the genome did each read come from?** This is the alignment problem.

The naive approach — sliding each read along the entire genome and counting mismatches — would be astronomically slow. A 150 bp read compared against the 4.6 Mb *E. coli* genome requires ~4.6 million comparisons per read. With 10 million reads that is 46 trillion comparisons. At one comparison per nanosecond that would take over a day per sample. For a human genome (3 billion bp) it becomes impossible.

### The Burrows-Wheeler Transform

BWA solves this with an index built from the **Burrows-Wheeler Transform (BWT)**, a data structure that allows any substring to be located in a reference in time proportional to the length of the substring, not the length of the reference.

The BWT works by cyclically rotating the reference string and sorting all rotations. The sorted matrix has a special property: it can be searched using **backwards search**, where you extend a match one character at a time from right to left. Each extension either narrows the set of possible positions or eliminates them entirely. This allows a 150 bp read to be located in the genome in ~150 operations rather than ~4,600,000.

You build this index once (`bwa index`) and reuse it for all samples aligned to the same reference.

### What BWA-MEM does

`bwa mem` (Maximal Exact Matches) is the algorithm used for reads of ~70 bp or longer. It:

1. **Seeds** — finds maximal exact matches between the read and the reference using the BWT index. These are short stretches where the read matches the reference perfectly.
2. **Chains** — groups seeds that are co-linear (appear in the same order and orientation in both read and reference), suggesting they come from the same alignment.
3. **Extends** — fills in the gaps between seeds using Smith-Waterman dynamic programming, which can handle mismatches and small insertions/deletions.
4. **Scores and selects** — assigns a score to each possible alignment and selects the best one. If a read could align equally well in two places, it is marked as **multi-mapping** and given a low mapping quality score (MAPQ = 0).

### The SAM/BAM format

The output of BWA is a **SAM file** (Sequence Alignment/Map). Each line represents one read:

```
read_42   0   CP000819.1   9972   60   150M   *   0   0   ATCGATCG...   IIIIIIII...
          ^   ^            ^      ^    ^
        flag  chromosome  pos   mapQ CIGAR
```

**Flag** — a bitwise integer encoding alignment properties. Flag 0 = forward strand mapped. Flag 16 = reverse strand. Flag 4 = unmapped. Flag 2 = read mapped in proper pair.

**MAPQ** — mapping quality. Like Phred, it is `-10 × log₁₀(P)` where P is the probability the read is mapped to the wrong position. MAPQ 60 means one in a million chance of being wrong. MAPQ 0 means the read mapped equally well to multiple places.

**CIGAR string** — a compact description of how the read aligns:

```
150M       = 150 matches or mismatches (no gaps)
10M2I138M  = 10 bp match, 2 bp insertion in the read, 138 bp match
10M2D140M  = 10 bp match, 2 bp deletion in the read, 140 bp match
5S145M     = 5 bp soft-clipped (ignored), 145 bp match
```

**BAM** is the binary compressed version of SAM. It is typically 3–5x smaller and required by tools like samtools and freebayes. Sorting the BAM by genomic position and building an index (`.bai` file) allows tools to jump directly to any region of the genome — like a book index — without reading the whole file from start to finish.

### What flagstat tells you

`samtools flagstat` reports:
- **Total reads** — how many reads were in the input
- **Mapped reads** — what fraction aligned to the reference
- **Properly paired** — pairs where both reads aligned in the expected orientation and distance
- **Singletons** — one read of a pair mapped, the other did not

For a good quality library you expect >90% of reads to map. A low mapping rate suggests the wrong reference was used, the sample is contaminated, or the library quality was poor.

---

## 3. Coverage

### Why coverage matters

**Coverage** (also called **depth**) at a position is simply the count of reads overlapping that base. It is the single most important factor determining whether a variant call is reliable.

Consider a position with true variant allele frequency of 50% (heterozygous in a diploid). If only 4 reads cover that position, you expect 2 reads to show the reference and 2 to show the variant — but by chance you might see 4:0 or 0:4, causing you to either miss the variant or call a false homozygous variant. With 30 reads covering the position you expect 15:15, and the binomial probability of seeing 0 variant reads by chance is vanishingly small.

For haploid bacteria like *E. coli*, every real variant should appear in nearly 100% of reads. The question becomes: do I have enough reads to call this confidently against the background noise of sequencing errors (~0.1% per base)?

**Coverage calculation:**

```
Expected coverage = (read length × number of reads) / genome size
```

For this tutorial: 150 bp reads, ~10 million reads, 4.6 Mb genome → ~326x mean coverage. This is more than sufficient for confident variant calling.

### Non-uniform coverage

Coverage is never perfectly uniform. Regions that are:
- **AT-rich** are harder to amplify by PCR during library preparation (AT bonds are weaker than GC bonds)
- **Repetitive** cause reads to map to multiple locations (low MAPQ, often filtered out)
- **At the ends of contigs** have fewer overlapping reads by definition

This is why `samtools depth -a` with the `-a` flag (report all positions, including zero) is important — you want to see the full picture, not just positions that have coverage.

---

## 4. Variant Calling — freebayes

### The statistical problem

At a given position, you observe a pile of bases from your reads. Some say A, some say T. How do you decide whether this is a real variant or just sequencing error?

The naive approach — call a variant if more than X% of reads show an alternative base — is fragile. It does not account for read quality, mapping quality, strand bias, or local sequence context.

**freebayes** uses a **Bayesian probabilistic model**. It asks:

> Given the observed reads at this position, what is the most likely genotype?

Using Bayes' theorem:

```
P(genotype | data) ∝ P(data | genotype) × P(genotype)
```

- **P(data | genotype)** — the likelihood: how probable are the observed bases given that the true genotype is G? This incorporates base quality (via the Phred scores) and mapping quality.
- **P(genotype)** — the prior: what genotypes are expected? For a haploid organism, the prior strongly favours either all-reference or all-alternative, not 50:50.

freebayes computes this probability for all possible genotypes and reports the most likely one, along with a quality score (QUAL) that is `-10 × log₁₀` of the probability that the call is wrong.

### VCF format in detail

The Variant Call Format (VCF) is the standard output:

```
#CHROM      POS    ID  REF  ALT  QUAL  FILTER  INFO                FORMAT  SAMPLE
CP000819.1  9972   .   G    T    1763  .       DP=126;AF=1.000     GT:DP   1:126
```

**CHROM and POS** — chromosome and 1-based position of the variant.

**REF and ALT** — the reference base and the alternative base(s) observed. For SNPs these are single bases. For indels they can be longer strings.

**QUAL** — Phred-scaled probability that the variant call is wrong. QUAL 30 means 1 in 1,000 chance. QUAL 1763 is extremely confident.

**INFO field** — a semicolon-separated list of key=value pairs:
- `DP` — total depth at this position
- `AF` — allele frequency of the alternative allele
- `SB` — strand bias (if reads only show the variant on one strand, it is suspicious)
- `MQM` — mean mapping quality of reads supporting the variant

**GT (genotype)** — `0` = reference, `1` = first alternative. For haploid: `0` = reference homozygous, `1` = variant. For diploid: `0/0` = homozygous reference, `0/1` = heterozygous, `1/1` = homozygous alternative.

### Why ploidy matters

The `--ploidy 1` flag tells freebayes to expect a haploid genome. This changes the prior:

- **Haploid (ploidy 1):** only two possible states — reference (AF = 0) or variant (AF = 1.0). Any read showing the reference is either correct or a sequencing error on a variant read. Expected AF for a real fixed mutation: ~1.0.
- **Diploid (ploidy 2):** three possible states — homozygous reference (AF = 0), heterozygous (AF = 0.5), homozygous alternative (AF = 1.0). Expected AF for a heterozygous SNP: ~0.5.

Getting ploidy wrong leads to systematic errors. Calling a haploid sample with `--ploidy 2` would cause freebayes to look for 50:50 splits and potentially miss fixed mutations that appear in 100% of reads.

---

## 5. Variant Filtering

### Why filter?

freebayes reports every position that looks even slightly different from the reference, including:
- Positions with only 2 reads covering them (low confidence)
- Positions where only 1 in 50 reads shows the variant (likely sequencing error)
- Positions in repetitive regions with poor mapping quality

Filtering removes these low-confidence calls and keeps only variants you can trust.

### The three standard filters

**Minimum depth (DP ≥ 10)**

With fewer than 10 reads at a position, the statistics are unreliable. Even a 100% allele frequency at a position covered by 3 reads could be a coincidence — three reads happening to carry the same sequencing error. Requiring at least 10 reads is a minimal safety net.

**Minimum allele frequency (AF ≥ 0.95 for haploid)**

For a haploid fixed mutation, every read should carry the variant. In practice you expect AF of 0.97–1.0. A position where only 60% of reads show the variant in a haploid organism is either:
- A heterogeneous population (mixed strains)
- A sequencing or mapping artefact
- A genuine low-frequency variant in a mixed sample

Setting AF ≥ 0.95 retains high-confidence fixed mutations while discarding noise.

For a cancer somatic variant or a diploid heterozygous SNP, this threshold would be completely wrong — you would want AF ≥ 0.20 or similar.

**Minimum quality (QUAL ≥ 20)**

QUAL 20 means a 1 in 100 chance the call is wrong. This is a low threshold — QUAL 30 or higher is often used in production pipelines. For a tutorial dataset with high coverage it is sufficient.

### Why these thresholds are not universal

The correct thresholds depend entirely on the application:

| Application | DP | AF | QUAL |
|-------------|----|----|------|
| *E. coli* haploid | ≥10 | ≥0.95 | ≥20 |
| Human germline | ≥20 | ≥0.25 | ≥30 |
| Cancer somatic | ≥50 | ≥0.05 | ≥20 |
| Liquid biopsy | ≥1000 | ≥0.01 | ≥20 |

A cancer somatic variant in 5% of reads is real and clinically important. The same 5% allele frequency in a bacterial dataset is almost certainly noise.

---

## 6. Variant Annotation — NCBI Entrez

### From position to biology

A VCF entry tells you: *at position 9,972, the base is T instead of G*. This is a genomic coordinate. To understand the biological significance you need to know:

- What gene is at position 9,972?
- Is it in a coding region, a promoter, an intergenic region?
- What protein does that gene encode?
- What does the amino acid change look like?
- Has this exact mutation been seen before?

The NCBI Entrez API allows you to query the annotated genome record (CP000819.1 for *E. coli* REL606) and retrieve the gene feature table for any position. This is the same annotation you would see if you opened the genome in NCBI's browser — you are just querying it programmatically.

In a real pipeline, variant annotation is handled by dedicated tools such as **SnpEff** or **VEP (Variant Effect Predictor)**, which annotate every variant with predicted effects on transcripts and proteins. For this tutorial, querying NCBI directly demonstrates the principle.

---

## Summary — What each step is protecting against

| Step | Problem it solves |
|------|------------------|
| FastQC | Identifies low-quality reads or adapters that would cause misalignments or false variants |
| BWA index | Makes alignment computationally feasible at scale |
| BWA-MEM | Places each read at its correct genomic origin, tolerating real variants and errors |
| SAM → BAM sort/index | Enables fast random access to any genomic region |
| samtools depth | Reveals regions of insufficient coverage where variant calls cannot be trusted |
| freebayes | Distinguishes real variants from sequencing errors using probabilistic modelling |
| VCF filtering | Removes low-confidence calls that passed the caller but fail basic quality thresholds |
| NCBI annotation | Converts genomic coordinates into biological meaning |

---

*BSc Bioinformatics — NGS Variant Calling Tutorial*

*Further reading: Langmead & Salzberg (2012) Nature Methods — Fast gapped-read alignment with Bowtie 2. Garrison & Marth (2012) arXiv — Haplotype-based variant detection from short-read sequencing.*
