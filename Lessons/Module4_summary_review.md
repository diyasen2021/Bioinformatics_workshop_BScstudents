# Module 4 — Day 1 Review, Discussion & Big Picture

> **Type:** Interactive review + open discussion  
> **Duration:** ~45 minutes  
> **Level:** Beginner

---

## Table of Contents

1. [What We Covered Today](#1-what-we-covered-today)
2. [Connecting the Dots — A Complete Workflow](#2-connecting-the-dots--a-complete-workflow)
3. [Common Beginner Questions](#3-common-beginner-questions)
4. [Review Quiz](#4-review-quiz)
5. [Reflection Exercises](#5-reflection-exercises)
6. [Preview of Day 2](#6-preview-of-day-2)
7. [Glossary of Key Terms](#7-glossary-of-key-terms)

---

## 1. What We Covered Today

### Module 1 — What is Bioinformatics?
- Bioinformatics is the intersection of biology, computer science, and mathematics
- It exists because modern biology generates more data than humans can interpret manually
- It solves real problems: vaccine design, cancer diagnosis, pathogen tracking, crop improvement

### Module 2 — Sequence Data, Formats & Databases
- Biological data is stored as sequences of letters (DNA: ATGC; protein: 20 amino acid letters)
- **FASTA**: the universal format for storing any sequence
- **FASTQ**: raw sequencing data with per-base quality scores
- **GenBank**: richly annotated sequence records with metadata
- Four major databases, each with a distinct focus:

| Database | Focus | Unique value |
|----------|-------|--------------|
| NCBI | Nucleotide sequences, genes, literature | Most comprehensive; connects to PubMed |
| UniProt | Protein function & annotation | Best for understanding what a protein does |
| EMBL-EBI | European nucleotide archive, genome browsers | Ensembl genome browser; InterPro domains |
| PDB | 3D molecular structures | Only source for atomic coordinates |

### Module 3 — Alignments: BLAST & MSA
- A sequence alignment arranges sequences so matching positions line up
- **BLAST** searches databases to find sequences similar to your query
- The **E-value** measures statistical significance — smaller is better
- **Multiple Sequence Alignment (MSA)** aligns 3+ sequences to reveal conservation patterns
- Conserved positions across many species = functionally important

---

## 2. Connecting the Dots — A Complete Workflow

Here is how everything you learned today fits into a single real-world scenario.

---

### Scenario: A researcher sequences a patient's tumor biopsy and finds a gene she doesn't recognize

**Step 1 — Get the sequence data**  
The sequencer produces raw **FASTQ** files with millions of short reads and quality scores.

**Step 2 — Extract the sequence of interest**  
After processing the raw data, the researcher isolates a 300 bp fragment of interest and saves it as a **FASTA** file.

**Step 3 — BLAST it**  
She runs **blastn** at NCBI. The top hit has E-value `1e-142`, 98% identity to a known oncogene.  
→ *She now knows what gene she's likely looking at.*

**Step 4 — Retrieve the known record**  
She pulls the full **GenBank** record from NCBI to see what is annotated about this gene — where the coding sequence is, what exons it has, what protein it encodes.

**Step 5 — Look up the protein**  
She searches **UniProt** for the encoded protein. She learns it is a kinase involved in cell cycle control, and there are known disease variants.

**Step 6 — Multiple sequence alignment**  
She downloads orthologous sequences from 10 species and runs **Clustal Omega**. She discovers her patient's mutation falls at a position that is **100% conserved across all vertebrates** — a strong indication the mutation is functionally significant.

**Step 7 — Check the structure**  
She searches **PDB** for the protein's structure. The conserved position is in the ATP-binding pocket — this mutation may disrupt kinase activity.

**Conclusion:** A sequencing experiment → a biologically and clinically meaningful finding. Every step used tools from today's workshop.

---

## 3. Common Beginner Questions

**Q: Do I need to learn programming to do bioinformatics?**  
A: For the tools in today's workshop, no — everything we used was browser-based. However, as analyses become more complex (processing thousands of samples, custom pipelines), programming becomes essential. Python and R are the two most important languages in bioinformatics.

**Q: How do I know which database to use?**  
A: Start with this decision tree:
- "I have a DNA sequence and want to identify it" → NCBI BLAST (blastn)
- "I want to know everything about a protein's function" → UniProt
- "I want to see the 3D structure of a protein" → PDB (RCSB)
- "I want to visualize a gene in the context of the genome" → Ensembl (EMBL-EBI)

**Q: What does it mean if my BLAST search returns no hits?**  
Possible reasons:
1. The sequence has never been sequenced/deposited before (novel organism)
2. You chose the wrong BLAST program (e.g., running blastn on a protein)
3. The sequence is very short — BLAST needs at least 20–30 nt to work reliably
4. The sequence is heavily contaminated (low quality)

**Q: If two sequences are 40% identical, are they homologous?**  
A: With protein sequences, 40% identity over a full alignment almost certainly means they share common ancestry (are homologous) and likely have related function. With DNA, 40% may not be meaningful because DNA is only 4 letters and random sequences already match ~25%.

**Q: Is 100% identity between two sequences enough to say they are the same gene?**  
A: Not necessarily. Two genes can have identical sequences in the region you sequenced but differ elsewhere. Always check that query coverage is high (>90%) before concluding they are the same.

**Q: Why are there so many different sequence formats?**  
A: History. Different research groups developed different tools at different times, each creating their own format. FASTA is the oldest and simplest; FASTQ was created specifically for next-generation sequencing to carry quality information. Most modern tools accept multiple formats.

---

## 4. Review Quiz

Test your understanding. Answers are at the bottom of this section.

**Question 1**  
A student performs a BLAST search and gets the following results. Which hit is most significant?

| Hit | E-value |
|-----|---------|
| A | 0.45 |
| B | 1e-85 |
| C | 0.003 |
| D | 1e-12 |

**Question 2**  
You have a protein sequence and want to find what it looks like in 3D. Which database do you search?  
a) NCBI GenBank  
b) UniProt  
c) PDB  
d) EMBL ENA  

**Question 3**  
In a FASTA file, what character begins the header line?  
a) `#`  
b) `@`  
c) `>`  
d) `!`  

**Question 4**  
A position in a multiple sequence alignment is marked with `*` (asterisk). What does this mean?  
a) The position is a gap in all sequences  
b) The position is identical in all aligned sequences  
c) The position is variable across sequences  
d) The position is deleted in one sequence  

**Question 5**  
You have a DNA sequence and want to find what protein it might encode, by searching against a protein database. Which BLAST program do you use?  
a) blastn  
b) blastp  
c) blastx  
d) tblastn  

**Question 6**  
Which UniProt section contains manually reviewed, expert-curated entries?  
a) TrEMBL  
b) Swiss-Prot  
c) RefSeq  
d) GenBank  

**Question 7**  
You run BLAST and get a hit with 100% percent identity but only 15% query coverage. What does this tell you?  
a) The hit is a perfect match for your entire sequence  
b) The hit matches a very small portion of your query  
c) The match is not statistically significant  
d) The hit is from a different organism  

**Question 8**  
Which file format stores raw sequencing reads with base quality scores?  
a) FASTA  
b) GenBank  
c) PDB  
d) FASTQ  

---

**Answers:**  
1→B (smallest E-value = most significant) | 2→C | 3→C | 4→B | 5→C | 6→B | 7→B | 8→D

---

## 5. Reflection Exercises

These are discussion questions — there is no single right answer. Think about them individually, then discuss with your group.

---

**Exercise 1 — Interpreting a result**

You BLAST an unknown protein sequence. The results show:
- Top hit: "Hypothetical protein XYZ123 [uncultured marine bacterium]" — E-value 1e-47, 41% identity
- Second hit: "Alcohol dehydrogenase [Bacillus subtilis]" — E-value 1e-31, 38% identity

What can you reasonably conclude? What would your next step be?

*Discussion points: What does "hypothetical protein" mean? Would you trust the top hit or look more carefully at the second? How would you verify the function?*

---

**Exercise 2 — Conservation and disease**

You are analyzing a patient with a suspected hereditary disease. You align the patient's protein sequence with the same protein from 20 species. You find a mutation at position 112 (Arginine → Cysteine). In all 20 other species, position 112 is Arginine.

- Is this mutation likely to be disease-causing? What is your reasoning?
- What additional evidence would you want to collect?
- Is evolutionary conservation alone enough to confirm pathogenicity?

---

**Exercise 3 — Choosing the right tool**

For each scenario, which database or tool would you use first and why?

a) You isolated a new antibiotic-producing bacterium and want to know what species it is.  
b) You want to know the function of the protein encoded by the gene FOXP2.  
c) You want to see the 3D structure of the insulin receptor to understand how metformin might bind.  
d) You want to find all published research papers about a specific gene.  
e) You want to compare the genome structure of humans and chimpanzees side by side.  

---

**Exercise 4 — Real-world thinking**

The COVID-19 pandemic killed millions of people. Bioinformatics played a crucial role in the response.

- On January 10, 2020, the SARS-CoV-2 genome sequence was publicly released in GenBank. Within 2 days, vaccine sequences were designed. What bioinformatics steps were likely involved?
- During the pandemic, viral sequences were collected globally and deposited in the GISAID database. How would multiple sequence alignment help epidemiologists track the virus?
- Why is it important that these databases are publicly accessible and free to use?

---

## 6. Preview of Day 2

Tomorrow we will build on what we learned today and move into more quantitative and applied topics.

| Module | Topic |
|--------|-------|
| Module 5 | Introduction to Genomics — genome assembly and annotation |
| Module 6 | Gene expression — what is RNA-seq and how do we analyze it? |
| Module 7 | Phylogenetics — building evolutionary trees |
| Module 8 | Mini-project — an end-to-end analysis from sequence to conclusion |

**To prepare:**  
- Make sure you can recall the BLAST E-value rule: *smaller = more significant*
- Remember the four nucleotide bases: A, T, G, C (DNA) and A, U, G, C (RNA)
- Be ready to use the tools from today — we will build directly on them

---

## 7. Glossary of Key Terms

| Term | Definition |
|------|-----------|
| **Accession number** | A unique identifier assigned to a biological sequence when it is deposited in a database |
| **Alignment** | Arranging sequences so that corresponding positions are in the same column |
| **Amino acid** | One of 20 building blocks of proteins, each encoded by a three-nucleotide codon |
| **Annotation** | Adding biological meaning to a raw sequence — marking genes, exons, regulatory regions, etc. |
| **BLAST** | Basic Local Alignment Search Tool — searches a database for sequences similar to a query |
| **Bit score** | A normalized alignment score in BLAST; higher = stronger match |
| **Conservation** | A sequence position that remains identical or similar across many species |
| **CDS** | Coding sequence — the portion of an mRNA that encodes protein |
| **E-value** | Expected value — the number of matches expected by chance; lower = more significant |
| **FASTA** | A simple text format for biological sequences (header line + sequence) |
| **FASTQ** | A format for sequencing reads that includes per-base quality scores |
| **Gap** | A `-` character inserted into an alignment to represent an insertion or deletion |
| **GenBank** | NCBI's primary nucleotide sequence database; also a file format with rich annotation |
| **Genome** | The complete set of DNA of an organism |
| **Genomics** | The study of entire genomes — sequencing, assembly, and annotation |
| **Homolog** | A sequence related to another by descent from a common ancestor |
| **Indel** | Insertion or deletion — a type of mutation where nucleotides are added or removed |
| **MSA** | Multiple Sequence Alignment — alignment of three or more sequences simultaneously |
| **Mutation** | A change in the DNA sequence compared to a reference |
| **Nucleotide** | A building block of DNA/RNA (adenine, thymine, guanine, cytosine, or uracil) |
| **Ortholog** | A homologous gene in a different species that evolved from a common ancestral gene by speciation |
| **PDB** | Protein Data Bank — the global repository for 3D structures of biological molecules |
| **Percent identity** | The fraction of identical positions in an alignment |
| **Phred score** | A logarithmic measure of base call quality in FASTQ files (Q30 = 99.9% accuracy) |
| **Protein** | A chain of amino acids that folds into a 3D structure and performs biological functions |
| **Proteomics** | The large-scale study of all proteins in a cell or organism |
| **Query** | The input sequence you provide to a BLAST search |
| **Query coverage** | The fraction of the query sequence that is covered by a BLAST alignment |
| **RefSeq** | NCBI's reference sequence database — curated, non-redundant, high-quality |
| **RNA** | Ribonucleic acid — the intermediate molecule between DNA and protein |
| **Subject** | The database sequence that matches your query in a BLAST search |
| **Swiss-Prot** | The manually curated, high-quality section of the UniProt database |
| **Transcriptomics** | The study of all RNA molecules expressed by a cell at a given time |
| **TrEMBL** | The computationally annotated (unreviewed) section of UniProt |
| **UniProt** | The world's primary database for protein sequences and functional information |

---

> 🏠 **End of Day 1**  
> See you tomorrow for Day 2 — we go deeper into genomics, gene expression, and evolutionary analysis!
