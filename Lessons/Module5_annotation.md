# Module 5 — Gene & Protein Annotation: InterProScan, Pfam & KEGG

> **Type:** Lecture + Hands-on  
> **Duration:** ~90 minutes (40 min lecture, 50 min hands-on)  
> **Level:** Beginner  
> **Builds on:** Module 2 (databases), Module 3 (BLAST)

---

## Table of Contents

1. [What is a Gene?](#1-what-is-a-gene)
2. [From Gene to Protein — The Central Dogma](#2-from-gene-to-protein--the-central-dogma)
3. [What Can You Learn from a Sequence Alone?](#3-what-can-you-learn-from-a-sequence-alone)
4. [Gene & Protein Annotation Tools](#4-gene--protein-annotation-tools)
   - [InterProScan](#41-interproscan)
   - [Pfam](#42-pfam)
   - [KEGG](#43-kegg)
5. [Hands-on Session](#5-hands-on-session)

---

## 1. What is a Gene?

A gene is a stretch of DNA that contains the instructions to make a functional product — usually a protein, but sometimes a functional RNA molecule.

Think of the genome as a very long book. The gene is a single chapter. But unlike chapters in a book, genes are not neatly separated — they are scattered across chromosomes, interrupted by non-coding sequences, and can overlap.

### 1.1 The Parts of a Gene

Here is a diagram of a typical protein-coding gene in a eukaryote (animals, plants, fungi):

```
Chromosome DNA:

5'─────────────────────────────────────────────────────────────3'
     [Promoter]──[5'UTR]──[Exon1]──[Intron1]──[Exon2]──[Intron2]──[Exon3]──[3'UTR]──[Poly-A signal]
                  ←──────────────── Pre-mRNA (transcribed region) ────────────────→
```

Each part has a specific role:

---

#### Promoter
- Located **upstream** (before) the gene, on the DNA
- The region where the RNA polymerase enzyme binds to start transcription
- Contains regulatory signals — switches that control **when**, **where**, and **how much** the gene is expressed
- Not transcribed into RNA — it stays as DNA

> **Analogy:** The promoter is like the "on" button for the gene. Transcription factors (proteins) bind the promoter and either push the button or block it.

---

#### 5' UTR (5 Prime Untranslated Region)
- The beginning of the mRNA that is transcribed but **not translated into protein**
- Contains signals that help ribosomes find and bind the mRNA
- Can contain regulatory elements that control translation efficiency

> **Analogy:** The cover page and instructions before the actual story begins.

---

#### Exons
- The coding portions of the gene — these are the sequences that **end up in the final mRNA** and get translated into protein
- A typical human gene has 8–10 exons, but this varies enormously (Titin, the largest human gene, has 363 exons)
- Exons are numbered from 5' to 3': Exon 1, Exon 2, etc.

---

#### Introns
- Sequences **between exons** that are transcribed into the pre-mRNA but then **removed** in a process called splicing
- Make up the majority of most eukaryotic genes — a typical human gene is ~27,000 bp long, but the coding sequence is only ~1,500 bp
- Not present in bacteria (prokaryotes have no introns)
- Some introns contain regulatory sequences; some encode small RNAs

> **Analogy:** Introns are like the outtakes in a film — they are recorded but cut out before the final movie.

---

#### Splice Sites
- The boundaries between exons and introns
- The spliceosome (a molecular machine) recognizes specific sequences at splice sites to cut out introns precisely
- Mutations at splice sites are a common cause of genetic disease

---

#### 3' UTR (3 Prime Untranslated Region)
- The end of the mRNA, after the stop codon — transcribed but not translated
- Contains signals for mRNA stability, localization in the cell, and regulation by microRNAs
- The length of the 3' UTR influences how long the mRNA survives before being degraded

---

#### Poly-A Signal & Poly-A Tail
- A signal sequence (AATAAA) near the end of the gene that triggers cleavage and polyadenylation
- After cleavage, ~200 Adenine nucleotides are added to form the **Poly-A tail**
- The poly-A tail protects mRNA from degradation and helps with nuclear export

---

#### Open Reading Frame (ORF)
- The stretch of codons (triplet nucleotides) from the **start codon (ATG)** to the **stop codon (TAA, TAG, or TGA)**
- An ORF is the actual blueprint for the protein
- Finding ORFs in a raw DNA sequence is one of the first steps of gene annotation

```
5'--[5'UTR]--ATG AAA GGT TGC AAC ... TGA--[3'UTR]--3'
              ↑                       ↑
           Start codon             Stop codon
           (Met)                (end of protein)
           |←────────── ORF ──────────→|
```

---

### 1.2 Prokaryote vs Eukaryote Gene Structure

| Feature | Prokaryote (e.g., bacteria) | Eukaryote (e.g., humans) |
|---------|---------------------------|--------------------------|
| Introns | Absent | Present (and numerous) |
| 5' cap | No | Yes |
| Poly-A tail | No | Yes |
| Splicing required | No | Yes |
| Gene density | High — genes close together | Low — genes spread out |
| Operon structure | Yes — related genes clustered | No |
| mRNA processed before export | No — translation starts immediately | Yes |

---

### 1.3 Alternative Splicing — One Gene, Many Proteins

In eukaryotes, the exons of a single gene can be joined together in different combinations. This is called **alternative splicing**.

```
Pre-mRNA:    [Exon 1]──[Intron]──[Exon 2]──[Intron]──[Exon 3]──[Intron]──[Exon 4]

Transcript A:  [Exon 1]──────────────────────[Exon 3]──────────────[Exon 4]
Transcript B:  [Exon 1]──[Exon 2]────────────────────────────────[Exon 4]
Transcript C:  [Exon 1]──[Exon 2]──[Exon 3]──[Exon 4]
```

This means a single gene can produce multiple different proteins. The human genome has ~20,000 genes but produces well over 100,000 distinct proteins — largely because of alternative splicing.

---

## 2. From Gene to Protein — The Central Dogma

The flow of biological information follows a fundamental rule called the **Central Dogma of Molecular Biology**:

```
  DNA  ──[Transcription]──▶  RNA  ──[Translation]──▶  Protein
   ↑                                                      ↓
(stored in nucleus)                              (performs function)
```

### Step 1: Transcription (DNA → RNA)
- RNA polymerase reads the DNA template strand
- Produces a pre-mRNA copy (which includes introns)
- Pre-mRNA is processed: introns removed, 5' cap added, poly-A tail added
- Mature mRNA is exported from the nucleus to the cytoplasm

### Step 2: Translation (mRNA → Protein)
- Ribosomes read the mRNA in triplets called **codons**
- Each codon specifies one amino acid (or a start/stop signal)
- Transfer RNAs (tRNAs) bring the correct amino acids
- The ribosome links amino acids together to form a polypeptide chain
- The polypeptide folds into its final 3D protein structure

### The Genetic Code

| Codon | Amino acid | Codon | Amino acid |
|-------|-----------|-------|-----------|
| AUG | Methionine (start) | UGG | Tryptophan |
| UUU / UUC | Phenylalanine | CAU / CAC | Histidine |
| UUA / UUG / CUU / CUC / CUA / CUG | Leucine | AAA / AAG | Lysine |
| UAA / UAG / UGA | **STOP** | GGU / GGC / GGA / GGG | Glycine |

The genetic code is **redundant** — most amino acids are encoded by more than one codon. This is protective: many single-nucleotide changes result in the same amino acid (silent/synonymous mutations).

### What is a Protein?

A protein is a chain of amino acids that folds into a specific 3D shape. That shape determines what it does.

Proteins can be:
- **Enzymes** — catalyze chemical reactions (e.g., lactase breaks down lactose)
- **Structural proteins** — provide physical support (e.g., collagen in skin; keratin in hair)
- **Signaling proteins** — transmit information between cells (e.g., insulin)
- **Transport proteins** — carry molecules (e.g., hemoglobin carries oxygen)
- **Transcription factors** — regulate gene expression
- **Antibodies** — recognize and neutralize pathogens
- **Motor proteins** — generate movement (e.g., myosin in muscle)

### Protein Domains — the modular architecture of proteins

Most proteins are not made of a single, continuous functional unit. They are assembled from **domains** — discrete, independently folding regions, each with a specific function.

```
Example: A receptor tyrosine kinase

N-terminus ──[Signal peptide]──[Ligand-binding domain]──[Transmembrane domain]──[Kinase domain]──[SH2 domain]── C-terminus

Each domain = a functional module
```

Why domains matter for bioinformatics:
- Domains are **evolutionarily conserved** — the same domain appears in thousands of different proteins across all life
- If your unknown protein contains a known domain, you immediately know something about its function
- This is exactly what InterProScan and Pfam exploit

---

## 3. What Can You Learn from a Sequence Alone?

If someone hands you a raw protein or DNA sequence, a surprisingly large amount can be inferred computationally — without ever going into the lab.

### From a DNA sequence

| Analysis | What you can find | Tool |
|----------|------------------|------|
| **ORF finding** | Where are the start and stop codons? | NCBI ORF Finder |
| **Gene prediction** | Where are the likely genes? | Augustus, GenScan |
| **Codon usage** | Is this sequence from bacteria, yeast, human? | Codon Usage Database |
| **Repeat regions** | Are there repetitive elements (transposons, satellites)? | RepeatMasker |
| **CpG islands** | Is there a promoter region nearby? | CpG Island Finder |
| **Splice sites** | Where are likely intron/exon boundaries? | NetGene2 |
| **Homology** | What known genes does it resemble? | BLAST |

### From a protein sequence

| Analysis | What you can find | Tool |
|----------|------------------|------|
| **Domains** | What functional modules does it contain? | InterProScan, Pfam |
| **Signal peptide** | Is it secreted outside the cell? | SignalP |
| **Transmembrane regions** | Does it span the membrane? | TMHMM |
| **Subcellular localization** | Where in the cell does it go? | DeepLoc, WoLF PSORT |
| **Post-translational modifications** | Where is it phosphorylated/glycosylated? | NetPhos, NetNGlyc |
| **Disorder regions** | Which regions are unstructured? | IUPred |
| **Pathway membership** | What biological process does it participate in? | KEGG, Reactome |
| **Function prediction** | What does it do? | InterProScan, BLAST, UniProt |
| **3D structure prediction** | What does it look like? | AlphaFold, Phyre2 |

This is why annotation matters — a raw sequence is just letters. Annotation transforms those letters into biological knowledge.

---

## 4. Gene & Protein Annotation Tools

### 4.1 InterProScan

🌐 **Website:** https://www.ebi.ac.uk/interpro/  
🏛️ **Run by:** EMBL-EBI  
💰 **Cost:** Free

#### What is InterPro?

InterPro is a database that **integrates protein family and domain information from 13 different databases** into a single unified resource. Instead of searching 13 separate databases, InterProScan does it all at once.

The member databases InterPro integrates include:

| Database | What it focuses on |
|----------|-------------------|
| **Pfam** | Protein families and domains (HMM-based) |
| **PRINTS** | Protein fingerprints (conserved motifs) |
| **ProSite** | Functional sites and patterns |
| **SMART** | Signaling domain annotations |
| **TIGRFAM** | Protein families, especially microbial |
| **CDD** | NCBI's Conserved Domain Database |
| **Gene3D** | Structural domain families |
| **SUPERFAMILY** | Structural classifications |
| And more... | |

#### What InterProScan produces

When you submit a protein sequence, InterProScan returns:
- **All domains** detected in your sequence, with their positions
- **Protein family** classification — what family does this protein belong to?
- **GO terms** (Gene Ontology) — standardized descriptions of molecular function, biological process, and cellular component
- **Pathway annotations** — which pathways is this protein part of?
- A **graphical domain architecture** — a visual map of your protein

#### A sample InterProScan result (conceptual)

```
Your protein (450 aa):
|──────────────────────────────────────────────────────|

Pfam:
|──[PF00069: Protein kinase domain (aa 50–310)]──────────────[PF00433: Kinase-associated domain (aa 350–420)]──|

SMART:
|──[SM00220: S_TKc: Serine/Threonine kinase catalytic domain]───────────────────────────────────────────────|

GO Terms assigned:
  Molecular Function: ATP binding (GO:0005524)
  Molecular Function: Protein serine/threonine kinase activity (GO:0004674)
  Biological Process: Phosphorylation (GO:0016310)
  Cellular Component: Nucleus (GO:0005634)

Predicted function: This is a serine/threonine kinase — it adds phosphate groups to other proteins.
```

#### Why InterProScan is so powerful

Before tools like InterProScan, characterizing a new protein required years of lab experiments. Now, submitting a sequence to InterProScan can reveal the protein's likely function, its structural family, its cellular location, and the pathways it participates in — in about one minute.

---

### 4.2 Pfam

🌐 **Website:** https://www.ebi.ac.uk/interpro/ (Pfam is now hosted within InterPro)  
🏛️ **Run by:** EMBL-EBI / Wellcome Sanger Institute  
💰 **Cost:** Free

#### What is Pfam?

Pfam is a database of **protein families**, defined by conserved sequence patterns called **Hidden Markov Models (HMMs)**.

#### What is an HMM?

A Hidden Markov Model is a statistical model built from a multiple sequence alignment of known family members. It captures not just the consensus sequence but also the **pattern of variation** at each position — which positions are flexible and which are strictly conserved.

```
How a Pfam family is built:

Step 1: Collect all known sequences of, e.g., "Protein kinase domain"
Step 2: Build a multiple sequence alignment of these sequences
Step 3: Train an HMM on this alignment — learning which positions are conserved
Step 4: Use the HMM as a "search profile" to find new family members

The HMM can find distant relatives that BLAST might miss.
```

#### Pfam entry types

| Type | Description | Example |
|------|-------------|---------|
| **Domain** | A structurally/functionally defined region | Protein kinase domain (PF00069) |
| **Family** | A group of proteins with shared function/structure | Globin family (PF00042) |
| **Repeat** | A short motif that is repeated | Ankyrin repeat (PF00023) |
| **Motif** | A short functional sequence | KDEL ER retention signal |

#### Each Pfam entry includes

- The HMM profile used for searching
- A seed alignment (the curated core alignment used to build the model)
- A full alignment of all detected family members
- A description of the function
- Links to solved structures in PDB
- Species distribution — which organisms have this domain?

#### Example: Pfam entry PF00069 — Protein Kinase Domain

This is one of the most common domains in the human proteome. The human genome encodes ~518 kinases — all containing a variant of PF00069.

```
PF00069: Protein kinase domain
- Found in: 518 human proteins
- Also found in: all eukaryotes, some bacteria
- Function: Transfers phosphate from ATP to a substrate protein
- Size: approximately 250 amino acids
- Key residues: Glycine-rich loop (GxGxxG), catalytic Asp, DFG motif
```

---

### 4.3 KEGG

🌐 **Website:** https://www.kegg.jp/  
🏛️ **Run by:** Kanehisa Laboratories, Kyoto University  
💰 **Cost:** Free for academic use

#### What is KEGG?

**KEGG** (Kyoto Encyclopedia of Genes and Genomes) answers a different question from InterPro and Pfam. Instead of asking "what domains does my protein have?", KEGG asks:

> *What biological process does this protein participate in, and how does it connect to other genes and proteins?*

KEGG organizes biological knowledge into **pathway maps** — hand-drawn diagrams showing how genes, proteins, and metabolites interact in biological processes.

#### KEGG's core databases

| Database | Contents |
|----------|----------|
| **KEGG PATHWAY** | ~550 hand-drawn pathway maps covering metabolism, signaling, disease |
| **KEGG ORTHOLOGY (KO)** | A system for classifying genes by function across all organisms |
| **KEGG GENES** | Gene catalogs from thousands of sequenced organisms |
| **KEGG COMPOUND** | Chemical compounds in metabolic pathways |
| **KEGG DISEASE** | Disease entries linked to pathway genes |
| **KEGG DRUG** | Drug molecules and their targets in pathways |

#### Types of KEGG pathways

| Category | Examples |
|----------|---------|
| **Metabolism** | Glycolysis, TCA cycle, fatty acid synthesis |
| **Genetic information processing** | DNA replication, transcription, translation, repair |
| **Environmental information processing** | MAPK signaling, PI3K-Akt, Wnt, Notch, Toll-like receptor |
| **Cellular processes** | Cell cycle, apoptosis, autophagy, cell adhesion |
| **Organismal systems** | Immune system, nervous system, endocrine system |
| **Human diseases** | Cancer pathways, diabetes, infectious diseases |
| **Drug development** | Drug targets and resistance mechanisms |

#### Reading a KEGG pathway map

KEGG pathway maps use a standardized visual language:

| Symbol | Meaning |
|--------|---------|
| Rectangle | Gene / enzyme |
| Circle | Metabolite / compound |
| Arrow | Reaction or activation |
| ┤ (flat-headed arrow) | Inhibition |
| Dashed arrow | Indirect effect |
| Green highlight | Present in your organism |
| Red/pink highlight | User-highlighted gene (e.g., from your expression data) |

#### Why KEGG matters

- A protein rarely acts alone — it works in a network
- Knowing which pathway a protein belongs to tells you its biological context
- KEGG lets you go from "this is a kinase" → "this kinase is part of the MAPK signaling pathway" → "this pathway controls cell division and is often dysregulated in cancer"
- In RNA-seq analysis, KEGG pathway enrichment analysis tells you which entire pathways are up- or down-regulated — much more meaningful than individual gene lists

---

## 5. Hands-on Session

> ⏱️ **Time:** ~50 minutes  
> 🔧 **Tools:** InterPro, Pfam, KEGG — all browser-based  
> 🎯 **Goal:** Annotate unknown proteins and understand their function and biological context

---

### Background: What we're analysing today

You are a bioinformatics analyst at a research institute. Your PI has handed you three protein sequences pulled from a newly sequenced organism. Nothing is known about them. Your task is to annotate them — figure out what they are, what they do, and what pathways they are involved in.

---

### Exercise 1 — Annotate an Unknown Protein with InterProScan (15 minutes)

**Your unknown sequence (Protein A):**

```fasta
>ProteinA_unknown
MGSSHHHHHHSSGLVPRGSHMASRGVNLKVIGQDSSEIHFKVKMTTHLKKLKEEFGRKCA
ELGSMIPELRQHAKEFGDNLQESVKGIIKQIAKTILDTDAQFEDIEEALRREVEKRVSIF
GQIISGIASSGDNKDAAQFLEEFNLKTLQNILEKISKEQEKVKIAKRLGLEEQEDEERI
IEEAEQRRLHEAMEEYNHLARATLQCMEEQERQQVQEMQLQLRDGLIQVTPEEDEAAATM
SDAGPNVQPASRAARSATRSTTSTQRKPASSNRKNKKPPKKNQKPKPVPTPPKAKPPAKP
KAAAKDKSSDKKVQTKGKRGAKGKSGKSKKVGSTSSRHSSGLAIGKKVNPAPKVSKAAKK
SKKVTKKAVTKKAVTKKVAKSSTAATPKKVKSSTSTTPKKVKSVTAAATPKKVKSSTTVT
PKKVKSNSTASSPKKVSTSATPGKKVNSSTSSPKKVKS
```

**Steps:**

1. Go to https://www.ebi.ac.uk/interpro/

2. Click **"Search"** in the top menu, then **"By Sequence"**

3. Paste the protein sequence above (include the header line)

4. Click **"Search"**

5. The search takes 1–3 minutes. While waiting, read the next section.

**Reading the results:**

The InterPro results page has several sections:

**A — Protein Overview Strip**  
At the top you will see a horizontal bar representing your protein, with coloured blocks showing where domains were found.

**B — InterPro Entries**  
Grouped hits from the member databases. Each row tells you:
- The InterPro entry name and accession (IPR...)
- What type it is (Domain / Family / Homologous superfamily)
- The position (start–end) in your protein
- Which member databases contributed to the annotation

**C — GO Terms**  
Scroll down to find automatically assigned Gene Ontology terms. They are grouped into:
- **Molecular Function** — what biochemical activity does this protein have?
- **Biological Process** — what biological process does it contribute to?
- **Cellular Component** — where in the cell is it found?

**Questions to answer:**

1. What protein family does InterPro assign this sequence to?
2. What are the domain names detected, and at what positions?
3. List the GO terms under "Molecular Function". What does this tell you about what this protein does?
4. List the GO terms under "Biological Process". What cellular process is this protein involved in?
5. Is there a signal peptide or transmembrane domain? (Look at the domain architecture graphic)
6. Click on one of the Pfam domain accession numbers (e.g., PFxxxxx). You will be taken to the Pfam/InterPro entry page. How many species have this domain? Is it an ancient, conserved domain?

> 💡 **Hint:** Look carefully at the N-terminal domain. Many DNA-interacting proteins have characteristic positively charged domains.

---

### Exercise 2 — Explore a Pfam Domain Entry (10 minutes)

**Scenario:** You want to understand the Kinase domain family in depth — because a colleague tells you that many drug targets in cancer are kinases.

**Steps:**

1. Go to https://www.ebi.ac.uk/interpro/

2. In the search box, type: `Protein kinase` and press Enter

3. In the results, look for the entry **IPR000719 — Protein kinase domain**

4. Click on it to open the full entry

**Explore the following tabs/sections:**

**A — Overview**
- What is the function of this domain as described?
- What type of reaction does it catalyse?

**B — Taxonomy** (click the Taxonomy tab)
- Is this domain found in bacteria?
- Is it found in archaea?
- What does this tell you about when this domain evolved?

**C — Proteins** (click the Proteins tab)
- How many proteins in UniProt contain this domain?
- Look at a few of the top hits — are they all kinases?

**D — AlphaFold Structures** (if available)
- Click on one of the protein entries and look for an AlphaFold structure
- Can you see the characteristic fold of the kinase domain?

**Questions to answer:**
1. In how many UniProt entries is the Protein Kinase domain found?
2. Is this domain present in prokaryotes (bacteria)?
3. Name 3 specific proteins (from the Proteins tab) that contain this domain
4. Based on the taxonomy distribution, would you say the protein kinase domain is ancient or recently evolved?

---

### Exercise 3 — Annotate a Metabolic Enzyme with Pfam and KEGG (25 minutes)

This exercise links protein domain annotation (Pfam/InterPro) to biological pathway context (KEGG).

**Your unknown sequence (Protein B):**

```fasta
>ProteinB_unknown
MSTEGKPTVVVLDSGRGKSTLGQKLIESGLKSAGMIDPTIEEALFREVRGLDLHPHSRHI
VHDCFVDLNQKLSDSEGLRQDYEQIMQQAIQKIKLEEKGVQMVSAGHDEYALQKIGQIAD
ALTLSGITPAQCTEIMRDRTKLDDLNVPQYYKEVTKQLHTELGASRSLDPTLKSTLKQLL
SVRAAAIIATGGSGSGKSTILNLILGELQKAGKRVLLYADGPRTLQTAREIKQHLAGMSC
IRVNHSNMTDEQVQEVLENFKQVVTPQNRKIAIVQDSGATLLQQLQKHMKHTGMFKNNL
DCVAQGLDYLGVKIQSTLNFQPPQNLGMDPNSPIEIKASGTANALTDQAQPFVEGVSIIA
GKDQSMFEIVNEHYSGIPYNVLSAYANDIKAMGLGNPQVKDVFRKYGVDLKMDTFKPQTY
KLMAQLSQLPQLHKQLFEQAVKQASRKVTGALTPIPQNLNVKIESLADLPDNIPEVVN
```

**Part A — Domain annotation with InterProScan (10 min)**

1. Go to https://www.ebi.ac.uk/interpro/ → Search → By Sequence
2. Paste ProteinB_unknown and run the search
3. Record:
   - What domain(s) are detected?
   - What is the predicted molecular function (GO terms)?
   - What biological process GO term is assigned?

**Part B — Explore the KEGG pathway (15 min)**

Now that you have an idea of what this protein does, you will find it in KEGG and place it in a biological pathway.

1. Go to https://www.kegg.jp/

2. In the search bar, type the function you identified in Part A (e.g., "ATP synthase" or whatever domain was detected — use the GO molecular function as a guide)

3. Click on the top result in **KEGG ORTHOLOGY**

4. On the KO entry page, scroll to **"Pathways"** — these are all the pathways this protein participates in

5. Click on the most relevant pathway map

**Reading the KEGG pathway map:**

- The map shows a network of genes/proteins (rectangles) and metabolites (circles)
- Your protein of interest should be highlighted or searchable
- Arrows show the direction of reactions
- Look for your protein's position — is it early or late in the pathway? Is it a central node?

6. On the pathway map page, click **"Search pathway"** and enter the enzyme name to highlight it in the map

**Questions to answer:**

1. What domain(s) did InterProScan detect in ProteinB?
2. What is the predicted molecular function?
3. In KEGG, which pathway does this enzyme appear in?
4. On the pathway map, what molecule does this enzyme act on? What does it produce?
5. Is this pathway involved in energy production, biosynthesis, signaling, or something else?
6. Look at the pathway map — what enzyme comes before yours in the pathway? What enzyme comes after?
7. Is this pathway present in bacteria, or only in eukaryotes? (KEGG pathway pages list organisms)

---

### Exercise 4 — Putting It All Together: Annotate a Disease-Relevant Protein (Bonus)

**Your unknown sequence (Protein C):**

```fasta
>ProteinC_unknown
MAALSGGGGSTTGSGLPGKRALPTMARVGAAAQPDARPQSASAAEDDEDEDDEDEPPTPPP
AQPEPPAAAEAAPAASKGLGTDEDSIGDTTSALPVPAPTPAELRQFLLQQASQPPEEPPPP
RPPQKQPPPPPPPQPPPQKQQPPPPQPPPQKQPPPQPQKQPPPQPPPQPPPQPQPPPQPP
PQPQQPPPQPPPQPPPQKQPPPQPPPQPPPHQPPPQPPPQPPPQKPPPQPPPQPPPQPPP
QPPPPQKQPPPQPPPQPPPQPPPQPPPQPQPQPPPNQPPPQPPPQPPPQPPPQPPPPQKQ
KELVLSQLAKRPVKAHPAPNVTAAAESSPEEDEDDSDTTTEESTGESSGNQAEKKKDKDKL
KKKLKEQNKAALLQANLTVSKLNHSPVPQVPGPKSIRSAVSSVKPQMQPNEASSASEGMD
IPTVIRSMDYQKFLEKHQQELRQKNEDLQKKLQAMEDEYQQLKNFLEAQKQDAKDLEEKQ
KRLQQEIEDQKQAVEQHQDELEELRQKLQEALDQIKQALETQQNQLQQELEKQKAAQELE
MQRQQLEEQKTQLEEAKMQLEENIQKVHQELEQRMEDQLQLKNFVQQLEQERNQLQNDLQ
ELQQQISEINAQLERKLQQELEQERNQLQLKLQELEDQIKQAFETQAQELRQKLEDELNKN
IQTLERDLNQEQERIRMQQERDIQQMQEQLEKQKQELQDQHQALQQQLEELQQELNQKIQ
ALEKQKQELEAQTQELQQK
```

This sequence is from a human protein. Run it through InterProScan and then answer:

1. What repeating structural feature dominates this protein?
2. What broad protein family does it belong to?
3. Is this a globular (compact, folded) protein, or is it likely to be a more extended/fibrous protein?
4. Search for this protein in UniProt using BLAST (from Module 3) — what is its name and what does it do in the cell?
5. Does KEGG list any disease associations for this gene?

> 💡 **Hint:** Proteins dominated by a single type of repetitive domain often act as structural scaffolds or molecular rulers. The biological role often becomes obvious once you see what the repeat domain is.

---

## Summary

### The parts of a gene

| Component | Key role |
|-----------|---------|
| Promoter | Controls when/where/how much the gene is expressed |
| 5' UTR | Ribosome binding, translation regulation |
| Exons | Protein-coding sequence (retained in mature mRNA) |
| Introns | Non-coding sequences, removed by splicing |
| 3' UTR | mRNA stability, localization, microRNA regulation |
| Poly-A tail | Protection from degradation, export from nucleus |

### Annotation tools at a glance

| Tool | Question it answers | Output |
|------|-------------------|--------|
| **InterProScan** | What domains/families does my protein have? | Domain map, GO terms, family classification |
| **Pfam** | Which protein family does this belong to? (statistical model) | Domain positions, family description, HMM profile |
| **KEGG** | What pathway is this gene part of? | Pathway map, reaction context, disease links |

### The annotation workflow

```
Unknown sequence
      │
      ▼
Uniprot ──▶ Map to orthologs
      │
      ▼
InterProScan ──▶ Domains, GO terms, family
      │
      ▼
KEGG ──▶ Pathway context, upstream/downstream partners, disease
      │
      ▼
Biological hypothesis formed
```

---
