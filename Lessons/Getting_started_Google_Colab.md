# Biopython: A Quick Guide 

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

## Tips for Students

- **Save to Drive** — Go to `File → Save a copy in Drive` to keep your work permanently
- **Use GPU** — Go to `Runtime → Change runtime type → T4 GPU` for ML tasks
- **Keyboard shortcuts** — `Ctrl + Enter` runs a cell; `Shift + Enter` runs and moves to the next
- **Comment your code** — Use `#` to add comments; your professors will thank you

---

## Limitations

- Sessions disconnect after ~90 minutes of inactivity (free tier)
- Storage is temporary unless saved to Google Drive
- Limited GPU hours per day on the free plan

---

## Common Use Cases in Your Degree

- 📊 **Data Analysis** — with Pandas, Matplotlib
- 🤖 **Machine Learning** — with Scikit-learn, TensorFlow, PyTorch
- 📐 **Numerical Computing** — with NumPy, SciPy
- 📝 **Assignments & Projects** — shareable, runnable reports

---

*Happy coding! 🚀*
