# Module 2 — Extended Exercises: Database Navigation & Cross-Linking

> **Type:** Hands-on exercises  
> **Duration:** ~90 minutes  
> **Level:** Intermediate → Challenging  
> **Tools:** NCBI, UniProt, EMBL-EBI, PDB, AlphaFold — all browser-based  
> **Builds on:** Module 2 (basic database retrieval) and Module 3 (BLAST, MSA)

---

> ### Before you begin
> These exercises are designed to stretch you. You will not always be told exactly which button to click — part of the challenge is learning to **navigate** a database you haven't fully explored yet. When you get stuck, try:
> 1. Reading the page more carefully — the answer is usually visible
> 2. Using the database's own search bar
> 3. Following a cross-link to another database
>
> There are no trick questions. Everything asked can be found using the databases listed.

---

## Table of Contents

- [Intermediate Exercises](#intermediate-exercises)
  - [Exercise A — Follow a Disease Variant Across Four Databases](#exercise-a--follow-a-disease-variant-across-four-databases)
  - [Exercise B — Trace a Drug Target from Sequence to Structure to Pathway](#exercise-b--trace-a-drug-target-from-sequence-to-structure-to-pathway)
  - [Exercise C — Reconstruct a Protein Family from Scratch](#exercise-c--reconstruct-a-protein-family-from-scratch)
- [Challenging Exercises](#challenging-exercises)
  - [Exercise D — The Unknown Pathogen](#exercise-d--the-unknown-pathogen)
  - [Exercise E — Reverse Engineering a Research Paper](#exercise-e--reverse-engineering-a-research-paper)
  - [Exercise F — The Database Contradiction](#exercise-f--the-database-contradiction)

---

# Intermediate Exercises

---

## Exercise A — Follow a Disease Variant Across Four Databases

> ⏱️ **Estimated time:** 20 minutes  
> 🎯 **Learning goal:** Understand how clinical variant data, protein function, and 3D structure are all connected

### Background

Phenylketonuria (PKU) is one of the most common inherited metabolic disorders — babies are routinely screened for it at birth. It is caused by mutations in the gene *PAH*, which encodes the enzyme **phenylalanine hydroxylase**. Without this enzyme, the amino acid phenylalanine accumulates to toxic levels in the brain, causing severe intellectual disability if untreated.

The treatment is entirely dietary — avoiding phenylalanine — which is why PKU patients cannot consume aspartame (an artificial sweetener that contains phenylalanine).

You are going to follow a single disease-causing mutation from a clinical database all the way to its position in the 3D structure of the protein.

---

### Part 1 — Find the gene at NCBI (5 min)

1. Go to **https://www.ncbi.nlm.nih.gov/**
2. Change the database to **"Gene"** and search: `PAH Homo sapiens`
3. Open the top result (Gene ID: 5053)

Answer the following from the Gene page:

| Question | Your answer |
|----------|-------------|
| On which chromosome is PAH located? | |
| How many exons does the RefSeq transcript have? | |
| What is the RefSeq mRNA accession number (NM_...)? | |
| What is the RefSeq protein accession number (NP_...)? | |

---

### Part 2 — Find disease variants at ClinVar (5 min)

ClinVar is NCBI's database of clinically relevant genetic variants. You can reach it from the PAH Gene page.

1. On the PAH Gene page, scroll to the **"Genomic regions, transcripts, and products"** section
2. Look for a link to **ClinVar** in the left panel under "Related information", OR go directly to:  
   **https://www.ncbi.nlm.nih.gov/clinvar/?term=PAH[gene]**

3. Filter results to show only **"Pathogenic"** variants (use the filter panel on the left)

Answer the following:

| Question | Your answer |
|----------|-------------|
| Approximately how many pathogenic variants are listed for PAH? | |
| Find the variant **p.Arg408Trp** — what does this notation mean? (Hint: what amino acid changes to what?) | |
| What is the clinical significance of p.Arg408Trp? | |
| Is this variant common or rare in affected populations? (Look at the "Condition" section) | |

> 💡 **Reading variant notation:** `p.Arg408Trp` means: in the **p**rotein, at position **408**, **Arg**inine (R) has changed to **Tr**y**p**tophan (W). This is the standard HGVS nomenclature used in all clinical genetics.

---

### Part 3 — Examine the protein in UniProt (5 min)

1. Go to **https://www.uniprot.org/**
2. Search for: `PAH_HUMAN` or accession `P00439`
3. Open the entry

Answer the following:

| Question | Your answer |
|----------|-------------|
| What type of reaction does this enzyme catalyse? | |
| What cofactor does PAH require to function? | |
| Scroll to "Pathology & Biotech" — how many disease variants are annotated? | |
| Find position 408 in the "Natural variants" section — what is listed there? | |
| In the "Subcellular location" section, where in the cell is PAH found? | |

---

### Part 4 — Find the 3D structure and locate the mutation (5 min)

1. On the UniProt entry for PAH (P00439), scroll to the **"Structure"** section
2. Find a PDB entry for the structure of PAH — click on one of the PDB accession links
3. You are now on the RCSB PDB page for this structure

On the PDB page:

| Question | Your answer |
|----------|-------------|
| What is the PDB accession code you opened? | |
| What experimental method was used to solve the structure? | |
| What is the resolution (in Ångströms)? | |
| Click "3D View" — can you identify the active site region? (It is usually at the centre of the protein) | |

4. Now the key question: **Where is Arg408 in the structure?**
   - In the 3D viewer, look for the sequence viewer at the bottom
   - Try to scroll to position 408
   - Is Arg408 buried inside the protein or on the surface?

> 💡 **Why does this matter?** If a mutation affects a buried residue, it usually disrupts the protein's folding. If it affects the surface, it may disrupt binding to partners or substrates. Arg408 is in the catalytic core of PAH — changing it to Trp (a bulky aromatic residue) destroys the active site geometry.

---

### Synthesis question

Write 3–4 sentences connecting everything you found:  
*"The PAH gene encodes... The p.Arg408Trp mutation... In the 3D structure... This explains why..."*

---

---

## Exercise B — Trace a Drug Target from Sequence to Structure to Pathway

> ⏱️ **Estimated time:** 20 minutes  
> 🎯 **Learning goal:** Understand how databases connect molecular sequence → 3D structure → biological pathway → drug

### Background

Imatinib (brand name: Gleevec) was one of the first targeted cancer drugs and is often called the drug that proved precision medicine was possible. It works by inhibiting a protein called **BCR-ABL**, an abnormal fusion protein found in chronic myeloid leukaemia (CML) cells.

You are going to trace the drug target from sequence all the way to how the drug binds it.

---

### Part 1 — Find the normal ABL1 protein in UniProt (5 min)

1. Go to **https://www.uniprot.org/**
2. Search for: `ABL1 AND organism_id:9606 AND reviewed:true`
3. Open the entry **P00519 · ABL1_HUMAN**

Answer:

| Question | Your answer |
|----------|-------------|
| What is the full protein name? | |
| What type of kinase is ABL1? (serine/threonine or tyrosine?) | |
| What is the length of the canonical sequence in amino acids? | |
| Scroll to "Disease involvement" — what cancers is ABL1 associated with? | |
| In the "Family & Domains" section, what domains does ABL1 contain? List at least 3. | |

---

### Part 2 — Examine the kinase domain structure with a drug bound (7 min)

The power of structural biology is that we can see exactly how a drug molecule fits into a protein.

1. On the ABL1 UniProt page, scroll to **"Structure"**
2. Look for PDB entries — specifically find one that contains the word **"imatinib"** or **"STI-571"** in the title/ligand list  
   OR go directly to RCSB and search: `ABL1 imatinib`
3. A key structure is **PDB: 1IEP** — search for it directly at https://www.rcsb.org/

On the 1IEP PDB page:

| Question | Your answer |
|----------|-------------|
| What is the full title of this structure? | |
| What ligand (drug molecule) is bound? What is its 3-letter code? | |
| How many protein chains are in the structure? | |
| Open the **3D Viewer** — can you see the small drug molecule (usually shown as coloured sticks) nestled in the protein? | |

4. In the 3D viewer, try to find the **DFG motif** — a critical sequence in all kinases (Asp-Phe-Gly). In BCR-ABL positive CML, a mutation T315I in this region is called the "gatekeeper mutation" because it blocks imatinib binding.

---

### Part 3 — Connect to KEGG pathway (5 min)

1. Return to the UniProt page for ABL1 (P00519)
2. Scroll to **"Pathways"** in the left panel, or look in the cross-references section for **KEGG**
3. Click the KEGG link — it should open the KEGG entry for ABL1

On the KEGG page:

| Question | Your answer |
|----------|-------------|
| What is the KEGG orthology (KO) number for ABL1? | |
| List two KEGG pathways that ABL1 participates in | |
| Click on "Chronic myeloid leukemia" pathway — is ABL1 in the centre of this pathway map? | |
| What happens downstream of ABL1 signalling in this pathway? (Follow the arrows) | |

---

### Part 4 — Resistance mutations (3 min, challenge extension)

In the clinic, some CML patients stop responding to imatinib because of resistance mutations in BCR-ABL.

1. Go back to UniProt P00519
2. In **"Natural variants"**, find position **315**
3. What is the wild-type amino acid at 315?
4. Go to ClinVar or search the literature — what does T315I do to imatinib binding? (Hint: look at the 3D structure — does Thr315 contact the drug?)

---

---

## Exercise C — Reconstruct a Protein Family from Scratch

> ⏱️ **Estimated time:** 20 minutes  
> 🎯 **Learning goal:** Use BLAST to discover a protein family, then use MSA to understand it, entirely from a single starting sequence

### Background

You have been given a single protein sequence from a deep-sea bacterium. Nobody has characterised this protein before. Your task is to figure out what it is, what family it belongs to, and whether it is found in humans.

---

### Your mystery sequence

```fasta
>deep_sea_bacterium_protein_X
MKVAVLGAAGGIGQALALLLKDQLPSSVVVDITKDEFTGGVAVHPFAITPHTLKDAAKRN
GADVITFIEDVGHILKAFDEKYRPVVEGAKLAIEMTCGEIVVNRFADEHPKLVPQDLADL
LAEVLKQDKLTGLKAEQIVTAGKHLNKRFADLHDVQALLDSADAAIRERIKQLSQADIVL
AGHAVQSGMAGLEFKGADYDPEIYQGMQQYAKKHGLGGLIMAGEHAKQSIDAVIKEATQH
KALDIVEHMRKTAAKLTHPVFLPDKKKLNFDQPMFSLLDRFWVPSTSGKKAANLVPQALA
KIAQSHLVQAKEFEVPMPQAISGQNLRAMVSSMTEAIEQLQKAIEEHNQD
```

---

### Part 1 — BLAST it (7 min)

1. Go to **https://blast.ncbi.nlm.nih.gov/** → **Protein BLAST (blastp)**
2. Paste the mystery sequence
3. Database: **Non-redundant protein sequences (nr)**
4. No organism restriction — search all of life

**From the results:**

| Question | Your answer |
|----------|-------------|
| What is the top hit? What protein is it? | |
| What organism does the top hit come from? | |
| What is the E-value and % identity of the top hit? | |
| Is there a human version of this protein in the hits? What is the % identity to the deep-sea sequence? | |
| Scroll to the "Graphic Summary" — does the query align along its full length, or only partially? | |
| Look at hits from different kingdoms of life — is this protein in bacteria only, or also in eukaryotes? | |

---

### Part 2 — Understand the protein in UniProt (5 min)

1. Click on the accession number of the **human hit** from your BLAST results
2. This should take you to a UniProt entry

| Question | Your answer |
|----------|-------------|
| What is the human protein's name and UniProt accession? | |
| What is its molecular function? | |
| In which metabolic pathway does this enzyme participate? (Check "Pathways" or "GO terms") | |
| Is it essential for life? (Hint: look at the "Function" section — what happens without it?) | |
| What disease, if any, is associated with mutations in the human version? | |

---

### Part 3 — Find the structure and active site (5 min)

1. From the UniProt page of the human protein, navigate to PDB
2. Find a structure of this protein

| Question | Your answer |
|----------|-------------|
| What PDB code did you find? | |
| Is there a cofactor (small molecule, metal ion) bound in the active site? | |
| In the 3D viewer, is the protein a monomer (one chain) or oligomer (multiple chains)? | |

---

### Part 4 — Align the deep-sea sequence with the human version (3 min)

1. Go to **Clustal Omega**: https://www.ebi.ac.uk/Tools/msa/clustalo/
2. Paste BOTH sequences — the deep-sea mystery sequence AND the human sequence you retrieved from UniProt (download it as FASTA first)
3. Run the alignment

| Question | Your answer |
|----------|-------------|
| What is the overall % identity between the deep-sea and human versions? | |
| Are the active site residues conserved between the two? | |
| What does this level of conservation tell you about the importance of this protein? | |

### Reflection

If this protein is conserved from deep-sea bacteria to humans, what does that tell you about *when* in evolution this protein first appeared, and why it has been maintained unchanged for billions of years?

---
---

# Challenging Exercises

---

## Exercise D — The Unknown Pathogen

> ⏱️ **Estimated time:** 25 minutes  
> 🎯 **Learning goal:** Use multiple databases in sequence to characterise a completely unknown sequence — mimicking real outbreak investigation
> 
> ⚠️ **This exercise is deliberately open-ended. There is no single answer page to check. You are expected to reason from evidence, just like a real researcher.**

### Scenario

You are working at a public health laboratory. A patient has come in with a severe respiratory illness. Standard tests for influenza, COVID-19, and RSV are all negative. You extract RNA from the patient's throat swab, convert it to DNA (a standard technique called RT-PCR), and sequence a fragment. You obtain the following sequence:

```fasta
>patient_respiratory_isolate_unknown
ATGGATCCAAACACTGTGTCAAGCTTTCAGCAGATTGAATGGGATCTTGATATTGAAAGT
GCCTACAAATCAGGCAAGTTTGAGAAGGAAGATGCAAACAGCGATGAGATCAAGCGGATG
CAGAATAAGATCACTGTGAGCAAATACACAGGCAAGACAAAGCAGCAGATCATGGAGCAG
AAGCTTCAGTCAGAGCAGAAGCTCTCAGATCTTAATGACAAGATCGAGCAGAAGCAGCAG
CAGAAAATCCAGGATCTCAAGCAAATGGAGATCAATGAGCAGTTGCAGAAGGAGTTCAAG
AACAAGCAAGGCAAAAAGCGACAGCAGAGCGTCATCACCATCAAGCCAACTCAGAGCAAG
AAGCAGCAGCAGCAGAAGCAGCAGAAGCAGCTCAAGCAGAAGCAGCAGCAGATGCAGCAG
CAGCAGCAGCAGCAGCAGCAGCAGCAGCAGCAGCAGCAGCAGCAGCAGCAGCAGCAGCAG
CAGAAGCAGCAGCAGCAGCAGCAGCAGAAGCAGCTGAAACAGCAGCAGCAGCAGCAGCAG
```

---

### Stage 1 — Initial identification (8 min)

1. **BLAST the sequence** (blastn, nr database, no organism filter)

Record your findings:

| Question | Your answer |
|----------|-------------|
| What is the top BLAST hit? | |
| What organism does it come from? | |
| Is this a known human pathogen? | |
| What is the E-value and % identity? | |
| What gene/protein does this sequence encode? | |

2. **Is this what you expected?** Look at hits 1–10. Are they all from the same organism, or from multiple organisms?

---

### Stage 2 — Characterise the pathogen (8 min)

Now that you have a candidate identification, investigate this organism systematically.

**At NCBI:**
1. Search the **"Taxonomy"** database for your identified organism
   - Go to https://www.ncbi.nlm.nih.gov/taxonomy
   - What kingdom, phylum, class, and order does it belong to?
   - Is it a virus, bacterium, or something else?

2. Search the **"Genome"** database for this organism
   - Does it have a fully sequenced and annotated genome?
   - How large is its genome?
   - How many protein-coding genes does it have?

**At UniProt:**
3. Search for proteins from this organism
   - Find the protein encoded by your sequenced gene
   - What is its function?

| Question | Your answer |
|----------|-------------|
| What is the full taxonomic classification? | |
| Genome size? | |
| What does the protein encoded by your fragment do? | |
| Is this organism typically associated with respiratory disease? | |

---

### Stage 3 — Clinical and epidemiological context (5 min)

1. Go back to your BLAST results. Look at the **geographic distribution** of deposited sequences from this organism — where have samples been collected?

2. In the NCBI **PubMed** database (https://pubmed.ncbi.nlm.nih.gov/), search for the organism name + "respiratory" + "human"
   - Are there published cases of human infection?
   - What were the clinical outcomes?

3. Does this organism have a known animal reservoir? (Search PubMed: organism name + "zoonosis" OR "animal reservoir")

| Question | Your answer |
|----------|-------------|
| Has this organism caused human disease before? | |
| What is the likely transmission route? | |
| Should this patient be isolated? What would you recommend? | |

---

### Stage 4 — Write your report (4 min)

You must brief the hospital infectious disease team in 5 sentences. Include:
1. What organism you identified and how confident you are
2. What gene you sequenced
3. Whether this is a known human pathogen
4. What you know about clinical severity
5. What additional tests you would order

---

---

## Exercise E — Reverse Engineering a Research Paper

> ⏱️ **Estimated time:** 25 minutes  
> 🎯 **Learning goal:** Understand that the databases you have learned are the foundation of real published research — and practice reading biological data the way a researcher does

### Scenario

A landmark 1993 paper by Lane and Crawford identified that the tumour suppressor protein **p53** is mutated in more than 50% of all human cancers. Today, p53 is the most studied protein in all of biology. You are going to reconstruct some of the key database work that underpins our understanding of p53 — using only the tools you have learned.

---

### Part 1 — The sequence (5 min)

1. Go to **UniProt** and retrieve the entry for human p53: **P04637**
2. Download the canonical sequence in FASTA format

Answer:

| Question | Your answer |
|----------|-------------|
| How many amino acids long is human p53? | |
| What are the five main functional domains listed in UniProt? (look at "Family & Domains" section) | |
| At which amino acid positions does the DNA-binding domain begin and end? | |
| How many isoforms of p53 are annotated in UniProt? | |

---

### Part 2 — The hotspot mutations (7 min)

In cancer, p53 mutations are not random — they cluster at specific positions called **hotspot mutations**. The most common are at positions: **R175, G245, R248, R249, R273, R282**.

1. On the UniProt page for P04637, scroll to **"Pathology & Biotech → Natural variants"**
2. Find each of the hotspot positions listed above

For each position, complete the table:

| Position | Wild-type AA | Most common cancer mutation | Domain affected |
|----------|-------------|----------------------------|-----------------|
| 175 | | | |
| 248 | | | |
| 273 | | | |

3. Now go to **ClinVar** (https://www.ncbi.nlm.nih.gov/clinvar/)
4. Search for `TP53[gene] AND pathogenic`
5. How many pathogenic variants are annotated for TP53? Does this surprise you given what you know about p53?

---

### Part 3 — The 3D structure and why hotspots matter (8 min)

The hotspot mutations are not random — they affect specific positions in the 3D structure that are critical for DNA binding.

1. Go to **RCSB PDB**: https://www.rcsb.org/
2. Search for: **2OCJ** (this is a high-resolution structure of the p53 DNA-binding domain bound to DNA)
3. Open the entry

On the PDB page:

| Question | Your answer |
|----------|-------------|
| What is the resolution of this structure? | |
| How many chains are in the asymmetric unit? | |
| Is DNA present in this structure? | |

4. Open the **3D viewer**
5. The p53 protein makes direct contact with the DNA helix. Look for:
   - The DNA (usually shown as two strands of nucleotides)
   - The protein wrapped around it

6. Now: **Arg248 (R248)** is one of the hotspot residues. It makes a direct contact with the DNA backbone. In the 3D viewer, try to find residue 248 using the sequence viewer at the bottom.
   - When Arg248 is mutated to Trp (R248W), why would this break DNA binding? (Think about what Arginine looks like vs Tryptophan — Arg is positively charged, Trp is a large hydrophobic ring)

---

### Part 4 — Conservation across species (5 min)

If p53 is so important, it should be conserved across many species.

1. Go to **NCBI BLAST** (blastp)
2. Use the p53 sequence from UniProt (P04637) as your query
3. Search against the **nr** database with **no organism restriction**
4. Collect data for the following species from the BLAST results:

| Species | % Identity to human p53 | E-value |
|---------|------------------------|---------|
| Chimpanzee | | |
| Mouse | | |
| Chicken | | |
| Zebrafish | | |
| Fruit fly | | |
| Nematode worm (*C. elegans*) | | |

5. Are the hotspot residues (R175, R248, R273) conserved in all of these species?  
   (Go to Clustal Omega, align human + mouse + zebrafish + fruit fly p53, and look at those positions)

---

### Synthesis

Based on everything you found:
1. Why do you think cancer mutations cluster at specific positions rather than being randomly distributed across the whole protein?
2. What does the deep evolutionary conservation of p53 tell you about its function?
3. If a company wanted to design a drug to restore mutant p53 function, what structural information would they need, and where would they find it?

---

---

## Exercise F — The Database Contradiction

> ⏱️ **Estimated time:** 20 minutes  
> 🎯 **Learning goal:** Understand that databases are not infallible — entries can be incomplete, outdated, or inconsistent. Learn to cross-check and think critically.
>
> ⚠️ **This exercise teaches you one of the most important real-world bioinformatics skills: knowing when NOT to trust a database entry.**

### Background

Databases are built by humans and updated continuously. Sometimes:
- An entry is annotated based on a close homologue, not direct experimental evidence
- Different databases disagree on the same protein's function or location
- A protein has been reclassified since the original annotation
- The evidence codes reveal that an annotation is computational, not experimental

You are going to investigate a protein where careful database reading reveals important nuances.

---

### The protein: Human FOXP2

**FOXP2** is famous as the "language gene" — mutations in it cause severe speech and language disorder. It was reported as uniquely human and linked to the evolution of language. However, the full story is more complicated, and the databases reflect this complexity.

---

### Part 1 — Read the UniProt entry carefully (8 min)

1. Go to UniProt and search for: `FOXP2_HUMAN` or accession **O15409**
2. Open the entry

**Read the "Function" section very carefully.**

| Question | Your answer |
|----------|-------------|
| What does UniProt say FOXP2 does at the molecular level? | |
| What evidence codes are given? (Look for ECO codes or tags like "By similarity", "Experimental") | |
| In "Subcellular location" — where is FOXP2 found? Is this experimentally confirmed or predicted? | |
| How many isoforms are annotated? | |
| Look at "Tissue specificity" — in which tissues is FOXP2 expressed? | |

Now look for the **evidence tags** (little coloured tags next to annotations — e.g., "By similarity", "1 Publication", "Inferred from homology"):

| Annotation | Evidence type (experimental or inferred?) |
|------------|------------------------------------------|
| Subcellular location | |
| Function description | |
| Disease association | |

---

### Part 2 — Check what NCBI says (5 min)

1. Go to **NCBI Gene**: https://www.ncbi.nlm.nih.gov/gene/93986
2. This is the FOXP2 gene entry

| Question | Your answer |
|----------|-------------|
| According to NCBI, what is the official full name of FOXP2? | |
| How many RefSeq transcripts are listed (NM_ entries)? | |
| Is FOXP2 found only in humans, or in other species too? | |

3. Look at the **"Expression"** section on the NCBI Gene page
   - In which brain regions is FOXP2 expressed?
   - Does this match what UniProt said about tissue specificity?

---

### Part 3 — The evolutionary claim (7 min)

FOXP2 was originally described as uniquely human, but this is an oversimplification. Let's test it.

1. Go to **NCBI BLAST** (blastp)
2. Get the human FOXP2 protein sequence from UniProt (O15409) — copy the FASTA
3. Run blastp against **nr**, **no organism restriction**

From the results:

| Question | Your answer |
|----------|-------------|
| Is FOXP2 found in other primates? What % identity? | |
| Is FOXP2 found in mice? What % identity? | |
| Is FOXP2 found in birds? (Hint: look for *Taeniopygia guttata* — the zebra finch) | |
| Is FOXP2 found in crocodiles or other reptiles? | |
| What is the most distantly related organism in the top 50 hits? | |

4. The famous human-specific changes in FOXP2 are just **2 amino acid substitutions** compared to chimpanzee. Look at the alignment between human and chimp FOXP2 in the BLAST results (click "Alignment" for the chimp hit). Can you identify the positions that differ?

---

### Part 4 — The contradiction and what it means (in class discussion)

Based on your findings, answer:

1. Early papers described FOXP2 as "the language gene" unique to humans. Is this accurate based on your database investigation?

2. If FOXP2 is highly conserved in other mammals and even birds (zebra finches use it for song learning), what does this suggest about its *general* function vs its *human-specific* function?

3. Some annotations in databases are marked "By similarity" or "Inferred from homology". What does this mean, and why should you treat these annotations with more caution than experimentally confirmed ones?

4. If you were writing a paper and needed to describe the function of FOXP2, which database — NCBI or UniProt — would you cite for the function description, and why? What would you look for to decide?

5. **The big lesson:** A database entry is not the same as experimental truth. What steps should a careful researcher take before relying on a database annotation to make a biological claim?

---

## Summary — What These Exercises Taught You

### Database navigation skills

| Skill | Where you used it |
|-------|------------------|
| Following cross-database links | Every exercise — NCBI ↔ UniProt ↔ PDB ↔ ClinVar |
| Reading evidence codes | Exercise F (FOXP2 annotations) |
| Using accession numbers to navigate | All exercises |
| Interpreting variant notation (p.Arg408Trp) | Exercise A |
| Connecting sequence → structure → pathway | Exercise B |
| Using BLAST to discover a protein family | Exercise C |
| Cross-checking databases for consistency | Exercise F |

### The database ecosystem

```
You found a gene
       │
       ├──▶ NCBI Gene ──▶ Chromosome location, exon structure, transcripts
       │         │
       │         └──▶ ClinVar ──▶ Disease variants, clinical significance
       │
       ├──▶ UniProt ──▶ Protein function, domains, disease, evidence quality
       │         │
       │         └──▶ PDB ──▶ 3D structure, drug binding, active site
       │
       ├──▶ BLAST ──▶ Homologs across species ──▶ Conservation, function inference
       │
       └──▶ KEGG ──▶ Pathway context ──▶ What process does this gene control?
```

### Critical thinking reminders

- **Always check the evidence code.** "By similarity" is not the same as "experimentally demonstrated."
- **Databases can disagree.** When they do, go to the primary literature.
- **Conservation implies importance** — but absence from a database does not mean absence from biology.
- **One database is never enough** for an important claim. Always cross-check.

---

*These exercises were designed to mirror how professional bioinformaticians actually use these tools — navigating fluidly between databases, following evidence trails, and thinking critically about what the data does and does not tell you.*
