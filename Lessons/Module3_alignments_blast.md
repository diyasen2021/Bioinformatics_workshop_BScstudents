# Module 3 — Sequence Alignments: BLAST & Multiple Sequence Alignment

> **Type:** Lecture + Hands-on  
> **Duration:** ~90 minutes (45 min lecture, 45 min hands-on)  
> **Level:** Beginner

---

## Table of Contents

1. [What is a Sequence Alignment?](#1-what-is-a-sequence-alignment)
2. [Pairwise Alignment — Comparing Two Sequences](#2-pairwise-alignment--comparing-two-sequences)
3. [BLAST — Finding Sequence Relatives](#3-blast--finding-sequence-relatives)
4. [Multiple Sequence Alignment (MSA)](#4-multiple-sequence-alignment-msa)
5. [Hands-on Session — BLAST](#5-hands-on-session--blast)
6. [Hands-on Session — Multiple Sequence Alignment](#6-hands-on-session--multiple-sequence-alignment)

---

## 1. What is a Sequence Alignment?

A sequence alignment is the process of arranging two or more sequences to identify regions of similarity.

### The core idea

Imagine two sentences:
```
Sentence A:   THE CAT SAT ON THE MAT
Sentence B:   THE BAT SAT ON THE MAP
```

You can instantly see that these sentences are mostly identical, with two differences (`C→B` in CAT/BAT and `T→P` in MAT/MAP). Sequence alignment does the same thing for biological sequences.

### Why does similarity matter?

In biology, **sequence similarity implies shared ancestry and often shared function**.

- If your unknown gene looks like a known gene from another organism → it likely has the same function
- If a mutation changes a highly conserved (always the same across species) position → it's probably important
- If a patient's tumor has a mutation at a critical residue → it may affect protein function

### Gaps — a crucial concept

Real biological sequences are not the same length. Evolution inserts and deletes stretches of DNA (called **insertions** and **deletions**, or **indels**). Alignments use **gap characters** (`-`) to handle this:

```
Sequence A:   MKTAYIAKQRQISFVKSHFSRQ--STLYSEDITKQGDRGLAIPQAVHIE
Sequence B:   MKTAYIAKQRQISFVKSHFSRQVSTLYSEDITKQGDRGLAIPQAVHIE
                                     ^^
                      (two gaps inserted here to optimize alignment)
```

---

## 2. Pairwise Alignment — Comparing Two Sequences

A **pairwise alignment** compares exactly two sequences.

### Two types: global vs. local

| Type | What it does | When to use |
|------|-------------|-------------|
| **Global alignment** | Aligns sequences across their entire length — from end to end | Two sequences of similar length that are likely the same gene |
| **Local alignment** | Finds the best-matching region(s) — ignores poorly-matching ends | One sequence is much longer, or you're looking for a shared domain |

### Visual comparison

**Global alignment (Needleman-Wunsch algorithm):**
```
Seq A:  GCATGCU
Seq B:  GATTACA

Result:
GCA-TGCU
G-ATTACA
```
Every position in both sequences is accounted for.

**Local alignment (Smith-Waterman algorithm):**
```
Seq A:  GGTTGACTA
Seq B:  TGTTACGG

Best local match:
GTTAC
GTTAC
```
Only the best-matching region is returned.

### Scoring an alignment

How do we decide which alignment is "best"? We use a **scoring scheme**:
- A **match** gets a positive score (e.g., +2)
- A **mismatch** gets a penalty (e.g., -1)
- A **gap opening** gets a large penalty (e.g., -5)
- A **gap extension** gets a smaller penalty (e.g., -1 per additional gap)

The algorithm tries every possible alignment and finds the one with the highest score.

### Substitution matrices

Not all mismatches are equal. In proteins, some amino acid swaps are biochemically similar (e.g., Leucine → Isoleucine, both are hydrophobic). These are penalized less than dissimilar swaps (e.g., Lysine → Glutamate, positive→negative charge).

This is captured in a **substitution matrix**:

| Matrix | Used for | Description |
|--------|----------|-------------|
| **BLOSUM62** | Protein alignment | Most commonly used. Based on real protein alignments. |
| **PAM250** | Distantly related proteins | Good for comparing proteins across distant species |
| **EDNAFULL** | DNA alignment | Simple match/mismatch for nucleotides |

---

## 3. BLAST — Finding Sequence Relatives

### What is BLAST?

**BLAST** (Basic Local Alignment Search Tool) is the most widely used bioinformatics program in the world. It answers a simple but powerful question:

> *Given my query sequence, what known sequences in the database look most similar to it?*

Think of BLAST as a **Google search for biological sequences**. You provide a sequence, and BLAST searches a database of millions of sequences to find the closest matches.

### What BLAST does NOT do

BLAST uses a **heuristic** (a smart shortcut) — it sacrifices a tiny amount of sensitivity in exchange for being millions of times faster than a full Smith-Waterman search of the entire database.

### Types of BLAST

| Program | Query | Database | When to use |
|---------|-------|----------|-------------|
| **blastn** | Nucleotide | Nucleotide | Identify a DNA sequence, find similar genes |
| **blastp** | Protein | Protein | Find proteins similar to your query protein |
| **blastx** | Nucleotide | Protein | Translate your DNA in all 6 frames, search protein DB |
| **tblastn** | Protein | Nucleotide (translated) | Find DNA sequences that encode similar proteins |
| **tblastx** | Nucleotide (translated) | Nucleotide (translated) | Sensitive cross-species DNA comparison |

### Understanding BLAST output

A BLAST result contains several key statistics for each hit:

#### E-value (Expect value)
The most important number in a BLAST result.

> The E-value is the **number of hits you would expect to find by chance** when searching a database of that size.

| E-value | Interpretation |
|---------|---------------|
| `0.0` | Perfect or near-perfect match — almost certainly the same gene |
| `< 1e-50` | Excellent — very strong match |
| `1e-10` to `1e-50` | Good match — likely homologous |
| `0.001` to `0.01` | Weak match — possible homology, check carefully |
| `> 0.1` | Likely a random match — probably not biologically meaningful |

**Key rule:** The smaller the E-value, the more significant the match.

#### Bit score
A normalized score that allows comparison across different database sizes. Higher is better. A bit score above 50 is generally considered significant.

#### Percent identity
The percentage of positions in the aligned region that are identical between query and subject. `100%` = identical sequences. `40%` = 40% of aligned positions match.

#### Query coverage
What percentage of your query sequence is covered by the alignment. A hit with `100% coverage` aligns across your entire query. Low coverage may indicate only a partial domain match.

### A real BLAST result looks like this:

```
Query: My unknown protein sequence (245 aa)

Hits:
                                                    Score    E-value   Ident   Coverage
1. Tumor protein p53 [Homo sapiens]                 502 bits  1e-178   99%     100%
2. Tumor protein p53 [Pan troglodytes] (chimpanzee) 499 bits  5e-177   99%     100%
3. Tumor protein p53 [Mus musculus] (mouse)         487 bits  3e-172   84%     100%
4. Tumor protein p53 [Danio rerio] (zebrafish)      312 bits  6e-106   53%     98%
5. Transformation-related protein 63 [H. sapiens]  288 bits  2e-97    42%     95%
```

From this you can conclude:
- The unknown protein is almost certainly human p53
- p53 is **highly conserved** — even the zebrafish version is 53% identical
- p63 (a related family member) is also detected — it shares a common ancestor

---

## 4. Multiple Sequence Alignment (MSA)

### What is MSA?

A **Multiple Sequence Alignment** aligns three or more sequences simultaneously, arranging them so that corresponding positions line up in columns.

```
Human p53:    MEEQSDPSVEPPLSQETFSDLWKLLPENN
Mouse p53:    MEEPQ-DPSVEPPLSQETFSDLWKLLPENN
Zebrafish p53: M---GSPSVEAPLSQDTFSDLWKLLPENS
Frog p53:     MEEPHADPSVEPPLTQETFSDLWKLLPEND
                                  *** ******* ****
                            (conserved positions marked with *)
```

### What MSA reveals

1. **Conserved positions** — residues that are the same across all species. These are likely functionally essential.
2. **Variable positions** — residues that differ between species. These are under less evolutionary constraint.
3. **Insertions/deletions** — lineage-specific sequence additions or losses.
4. **Sequence identity/similarity** — how related the sequences are overall.

### Use cases for MSA

#### Identifying functional residues
If an amino acid is identical in 50 species, changing it in an experiment will almost certainly break the protein's function. This tells researchers which positions to study.

#### Building phylogenetic trees
Once sequences are aligned, the pattern of similarity/difference between species can be used to build an **evolutionary tree** — showing who is related to whom.

#### Predicting protein structure
Closely aligned positions tell structure prediction algorithms which amino acids are likely near each other in 3D space.

#### Finding disease mutations
When a patient has a mutation, comparing the position across species tells you whether that position is conserved — if it is, the mutation is much more likely to be pathogenic.

### MSA algorithms and tools

| Tool | Type | Website | Best for |
|------|------|---------|----------|
| **Clustal Omega** | Progressive | https://www.ebi.ac.uk/Tools/msa/clustalo/ | General purpose, easy to use |
| **MUSCLE** | Iterative | https://www.ebi.ac.uk/Tools/msa/muscle/ | Slightly more accurate for protein |
| **MAFFT** | Fast progressive | https://mafft.cbrc.jp/ | Large datasets |
| **T-Coffee** | Consistency-based | http://tcoffee.crg.eu/ | Highest accuracy, slower |

For this workshop, we will use **Clustal Omega** through the EBI web interface.

### Reading an MSA output

```
Human     MKTAYIAKQRQISFVKSHFSRQVSTLYSEDITKQGDRGLAIPQAVHIEL
Chimp     MKTAYIAKQRQISFVKSHFSRQVSTLYSEDITKQGDRGLAIPQAVHIEL
Mouse     MKTAYIAKQRQISFVKSHFSRQVSTLYSEDVTKQGDRGLAIPQAVHIEL
Zebrafish MK-AYIAKQRQISFAKSHFSRQ-STLYSEDITKQGDRGLAIPQAVHIEL
Frog      MKTAYIAKQRQISFVKSHFSRQVSTLYSEDITKQGDRGLAIPQAVHMEL
          ** ********* **:****** ******* **********************:*

Legend:
*  (asterisk)   = identical in all sequences
:  (colon)      = conserved substitution (similar properties)
.  (period)     = semi-conserved substitution
   (space)      = not conserved
```

---

## 5. Hands-on Session — BLAST

> ⏱️ **Time:** ~25 minutes  
> 🔧 **Tool:** NCBI BLAST — https://blast.ncbi.nlm.nih.gov/  
> 🎯 **Goal:** Identify unknown sequences and explore homology

---

### Exercise 1 — Identify an Unknown DNA Sequence (10 minutes)

**Scenario:** You are working in a microbiology lab. You have sequenced a bacterial colony and obtained the following 16S rRNA gene fragment. You need to identify what organism this is.

**Your unknown sequence:**
```fasta
>unknown_bacterial_16S
AGAGTTTGATCCTGGCTCAGATTGAACGCTGGCGGCAGGCCTAACACATGCAAGTCGAAC
GGTAACAGGAAGAAGCTTGCTCTTTGCTGACGAGTGGCGGACGGGTGAGTAATGTCTGG
GAAACTGCCTGATGGAGGGGGATAACTACTGGAAACGGTAGCTAATACCGCATAACGTCG
CAAGACCAAAGAGGGGGACCTTCGGGCCTCTTGCCATCAGATGTGCCCAGATGGGATTAGC
TAGTAGGTGGGGTAACGGCTCACCTAGGCGACGATCCCTAGCTGGTCTGAGAGGATGACC
AGCCACACTGGAACTGAGACACGGTCCAGACTCCTACGGGAGGCAGCAGTGGGGAATATT
GCACAATGGGCGCAAGCCTGATGCAGCCATGCCGCGTGTATGAAGAAGGCCTTCGGGTTG
TAAAGTACTTTCAGCGGGGAGGAAGGCGATAAGGTTAATAACCTTGTCGATTGACGTTAC
CCGCAGAAGAAGCACCGGCTAACTCCGTGCCAGCAGCCGCGGTAATACGGAGGGTGCAAG
CGTTAATCGGAATTACTGGGCGTAAAGCGCACGCAGGCGGTTTGTTAAGTCAGATGTGAA
```

**Steps:**

1. Go to https://blast.ncbi.nlm.nih.gov/

2. Click **"Nucleotide BLAST" (blastn)**

3. Paste the sequence above into the query box (include the `>unknown_bacterial_16S` header line)

4. Under **Database**, select **"16S ribosomal RNA sequences (Bacteria and Archaea)"**

5. Leave all other settings as default

6. Click **BLAST** and wait (~30–60 seconds)

**Interpreting the results:**

Look at the results table. You should see:

| Column | What it means |
|--------|--------------|
| Description | Name of the matching organism/sequence |
| Max Score | Highest alignment score |
| Total Score | Combined score across all HSPs |
| Query Cover | % of your sequence aligned |
| E value | Statistical significance |
| Per. Ident | % identical nucleotides |

**Questions to answer:**
1. What organism does this sequence most likely belong to? (Top hit)
2. What is the E-value for the top hit?
3. What is the percent identity?
4. Is the identification unambiguous, or are there several equally good hits from different species?
5. Click on the top hit — click "Sequence ID" to open the full record. What type of organism is this (gram positive/negative? aerobic/anaerobic)?

> 💡 **16S rRNA gene** is a highly conserved gene present in all bacteria. It is routinely sequenced to identify bacterial species — this technique is called **barcoding** and is the basis of all microbiome studies.

---

### Exercise 2 — Identify an Unknown Protein and Explore Conservation (15 minutes)

**Scenario:** You are studying a rare genetic disease. A collaborator has sent you a protein sequence they isolated from an affected patient's biopsy. They believe it is a mutant form of a cancer-related protein. Identify it and find its homologs.

**Your unknown protein sequence:**
```fasta
>patient_protein_unknown
MTEYKLVVVGAGGVGKSALTIQLIQNHFVDEYDPQIEDGQGIPYRMQQEGKLVVTDVEEG
RPVFPSPVQKSTIVNIDQPIYLEYAMRQKELTMDVIRDYQYKKNDRLQHGLQLNSDNPQF
ERPVADIIFVGNKCDLAARTVESRQAQDLARSYGIPYIETSAKTRQHVREVDREQKLISSA
GEDELLSGAALEAILQEAQDQRLQAARQRAEGLKEVIQAQRLIRQLREEHIAARQAQLEEF
KAAAAGRPESSGPEGPGGGAQASDV
```

**Steps:**

1. Go to https://blast.ncbi.nlm.nih.gov/

2. Click **"Protein BLAST" (blastp)**

3. Paste the protein sequence above

4. Database: **Non-redundant protein sequences (nr)**

5. Organism: type `9606` in the organism field and select **Homo sapiens** — we will first search only in human sequences

6. Click **BLAST**

**Part A — Identify the protein:**

Look at the top hit:
1. What protein is this?
2. What is the full name?
3. What does this protein do? (Click the accession number of the top hit to open UniProt/GenBank)

**Part B — Explore conservation across species:**

Now run the same BLAST again, but this time:
- **Remove** the organism restriction (delete "Homo sapiens")
- This will search ALL organisms

After BLAST runs:
1. List the organisms found in the top 10 hits and their % identity
2. Fill in this table:

| Organism | % Identity | E-value |
|----------|-----------|---------|
| Homo sapiens (human) | | |
| Pan troglodytes (chimpanzee) | | |
| Mus musculus (mouse) | | |
| Rattus norvegicus (rat) | | |
| Danio rerio (zebrafish) | | |
| Drosophila melanogaster (fruit fly) | | |

3. Is this protein highly conserved? What does that tell you about its function?

**Part C — Find the mutation (challenge):**

The normal (wild-type) human version of this protein is in UniProt. Search UniProt for this protein.

Compare position **12** of your patient sequence against the wild-type sequence. The standard amino acid there is **Glycine (G)**. What is it in your patient sequence? 

> 💡 This is actually a famous real-world mutation — G12V in KRAS — found in approximately 30% of all human cancers. It locks the protein in a permanently active state, driving uncontrolled cell growth.

---

### Exercise 3 — Use BLAST to Confirm Domain Structure (Bonus, 5 min)

**Scenario:** You have a short, unknown protein fragment and want to know if it contains a known functional domain.

**Short query sequence:**
```fasta
>unknown_domain_fragment
MGSSHHHHHHSSGLVPRGSHMASMTGGQQMGRDPNSSSVDKLAALDSSGEGKDDQPRGEQ
LLDALNLSKTIVKALKAAAGVTMNSKVAEHEQFLEKERTSLESRLVQTLKQTYAEHLNLQ
NNLNQLEKNMSQLEKKIQELEAELAQKQKEQELQKQLEELQQQIQELQQQIEELKAKLAS
ELEQALQKAKQQLEQELQALEDKLQAEVTSKNHLETLRQTLSSEEKVKAEIEGELEQLRR
```

1. Run blastp against the nr database
2. In the results, look for the **"Graphic Summary"** at the top — it shows you a visual map of where alignments fall in your query
3. Also look for "Conserved Domains" — BLAST automatically searches the CDD (Conserved Domain Database)

**Questions:**
1. What protein family does this belong to?
2. What conserved domain is detected?
3. Is this an intracellular or extracellular protein?

---

## 6. Hands-on Session — Multiple Sequence Alignment

> ⏱️ **Time:** ~20 minutes  
> 🔧 **Tool:** Clustal Omega at EMBL-EBI — https://www.ebi.ac.uk/Tools/msa/clustalo/  
> 🎯 **Goal:** Align homologous sequences and identify conserved regions

---

### Exercise 1 — Align p53 Across Species (12 minutes)

**Scenario:** The p53 protein is one of the most important tumor suppressors in biology. It is found in virtually all animals. You want to understand which parts of p53 are most conserved across evolution — these are likely the most functionally critical regions.

**Your sequences (copy everything from the opening `>` to the next `>`):**

```fasta
>p53_human
MEEPQSDPSVEPPLSQETFSDLWKLLPENNVLSPLPSQAMDDLMLSPDDIEQWFTEDPGPDEAPRMPEAAPPVAPAPAAPTPAAPAPAPSWPLSSSVPSQKTYPQGLAEDESAGRSRAGSLLGRNSFEVRACPGRDRRTEEENLRKKGQVLKEIREGQRLPDPASSTCSSALHSSTSRHKKLMFKTEGPDSD
GPQHLIRVEGSQLAQDDCMFGQRMPDDLRDNLPQHSFQENISKVHLNVLNKLREQLGQHFLQQDLEKEPMVKPTSEEMSSPELLFQRQIRQELKEDLKELEKQIGELRQQIKELQEELKRQLQHYRERIKQELDQETGEFLQEQSQQLEDSNLRKLEGAKREGGPKLKIVLRDLAQELQEQVEDLKQKLEDYLQEKQHELEQASESLRQRLEELQDTSRKLQDLEQRLQELTQRYQNLLEQKQKLEMELQAQLENQHQAELGQ
MEGQAVKQYIRENSQLLKKKQLVNELKRLQDVYNEFYLVNEYKLDMDLNIQKTLRDIQKLQDILEELQKRLETSDGKLELERMKKQQGEALTSEFPKIACSQDLYSVMPLLQDFLAHSASRGEETLQHLQQLLENERNKLMQELQHFRQKIESLDKNIQQLEDQNSLEKENIKFIVQNLHQLREKNELLENALEALQNQIADLHQNILEELKQMNAELELERNNIAQFEAQLKDEQIQQQLKDIHTQHIQELEDQISSQLQKLQQEL
QHYAQKLAKELQEKIMELTQRFQQQIKELQQKLHQDSVEELQTHQQKLNELQTLTQKLHQ

>p53_mouse
MEEPQSDPSVEPPLSQETFSDLWKLLPENNVLSPLPSQAMDDLMLSPDDIEQWFTEDPGPSEAPRMPEAAPPVAPAPAAPAPAAPAASSWPLSSSIPSQKTYPQGLASDESAGRSRAGSLLGRNSFEVRVCPGRDRRTEAENLHKKGQVLKEIREGQRLPDPAASTCSSSLHSGTSRHKKLMFKTEGPDSD

>p53_zebrafish
MEEPGSPSVEAPLSQDTFSDLWKLLPENNLLSPLPSQATDDLMLSPEDIEQWFTEDPGPADSARLPEAAPSAASVAPAAPSWPLSSSIPSQKSYPQGLAQDETVSRAGSLLGRSPFEVRACPGRDRRSAEVNLQKRGQVLKEIREGQRLPNPVASTCSAALHSATSRHKKLMFKTEG

>p53_frog
MEEPHPDPSVEPPLTQETFSDLWKLLPENNVLSPLPSQAMDDLMLSPDDIEQWFTEDPGPADAAPMPEPAPPVAPAPAAPAPAAPAASSWPLSSSIPSQKTYPQGLATDESSGRSRAGSLLGRNSFEVRVCPGRDRRTEEENLHKKGQVLKEIREGQRLPDPAASTCSSALHSGTSRHKKLMFKTEG

>p53_fruitfly
MSANQETVSQDDLHLESPTTDLWDLLQAQMQNQSIFPSSASLNLPAAMLSSPELIQWFA
EDPNSDSGPKAHTPLSSTLNQKVYPQGLAQDEEAGRSRPGSILGRNTLDVRVCPGRDRRT
EEENLHKKGEMLKEIREGQRLADKAALTCADALHSGTSRHKKLMFKTEGP
```

**Steps:**

1. Go to https://www.ebi.ac.uk/Tools/msa/clustalo/

2. Paste all five sequences above into the **"Sequence"** box

3. Output Format: select **"ClustalW with character counts"**

4. Check the box: **"Output order: Aligned"**

5. Click **Submit**

**Reading the output:**

The alignment will look something like:
```
p53_human    MEEPQSDPSVEPPLSQETFSDLWKLLPENN-----VLSPLPSQAMDDS...
p53_mouse    MEEPQSDPSVEPPLSQETFSDLWKLLPENN-----VLSPLPSQAMDDS...
p53_zebrafish MEEPGSPSVEAPLSQDTFSDLWKLLPENNLL---SPLPSQATDDLMLS...
p53_frog     MEEPHPDPSVEPPLTQETFSDLWKLLPENNVLSPLPSQAMDDLMLSP...
p53_fruitfly MSANQETVSQDDLHLESPTTDLWDLLQAQMQN--QSIFPSSASLNLPA...
             *      *  * * * **:***:* * **         *   *   *
```

**Questions to answer:**

1. **Find the most conserved block** — where is the longest stretch of `*` (asterisks) in the alignment? These are positions identical across all 5 species.

2. In the human sequence, the region `VVRCPHHERCSDSDGLAPPQHLIRVEGNLHAQDDFM...` (approximately positions 100–300 in the full alignment) is the DNA-binding domain. Is this region more or less conserved than the N-terminal region?

3. Look at positions where only the fruit fly sequence diverges. What might this indicate about evolutionary distance?

4. Why is fruitfly p53 shorter than human p53?

---

### Exercise 2 — Align Hemoglobin Subunits to Understand a Protein Family (8 minutes)

**Scenario:** Hemoglobin carries oxygen in your blood. It is made of four subunits — alpha (α), beta (β), gamma (γ), and delta (δ). These all evolved from a single ancestral gene through **gene duplication**. You will align them to see how related they are.

```fasta
>HBA_HUMAN_alpha
MVLSPADKTNVKAAWGKVGAHAGEYGAEALERMFLSFPTTKTYFPHFDLSHGSAQVKGHG
KKVADALTNAVAHVDDMPNALSALSDLHAHKLRVDPVNFKLLSHCLLVTLAAHLPAEFTP
AVHASLDKFLASVSTVLTSKYR

>HBB_HUMAN_beta
MVHLTPEEKSAVTALWGKVNVDEVGGEALGRLLVVYPWTQRFFESFGDLSTPDAVMGNPK
VKAHGKKVLGAFSDGLAHLDNLKGTFATLSELHCDKLHVDPENFRLLGNVLVCVLAHHFG
KEFTPPVQAAYQKVVAGVANALAHKYH

>HBG1_HUMAN_gamma
MGHFTEEDKATITSLWGKVNVEDAGGETLGRLLVVYPWTQRFFDSFGNLSSASAIMGNPK
VKAHGKKVLTSLGDAIKHLDDLKGTFAQLSELHCDKLHVDPENFKLLGNVMVIILATHFG
KEFTPELQASYPKVVAGVANALAHKYH

>HBD_HUMAN_delta
MVHLTPEEKTAVNALWGKVNVDAVGGEALGRLLVVYPWTQRFFASFGNLSSPTAILGNPM
VRAHGKKVLTAFNDGLKHLDNLKGTFAHLSELHCDKLHVDPENFRLLGNVLVIVLATHFG
KEFTPQMQAAYQKVVAGVANALAHKYH
```

**Steps:**

1. Run these 4 sequences through Clustal Omega (same tool as above)

2. Observe the output

**Questions to answer:**

1. Which two subunits are most similar to each other? (Look at the percent identity in the output, or count the conserved positions)

2. Are there regions that are identical across all four subunits? If so, what might those positions be important for?

3. The heme-binding site involves a critical **Histidine (H)** residue. Can you find a conserved `H` in the alignment? (Hint: it appears in the central portion of all four sequences)

4. If you were to build a phylogenetic tree from this alignment, what would it show about how these four genes are related to each other?

---

## Summary

### Alignment types at a glance

| Method | Sequences | Algorithm | Tool | Use case |
|--------|-----------|-----------|------|----------|
| Pairwise global | 2 | Needleman-Wunsch | EMBOSS Needle | Two full-length sequences |
| Pairwise local | 2 | Smith-Waterman | EMBOSS Water | Finding shared domains |
| Database search | 1 vs. millions | BLAST (heuristic) | NCBI BLAST | Identifying unknowns |
| Multiple alignment | 3+ | Progressive / Iterative | Clustal Omega, MUSCLE | Conservation, phylogeny |

### BLAST quick reference

| You have | You want to find | Use |
|----------|-----------------|-----|
| DNA sequence | Similar DNA sequences | blastn |
| Protein sequence | Similar proteins | blastp |
| DNA sequence | Proteins it might encode | blastx |
| Protein sequence | DNA encoding it | tblastn |

### Key concepts to remember

- **E-value < 0.001** = statistically significant BLAST hit
- **High conservation = high functional importance**
- BLAST finds relatives; MSA reveals exactly where and how they differ
- Gaps (`-`) in an alignment represent insertions or deletions that occurred during evolution

---

> ⏭️ **Next:** [Module 4 — Day 1 Review & Discussion](module4_summary_and_review.md)
