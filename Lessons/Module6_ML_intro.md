# Module 6 — Introduction to AI & Machine Learning in Bioinformatics

> **Type:** Lecture + Case Studies + Interactive Demos  
> **Duration:** ~120 minutes (70 min lecture, 50 min case studies & discussion)  
> **Level:** Beginner — assumes no maths or programming background  
> **Day:** 2, Session 1

---

## Table of Contents

1. [Why AI in Biology?](#1-why-ai-in-biology)
2. [The Vocabulary — AI, ML, DL Demystified](#2-the-vocabulary--ai-ml-dl-demystified)
3. [Core Concepts in Machine Learning](#3-core-concepts-in-machine-learning)
4. [Deep Learning — Teaching Machines to See Patterns](#4-deep-learning--teaching-machines-to-see-patterns)
5. [How Biological Data Becomes ML Input](#5-how-biological-data-becomes-ml-input)
6. [Case Studies in Genomics](#6-case-studies-in-genomics)
7. [Case Studies in Proteomics](#7-case-studies-in-proteomics)
8. [Limitations & Critical Thinking](#8-limitations--critical-thinking)
9. [Discussion Exercises](#9-discussion-exercises)
10. [Summary](#10-summary)

---

## 1. Why AI in Biology?

You already understand from Day 1 that biology generates enormous amounts of data — genomes, protein sequences, expression profiles, 3D structures. The question is: what do we *do* with it all?

### The pattern recognition problem

Biology is fundamentally about patterns:
- Which DNA sequences are genes, and which are not?
- Which mutations are harmful, and which are harmless?
- Which protein sequence folds into a kinase, and which folds into an antibody?
- Which patient's cancer will respond to chemotherapy?

These patterns exist in the data. The challenge is that they are often:
- Extremely complex — influenced by hundreds of variables simultaneously
- Non-linear — small changes can have large effects
- Subtle — not visible to the human eye scanning sequences

Traditional bioinformatics tools (like BLAST) use **hand-crafted rules** — a scientist writes the algorithm based on prior knowledge. Machine learning takes a different approach: **let the computer learn the rules directly from data**.

### The scale argument

| Task | Traditional approach | ML approach |
|------|---------------------|-------------|
| Classify 1 protein sequence | BLAST + expert review: minutes | Milliseconds |
| Classify 10 million protein sequences | Years of expert time | Hours |
| Predict protein structure | 50 years of crystallography | AlphaFold: seconds |
| Identify cancer driver mutations | Manual curation of literature | Automated from training data |
| Predict drug-target interaction | Experimental screening of thousands | Virtual screening of billions |

The biology hasn't changed. What has changed is our ability to find patterns in it at scale.

---

## 2. The Vocabulary — AI, ML, DL Demystified

These terms are used interchangeably in the news but they mean different things. They are nested concepts:

```
┌─────────────────────────────────────────────────────────────┐
│                    ARTIFICIAL INTELLIGENCE                  │
│         (any technique that mimics human cognition)         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │               MACHINE LEARNING                      │   │
│   │     (systems that learn from data)                  │   │
│   │                                                     │   │
│   │   ┌─────────────────────────────────────────────┐   │   │
│   │   │           DEEP LEARNING                     │   │   │
│   │   │  (ML using multi-layered neural networks)   │   │   │
│   │   └─────────────────────────────────────────────┘   │   │
│   └─────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

### Artificial Intelligence (AI)
The broadest term. Any computer system that performs tasks that would require human intelligence — reasoning, problem-solving, understanding language, recognizing patterns. Rule-based expert systems from the 1980s were "AI" by this definition.

### Machine Learning (ML)
A subset of AI where the system **learns from examples** rather than following explicit rules written by a programmer.

> **Traditional programming:** Programmer writes rules → Computer applies rules to data → Output  
> **Machine learning:** Data + desired output → Computer learns the rules → Rules applied to new data

### Deep Learning (DL)
A subset of machine learning that uses **artificial neural networks** with many layers. Deep learning has driven most of the headline breakthroughs since 2012 — image recognition, language models, AlphaFold, etc.

The "deep" refers to the depth (number of layers) of the network, not to any philosophical depth.

---

## 3. Core Concepts in Machine Learning

### 3.1 Training, Validation, and Testing

The fundamental workflow of any ML project:

```
All available data
        │
        ├──── Training set (70%) ──▶ Model learns patterns from this
        │
        ├──── Validation set (15%) ──▶ Used to tune the model during training
        │
        └──── Test set (15%) ──▶ Held back completely; used ONCE to evaluate final model
```

**Why split data?**

Imagine studying for an exam using only practice questions. If you see the same questions on the real exam, you might score 100% — but that doesn't mean you actually understand the material. The same applies to ML: a model that only does well on its training data has **memorized** rather than **learned**.

- **Overfitting:** The model learns the training data too well, including its noise — performs poorly on new data
- **Underfitting:** The model is too simple to capture real patterns — performs poorly everywhere
- **Generalization:** The ideal — a model that has learned the true underlying pattern and performs well on new, unseen data

### 3.2 Features

A **feature** is a measurable property of your data that you feed to the model.

In bioinformatics, features might be:
- The presence or absence of specific DNA kmers (short subsequences)
- The amino acid at each position in a protein
- The expression level of each gene in a patient sample
- The physicochemical properties of each residue (hydrophobicity, charge, size)
- The secondary structure propensity at each position

**Feature engineering** — deciding which features to include — was historically the most important (and difficult) step. Deep learning partially automates this by learning relevant features directly from raw data.

### 3.3 Types of Machine Learning

#### Supervised Learning
The model learns from **labelled examples** — data where the correct answer is already known.

```
Input (features)     Label (answer)
ATGGCTAGCTAG...  →   "This IS a coding sequence"
NNNNNNTTTTTT...  →   "This is NOT a coding sequence"
(model learns the difference)
```

**Bioinformatics examples:**
- Predicting whether a mutation is pathogenic (label: disease-causing / benign)
- Classifying tumour subtypes from gene expression (label: subtype A / B / C)
- Predicting protein subcellular location from sequence (label: nucleus / cytoplasm / membrane)

**Common algorithms:**

| Algorithm | Intuition | Bioinformatics use |
|-----------|-----------|-------------------|
| **Logistic regression** | Draws a line/boundary between classes | Variant classification |
| **Random Forest** | Asks many decision trees, takes the majority vote | Gene expression classification |
| **Support Vector Machine (SVM)** | Finds the widest possible boundary between classes | Protein function prediction |
| **Neural network** | Layers of interconnected nodes that transform input | Almost everything in modern bio-ML |

#### Unsupervised Learning
The model finds patterns in data **without labels** — discovering structure that humans haven't defined.

```
Input: Expression levels of 20,000 genes across 500 patients
(no labels provided)

Output: "I found 4 natural groups in this data you didn't know about"
```

**Bioinformatics examples:**
- **Clustering** patient samples by gene expression to discover tumour subtypes
- **Dimensionality reduction** (PCA, UMAP, t-SNE) to visualise high-dimensional genomic data
- **De novo motif discovery** — finding recurring sequence patterns without knowing what to look for

#### Reinforcement Learning
The model learns by trial and error — it takes actions and receives rewards or penalties. Less common in classic bioinformatics but emerging in drug design (designing molecules by iterative improvement).

---

### 3.4 Evaluating a Model — Are the Results Real?

A model's accuracy means nothing without context. Here are the key metrics:

#### Accuracy
Percentage of all predictions that are correct. Misleading when classes are imbalanced.

> **Example of why accuracy alone is misleading:**  
> If 99% of patients are healthy and 1% have a rare disease, a model that always predicts "healthy" achieves 99% accuracy — but catches zero actual cases. This is useless.

#### Sensitivity (Recall)
Of all the true positives (e.g., all actual disease mutations), what fraction did the model correctly identify?

`Sensitivity = True Positives / (True Positives + False Negatives)`

High sensitivity = few cases missed. Critical when missing a case is dangerous (e.g., cancer diagnosis).

#### Specificity
Of all the true negatives (e.g., benign variants), what fraction did the model correctly identify as negative?

`Specificity = True Negatives / (True Negatives + False Positives)`

High specificity = few false alarms. Important when false positives are costly (e.g., unnecessary surgery).

#### The sensitivity-specificity trade-off
There is almost always a trade-off between sensitivity and specificity. Raising the threshold to be more certain before predicting "positive" increases specificity but decreases sensitivity. The right balance depends on the application.

#### AUROC (Area Under the Receiver Operating Characteristic curve)
A single number summarizing the model's ability to discriminate between classes across all possible thresholds.

| AUROC | Interpretation |
|-------|---------------|
| 1.0 | Perfect model |
| 0.9–1.0 | Excellent |
| 0.8–0.9 | Good |
| 0.7–0.8 | Fair |
| 0.5 | No better than random guessing |
| < 0.5 | Worse than random (check your labels!) |

---

## 4. Deep Learning — Teaching Machines to See Patterns

### 4.1 The Neuron — Building Block

A biological neuron receives signals from many inputs, processes them, and fires if the combined signal is strong enough. An artificial neuron works similarly:

```
Input 1 ──(weight 1)──┐
Input 2 ──(weight 2)──┤──▶ [Sum + Bias] ──▶ [Activation function] ──▶ Output
Input 3 ──(weight 3)──┘
```

- Each input is multiplied by a **weight** (how important is this input?)
- The weighted inputs are summed
- An **activation function** decides whether the neuron "fires" — introduces non-linearity so the network can learn complex patterns
- The **bias** term shifts the activation threshold

### 4.2 The Neural Network

Many neurons arranged in layers:

```
Input layer     Hidden layer 1    Hidden layer 2    Output layer
(raw features)                                      (prediction)

   ○               ●                  ●                  ○
   ○     ────▶     ●    ────▶         ●     ────▶        ○
   ○               ●                  ●
   ○               ●                  ●
   ○               ●

(each ○/● is one neuron; every neuron connects to every neuron in the next layer)
```

- **Input layer:** One node per feature (e.g., one node per amino acid position)
- **Hidden layers:** Layers that transform the representation — each layer learns increasingly abstract features
- **Output layer:** The final prediction (e.g., "0.92 probability this protein is in the nucleus")

**Training** adjusts all the weights using an algorithm called **backpropagation** — the model makes a prediction, calculates how wrong it was (the "loss"), and adjusts weights to reduce the error, repeatedly, across thousands of examples.

### 4.3 Deep Learning Architectures in Bioinformatics

Different biological problems require different network architectures:

#### Convolutional Neural Networks (CNNs)
Originally developed for image recognition, CNNs scan across an input looking for local patterns. They are ideal for sequences.

```
DNA sequence:  A T G C A T G C A T G C ...
               │ │ │ │
    CNN filter: scans across, detecting motifs
               (e.g., TATAAA promoter box, GATAAG binding site)
```

**Used for:** Transcription factor binding site prediction, splice site detection, variant effect prediction.

#### Recurrent Neural Networks (RNNs) & LSTMs
Designed for sequential data — each position's output depends on what came before. The network has a form of "memory".

```
A ──▶ [RNN] ──▶ T ──▶ [RNN] ──▶ G ──▶ [RNN] ──▶ C ──▶ ...
         ↑                ↑               ↑
     (carries                  (carries
      context)                  context forward)
```

**Used for:** Protein sequence modelling, gene expression prediction, text-based literature mining.

#### Transformers & Attention Mechanisms
The architecture behind ChatGPT, BERT, and also the most powerful protein language models (ESM, ProtTrans). The key innovation is **attention** — the ability for every position in the sequence to directly influence every other position simultaneously, regardless of distance.

```
For the sequence: M K T A Y I A K ...
                  │ │ │ │ │ │ │ │
Attention allows: ● ─────────────▶ ● (position 1 directly attends to position 8)
                  (no need to pass information one step at a time)
```

**Used for:** AlphaFold2 (structure prediction), ESM-2 (protein language model), Nucleotide Transformer (DNA language model).

#### Graph Neural Networks (GNNs)
For data that is naturally represented as a network — molecules, protein interaction networks, metabolic pathways.

**Used for:** Drug-target interaction prediction, protein-protein interaction prediction, molecular property prediction.

---

### 4.4 Transfer Learning — Standing on the Shoulders of Giants

Training a deep learning model from scratch requires enormous amounts of labelled data — often more than biology can provide. **Transfer learning** solves this by:

1. Pre-train a very large model on a huge dataset (e.g., train on 250 million protein sequences)
2. **Fine-tune** this pre-trained model on your smaller, specific dataset (e.g., 10,000 labelled disease variants)

The pre-trained model has already learned the "grammar" of protein sequences. Fine-tuning teaches it the specific "dialect" you care about.

**This is exactly what protein language models like ESM-2 and ProtTrans do.** They are trained like language models (predicting masked amino acids, similar to how BERT predicts masked words), then fine-tuned for specific tasks.

---

## 5. How Biological Data Becomes ML Input

Computers work with numbers. Biology works with letters and shapes. The bridge between them is **representation**.

### 5.1 Encoding DNA/Protein Sequences

**One-hot encoding** — the simplest approach

Each nucleotide is represented as a vector where one position is 1 and the rest are 0:

```
A = [1, 0, 0, 0]
T = [0, 1, 0, 0]
G = [0, 0, 1, 0]
C = [0, 0, 0, 1]

Sequence ATGC:
A  T  G  C
[1, 0, 0, 0]
[0, 1, 0, 0]
[0, 0, 1, 0]
[0, 0, 0, 1]
→ A 4×4 matrix fed to the model
```

For proteins, one-hot encoding gives a 20-dimensional vector per amino acid.

**Embeddings** — learned representations

More powerful: let the model learn a vector for each amino acid that captures its chemical relationships. Amino acids that behave similarly end up with similar vectors. This is what protein language models produce — rich, information-dense embeddings.

### 5.2 Encoding Gene Expression Data

A gene expression matrix from RNA-seq looks like this:

```
           Gene1   Gene2   Gene3  ...  Gene20000
Patient1:   5.2     0.1    12.3   ...    0.4
Patient2:   4.8     0.3     9.1   ...    1.2
Patient3:   8.1     2.1    11.4   ...    0.1
...
```

Each row (patient) is a vector of 20,000 numbers — one per gene. This vector IS the feature set. Dimensionality reduction (PCA, UMAP) can compress 20,000 dimensions into 2–3 for visualization.

### 5.3 Encoding Protein Structures

Protein structures can be encoded as:
- **Distance matrices** — a 2D matrix where cell (i, j) = the distance between residue i and residue j in 3D space
- **Contact maps** — binary version: 1 if residues are close in 3D, 0 if not
- **Graph representation** — residues as nodes, spatial proximity as edges

AlphaFold2 uses multiple sequence alignments (MSAs) and pairwise residue representations to predict structures.

---

## 6. Case Studies in Genomics

### Case Study 1 — DeepVariant: Calling Genetic Variants with CNNs

**The problem:**  
When you sequence a human genome, you get millions of short reads (30–150 bp each). Aligning them to the reference genome and identifying where they differ (variant calling) is error-prone — sequencing machines make mistakes, and repetitive regions cause misalignments.

**Traditional approach:**  
GATK (Genome Analysis Toolkit) — a rule-based statistical model developed over many years of careful engineering.

**The ML approach:**  
Google's DeepVariant (2017) reframed variant calling as an **image classification problem**.

```
Step 1: Take all the sequencing reads aligned to a genomic position
Step 2: Convert the pileup of reads into a colour-coded image
        (different colours = different bases, quality scores, strand)
Step 3: Feed the image to a CNN trained to classify:
        → Is this a real variant or a sequencing error?
        → Is it homozygous (both copies changed) or heterozygous (one copy changed)?
```

**Result:** DeepVariant outperformed GATK on benchmark datasets — achieving higher precision and recall — despite the engineers who built it having less domain expertise than the GATK team. The CNN learned features the human experts hadn't thought to encode.

**Key insight:** Framing a bioinformatics problem as a computer vision problem unlocked 10 years of image recognition advances.

---

### Case Study 2 — Predicting Splice Sites and Splicing Mutations

**The problem:**  
Alternative splicing produces tissue-specific protein isoforms. Mutations at or near splice sites can cause catastrophic mis-splicing — leading to truncated proteins, exon skipping, or intron retention. About 15% of all disease-causing mutations affect splicing.

**Traditional approach:**  
Simple scoring matrices based on the consensus splice site sequences (GT at the start of an intron, AG at the end).

**The ML approach:**  
SpliceAI (2019, Illumina) — a deep residual CNN trained on thousands of annotated human genes.

```
Input: 10,000 nucleotides of raw DNA sequence centred on a position of interest
Model: Deep CNN with "skip connections" (residual connections)
Output: Probability that each position is a:
        - Splice donor site (start of intron)
        - Splice acceptor site (end of intron)
        - Neither
```

**What makes it remarkable:**
- It can predict the effect of a mutation **400 nucleotides away** from the splice site — something rule-based tools completely miss
- It was validated against thousands of experimentally confirmed splice mutations
- It is now routinely used in clinical genetics labs to interpret variants of uncertain significance

**Real example:** A patient with a rare muscle disease had a variant flagged as "likely benign" by traditional tools. SpliceAI predicted it would activate a cryptic splice site, creating a premature stop codon. Experimental validation confirmed this — reclassifying the variant as pathogenic and enabling a diagnosis.

---

### Case Study 3 — Cancer Subtype Discovery with Unsupervised Learning

**The problem:**  
"Breast cancer" is not one disease — it is many. Patients with superficially similar tumours have wildly different outcomes and responses to treatment. Clinical classification (ER+, HER2+, triple-negative) was based on a few markers.

**The ML approach (Perou et al., 2000 — a landmark paper):**

```
Data: mRNA expression of ~8,000 genes across ~65 breast tumour samples
Method: Hierarchical clustering (unsupervised)
Question: "If we let the data speak, how many natural groups exist?"

Result: 4–5 intrinsic subtypes of breast cancer, each with:
- Distinct molecular profiles
- Distinct clinical outcomes
- Distinct responses to chemotherapy
```

The subtypes discovered computationally:

| Subtype | Key features | Prognosis |
|---------|-------------|-----------|
| Luminal A | ER+, low proliferation | Good |
| Luminal B | ER+, high proliferation | Moderate |
| HER2-enriched | ERBB2 amplification | Poor (before Herceptin) |
| Basal-like | Triple negative, high grade | Poor |
| Normal-like | Resembles normal breast tissue | Variable |

**Impact:** This unsupervised clustering result changed how breast cancer is classified, diagnosed, and treated. It is now the basis for the PAM50 clinical gene expression test used in oncology.

---

### Case Study 4 — CRISPR Guide RNA Design with ML

**The problem:**  
CRISPR-Cas9 edits the genome by cutting at a specific location defined by a guide RNA (gRNA). But not all gRNAs work equally well — some are highly efficient, some barely cut at all. The rules governing gRNA efficiency are complex.

**The ML approach:**  
Multiple models (Azimuth, DeepCRISPR, CRISPR-ML) trained on large-scale experimental screens:

```
Input features per guide RNA:
- The 20-nucleotide target sequence
- Thermodynamic properties of the guide
- Chromatin accessibility at the target site
- Secondary structure of the guide RNA
- GC content and nucleotide composition

Label: Measured cutting efficiency from experimental screens (0–1)

Model: Gradient boosting / CNN
Output: Predicted efficiency score for any candidate gRNA
```

**Result:** ML models reduced the cost and time of CRISPR experiment design by allowing researchers to select highly efficient guides in silico, avoiding experimental failures. Some models also predict **off-target effects** — unintended cuts elsewhere in the genome.

---

## 7. Case Studies in Proteomics

### Case Study 5 — AlphaFold2: The Protein Folding Revolution

**Background: The protein folding problem**

A protein's sequence (1D) fully determines its 3D structure — but predicting that structure from sequence alone had been an unsolved challenge for 50 years. Experimental structure determination (X-ray crystallography, cryo-EM) is slow and expensive.

**The CASP competition:**  
Critical Assessment of Protein Structure Prediction (CASP) is a biennial blind competition. Researchers submit structure predictions for proteins whose structures have been solved but not yet published.

```
CASP13 (2018): Best methods achieved ~40–50 TM-score
              (TM-score = structural similarity, 1.0 = identical)
CASP14 (2020): AlphaFold2 achieved ~90 TM-score
              — judges called this "effectively solved"
```

**How AlphaFold2 works (simplified):**

```
Input:
  1. The protein sequence
  2. A multiple sequence alignment (MSA) of related sequences from other species

Key insight 1 — Coevolution:
  If two positions in the protein are always mutated together across species,
  they are probably in contact in the 3D structure.
  (AlphaFold mines this signal from the MSA)

Key insight 2 — Evoformer:
  A transformer-based architecture processes the MSA and pairwise residue
  relationships simultaneously, allowing every position to attend to every other.

Key insight 3 — Structure module:
  Iteratively refines 3D coordinates using geometric deep learning,
  rotating and translating residue frames until a self-consistent structure is reached.

Output: 3D coordinates for every atom, with a per-residue confidence score (pLDDT)
```

**Impact:**
- AlphaFold predicted structures for **200+ million proteins** — virtually every known protein
- All structures are freely available at https://alphafold.ebi.ac.uk/
- This is arguably the single largest contribution to structural biology in history
- Drug companies are now using AlphaFold structures as starting points for drug design

**Reading AlphaFold confidence scores:**

| pLDDT score | Colour | Interpretation |
|-------------|--------|----------------|
| > 90 | Dark blue | Very high confidence — trust for all applications |
| 70–90 | Light blue | Confident — good for most applications |
| 50–70 | Yellow | Low confidence — treat with caution |
| < 50 | Orange/Red | Very low confidence — often intrinsically disordered regions |

---

### Case Study 6 — Protein Language Models: ESM-2

**The analogy to language:**

Natural language: words → sentences → meaning  
Proteins: amino acids → sequences → structure & function

If we train a model to predict masked words in sentences, it learns grammar, syntax, and meaning. If we train a model to predict masked amino acids in protein sequences, it learns the "grammar" of proteins — co-evolutionary constraints, structural propensities, functional patterns.

**ESM-2 (Evolutionary Scale Modeling, Meta AI, 2022):**

```
Pre-training:
  - 250 million protein sequences from UniRef
  - Task: predict randomly masked amino acids
  - Architecture: Transformer (like GPT/BERT but for proteins)
  - Parameters: up to 15 billion (largest version)

What ESM-2 learns (without any structural labels):
  - Secondary structure propensities
  - Contact maps
  - Mutation effects
  - Subcellular localization
  - Functional site positions
```

**Applications of ESM-2 embeddings:**

| Task | How ESM-2 helps |
|------|----------------|
| Zero-shot mutation effect prediction | Predict how a mutation changes fitness without training on mutation data |
| Protein design | Generate novel sequences with desired properties |
| Structural prediction | ESMFold uses ESM-2 embeddings instead of MSAs — 60x faster than AlphaFold |
| Distant homology detection | Finds evolutionary relationships that BLAST misses entirely |

---

### Case Study 7 — Mass Spectrometry & ML for Protein Identification

**The problem:**  
In a proteomics experiment, you grind up thousands of cells, digest all proteins with enzymes, and feed the resulting peptide mixture into a mass spectrometer. The instrument measures the mass-to-charge ratio of each fragment. You then need to match these measurements back to known proteins.

**Traditional approach:**  
Database search tools (Mascot, Sequest) match observed spectra to theoretical spectra computed from protein sequences.

**The ML approach:**  
Tools like Prosit and DeepMass use deep learning to:

```
Input: Peptide sequence + charge state + collision energy
Model: Bidirectional LSTM / Transformer
Output: Predicted fragmentation spectrum

Key advantage:
  Traditional tools match observed to theoretical spectra calculated
  from simple physical rules. ML models learn the true fragmentation
  patterns from millions of experimental spectra — far more accurate.

Result:
  - More peptides identified per experiment
  - Confident identification of post-translational modifications
  - Better detection of low-abundance proteins
```

---

### Case Study 8 — Drug Discovery: Predicting Molecule Properties

**The problem:**  
Finding a drug molecule that binds a target protein, is non-toxic, is stable in the body, and can be manufactured is extraordinarily difficult. Traditional drug discovery takes ~12 years and $2 billion per drug.

**The ML approach — Graph Neural Networks for molecules:**

A molecule can be represented as a graph:
- **Nodes** = atoms
- **Edges** = chemical bonds
- **Node features** = atom type, charge, hybridization
- **Edge features** = bond type, bond length

```
Molecule (aspirin):      Graph representation:
                              C─C─C
    O   O                    │   │
    ‖   │                    C   O
    C─C─C                    │   │
    │   │                    C─C═O
    └───┘                   (each atom is a node;
                             each bond is an edge)
```

**GNNs trained on this representation can predict:**
- Binding affinity to a target protein
- Solubility (will it dissolve in water?)
- Toxicity (will it harm the liver?)
- Blood-brain barrier penetration (for neurological drugs)
- Metabolic stability (how quickly will the body break it down?)

**Real-world impact:**  
Insilico Medicine used generative AI to design a novel drug candidate for idiopathic pulmonary fibrosis from scratch — identifying a target, generating candidates, and selecting a lead compound in **18 months** (versus the typical 5+ years). It entered human clinical trials in 2021.

---

## 8. Limitations & Critical Thinking

Machine learning in biology is powerful but not magic. Students must develop a critical eye.

### 8.1 The Data Quality Problem

> **"Garbage in, garbage out."**

ML models learn from their training data. If the training data is:
- **Biased** — model will perpetuate the bias (most genomic studies use European-ancestry populations → variant classifiers trained on this data perform worse on African or Asian populations)
- **Mislabelled** — the model learns the wrong thing
- **Too small** — the model overfits and doesn't generalize
- **Not representative** — the model fails on samples outside its training distribution

### 8.2 The Black Box Problem

Many deep learning models — particularly large neural networks — are difficult to interpret. You can see the input and the output but not understand *why* the model made a prediction.

This matters in biology because:
- If a model predicts a mutation is pathogenic, clinicians need to know why
- If a model finds a drug candidate, chemists need to understand what makes it good
- **Explainability tools** (SHAP values, attention visualization, saliency maps) partially address this but remain an active research area

### 8.3 Reproducibility Crisis

Many published ML papers in biology:
- Report performance on their own curated test sets that may be easier than real-world data
- Fail to share code or training data, making results impossible to verify
- Show inflated metrics due to data leakage (test data inadvertently influencing training)

**What to look for in a paper:**
- Is the code publicly available?
- Is the training/test data split clearly described?
- Was the model tested on an **independent** external dataset?
- How does it compare to simple baselines?

### 8.4 Biology Is Not Just Pattern Recognition

ML excels at prediction but not at explanation. Knowing *that* a sequence is likely to be expressed highly is not the same as understanding *why*. Experimental biology remains essential for:
- Validating computational predictions
- Establishing causality (not just correlation)
- Discovering mechanisms that weren't in the training data

---

## 9. Discussion Exercises

These exercises are designed for group discussion — there is no single correct answer.

---

### Discussion 1 — Framing a Biological Problem for ML (10 minutes)

Work in pairs. For each scenario, identify:
- What is the **input** (features)?
- What is the **output** (label/prediction)?
- Is this supervised or unsupervised?
- What type of ML algorithm might work?
- What data would you need to train the model?

**Scenarios:**

a) You want to predict which patients with early-stage lung cancer will relapse within 2 years after surgery.

b) You have RNA-seq data from 1,000 patients across 20 different cancer types. You want to find natural groupings that might reveal new cancer subtypes nobody has described before.

c) You want to predict whether any given mutation in the p53 protein will disrupt its DNA-binding function.

d) You want to design a novel antimicrobial peptide (short protein) that kills bacteria but is non-toxic to human cells.

---

### Discussion 2 — AlphaFold: Revolutionary Tool or Overhyped? (10 minutes)

Read these two (fictitious but representative) perspectives:

**Perspective A — Structural biologist, 30 years experience:**  
*"AlphaFold predictions look good on paper but they are models, not structures. They can't tell you about the dynamics of a protein — how it moves, how it changes shape when it binds a drug. Half my students now think they don't need to learn crystallography. That worries me."*

**Perspective B — Computational drug discovery scientist:**  
*"Before AlphaFold, we had structures for maybe 150,000 proteins. Now we have models for 200 million. That's a 1000-fold increase in what we can study. Yes, the models aren't perfect — but a 70% accurate structural model generated in seconds is more useful than no structure at all."*

**Questions:**
1. What is each person's core concern or enthusiasm?
2. Is Perspective A's concern about dynamics valid? How might that matter in drug discovery?
3. Is Perspective B's argument about coverage compelling?
4. If you were designing a bioinformatics curriculum for medical students, would you teach AlphaFold as a reliable tool or a hypothesis-generating tool?

---

### Discussion 3 — Bias in Genomic ML (10 minutes)

The figure below (described in text) represents a real issue:

> In a 2019 study, a commercial algorithm used in US hospitals to identify patients needing extra medical care was found to be severely biased against Black patients. The algorithm used healthcare spending as a proxy for health needs — but Black patients spent less on healthcare due to systemic barriers, not because they were healthier. The algorithm thus underestimated the health needs of Black patients and assigned them lower risk scores.

Although this specific example is from clinical medicine (not genomics), the same bias mechanisms exist in bioinformatics:
- GWAS studies have historically been 80%+ European-ancestry
- Variant classifiers trained on ClinVar have less data for non-European variants
- Drug target models trained on predominantly Western patient data

**Questions:**
1. How might a genomic variant classifier that was trained mostly on European-ancestry data behave when applied to a patient of African ancestry?
2. Who is harmed by this bias? Who bears responsibility for fixing it?
3. What practical steps could a bioinformatician take when developing an ML model to reduce demographic bias?
4. Should bioinformatics tools that may be biased be used in clinical settings? Under what conditions?

---

## 10. Summary

### The big picture

```
Biological data                  ML techniques                     Biological insight
─────────────────                ────────────────                  ─────────────────
DNA sequences         ──▶        CNNs, Transformers        ──▶    Gene function,
                                                                   splice sites, variants

Protein sequences     ──▶        Language models,           ──▶   Structure, function,
                                 GNNs, CNNs                        drug targets

Gene expression       ──▶        Clustering, Random         ──▶   Disease subtypes,
matrices                         Forests, Neural nets              biomarkers

3D structures         ──▶        AlphaFold, ESMFold         ──▶   Drug binding sites,
                                                                   mechanism of action

Molecular graphs      ──▶        Graph Neural Networks      ──▶   Drug candidate
                                                                   design & optimization
```

### ML concepts to remember

| Concept | One-line definition |
|---------|-------------------|
| Supervised learning | Learning from labelled examples |
| Unsupervised learning | Finding structure without labels |
| Overfitting | Memorizing training data instead of generalizing |
| Feature | A measurable property of the input data |
| E-value | Not ML — but remember: smaller = more significant (from yesterday) |
| Training/Test split | Never evaluate a model on data it was trained on |
| AUROC | A model's ability to discriminate between classes (1.0 = perfect) |
| Transfer learning | Reuse knowledge from a large pre-trained model |
| CNN | Good for finding local patterns in sequences/images |
| Transformer | Good for long-range dependencies; powers AlphaFold, ESM-2 |

### Questions to always ask about an ML paper

1. What was the training data? Is it biased?
2. Was the test set truly independent?
3. Does the model outperform simple baselines?
4. Is the code/data publicly available?
5. Has it been validated experimentally?

---

> ⏭️ **End of Day 2 — Workshop Complete**  
> You have covered the full spectrum from raw biological sequences to AI-powered prediction. The tools and concepts introduced across these modules form the foundation of modern bioinformatics.

---

*Further reading:*
- *Eraslan et al. (2019) "Deep learning: new computational modelling techniques for genomics" — Nature Reviews Genetics*
- *Jumper et al. (2021) "Highly accurate protein structure prediction with AlphaFold" — Nature*
- *Lin et al. (2023) "Evolutionary-scale prediction of atomic-level protein structure with a language model" — Science (ESM-2)*
- *Topol (2019) "High-performance medicine: the convergence of human and artificial intelligence" — Nature Medicine*
