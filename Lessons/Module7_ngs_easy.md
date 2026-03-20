# Introduction to Sequencing Data
### A Beginner's Tutorial — Google Colab

---

## What are we doing and why?

When scientists sequence DNA, the sequencing machine produces millions of short DNA reads stored in a file format called **FASTQ**. Before doing anything with this data, we need to ask: *is the data any good?*

In this tutorial you will:
1. Download a real FASTQ file from a famous evolution experiment
2. Open it and understand what it contains
3. Run a quality control tool called **FastQC** to check the data quality
4. Interpret the results

That's it — no complex pipelines, just the first and most important step in any sequencing analysis.

---

## The data

We are using data from the **Lenski Long-Term Evolution Experiment**. Since 1988, Richard Lenski has been growing *E. coli* bacteria every day and watching them evolve in real time — over 60,000 generations so far. We have Illumina sequencing reads from one of these evolved strains.

---

## Set up Google Colab

1. Go to [colab.research.google.com](https://colab.research.google.com)
2. Click **New notebook**
3. You will see an empty cell — this is where you type commands
4. To run a cell: press **Shift + Enter**

> 💡 **Tip:** In Colab, cells starting with `%%bash` run as terminal commands. Everything else is Python.

---

## Step 1 — Install FastQC

FastQC is not installed by default in Colab. Run this cell to install it:

```bash
%%bash
apt-get install -y -q fastqc 2>/dev/null
fastqc --version
```

You should see something like `FastQC v0.11.9`. If you do, it worked.

---

## Step 2 — Download the Data

We will download just one FASTQ file (the first of a paired-end set). This file contains sequencing reads from our evolved *E. coli* strain.

```bash
%%bash
wget -q ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR258/003/SRR2584863/SRR2584863_1.fastq.gz
gunzip -f SRR2584863_1.fastq.gz
echo "Done! File is ready."
```

Check it downloaded correctly:

```bash
%%bash
ls -lh SRR2584863_1.fastq
```

You should see a file around 200–300 MB.

---

## Step 3 — Look Inside the FASTQ File

A FASTQ file stores sequencing reads. Every single read takes up exactly **4 lines**:

```
Line 1:  @SRR2584863.1       ← the read name (always starts with @)
Line 2:  ATCGATCGATCG...     ← the DNA sequence
Line 3:  +                   ← a separator (always just a + sign)
Line 4:  IIIIIIIIIIIII...    ← quality scores (one character per base)
```

Let's look at the first read in our file:

```bash
%%bash
head -4 SRR2584863_1.fastq
```

**🔍 Exercise 1:** Look at lines 2 and 4. Line 2 is the DNA sequence and line 4 is the quality scores. Do they have the same number of characters? Why do you think that is?

---

### How many reads are in the file?

Because every read is exactly 4 lines, we can count reads by counting lines and dividing by 4:

```bash
%%bash
wc -l SRR2584863_1.fastq
```

**🔍 Exercise 2:** Take the number of lines and divide by 4. How many reads are in the file?

---

### What do the quality scores mean?

Each character in line 4 represents the confidence the sequencing machine had when reading that base. This is called a **Phred quality score**.

| Q score | Accuracy | Error rate |
|---------|----------|------------|
| Q10     | 90%      | 1 in 10    |
| Q20     | 99%      | 1 in 100   |
| Q30     | 99.9%    | 1 in 1,000 |
| Q40     | 99.99%   | 1 in 10,000|

The scores are encoded as ASCII characters — the letter `I` represents Q40, the letter `!` represents Q0. For good Illumina data we want most bases to be Q20 or above.

**🔍 Exercise 3:** Look at the quality line of your read. Are the characters near the beginning and end of the read different? What might this tell you about where quality tends to drop off?

---

## Step 4 — Run FastQC

Inspecting reads one at a time tells us very little. **FastQC** summarises quality across all reads in the file and produces a visual HTML report.

```bash
%%bash
mkdir -p fastqc_output
fastqc SRR2584863_1.fastq --outdir fastqc_output --quiet
echo "FastQC finished!"
ls fastqc_output/
```

You should see two output files: a `.html` report and a `.zip` archive.

---

## Step 5 — View the FastQC Report

Run this Python cell to open the HTML report directly in Colab:

```python
from IPython.display import IFrame, display
import glob

for report in sorted(glob.glob('fastqc_output/*.html')):
    print(f'Opening: {report}')
    display(IFrame(report, width=950, height=650))
```

---

## Step 6 — Interpret the Report

FastQC produces several quality modules. Here are the most important ones for beginners:

### Per Base Sequence Quality

This is the most important plot. It shows the quality score at each position along the read.

- **Green zone (Q ≥ 28):** great quality
- **Orange zone (Q 20–28):** acceptable
- **Red zone (Q < 20):** poor quality

It is normal for quality to drop slightly at the **end of reads** — this is a known property of Illumina sequencing chemistry.

**🔍 Exercise 4:** Does quality drop at the ends of reads in your data? At roughly what position does it start to fall?

---

### Per Sequence Quality Scores

This shows the overall quality of each read as a single number. You want a sharp peak at the high end (Q30+). A broad spread towards low values means many reads are poor quality overall.

**🔍 Exercise 5:** Where is the peak in your data? Is the distribution narrow or broad?

---

### Sequence Length Distribution

Shows how long the reads are. For Illumina data you typically expect all reads to be the same length (e.g. 150 bp or 250 bp).

---

### Adapter Content

Adapters are short synthetic DNA sequences added during library preparation. If they appear in your reads it means some reads are shorter than expected and the sequencer read into the adapter. They need to be removed before alignment.

**🔍 Exercise 6:** Is there any adapter contamination in your data?

---

### The PASS / WARN / FAIL Summary

At the top of the report you will see a summary with green ticks, orange exclamation marks, and red crosses. A **WARN** or **FAIL** does not always mean the data is unusable — it depends on the module and your downstream analysis. FastQC was designed for many types of sequencing data, and some modules that flag bacterial samples are expected behaviour, not problems.

**🔍 Exercise 7:** List any modules that show WARN or FAIL in your report. For each one, discuss with a neighbour whether you think it is a real problem or expected behaviour.

---

## Summary

In this tutorial you have:

| Step | What you did |
|------|-------------|
| Step 1 | Installed FastQC in Colab |
| Step 2 | Downloaded a real FASTQ file from a genome sequencing experiment |
| Step 3 | Explored the FASTQ format — 4 lines per read, quality scores, Phred encoding |
| Step 4 | Ran FastQC to summarise quality across all reads |
| Step 5–6 | Opened and interpreted the HTML report |

### Key concepts to remember

- FASTQ is the standard format for raw sequencing reads
- Every read = 4 lines: name, sequence, separator, quality scores
- Quality scores tell you how confident the machine was at each base
- FastQC gives a quick visual overview of data quality
- Checking quality is always the **first step** before any analysis

---

## Discussion questions

1. Why do you think quality tends to drop at the end of Illumina reads?
2. If you found that quality was very poor across the whole read — not just at the ends — what might that tell you about the experiment?
3. We only looked at one of the two FASTQ files (the `_1` file). Why do you think the data comes in two files?
4. We have reads from an *E. coli* genome of about 4.6 million bases. If you have ~500,000 reads each 250 bases long, roughly how many times is each position in the genome covered? Is that enough?

---

*BSc Bioinformatics — Introduction to Sequencing Data*

*Dataset: Lenski Long-Term Evolution Experiment (SRR2584863), via the European Nucleotide Archive.*
