# Module 3 — Day Review, Discussion & Big Picture

# Practical Exercises — Sequence Alignment & Biological Databases

> **Level:** BSc Bioinformatics  
> **Tools:** NCBI BLAST, NCBI Databases, Clustal Omega, Simple Phylogeny (EBI)  
> **Format:** Three exercises, increasing in complexity

---

## Exercise 1 — Outbreak Investigation ⭐⭐⭐

### Scenario

A hospital in Southeast Asia has reported a cluster of severe respiratory illness. Five patients have been admitted over two weeks with similar symptoms — high fever, rapid lung deterioration, and poor response to standard antibiotics. Throat swabs were taken and sent for sequencing. You have been given five partial 16S rRNA sequences, one from each patient.

Your job is to identify the pathogen, determine whether all five cases are caused by the same organism or represent different sources of infection, and place it in evolutionary context relative to known pathogens. The clinical team is waiting for your report.

---

### Your sequences

```fasta
>Patient_1
AGAGTTTGATCCTGGCTCAGATTGAACGCTGGCGGCAGGCCTAACACATGCAAGTCGAGC
GGTAGCACAGAGAGCTTGCTCTCGGGTGACGAGCGGCGGACGGGTGAGTAATGTCTGGGA
AACTGCCCGATGGAGGGGGATAACTACTGGAAACGGTAGCTAATACCGCATAATGTCGACA
GACCAAAGAGGGGGACCTTCGGGCCTCTTGCCATCAGATGTGCCCAGATGGGATTAGCTAG
TAGGTGGGGTAACGGCTCACCTAGGCGACGATCCCTAGCTGGTCTGAGAGGATGACCAGCC

>Patient_2
AGAGTTTGATCCTGGCTCAGATTGAACGCTGGCGGCAGGCCTAACACATGCAAGTCGAGC
GGTAGCACAGAGAGCTTGCTCTCGGGTGACGAGCGGCGGACGGGTGAGTAATGTCTGGGA
AACTGCCCGATGGAGGGGGATAACTACTGGAAACGGTAGCTAATACCGCATAATGTCGACA
GACCAAAGAGGGGGACCTTCGGGCCTCTTGCCATCAGATGTGCCCAGATGGGATTAGCTAG
TAGGTGGGGTAACGGCTCACCTAGGCGACGATCCCTAGCTGGTCTGAGAGGATGACCAGCC

>Patient_3
AGAGTTTGATCCTGGCTCAGAGTGAACGCTGGCGGCAGGCTTAACACATGCAAGTCGAAC
GGCAGCACGGGTGCTTGCACCTGGTGGCGAGCGGCGGACGGGTGAGTAACGCGTGGGAAT
CTACCTTATAGTGGGGGATAACTATTGGAAACGATAGCTAATACCGCATAATGTCTACGGA
CCAAAGAGGGGGACCTTCGGGCCTCTTGCCATCAGATGAGCCCAGATGGGATTAGCTAGTA
GGTGGGGTAATGGCTCACCTAGGCGACGATCCCTAGCTGGTCTGAGAGGATGATCAGCCAC

>Patient_4
AGAGTTTGATCCTGGCTCAGATTGAACGCTGGCGGCAGGCCTAACACATGCAAGTCGAGC
GGTAGCACAGAGAGCTTGCTCTCGGGTGACGAGCGGCGGACGGGTGAGTAATGTCTGGGA
AACTGCCCGATGGAGGGGGATAACTACTGGAAACGGTAGCTAATACCGCATAATGTCGACA
GACCAAAGAGGGGGACCTTCGGGCCTCTTGCCATCAGATGTGCCCAGATGGGATTAGCTAG
TAGGTGGGGTAACGGCTCACCTAGGCGACGATCCCTAGCTGGTCTGAGAGGATGACCAGCC

>Patient_5
TTGAAGAGTTTGATCCTGGCTCAGAACGAACGCTGGCGGCAGGCTTAACACATGCAAGTC
GAACGGCAGCATGGGCGCTTGCACCTGGTGGCGAGCGGCGGACGGGTGAGTAACGCGTGG
GAATCTATCCTATAGTGGGGGATAACTATTGGAAACGATAGCTAATACCGCATAACGTCTC
TGAACCAAAGAGGGGGACCTTCGGGCCTCTTGCCATCAGATGCACCCAGATGGGATTAGCT
AGTAGGTGGGGTAATGGCTCACCTAGGCGACGATCCCTAGCTGGTCTGAGAGGATGATCAG
```

---

### Part A — Identify the pathogen (BLAST)

Run each sequence through NCBI BLAST. Use the **16S ribosomal RNA sequences** database for a clean, targeted search.

For each patient, record:
- Top hit organism
- % identity
- E-value
- Query coverage

> 💡 **Hint:** Pay close attention to which patients share identical or near-identical top hits and which do not. This has clinical implications.

**Questions:**
1. Are all five patients infected with the same organism?
2. If not — which patients cluster together, and which appear to be a different source?
3. Look up the organism(s) you identified. Are they known pathogens? What diseases do they cause?

---

### Part B — Multiple sequence alignment (Clustal Omega)

Now fetch the full 16S rRNA sequences for your top BLAST hits from NCBI. You want one representative sequence per distinct organism identified across the five patients — download them in FASTA format.

Run a multiple sequence alignment using **Clustal Omega** at https://www.ebi.ac.uk/Tools/msa/clustalo/

> 💡 **Hint:** Include your five patient sequences in the alignment alongside the reference sequences you fetched. This lets you see directly where each patient's sequence sits relative to known strains.

**Questions:**
1. Where are the most conserved regions in the alignment? Where does the most variation occur?
2. Can you identify positions that differ consistently between the two groups of patients?

---

### Part C — Build a simple phylogenetic tree

EBI provides a basic phylogeny tool that runs directly from a Clustal Omega alignment. After your alignment completes, look for the **"Phylogenetic Tree"** tab in the results.

You have not covered phylogenetics in detail yet — that is fine. Focus on reading the tree topology rather than the branch lengths.

> 💡 **Hint:** A phylogenetic tree groups sequences by similarity. Sequences that share a more recent common ancestor appear closer together. Look at which patient sequences cluster with which reference sequences.

**Questions:**
1. Draw or screenshot the tree. Do the patient sequences form one cluster or two?
2. Which known reference strain is each group of patients most closely related to?
3. Based on the tree, do you think this outbreak has a single source or multiple independent sources of infection? What would this mean for the clinical team's response?

---

## Exercise 2 — Genome Assembly Metadata ⭐⭐

### Scenario

You are starting a comparative genomics project on *Clostridioides difficile* — a notorious hospital-acquired pathogen responsible for severe gut infections and increasingly resistant to treatment. Before doing any analysis you need to survey what genome assemblies are publicly available and build a summary table of their key metadata.

Find **five complete (not scaffold or contig level) genome assemblies** for *Clostridioides difficile* in the NCBI Assembly database and compile their metadata into a spreadsheet.

---

### What to collect

For each assembly, record the following:

| Field | Where to find it |
|-------|-----------------|
| Assembly accession | Assembly page |
| Strain name | Assembly page |
| Assembly level | Complete / Scaffold / Contig |
| Genome size (Mb) | Assembly stats |
| Number of contigs | Assembly stats |
| GC content (%) | Assembly stats |
| Sequencing technology | Assembly page or linked BioSample |
| Country of isolation | Linked BioSample record |
| Isolation source | Linked BioSample record |
| Year submitted | Assembly page |

> 💡 **Hints:**
> - Start at https://www.ncbi.nlm.nih.gov/assembly
> - Filter by organism name and use the **"Assembly level"** filter on the left to show only complete genomes
> - Each assembly links to a **BioSample** record — this is where you will find isolation metadata like country and source
> - You can download a summary table directly from NCBI rather than clicking through each record individually — explore the **"Download"** options on the search results page

---

### Questions

1. Is there much variation in genome size across your five assemblies? What might cause genuine biological variation in genome size between strains of the same species?
2. Do the assemblies come from a variety of countries, or are they concentrated in particular regions? What might this reflect about research activity or disease burden?
3. What sequencing technologies were used? Do you notice a relationship between sequencing technology and assembly quality (e.g. number of contigs)?
4. *C. difficile* is known for carrying mobile genetic elements that confer antibiotic resistance. How might you use these assemblies to investigate that — what would your next step be?

---

## Exercise 3 — Exploring the Taxonomy Database ⭐

### Scenario

A colleague hands you a list of organism names from a metagenomic study of soil samples. Some are familiar, some are not. You need to verify each one, find its taxonomic lineage, and flag any that may be misidentified or outdated synonyms.

---

### Your organism list

1. *Candidatus Solibacter usitatus*
2. *Gemmatimonas aurantiaca*
3. *Conexibacter woesei*
4. *Ktedonobacter racemifer*
5. *Chloracidobacterium thermophilum*

---

### What to do

Use the **NCBI Taxonomy Browser** at https://www.ncbi.nlm.nih.gov/taxonomy to look up each organism.

For each one, record:

| Field | Description |
|-------|-------------|
| Full taxonomic lineage | From domain down to species |
| Phylum | |
| Is the name current or a synonym? | Check for any "also known as" notes |
| Any genome assemblies available? | How many? |
| Any nucleotide sequences in GenBank? | Rough count |

> 💡 **Hints:**
> - The taxonomy page for each organism links directly to all associated sequence data in NCBI — use these links
> - "Candidatus" is a designation for organisms that have been detected but never successfully cultured in the lab — note which organisms have this status and think about what it implies for the available data
> - Some of these organisms belong to phyla you may never have heard of — look them up briefly

---

### Questions

1. How many of these organisms belong to well-known phyla (e.g. Proteobacteria, Firmicutes) versus obscure or recently described phyla?
2. What does the *Candidatus* designation mean, and how does it affect the quantity and type of data available for that organism compared to the cultured organisms on the list?
3. Which organism has the most genome assemblies available? What might explain this?
4. If you were designing a PCR primer to detect one of these organisms in an environmental sample, which database within NCBI would you use next, and why?

---

## 2. Common Beginner Questions

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

## Review Quiz

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
