# Module 2 — Bioinformatics Data: Sequences, Formats & Databases

> **Type:** Lecture + Hands-on  
> **Duration:** ~75 minutes (40 min lecture, 35 min hands-on)  
> **Level:** Beginner

---

## Table of Contents

1. [Biological Sequence Data](#1-biological-sequence-data)
2. [Common Sequence File Formats](#2-common-sequence-file-formats)
   - [FASTA](#21-fasta-format)
   - [FASTQ](#22-fastq-format)
   - [GenBank](#23-genbank-format)
3. [Major Bioinformatics Databases](#3-major-bioinformatics-databases)
   - [NCBI](#31-ncbi--national-center-for-biotechnology-information)
   - [UniProt](#32-uniprot)
   - [EMBL-EBI](#33-embl-ebi)
   - [PDB](#34-pdb--protein-data-bank)
4. [Hands-on Session — Retrieving Data from Databases](#4-hands-on-session--retrieving-data-from-databases)

---

## 1. Biological Sequence Data

Everything in bioinformatics starts with a **sequence** — an ordered string of biological building blocks.

There are three main types:

### DNA Sequences
- Made of four **nucleotide bases**: **A** (Adenine), **T** (Thymine), **G** (Guanine), **C** (Cytosine)
- Stored as a string of letters
- Represents the genetic blueprint stored in chromosomes

```
ATGCGTAACGATCGATCGATCGATCGTTGCATCGATCGATCG
```

### RNA Sequences
- Same as DNA but **T is replaced by U** (Uracil)
- Represents messenger RNA — the intermediate between gene and protein
- Used in transcriptomics studies

```
AUGCGUAACGAUCGAUCGAUCGAUCGUUGCAUCGAUCGAUCG
```

### Protein Sequences
- Made of **20 amino acids**, each represented by a single letter
- Proteins are the functional molecules that carry out almost every process in a cell

```
MVHLTPEEKSAVTALWGKVNVDEVGGEALGRLLVVYPWTQRF
```

### Why sequences matter

A gene's sequence tells you:
- Where it starts and stops
- What protein it will make
- Whether it is mutated compared to a reference
- How similar it is to genes in other species

---

## 2. Common Sequence File Formats

Different experiments produce data in different formats. You will encounter these files constantly.

---

### 2.1 FASTA Format

**The most fundamental sequence format.** Used for storing one or more sequences of any type (DNA, RNA, or protein).

#### Structure

```
>sequence_identifier [optional description]
SEQUENCE_DATA_LINE_1
SEQUENCE_DATA_LINE_2
...
```

- Lines beginning with `>` are **header lines** — they name and describe the sequence
- All other lines are the sequence itself
- One file can contain many sequences (a "multi-FASTA")

#### Real example — Human Hemoglobin beta chain (protein)

```fasta
>sp|P68871|HBB_HUMAN Hemoglobin subunit beta OS=Homo sapiens
MVHLTPEEKSAVTALWGKVNVDEVGGEALGRLLVVYPWTQRFFESFGDLSTPDAVMGNPK
VKAHGKKVLGAFSDGLAHLDNLKGTFATLSELHCDKLHVDPENFRLLGNVLVCVLAHHFG
KEFTPPVQAAYQKVVAGVANALAHKYH
```

#### Real example — A piece of human BRCA1 gene (DNA)

```fasta
>NM_007294.4 Homo sapiens BRCA1 DNA repair associated, mRNA
ATGGATTTATCTGCTCTTCGCGTTGAAGAAGTACAAAATGTCATTAATGCTATGCAGAAA
ATCTTAGAGTGTCCCATCTGTCTGGAGTTGATCAAGGAACCTGTCTCCACAAAGTGTGAC
CACATATTTTGCAAATTTTGCATGCTGAAACTTCTCAACCAGAAGAAAGGGCCTTCACAG
```

#### When is FASTA used?
- Submitting sequences to databases
- Input to BLAST (sequence search)
- Input to multiple sequence alignment tools
- Storing reference genomes

---

### 2.3 GenBank Format

**The richest annotation format.** A GenBank file contains not just the sequence but extensive metadata — the organism, authors, literature references, and detailed feature annotations (where genes are, where coding sequences start, etc.).

#### Structure overview

```
LOCUS       NM_007294    7088 bp    mRNA    linear   PRI 17-APR-2023
DEFINITION  Homo sapiens BRCA1 DNA repair associated (BRCA1), transcript
            variant 1, mRNA.
ACCESSION   NM_007294
VERSION     NM_007294.4
KEYWORDS    RefSeq.
SOURCE      Homo sapiens (human)
  ORGANISM  Homo sapiens
            Eukaryota; Metazoa; Chordata; ...
REFERENCE   1  (bases 1 to 7088)
  AUTHORS   Miki Y, Swensen J, Shattuck-Eidens D, et al.
  TITLE     A strong candidate for the breast and ovarian cancer
            susceptibility gene BRCA1
  JOURNAL   Science 266 (5182), 66-71 (1994)
FEATURES             Location/Qualifiers
     source          1..7088
                     /organism="Homo sapiens"
                     /mol_type="mRNA"
     CDS             232..5824
                     /gene="BRCA1"
                     /product="breast cancer type 1 susceptibility protein"
                     /protein_id="NP_009225.1"
ORIGIN
        1 gagctcgctg agacttcctg gacgggggac aggctgtggg gtttctgagg tgtgggtcgc
       61 atgatcgggg tgggccgcgg ccggggctgg cgtaacggat ttgggtcagc ccggaagctt
      ...
//
```

#### When is GenBank format used?
- Retrieving annotated records from NCBI
- Working with gene sequences where you need the annotation
- Submitting newly sequenced genes to NCBI

---

## 3. Major Bioinformatics Databases

Databases are where biological knowledge is stored and shared. Think of them as enormous, curated, freely accessible libraries of biological sequences, structures, and annotations.

---

### 3.1 NCBI — National Center for Biotechnology Information

🌐 **Website:** https://www.ncbi.nlm.nih.gov/  
🏛️ **Run by:** US National Institutes of Health (NIH)  
💰 **Cost:** Free

NCBI is the largest bioinformatics resource in the world. It is not a single database — it is a **collection of databases** all connected together.

#### Key databases within NCBI

| Database | What it contains | Example use |
|----------|-----------------|-------------|
| **GenBank** | All publicly submitted DNA and RNA sequences | Find the mRNA sequence of a human gene |
| **RefSeq** | Curated, non-redundant reference sequences | The "gold standard" sequence for any gene |
| **PubMed** | Biomedical literature — 35+ million papers | Find research articles on your gene |
| **Protein** | All protein sequences | Find the amino acid sequence of an enzyme |
| **Gene** | Gene-centric information across species | Everything known about a specific gene |
| **SRA** | Raw sequencing data from published studies | Download raw RNA-seq data |
| **dbSNP** | Known single nucleotide variants | Find common genetic variants |
| **ClinVar** | Clinically relevant genetic variants | Is this mutation disease-causing? |

#### NCBI's accession number system

Every sequence in NCBI has a unique **accession number**. These are standardized identifiers used in research papers.

| Prefix | Type | Example |
|--------|------|---------|
| `NM_` | mRNA (RefSeq) | `NM_007294` (BRCA1 mRNA) |
| `NP_` | Protein (RefSeq) | `NP_009225` (BRCA1 protein) |
| `NC_` | Complete chromosome | `NC_000017` (Human chr 17) |
| `AF_`, `AY_`, etc. | Original GenBank submissions | `AF086833` |

---

### 3.2 UniProt

🌐 **Website:** https://www.uniprot.org/  
🏛️ **Run by:** European Bioinformatics Institute (EMBL-EBI), Swiss Institute of Bioinformatics, PIR  
💰 **Cost:** Free

UniProt is the **world's leading database for protein sequences and functional information**. It answers the question: *what does this protein do?*

#### Two sections of UniProt

| Section | Name | Description |
|---------|------|-------------|
| **Swiss-Prot** | UniProtKB/Swiss-Prot | Manually reviewed, high-quality annotations. Every entry has been read by a human expert. ~570,000 entries. |
| **TrEMBL** | UniProtKB/TrEMBL | Computationally annotated. Much larger (~250 million entries) but lower confidence. |

#### What a UniProt entry contains

A UniProt entry for a protein tells you:
- The complete amino acid sequence
- The organism it comes from
- What the protein does (function)
- Which domains and motifs it contains
- Known disease associations
- Subcellular location (is it in the nucleus? the membrane?)
- Post-translational modifications (phosphorylation, glycosylation, etc.)
- 3D structure links (to PDB)
- Links to NCBI, Ensembl, and other databases

#### UniProt accession numbers

UniProt accessions are 6 characters: `P68871` (Human Hemoglobin beta), `P04637` (Human p53 tumor suppressor).

---

### 3.3 EMBL-EBI

🌐 **Website:** https://www.ebi.ac.uk/  
🏛️ **Run by:** European Molecular Biology Laboratory  
💰 **Cost:** Free

EMBL-EBI (European Bioinformatics Institute) is Europe's primary bioinformatics resource. It mirrors and complements many NCBI databases.

#### Key resources at EMBL-EBI

| Resource | What it is |
|----------|-----------|
| **ENA (European Nucleotide Archive)** | European equivalent of GenBank — all nucleotide sequences |
| **Ensembl** | Genome browsers for vertebrates — visualize genome annotations |
| **InterPro** | Protein families, domains, and functional sites |
| **PDBe** | European access point for protein structures |
| **ArrayExpress** | Gene expression data from microarray/RNA-seq experiments |
| **ChEMBL** | Bioactive molecules and their drug targets |

#### When would you use EMBL-EBI instead of NCBI?

- Genome browsing for vertebrates (Ensembl is superior to NCBI's genome browser for this)
- Looking up protein family/domain information (InterPro)
- Accessing European-origin sequencing studies
- In practice, NCBI and EMBL-EBI databases are **synchronized** — the same data exists in both, but the tools differ

---

### 3.4 PDB — Protein Data Bank

🌐 **Website:** https://www.rcsb.org/  
🏛️ **Run by:** Research Collaboratory for Structural Bioinformatics (RCSB), worldwide consortium  
💰 **Cost:** Free

The PDB is the single global repository for the **three-dimensional structures** of biological macromolecules — proteins, DNA, RNA, and their complexes.

#### How structures are determined

Structures deposited in the PDB were solved by:
- **X-ray crystallography** (~85% of structures) — crystallize the protein, shine X-rays through it
- **Cryo-electron microscopy (cryo-EM)** — rapidly growing; freezes proteins and images them with electrons
- **NMR spectroscopy** — uses magnetic resonance to determine structure in solution

#### What a PDB entry contains

- The 3D coordinates (x, y, z) of every atom in the molecule
- The experimental method used
- Resolution of the structure
- Bound ligands (drug molecules, cofactors)
- The research publication

#### PDB identifiers

Every PDB entry has a **4-character alphanumeric code**, e.g.:
- `1HBS` — Hemoglobin structure
- `2HHB` — Deoxyhemoglobin
- `6VXX` — SARS-CoV-2 spike protein
- `1TUP` — p53 tumor suppressor bound to DNA

#### Why does structure matter?

Shape determines function. If you know the 3D structure of a protein's active site, you can:
- Understand how it works mechanistically
- Design a drug molecule that fits into it like a key in a lock
- Predict the effect of mutations

> 💡 **AlphaFold:** In 2021, DeepMind's AlphaFold AI predicted the structures of virtually all ~200 million known proteins — a revolution made possible by bioinformatics. These predicted structures are freely available at https://alphafold.ebi.ac.uk/

> **AlphaFold** (developed by Google DeepMind) is an AI system that predicts a protein’s 3D structure directly from its amino acid sequence.
It is transformative for drug discovery because protein structure determines how drugs bind, and traditional methods like crystallography are slow and expensive.
AlphaFold uses deep learning trained on known protein structures, meaning it learns folding patterns from data rather than relying on fixed rules.
It achieved near-experimental accuracy in the CASP14, marking a major breakthrough in computational biology.
It does not replace experimental methods, but accelerates research by guiding experiments and prioritizing the most promising targets.

---

## 4. Hands-on Session — Retrieving Data from Databases

> ⏱️ **Time:** ~35 minutes  
> 🔧 **Tools needed:** Web browser only  
> 🎯 **Goal:** Retrieve sequence data from NCBI, UniProt, and PDB; understand what you are looking at

---

### Exercise 1 — Retrieve a Gene Record from NCBI (10 minutes)

**Scenario:** You are reading a paper about breast cancer genetics. The paper mentions the *BRCA1* gene. You want to find its mRNA sequence.

**Step 1:** Go to https://www.ncbi.nlm.nih.gov/

**Step 2:** In the search bar, change the database dropdown from "All Databases" to **"Gene"**, then search for:
```
BRCA1 AND "Homo sapiens"[Organism]
```

**Step 3:** Click the top result: **BRCA1** — breast cancer type 1 susceptibility gene — *Homo sapiens*

**Step 4:** On the Gene page, scroll to the **"mRNA and Protein(s)"** section. Click the RefSeq accession **NM_007294.4**.

**Step 5:** You are now on a GenBank record. Explore:
- The **LOCUS** line — how long is this mRNA?
- The **FEATURES** section — where does the CDS (coding sequence) start?
- Scroll to the bottom to see the raw sequence in **ORIGIN**

**Step 6:** To download in FASTA format, click **Send to → File → Format: FASTA → Create File**

> ✅ **What you should have:** A `.fasta` file containing the BRCA1 mRNA sequence

**Questions to answer:**
1. What is the total length (bp) of the NM_007294.4 mRNA?
2. At which nucleotide position does the coding sequence (CDS) begin?
3. What protein does this gene encode?

---

### Exercise 2 — Retrieve a Protein Entry from UniProt (10 minutes)

**Scenario:** You want to find out everything known about the p53 protein — one of the most important tumor suppressor proteins in biology.

**Step 1:** Go to https://www.uniprot.org/

**Step 2:** Search for:
```
P53 AND organism_id:9606 AND reviewed:true
```
*(9606 is the NCBI taxonomy ID for Homo sapiens; `reviewed:true` restricts to Swiss-Prot)*

**Step 3:** Click the top result: **P04637 · P53_HUMAN** — Cellular tumor antigen p53

**Step 4:** Explore the entry:

| Section to find | What to look at |
|----------------|-----------------|
| **Function** | What does p53 actually do in the cell? |
| **Subcellular location** | Where in the cell is p53 found? |
| **Disease involvement** | What diseases is p53 associated with? |
| **PTM / Processing** | How many phosphorylation sites does it have? |
| **Structure** | Is there a 3D structure available? |

**Step 5:** Scroll to the **Sequences** section. Click **Download → FASTA (canonical)**

> ✅ **What you should have:** The p53 protein sequence in FASTA format

The sequence starts like this:
```fasta
>sp|P04637|P53_HUMAN Cellular tumor antigen p53 OS=Homo sapiens
MEEPQSDPSVEPPLSQETFSDLWKLLPENNVLSPLPSQAMDDLMLSPDDIEQWFTEDPGP
DEAPRMPEAAPPVAPAPAAPTPAAPAPAPSWPLSSSVPSQKTYPQGLAEDESAGRSRAGSL
...
```

**Questions to answer:**
1. How many amino acids long is p53?
2. What is the molecular function listed (under "Function")?
3. How many disease variants are annotated for p53?

---

### Exercise 3 — Retrieve a Protein Structure from PDB (8 minutes)

**Scenario:** You want to visualize the 3D structure of p53 bound to DNA.

**Step 1:** Go to https://www.rcsb.org/

**Step 2:** Search for: `1TUP`

**Step 3:** You are looking at the structure of the p53 DNA-binding domain in complex with DNA. Explore:
- **Experiment:** What method was used? What is the resolution?
- **Macromolecules:** How many chains are in this structure?
- **Ligands:** Are there any ligands present?

**Step 4:** Click **"3D View"** — this opens a molecular viewer directly in your browser. You can:
- **Rotate** the structure by clicking and dragging
- **Zoom** with the scroll wheel
- Observe the protein (coloured ribbons) wrapped around the DNA (ladder-like structure)

**Step 5:** Download the structure file — click **Download Files → PDB Format**

> ✅ **What you should have:** A `.pdb` file containing atomic coordinates of the p53–DNA complex

**Questions to answer:**
1. What is the resolution of this structure (in Ångströms)?
2. How many chains are in the asymmetric unit?
3. Can you identify the DNA double helix in the 3D viewer?

---

### Exercise 4 — Cross-database linking (7 minutes)

One of the most powerful features of bioinformatics databases is that they are **linked to each other**. A single entry connects you to information across multiple databases.

**Step 1:** Return to the UniProt entry for p53: https://www.uniprot.org/uniprotkb/P04637/entry

**Step 2:** Scroll to the **"Structure"** section — notice it links directly to multiple PDB entries.

**Step 3:** Scroll to the **"External links"** section at the bottom. Find:
- The link to **NCBI Gene** — click it. Where does it take you?
- The link to **Ensembl** — click it. What additional information is here?
- The link to **AlphaFold** — click it. This shows the AI-predicted full-length structure.

**Discussion points:**
- Why do multiple databases exist instead of just one?
- What type of information would you go to NCBI for vs. UniProt vs. PDB?
- Why is cross-linking between databases so valuable?

---

## Summary

| Format | Used for | Key feature |
|--------|----------|-------------|
| FASTA | Any sequence (DNA/RNA/protein) | Simple: header + sequence |
| FASTQ | Raw sequencing reads | Includes quality scores per base |
| GenBank | Annotated DNA/RNA records | Rich metadata, literature, features |

| Database | Primary content | Best for |
|----------|----------------|----------|
| NCBI | Nucleotide & protein sequences, literature | Finding gene/mRNA sequences, literature |
| UniProt | Protein function & annotation | Understanding what a protein does |
| EMBL-EBI | Nucleotide archive, genome browser | European data, genome visualization |
| PDB | 3D molecular structures | Structure-based analysis, drug design |

---

