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


### 4. Search NCBI by Keyword (Two-step method)

Use `esearch` to find matching IDs, then `efetch` to download them.

> **Important:** Use `[Title]` or `[Gene]` as field tags. The `[Gene Name]` tag is too strict and often returns an empty list.

```python
# Step 1 — search for IDs
handle = Entrez.esearch(
    db="nucleotide",
    term="insulin[Title] AND Homo sapiens[Organism]",
    retmax=5
)

results = Entrez.read(handle)
handle.close()

id_list = results["IdList"]
print("Found IDs:", id_list)

# Step 2 — fetch the sequences
handle = Entrez.efetch(
    db="nucleotide",
    id=",".join(id_list),
    rettype="fasta",
    retmode="text"
)

records = list(SeqIO.parse(handle, "fasta-blast"))
handle.close()

for rec in records:
    print(rec.id, "-", rec.description[:60])
```

---

## Common Issues & Fixes

| Problem | Cause | Fix |
|--------|-------|-----|
| `esearch` returns empty list | `[Gene Name]` tag is too strict | Use `[Title]` or `[Gene]` instead |
| `BiopythonDeprecationWarning` | NCBI FASTA has comment lines | Use `"fasta-blast"` instead of `"fasta"` |
| Too many requests error | Sending requests too fast | Add `time.sleep(0.5)` between requests in loops |

---

## Quick Reference

### Databases
| `db=` value | Contains |
|-------------|----------|
| `"nucleotide"` | DNA/RNA sequences (accessions: `NM_`, `NC_`, `AY`…) |
| `"protein"` | Protein sequences (accessions: `NP_`, `P0`, `Q`…) |

### Formats
| `rettype=` value | Description |
|-----------------|-------------|
| `"fasta"` | Sequence only |
| `"gb"` | Full GenBank record with annotations |

### Useful Search Field Tags
| Tag | Searches |
|-----|----------|
| `[Title]` | Sequence description line — most reliable |
| `[Gene]` | Gene name — broader than `[Gene Name]` |
| `[Organism]` | Species name e.g. `Homo sapiens[Organism]` |
| `[Accession]` | Exact accession number |

### SeqIO Parser Formats
| Format | Use when |
|--------|----------|
| `"fasta-blast"` | Fetching from NCBI — handles comment lines safely ✓ |
| `"fasta"` | Clean local FASTA files with no comments |
| `"fasta-pearson"` | Files with `;` comment lines |

### Key Rules
- Always call `handle.close()` after reading
- Use `SeqIO.read()` for one sequence, `SeqIO.parse()` for multiple
- Do not exceed 3 requests per second to NCBI

---

