# Module 8 — ML Model Building for Sequence Classification

> **Type:** Lecture + Guided Practical  
> **Duration:** ~120 minutes (40 min lecture, 80 min practical)  
> **Level:** Absolute beginner — assumes no prior coding experience  
> **Tools:** Google Colab (free, browser-based — nothing to install)  
> **Day:** 2, Session 3

---

## Table of Contents

1. [What Are We Going to Build?](#1-what-are-we-going-to-build)
2. [The Problem: Classifying Gene Sequences](#2-the-problem-classifying-gene-sequences)
3. [How ML Classifiers Work — Three Algorithms](#3-how-ml-classifiers-work--three-algorithms)
   - [Support Vector Machine (SVM)](#31-support-vector-machine-svm)
   - [Random Forest](#32-random-forest)
   - [Neural Network](#33-neural-network)
4. [Turning DNA into Numbers — Feature Engineering](#4-turning-dna-into-numbers--feature-engineering)
5. [The Full Workflow](#5-the-full-workflow)
6. [Setting Up Google Colab](#6-setting-up-google-colab)
7. [Practical Notebook](#7-practical-notebook)

---

## 1. What Are We Going to Build?

By the end of this session, you will have built three machine learning classifiers that can look at a DNA sequence and predict whether it is a **coding sequence** (part of a gene that encodes a protein) or a **non-coding sequence**.

You will:
- Write your first lines of Python code
- Convert DNA sequences into numbers a computer can understand
- Train three different ML models on the same data
- Compare their performance
- Make a prediction on a new sequence

This is a real bioinformatics task. The same approach — with more data and more complex features — is used in genome annotation pipelines worldwide.

---

## 2. The Problem: Classifying Gene Sequences

### What is a coding sequence?

A **coding sequence (CDS)** is a stretch of DNA that:
- Starts with ATG (the start codon)
- Encodes a series of amino acids via triplet codons
- Ends with TAA, TAG, or TGA (stop codons)
- Has a recognizable codon usage pattern

### What is a non-coding sequence?

The majority of most eukaryotic genomes is **non-coding** — introns, intergenic regions, regulatory sequences, and repetitive elements. These do not encode proteins.

### Why is this classification hard?

You cannot simply look for ATG and a stop codon — these appear everywhere by chance. The difference between coding and non-coding sequences is **statistical**: coding sequences have distinct patterns in their nucleotide composition, codon usage, and kmer frequencies that a machine learning model can learn.

### Our classification task

```
Input:  A DNA sequence of fixed length
Output: CODING (1) or NON-CODING (0)
```

---

## 3. How ML Classifiers Work — Three Algorithms

### 3.1 Support Vector Machine (SVM)

**The core idea:** Find the line (or boundary) that best separates two classes, with the largest possible gap between the boundary and the nearest data points.

```
          Non-coding sequences        Coding sequences
               ●  ●                      ★  ★
             ●   ●                          ★  ★
           ●  ●                           ★   ★
                  |<-- margin -->|
                        |
                    Decision boundary
                  (the "support vector")
```

The data points closest to the boundary on either side are called **support vectors** — they define and support the boundary. All other points are irrelevant to where the boundary goes.

**The kernel trick:**  
Sometimes data cannot be separated by a straight line. SVMs can use a mathematical trick called a **kernel** to project data into a higher dimension where a linear boundary does work.

```
2D (not separable)           3D (separable after transformation)
   ●  ★  ●                        ★ ★ ★
  ★  ●  ★    ──kernel──▶       ─────────────  (boundary plane)
   ●  ★  ●                        ● ● ● ●
```

**In bioinformatics:** SVMs were historically one of the most powerful methods for protein function prediction and splice site detection before deep learning. They still work very well on small datasets.

**Strengths:** Work well with small datasets; very good in high-dimensional spaces (many features); mathematically well-understood.  
**Weaknesses:** Slow to train on very large datasets; choosing the right kernel requires experimentation.

---

### 3.2 Random Forest

**The core idea:** Build many decision trees, each trained on a random subset of the data and a random subset of the features. Make the final prediction by majority vote.

**What is a decision tree?**

```
              Is GC content > 50%?
                /              \
             YES                NO
              |                  |
    Is ATG codon present?    Is there a stop codon?
       /       \                /         \
     YES        NO            YES          NO
      |          |             |            |
  CODING    NON-CODING     NON-CODING   NON-CODING
```

Each internal node asks a yes/no question about a feature. The tree keeps splitting until it reaches a leaf (a final prediction). A single tree is simple and often wrong.

**Why random and why a forest?**

A single decision tree overfits — it memorises the training data. By building hundreds of trees, each on a different random sample of data with a different random subset of features, and combining their votes, the errors average out.

```
Tree 1:  CODING
Tree 2:  CODING
Tree 3:  NON-CODING
Tree 4:  CODING
Tree 5:  CODING
──────────────────
Majority vote: CODING ✓
```

**In bioinformatics:** Random Forests are widely used for variant pathogenicity prediction (e.g., CADD score), gene expression classification, and genomic feature annotation.

**Strengths:** Easy to use; handles many features well; gives a measure of feature importance (which features matter most?); robust to outliers.  
**Weaknesses:** Large models can be slow; less interpretable than a single decision tree.

---

### 3.3 Neural Network

**The core idea:** Layers of interconnected nodes that progressively transform the input into increasingly abstract representations, eventually producing a prediction.

You already covered the architecture in Module 6. Here we focus on the practical mechanics.

```
Input layer         Hidden layer         Output layer
(one node per       (learns patterns)    (one node per class)
 feature)

  ○ ──────────┐
              ├──▶  ●  ──────────┐
  ○ ──────────┤                  ├──▶  ○  (P(CODING))
              ├──▶  ●  ──────────┤
  ○ ──────────┤                  ├──▶  ○  (P(NON-CODING))
              ├──▶  ●  ──────────┘
  ○ ──────────┘
```

**How training works — in plain English:**

1. Feed a sequence through the network → get a prediction (e.g., 70% coding)
2. Compare to the truth (it was actually non-coding) → calculate the error
3. Adjust every weight slightly in the direction that reduces the error
4. Repeat for thousands of sequences
5. After many repetitions, the weights settle into values that make good predictions

**In bioinformatics:** Neural networks underlie SpliceAI, DeepVariant, and protein language models. For our task, a small neural network works very well.

**Strengths:** Flexible; can learn complex non-linear patterns; scales to very large datasets.  
**Weaknesses:** Needs more data than SVM/RF; harder to interpret; many hyperparameters to tune.

---

## 4. Turning DNA into Numbers — Feature Engineering

All three algorithms require **numbers** as input. DNA is letters. We need to convert.

### Method 1: Nucleotide frequencies (simplest)

Count how often each base appears and divide by sequence length.

```
Sequence: ATGCATGCATGCATGCATGC (length = 20)
A count: 5  → frequency = 5/20 = 0.25
T count: 5  → frequency = 5/20 = 0.25
G count: 5  → frequency = 5/20 = 0.25
C count: 5  → frequency = 5/20 = 0.25

Feature vector: [0.25, 0.25, 0.25, 0.25]
```

This gives 4 features. Very fast but loses all positional information.

### Method 2: K-mer frequencies (what we will use)

Count all possible subsequences of length k.

For **k=2** (dinucleotides), there are 4² = 16 possible kmers:
`AA, AT, AG, AC, TA, TT, TG, TC, GA, GT, GG, GC, CA, CT, CG, CC`

For **k=3** (trinucleotides / codons), there are 4³ = 64 possible kmers — these correspond to codons!

```
Sequence: ATGCAT (length = 6)

2-mers (dinucleotides):
  AT, TG, GC, CA, AT = {AT:2, TG:1, GC:1, CA:1}
  Frequencies: AT=2/5, TG=1/5, GC=1/5, CA=1/5, others=0

3-mers (trinucleotides):
  ATG, TGC, GCA, CAT = {ATG:1, TGC:1, GCA:1, CAT:1}
  Frequencies: ATG=1/4, TGC=1/4, GCA=1/4, CAT=1/4, others=0
```

**Why do kmer frequencies work for coding vs non-coding classification?**

Coding sequences have a very characteristic **codon usage bias** — not all codons are used equally. Leucine has 6 codons (TTA, TTG, CTT, CTC, CTA, CTG) but most organisms strongly prefer some over others. Non-coding sequences have a different, more random kmer distribution.

By computing 3-mer frequencies (64 features), we are essentially measuring codon usage — even without explicitly identifying the reading frame.

### Feature vector — what it looks like

```
Each sequence becomes one row in a table:

         AAA    AAT    AAG    AAC  ...  GGG   | Label
Seq1:  [0.02,  0.05,  0.01,  0.03, ... 0.01] |   1   (coding)
Seq2:  [0.04,  0.03,  0.04,  0.02, ... 0.02] |   0   (non-coding)
Seq3:  [0.01,  0.06,  0.00,  0.04, ... 0.03] |   1   (coding)
...
```

This table is called the **feature matrix** — it is the input to our machine learning models.

---

## 5. The Full Workflow

```
Step 1: LOAD DATA
  → Real coding sequences from NCBI (human protein-coding genes)
  → Non-coding sequences (intergenic/intronic regions)

Step 2: COMPUTE FEATURES
  → For each sequence, compute 3-mer frequencies
  → Build the feature matrix (rows = sequences, columns = kmers)

Step 3: SPLIT DATA
  → 80% training set / 20% test set

Step 4: TRAIN MODELS
  → Train SVM on training set
  → Train Random Forest on training set
  → Train Neural Network on training set

Step 5: EVALUATE
  → Make predictions on test set
  → Calculate accuracy, sensitivity, specificity
  → Plot confusion matrix

Step 6: COMPARE & INTERPRET
  → Which model performed best?
  → Which kmers (features) were most important?

Step 7: PREDICT ON NEW SEQUENCE
  → Give the trained model a new sequence
  → Get a prediction: CODING or NON-CODING
```

---

## 6. Setting Up Google Colab

Google Colab is a free, cloud-based Jupyter notebook environment. You do not need to install anything — it runs entirely in your browser.

### Opening the notebook

1. Go to **https://colab.research.google.com/**
2. Sign in with a Google account (free — any Gmail works)
3. Click **File → Open notebook → GitHub tab**
4. Paste the link to the workshop notebook, OR
5. Click **File → New notebook** and copy-paste each code cell from this document

### Understanding Colab / Jupyter notebooks

A notebook is made of **cells**. There are two types:

| Cell type | What it contains | How to run |
|-----------|-----------------|-----------|
| **Text cell** (Markdown) | Explanations, headings, instructions | Just read it |
| **Code cell** | Python code | Click the ▶ play button, or press Shift+Enter |

**Important:** You must run cells **in order from top to bottom**. If you skip a cell, later cells will fail because variables haven't been defined yet.

### Your first cell

When you see a code cell with a `▶` button — press it. The code runs. Any output appears directly below the cell. That's it.

---

## 7. Practical Notebook

> The complete, runnable notebook is in the file:  
> **`module8_sequence_classifier.ipynb`**  
> Open it in Google Colab and run each cell in order.

The notebook is divided into clearly labelled sections, each with a plain-English explanation before every block of code. Every line of code that does something non-obvious has a comment explaining what it does.

---

*Workshop tip: When running the notebook for the first time, go to Runtime → Run all to execute every cell at once. Then re-read each cell carefully to understand what it did.*
