# Practical Exercises — UniProt & Protein Databases

---

## Before you start — GenBank or UniProt?

This is one of the most common points of confusion in bioinformatics. Here is the short answer:

| You have... | Go to... |
|-------------|----------|
| A DNA or RNA sequence | NCBI / GenBank / ENA |
| A protein sequence | UniProt |
| A gene and want the protein it encodes | Start at NCBI, follow the link to UniProt |
| A protein and want the gene that encodes it | Start at UniProt, follow the link to NCBI |

GenBank stores nucleotide sequences — genes, genomes, transcripts. UniProt stores protein sequences with rich functional annotation — what the protein does, where it is in the cell, what it interacts with, whether mutations in it cause disease. The two databases cross-reference each other constantly.

A practical rule of thumb: if your question is biological and functional ("what does this protein do?"), UniProt is almost always the better starting point.

---

## Exercise 1 — Navigating a UniProt Entry ⭐

### Scenario

You are investigating insulin — one of the most clinically important proteins in human biology. Before doing any computational analysis, you want to understand what a fully annotated UniProt entry looks like and what kinds of information it contains.

---

### Steps

1. Go to https://www.uniprot.org
2. Search for **human insulin**
3. You will see two types of results — entries labelled **Swiss-Prot** (with a gold star) and **TrEMBL** (unreviewed). Open the Swiss-Prot entry for human insulin — the accession is **P01308**

Spend a few minutes exploring the entry. UniProt organises information into sections — Function, Names & Taxonomy, Subcellular Location, Disease & Variants, PTM/Processing, Expression, Interaction, Structure, Family & Domains, Sequences.

---

### Questions

1. What is the function of insulin according to the UniProt entry? 
2. Where in the cell is it located, and where is it secreted?
3. UniProt lists several **natural variants** — mutations found in human populations. Find one that is associated with a disease. What is the amino acid change and what condition does it cause?
4. Insulin is processed from a precursor protein. What is the precursor called, and how many chains does mature insulin consist of?
5. The entry has a **Swiss-Prot** label. What does this mean, and how is it different from a TrEMBL entry? Why does it matter which one you use?

> 💡 **Swiss-Prot vs TrEMBL:** Swiss-Prot entries are manually reviewed and annotated by expert curators. TrEMBL entries are computationally predicted and not yet reviewed. For functional information, always prefer Swiss-Prot.

---

## Exercise 2 — UniProt BLAST ⭐⭐

### Scenario

You are working on a project studying antibiotic resistance in *Staphylococcus aureus*. A collaborator has sent you the amino acid sequence of a protein they found in a drug-resistant clinical isolate. They suspect it may be a penicillin-binding protein involved in resistance, but they are not sure. Your job is to identify it.

---

### Your mystery sequence

```
MKKNIFLSALVLSTLVLSGCATPTENTQNTNLEQKTKQRIAALFPQDKLTAPQKMNLNLS
QPGVKVDPVTGKIVADFNNDKAFSETLTTKGDIINNQMSYGTFLPHRYNSTYRQPYLNSY
VQNKQGQFKGLFAAYTGMSDKVNMAQFNNEFGIKNQDIITNDSMLVKKLQNKPETPEQLA
KTLNELQTKNPELAKQLKDDPSLRELRELRQTLREIQNGRESIQNLKQALADNQRKTIAE
LEQARQDLQNKIEDLQSQLNELQTQAKSLKNELEAKQKDLENLKTQLEELKTKAKDLEAQ
LKTLQEAKDQLKNLEQAKSDLKQKLEELKAKAEELKQKLDELKAKAEELKQKLDELKAKA
```

---

### Steps

1. Go to https://www.uniprot.org/blast
2. Paste the sequence above into the query box
3. Set the target database to **UniProtKB/Swiss-Prot** — this searches only reviewed entries, giving you cleaner results
4. Leave other settings as default and run the search

---

### Questions

1. What is the top hit? What is the protein name and which organism is it from?
2. What is the E-value and % identity of the top hit?
3. Read the functional annotation of the top hit. What does this protein do, and how is it linked to antibiotic resistance?
4. Now run the same search again but change the database to **UniProtKB** (the full database including TrEMBL). How many more hits do you get? Are the top hits the same?
5. Why might you choose Swiss-Prot over the full database for an initial search, even though it contains fewer sequences?

---

## Exercise 3 — Align Two Protein Sequences ⭐⭐

### Scenario

BRCA1 is a well-known human tumour suppressor — mutations in it dramatically increase the risk of breast and ovarian cancer. You want to compare the human BRCA1 protein to its mouse equivalent to understand how conserved it is, and whether the regions associated with human disease mutations are conserved in mice.

---

### Steps

1. Go to https://www.uniprot.org
2. Find the **human BRCA1** protein — UniProt accession **P38398**
3. Find the **mouse BRCA1** protein — search for "BRCA1 mouse" and take the Swiss-Prot entry (accession **P48754**)
4. To align them: from either entry, click **Align** in the toolbar, or go directly to https://www.uniprot.org/align
5. Add both accessions and run the alignment

---

### Questions

1. What is the overall % identity between human and mouse BRCA1?
2. Does the alignment show any large gaps? What might these represent biologically?
3. The BRCT domain (approximately the last 200 amino acids of the human protein) is the most functionally critical region. Is this region more or less conserved than the rest of the protein?
4. Many disease-causing mutations in human BRCA1 cluster in the BRCT domain. Based on the alignment, would you expect equivalent mutations in mice to also affect protein function? Why?
5. BRCA1 is notably **less conserved** between human and mouse than most proteins of similar importance. Search briefly — why is this thought to be the case?

---

## Exercise 4 — ID Mapping ⭐⭐

### Scenario

You have just finished analysing an RNA-seq dataset. Your pipeline has given you a list of differentially expressed genes as **Ensembl gene IDs** — the identifier format used by the Ensembl genome browser. Before you can look these genes up in UniProt or cross-reference them with protein databases, you need to convert them to UniProt accessions. Doing this one by one would take hours. ID Mapping does it in seconds.

---

### Your gene list

```
ENSG00000141736
ENSG00000012048
ENSG00000139618
ENSG00000171862
ENSG00000183765
ENSG00000116062
ENSG00000065559
ENSG00000197386
```

---

### Steps

1. Go to https://www.uniprot.org/id-mapping
2. Paste the list of Ensembl IDs above into the input box
3. Set **From** to `Ensembl` and **To** to `UniProtKB`
4. Run the mapping
5. Download the results as a TSV file

---

### Questions

1. How many of your 8 IDs mapped successfully to a UniProt entry?
2. For any that did not map — can you find out why? (Try searching the Ensembl ID directly in NCBI or Ensembl)
3. Look at the mapped proteins. Do any of them have a known link to cancer or genetic disease? (Check the Disease & Variants section of each UniProt entry)
4. Some Ensembl IDs may map to both a Swiss-Prot and a TrEMBL entry. If this happens, which would you use for downstream analysis and why?
5. ID mapping is useful beyond just Ensembl → UniProt. What other scenarios in a bioinformatics workflow might require converting between identifier systems?

---

## Putting it together — when to use what

After completing these exercises, you should have a clearer picture of when each database is the right tool:

| Task | Best resource |
|------|--------------|
| Identify an unknown nucleotide sequence | NCBI BLAST (blastn) |
| Identify an unknown protein sequence | UniProt BLAST or NCBI blastp |
| Understand what a protein does | UniProt Swiss-Prot entry |
| Find disease mutations in a protein | UniProt — Disease & Variants section |
| Compare two protein sequences | UniProt Align |
| Convert between gene/protein identifier systems | UniProt ID Mapping |
| Download raw sequencing reads | NCBI SRA or ENA |
| Find genome assemblies | NCBI Assembly |
| Find taxonomic lineage of an organism | NCBI Taxonomy |

No single database does everything. Real bioinformatics workflows move between several databases constantly — and knowing which one to reach for is a skill you will keep building throughout this course.

---

*BSc Bioinformatics — UniProt & Protein Databases Practical*
