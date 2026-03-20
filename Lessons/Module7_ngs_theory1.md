# Module 7: Introduction to Genomics and Sequencing Technologies


---

## Table of Contents

- [1.1 What is Genomics?](#11-what-is-genomics)
- [1.2 From Sanger Sequencing to Next Generation Sequencing](#14-from-sanger-sequencing-to-next-generation-sequencing)
- [1.3 Major NGS Platforms](#15-major-ngs-platforms)
- [1.4 Choosing the Right Sequencing Technology](#16-choosing-the-right-sequencing-technology)

---

## 1.1 What is Genomics?

Genomics is the branch of molecular biology that studies the complete set of DNA — the genome — of an organism. Unlike classical genetics, which focuses on individual genes and their roles in inheritance, genomics takes a broader view, examining the structure, function, evolution, and mapping of all genes and non-coding sequences together. The human genome contains approximately **3.2 billion base pairs** of DNA packed into 23 pairs of chromosomes, encoding roughly **20,000–25,000 protein-coding genes**. Remarkably, these genes account for only about 2% of the total genome; the remaining 98% consists of regulatory elements, introns, repetitive sequences, and regions whose functions are still being actively investigated.

Genomics has transformed biological research by shifting the focus from studying one gene at a time to interrogating entire genomes simultaneously. This shift has been made possible largely by advances in DNA sequencing technologies, which have become faster, cheaper, and more accurate over the past four decades.

---

## 1.2 From Sanger Sequencing to Next Generation Sequencing

### Sanger Sequencing (First Generation)

https://www.youtube.com/watch?v=X9566yI2cBo

The story of DNA sequencing begins with Frederick Sanger, who developed chain-termination sequencing in **1977** — a method that earned him his second Nobel Prize. The principle relies on incorporating **dideoxynucleotides (ddNTPs)** — modified bases that lack the 3'-OH group required for chain elongation. 
- In Sanger sequencing, special bases (Dideoxynucleotides) stop DNA synthesis at random positions, producing fragments of different lengths.
- Each fragment tells you the identity of the last base
- By ordering fragments by size (through gel electrophoresis), the DNA sequence can be reconstructed 

Modern automated Sanger sequencing replaced the radioactive gels with fluorescently labelled ddNTPs and capillary electrophoresis, allowing sequencing of a single fragment up to about **600–1000 base pairs** in a single run.

Sanger sequencing is still used today for:

- Validating variants discovered by NGS
- Sequencing short, targeted regions (e.g. checking a PCR product)
- Clinical confirmation of pathogenic mutations before reporting

Its fundamental limitation is **throughput** — it can only sequence one fragment at a time, making it impractical for sequencing entire genomes at scale.

### The NGS Revolution

Next Generation Sequencing (also called second-generation or massively parallel sequencing) emerged in the mid-2000s and fundamentally changed the field. Rather than sequencing one DNA fragment at a time, NGS sequences **millions of fragments simultaneously** in a single instrument run. This massive parallelisation reduced the cost of sequencing a human genome from approximately **$3 billion** (Human Genome Project, completed 2003) to under **$1,000** today — a cost reduction faster than Moore's Law.

- The key conceptual difference is that NGS breaks the DNA into many small fragments, sequences them all in parallel, and then uses computational alignment to reassemble the sequence by mapping each short read back to a reference genome.
- This introduces two important trade-offs: reads are much shorter than Sanger reads (typically **75–300 bp** for Illumina), and the analysis requires significant bioinformatics expertise.

---

## 1.3 Major NGS Platforms

### Illumina — Sequencing by Synthesis (SBS)

https://www.youtube.com/watch?v=CZeN-IgjYCo

Illumina is by far the most widely used NGS platform globally, dominating both research and clinical sequencing. 
- Its chemistry is based on **reversible terminator sequencing by synthesis**.
- DNA fragments are attached to a flow cell surface and amplified locally to form dense clusters.
- During each sequencing cycle, fluorescently labelled reversible terminator nucleotides are incorporated one at a time.
- The fluorescence is imaged, the terminator is chemically removed, and the cycle repeats.
- This generates highly accurate short reads (75–300 bp) with error rates as low as **0.1%**.

Illumina instruments range from the small **MiSeq** (suitable for small genomes or targeted panels) to the ultra-high-throughput **NovaSeq X Plus**, capable of sequencing over 20,000 whole human genomes per year. 

### Oxford Nanopore Technologies (ONT)

Nanopore sequencing represents a fundamentally different approach. 
- DNA or RNA is passed through a biological nanopore (a protein channel) embedded in a membrane.
- As individual bases move through the pore, they cause characteristic disruptions to an electrical current flowing across the membrane. --- These current signatures are decoded by a neural-network-based base caller into nucleotide sequences.

The major advantage of nanopore sequencing is **read length** — reads can span from a few kilobases to over **4 megabases**, making it exceptional for resolving repetitive regions, structural variants, and complete chromosome-scale assemblies. The **MinION** device is roughly the size of a USB stick and has been used in field settings and even on the International Space Station. The main limitation has historically been a higher raw error rate (~5–10%), though recent chemistry and basecalling improvements have substantially closed this gap.

### PacBio — Single Molecule Real Time (SMRT) Sequencing

PacBio is real-time continuous observation of polymerase activity,synthesise DNA in real time. 
- DNA fragments are converted into a circular molecule called a SMRTbell template.
- Each DNA molecule sits in a nanoscopic well called a Zero-Mode Waveguide (ZMW).
- A DNA polymerase enzyme is attached at the bottom of the well. It begins synthesizing a new strand by adding nucleotides (A, T, G, C).
- Each nucleotide (A, T, G, C) is labeled with a different fluorescent dye and when each nucleotide is incorporated, it emits a flash of light

The newer **HiFi (CCS) mode** sequences the same circular DNA molecule multiple times, generating consensus reads with accuracy exceeding **99.9%**, combining the length of long reads with the accuracy approaching short reads. PacBio HiFi is increasingly used for phased genome assembly, structural variant detection, and full-length isoform sequencing (Iso-Seq).

### Ion Torrent

Ion Torrent sequencing detects **hydrogen ions** released during DNA synthesis rather than fluorescence or electrical impulses. As a nucleotide is incorporated, a proton is released, causing a measurable change in pH that is detected by a semiconductor chip. The technology is faster and less expensive to instrument than optical methods but generates shorter reads (200–600 bp) with a higher error rate in homopolymer regions. Ion Torrent is commonly used in clinical settings for targeted sequencing panels, such as oncology panels testing a defined set of cancer-associated variants.

### Platform Comparison Summary

| Platform | Read Length | Accuracy | Throughput | Best For |
|---|---|---|---|---|
| Illumina | 75–300 bp | ~99.9% | Up to 6 Tb | WGS, RNA-seq, ChIP-seq |
| Oxford Nanopore | 1 kb – 4 Mb | ~90–99% | Up to 50 Gb | Structural variants, metagenomics |
| PacBio HiFi | 15–25 kb | ~99.9% | ~360 Gb | Genome assembly, phasing |
| Ion Torrent | 200–600 bp | ~99% | ~15 Gb | Targeted panels, amplicon |

---

## 1.4 Choosing the Right Sequencing Technology

One of the most important practical skills for any genomics researcher is matching the sequencing technology to the biological question. There is no single "best" platform — the right choice depends on read length requirements, accuracy needs, budget, sample type, and downstream analysis.

| Research Question | Recommended Platform |
|---|---|
| Routine variant calling (SNPs, small indels) | Illumina — best accuracy, cost, and coverage |
| De novo genome assembly | PacBio HiFi or ONT long reads |
| Structural variant detection | ONT or PacBio |
| Portable / field sequencing | Oxford Nanopore MinION |
| Clinical targeted panels | Ion Torrent or Illumina MiSeq |
| Full-length isoform sequencing | PacBio Iso-Seq or ONT cDNA |

> ⚠️ **Important:** Choosing the wrong platform at the start of a project can result in data that cannot answer your biological question. Always define your question first, then select the technology.

---


---
