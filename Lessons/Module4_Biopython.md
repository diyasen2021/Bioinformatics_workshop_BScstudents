# Biopython: Automate downloading sequences from NCBI

## What is Google Colab?

Google Colaboratory (Colab) is a **free, cloud-based platform** that lets you write and run Python code directly in your browser — no installation required. Think of it as Google Docs, but for code.

---

## Why Should You Use It?

- **Free to use** — just sign in with your Google account
- **No setup needed** — Python and popular libraries come pre-installed
- **Runs in the cloud** — no strain on your laptop's CPU/RAM
- **Free GPU/TPU access** — great for machine learning assignments
- **Easy sharing** — share notebooks like Google Docs

---

## Key Concepts

### Notebooks
A Colab file is called a **notebook** (`.ipynb`). It's made up of cells — either code or text (Markdown).

### Cells
| Cell Type | Purpose |
|-----------|---------|
| **Code cell** | Write and run Python code |
| **Text cell** | Add notes, headings, or explanations using Markdown |

### Runtime
Colab connects to a remote machine (called a **runtime**) that actually runs your code. If you're idle too long, it disconnects — so save your work often!

---

## Getting Started

1. Go to [colab.research.google.com](https://colab.research.google.com)
2. Sign in with your Google account
3. Click **New Notebook**
4. Start coding!

---

## Basic Usage

```python
# Click a code cell and type your code
print("Hello, World!")

# Install a library
!pip install numpy

# Import libraries
import numpy as np
import pandas as pd
```

### Uploading Files
```python
from google.colab import files
uploaded = files.upload()  # Opens a file picker
```

### Mounting Google Drive
```python
from google.colab import drive
drive.mount('/content/drive')  # Access your Drive files
```

---

## Limitations

- Sessions disconnect after ~90 minutes of inactivity (free tier)
- Storage is temporary unless saved to Google Drive
- Limited GPU hours per day on the free plan

---

## Biopython
Can we download sequences from NCBI using biopython?

**Step 1** - Install Biopython
```
!pip install biopython
```

**Step 2** - Set up your email
NCBI requires you to identify yourself with an email. This is mandatory.
```
from Bio import Entrez, SeqIO
Entrez.email = "your_email@example.com"
```

**Step 3** - Fetch a sequence by accession number
```
# Fetch the sequence
handle = Entrez.efetch(
    db="nucleotide",     # which database
    id="NM_000207",      # accession number
    rettype="fasta",      # format: fasta or gb
    retmode="text"
)

# Read and parse the result
record = SeqIO.read(handle, "fasta")
handle.close()

# Print what we got
print("ID:", record.id)
print("Length:", len(record.seq), "bp")
print("Sequence (first 60 bp):", record.seq[:60])
```

**Step 4** - Fetch multiple sequences
```
# List of accession numbers
ids = ["NM_000207", "NM_001185098", "NM_005228"]

handle = Entrez.efetch(
    db="nucleotide",
    id=",".join(ids),   # join IDs with comma
    rettype="fasta",
    retmode="text"
)

# Use parse() for multiple records
records = list(SeqIO.parse(handle, "fasta"))
handle.close()

for rec in records:
    print(rec.id, "-", len(rec.seq), "bp")
```

**Step 5** - Save sequences to a file
```
SeqIO.write(records, "my_sequences.fasta", "fasta")
print("Saved", len(records), "sequences to my_sequences.fasta")
```

### Retrieve a sequence with a keyword search?
Can you search NCBI by keyword (e.g., "find all insulin sequences in humans")?

### A few to note:
- NCBI asks you not to make more than 3 requests per second — add import time; time.sleep(0.5) between requests in loops to be polite.
- db="nucleotide" → DNA/RNA sequences  |  db="protein" → protein sequences
- rettype="fasta" → FASTA format  |  rettype="gb" → full GenBank record
- SeqIO.read() → one sequence  |  SeqIO.parse() → multiple sequences

