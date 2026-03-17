# Tutorial: Comparing Experimental and Predicted Protein Structures

## Overview

In this tutorial, you will compare a protein structure determined experimentally with a structure predicted using AI. This will help you understand:

* What experimental structure databases are
* What structure prediction tools do
* Why experimental validation is still important

---

## Learning Objectives

By the end of this tutorial, you should be able to:

1. Explore protein structures in the Protein Data Bank (PDB)
2. Visualize predicted structures from AlphaFold
3. Interpret confidence scores (pLDDT)
4. Compare experimental and predicted structures
5. Critically evaluate whether predictions can replace experiments

---

## Background

### What is the Protein Data Bank (PDB)?

The Protein Data Bank (PDB) is a global repository of experimentally determined protein structures.

Structures in PDB are obtained using techniques such as:

* X-ray crystallography
* Cryo-electron microscopy
* Nuclear Magnetic Resonance (NMR)

These structures represent real experimental data.

---

### What is AlphaFold?

AlphaFold is an AI-based system that predicts protein 3D structures from amino acid sequences.

Its predictions are stored in the AlphaFold Protein Structure Database.

Each prediction comes with a confidence score called **pLDDT**.

---

### What is pLDDT?

pLDDT (predicted Local Distance Difference Test) is a confidence score:

* 90–100 → very high confidence
* 70–90 → good confidence
* 50–70 → low confidence
* <50 → very low confidence

---

## Example Protein

We will use the Anti-CRISPR protein AcrIE7.

* PDB structure ID: 9LO1
* AlphaFold ID: Q9I4Q4

---

## Step 1: View Experimental Structure (PDB)

1. Go to: [https://www.rcsb.org](https://www.rcsb.org)
2. Search for: **9LO1**
3. Click on **3D View**

### What to observe:

* The protein is shown as a cartoon structure
* You may see multiple models (because this is an NMR structure)

### Important concept:

NMR structures often appear as an **ensemble of multiple conformations**, representing flexibility.

---

## Step 2: View AlphaFold Predicted Structure

1. Go to: [https://alphafold.ebi.ac.uk](https://alphafold.ebi.ac.uk)
2. Search for: **Q9I4Q4**
3. Click on **View Structure**

---

## Step 3: Visual Comparison

Open both structures side by side.

### Compare the following:

* Overall shape (fold)
* Presence of helices and loops
* Compact vs flexible regions

### Question:

Do the two structures look similar?

---

## Step 4: Explore Confidence (pLDDT)

In the AlphaFold viewer:

* Enable coloring by **confidence (pLDDT)**

### Observe:

* Blue regions → high confidence
* Yellow/red regions → low confidence

---

## Step 5: Connect Structure and Confidence

Compare:

* Regions that differ between PDB and AlphaFold
* Regions with low pLDDT

### Key observation:

Low-confidence regions often correspond to **flexible regions** in the experimental structure.

---

## Step 6: Think Critically

Answer the following questions:

1. Where do the structures match well?
2. Where do they differ?
3. Are differences located in high or low confidence regions?

---

## Final Discussion Question

**Do we still need experimental structure determination if we have AlphaFold?**

Think about:

* Accuracy of predictions
* Flexible regions
* Experimental validation

---

## Key Takeaways

* The PDB stores experimentally determined structures
* AlphaFold predicts protein structures using AI
* pLDDT scores indicate prediction confidence
* Predictions are highly accurate but not perfect
* Experimental data is still essential for validation

---

## Summary Statement

AlphaFold provides powerful predictions, but experimental methods are still required to confirm and understand real biological structures.

---

## Optional Extension

Try this workflow with another protein that has both:

* A PDB structure
* An AlphaFold prediction

Compare and analyze similarities and differences.
