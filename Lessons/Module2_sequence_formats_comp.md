# Module 2 — Challenging Exercises: Database Navigation & Cross-Linking

> **Type:** Hands-on exercises  
> **Duration:** ~60 minutes  
> **Level:** Challenging  
> **Tools:** NCBI, UniProt, EMBL-EBI, PDB — all browser-based  
> **Builds on:** Module 2 (basic database retrieval)

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

- [Exercise A — The Unknown Pathogen](#exercise-d--the-unknown-pathogen)
- [Exercise B — Reverse Engineering a Research Paper](#exercise-e--reverse-engineering-a-research-paper)
- [Exercise C — The Database Contradiction](#exercise-f--the-database-contradiction)

---

## Exercise A — The Unknown Pathogen

> ⏱️ **Estimated time:** 20 minutes  
> 🎯 **Learning goal:** Use multiple databases in sequence to characterise an unknown organism — mimicking real outbreak investigation
>
> ⚠️ **This exercise is deliberately open-ended. There is no single answer page to check. You are expected to reason from evidence, just like a real researcher.**

### Scenario

You are working at a public health laboratory. A patient has come in with a severe respiratory illness. Standard tests for influenza, COVID-19, and RSV are all negative. The organism has been identified from the sample. Your task is to investigate it systematically using databases.

---

### Stage 1 — Characterise the pathogen using NCBI Taxonomy (8 min)

Your identified organism is **Human metapneumovirus (hMPV)**. Investigate it systematically.

**At NCBI Taxonomy:**
1. Go to https://www.ncbi.nlm.nih.gov/taxonomy
2. Search for your organism
   - What kingdom, phylum, class, and order does it belong to?
   - Is it a virus, bacterium, or something else?

**At NCBI Genome:**
3. Search the **"Genome"** database for this organism
   - Does it have a fully sequenced and annotated genome?
   - How large is its genome?
   - How many protein-coding genes does it have?

**At UniProt:**
4. Search for proteins from this organism
   - Find the major surface protein
   - What is its function?

| Question | Your answer |
|----------|-------------|
| What is the full taxonomic classification? | |
| Genome size? | |
| What does the major surface protein do? | |
| Is this organism typically associated with respiratory disease? | |

---

### Stage 2 — Clinical and epidemiological context (7 min)

1. In the NCBI **PubMed** database (https://pubmed.ncbi.nlm.nih.gov/), search for the organism name + "respiratory" + "human"
   - Are there published cases of human infection?
   - What were the clinical outcomes?

2. Does this organism have a known animal reservoir? (Search PubMed: organism name + "zoonosis" OR "animal reservoir")

3. Search UniProt for any annotated proteins from this organism and check the "Pathology & Biotech" sections for disease annotations.

| Question | Your answer |
|----------|-------------|
| Has this organism caused human disease before? | |
| What is the likely transmission route? | |
| Should this patient be isolated? What would you recommend? | |

---

### Stage 3 — Write your report (5 min)

You must brief the hospital infectious disease team in 5 sentences. Include:
1. What organism you identified and how confident you are
2. Whether this is a known human pathogen
3. What you know about clinical severity
4. What populations are most at risk (check PubMed)
5. What additional tests you would order

---

---

## Exercise B — Reverse Engineering a Research Paper

> ⏱️ **Estimated time:** 20 minutes  
> 🎯 **Learning goal:** Understand that the databases you have learned are the foundation of real published research — and practice reading biological data the way a researcher does

### Scenario

A landmark 1993 paper by Lane and Crawford identified that the tumour suppressor protein **p53** is mutated in more than 50% of all human cancers. Today, p53 is the most studied protein in all of biology. You are going to reconstruct some of the key database work that underpins our understanding of p53 — using only the tools you have learned.

---

### Part 1 — The sequence (5 min)

1. Go to **UniProt** and retrieve the entry for human p53: **P04637**

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

### Synthesis

Based on everything you found:
1. Why do you think cancer mutations cluster at specific positions rather than being randomly distributed across the whole protein?
2. If a company wanted to design a drug to restore mutant p53 function, what structural information would they need, and where would they find it?

---

---

## Exercise C — The Database Contradiction

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

### Part 3 — The contradiction and what it means (in class discussion)

Based on your findings, answer:

1. Early papers described FOXP2 as "the language gene" unique to humans. Is this accurate based on your database investigation?

2. Some annotations in databases are marked "By similarity" or "Inferred from homology". What does this mean, and why should you treat these annotations with more caution than experimentally confirmed ones?

3. If you were writing a paper and needed to describe the function of FOXP2, which database — NCBI or UniProt — would you cite for the function description, and why? What would you look for to decide?

4. **The big lesson:** A database entry is not the same as experimental truth. What steps should a careful researcher take before relying on a database annotation to make a biological claim?

---

## Summary — What These Exercises Taught You

### Database navigation skills

| Skill | Where you used it |
|-------|------------------|
| Following cross-database links | Every exercise — NCBI ↔ UniProt ↔ PDB ↔ ClinVar |
| Reading evidence codes | Exercise F (FOXP2 annotations) |
| Using accession numbers to navigate | All exercises |
| Navigating taxonomy and genome databases | Exercise D |
| Connecting structure to disease variants | Exercise E |
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
       ├──▶ NCBI Taxonomy / Genome ──▶ Organism classification, genome size
       │
       └──▶ PubMed ──▶ Primary literature ──▶ Verify database claims
```

### Critical thinking reminders

- **Always check the evidence code.** "By similarity" is not the same as "experimentally demonstrated."
- **Databases can disagree.** When they do, go to the primary literature.
- **One database is never enough** for an important claim. Always cross-check.

---

*These exercises were designed to mirror how professional bioinformaticians actually use these tools — navigating fluidly between databases, following evidence trails, and thinking critically about what the data does and does not tell you.*
