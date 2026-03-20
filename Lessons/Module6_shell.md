# Introduction to the Shell

---

## What is the shell and why do we need it?

When you use a computer normally, you interact with it through a **graphical user interface (GUI)** — you click icons, drag files, open menus. This works fine for everyday tasks.

But in bioinformatics, we regularly need to:

- Work with files that are **gigabytes or terabytes** in size — too large to open in Excel or a text editor
- Run the same analysis on **hundreds of samples** at once
- Use specialist scientific software that has **no graphical interface** — it only works from the command line
- Work on **remote computers and servers** where there is no screen or mouse at all

For all of these things, we use the **shell** — a text-based interface where you type commands and the computer executes them.

The shell is not difficult. It just requires learning a new vocabulary. By the end of this tutorial you will know the commands you need for the rest of this course.

---

## How to use this tutorial in Google Colab

1. Go to [colab.research.google.com](https://colab.research.google.com) and open a new notebook
2. Each code block in this tutorial is a cell you type into Colab
3. Cells starting with `%%bash` run as shell commands
4. Press **Shift + Enter** to run a cell
5. Output appears directly below the cell

> ⚠️ **Important:** Google Colab resets every time you close it or it times out. Files you create during a session will be lost when the session ends. This is fine for learning — in a real research setting you would work on a server or your own machine where files persist.

---

## Part 1 — Where am I? Navigating the File System

### The file system

Your computer stores everything — documents, programs, data — in a **file system**. Think of it as a tree of folders (called **directories** in Linux), starting from a single root point at the top.

```
/                        ← the root, top of everything
├── home/
│   └── user/            ← your home directory
│       ├── documents/
│       └── data/
├── etc/                 ← system configuration files
└── tmp/                 ← temporary files
```

Every file and folder has an **address** called a **path**. For example:
- `/home/user/data/reads.fastq` — an absolute path (starts from root `/`)
- `data/reads.fastq` — a relative path (relative to wherever you currently are)

In Google Colab, you always start in `/content/`.

---

### `pwd` — print working directory

The first thing to know: where am I right now?

```bash
%%bash
pwd
```

`pwd` stands for **print working directory**. It tells you the full path of the folder you are currently in. You will use this constantly to orientate yourself.

---

### `ls` — list the contents of a directory

```bash
%%bash
ls
```

This lists everything in your current directory. At the start of a Colab session it will show `sample_data` — a folder Google provides.

`ls` has several useful options (called **flags**). Flags always start with a `-`:

```bash
%%bash
ls -l
```

`-l` gives a **long listing** — one item per line, with file size, permissions, and the date it was last modified.

```bash
%%bash
ls -lh
```

`-h` adds **human-readable** file sizes (KB, MB, GB instead of raw bytes). You will almost always use `ls -lh` rather than plain `ls`.

```bash
%%bash
ls -lh sample_data/
```

You can also list the contents of a specific folder by adding its name after `ls`.

> 💡 **Tip:** In Linux, capitalisation matters. `ls -lh` and `ls -LH` are different things. File names are also case-sensitive: `Reads.fastq` and `reads.fastq` are two different files.

**🔍 Exercise 1:** Run `ls -lh sample_data/`. How many files are in there? What is the largest file?

---

### `mkdir` — make a new directory

```bash
%%bash
mkdir my_first_folder
ls
```

You should now see `my_first_folder` listed. This is where we will organise our work.

It is good practice to keep your work organised from the start. In bioinformatics projects, a typical structure might look like:

```
project/
├── data/          ← raw input files
├── results/       ← output from analysis
└── scripts/       ← code you write
```

Let's create that structure:

```bash
%%bash
mkdir -p project/data project/results project/scripts
ls project/
```

The `-p` flag tells `mkdir` to create any parent directories that don't exist yet, and not to complain if they already exist. Very useful.

---

### `cd` — change directory

`cd` moves you from one directory to another.

```bash
%%bash
cd project/data
pwd
```

```bash
%%bash
cd project/data
ls
```

The folder is empty because we haven't put anything in it yet.

**Going up one level:** Two dots `..` always means "the directory above":

```bash
%%bash
cd project/data
cd ..
pwd
```

**Going back to your home/starting directory:** Just type `cd` with nothing after it, or use `~` which is a shortcut for your home directory:

```bash
%%bash
cd ~
pwd
```

> ⚠️ **Colab note:** Each `%%bash` cell starts fresh from `/content/`. So `cd` in one cell does not carry over to the next. In a real terminal session, `cd` would persist. Keep this in mind — in Colab you often need to navigate at the start of each cell if you want to work in a specific folder.

**🔍 Exercise 2:** Navigate into `project/results`, check you are in the right place with `pwd`, then go back up to `/content/`.

---

## Part 2 — Working with Files

### `touch` — create an empty file

```bash
%%bash
touch project/data/notes.txt
ls project/data/
```

`touch` creates an empty file if it doesn't exist, or updates its timestamp if it does. Useful for creating placeholder files.

---

### `cp` — copy a file

```bash
%%bash
cp project/data/notes.txt project/results/notes_copy.txt
ls project/results/
```

The syntax is always: `cp source destination`

To copy an entire folder and everything inside it, use `-r` (recursive):

```bash
%%bash
cp -r project/data project/data_backup
ls project/
```

---

### `mv` — move or rename a file

`mv` works like `cp` but removes the original. It is also how you **rename** files:

```bash
%%bash
# Rename a file
mv project/results/notes_copy.txt project/results/analysis_notes.txt
ls project/results/
```

```bash
%%bash
# Move a file to a different folder
mv project/data/notes.txt project/results/
ls project/data/
ls project/results/
```

---

### `rm` — remove a file

```bash
%%bash
rm project/results/notes.txt
ls project/results/
```

> ⚠️ **Warning:** There is no Recycle Bin in Linux. `rm` deletes files immediately and permanently. Be careful.

To delete a folder and everything inside it:

```bash
%%bash
rm -r project/data_backup
ls project/
```

The `-r` flag means recursive — it deletes the folder and all its contents. Use with caution.

---

### `cat` — print the contents of a file

`cat` prints the entire contents of a file to the screen. It is useful for small files.

First, let's put something in a file to look at:

```bash
%%bash
echo "This is line one" > project/results/analysis_notes.txt
echo "This is line two" >> project/results/analysis_notes.txt
echo "This is line three" >> project/results/analysis_notes.txt
```

> 💡 **Note:** `>` writes output to a file (overwrites if it exists). `>>` appends to a file without overwriting. You will see these constantly.

Now read it back:

```bash
%%bash
cat project/results/analysis_notes.txt
```

---

### `head` and `tail` — look at the start or end of a file

`cat` is fine for tiny files but useless for a FASTQ file with millions of lines. Instead:

```bash
%%bash
head -5 project/results/analysis_notes.txt
```

`head -5` shows the first 5 lines. You can change the number.

```bash
%%bash
tail -2 project/results/analysis_notes.txt
```

`tail -2` shows the last 2 lines. In bioinformatics you will use `head` constantly to quickly inspect files without opening them.

---

### `wc` — count words, lines, characters

```bash
%%bash
wc -l project/results/analysis_notes.txt
```

`wc -l` counts the number of lines in a file. This is extremely useful — for example, to count how many sequencing reads are in a FASTQ file (since each read is exactly 4 lines).

**🔍 Exercise 3:** Create a new file in `project/data/` using `echo` with at least 4 lines of text. Then use `head`, `tail`, and `wc -l` to inspect it.

---

## Part 3 — Downloading Files from the Internet

In bioinformatics you will frequently download data directly to your working environment — reference genomes, sequencing reads, databases. There are two main tools for this.

---

### `wget` — download a file from a URL

`wget` is the most common way to download files. You give it a URL and it saves the file.

```bash
%%bash
wget -q https://raw.githubusercontent.com/datacarpentry/shell-genomics/gh-pages/data/untrimmed_fastq/SRR097977.fastq \
    -O project/data/example.fastq
ls -lh project/data/
```

Flags you will use with `wget`:

| Flag | What it does |
|------|-------------|
| `-q` | Quiet mode — suppresses progress output (cleaner in notebooks) |
| `-O filename` | Save the file with a specific name |
| `-P folder/` | Save the file into a specific folder |

Use `wget` when you have a direct URL to a file — which is the case for most public databases like NCBI, ENA, and Ensembl.

---

### `curl` — another download tool

`curl` is similar to `wget` but behaves slightly differently. You will sometimes see it used to query web APIs (asking a database a question and getting an answer back as text).

```bash
%%bash
curl -s https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi?db=sra&term=SRR2584863 \
    | head -20
```

This is querying the NCBI database directly from the command line. You don't need to understand the output right now — just know that `curl` exists and that it is used for talking to web services.

**When to use which:**
- **`wget`** — downloading files (genomes, reads, databases)
- **`curl`** — querying APIs, downloading when you need more control over headers

---

## Part 4 — A Few More Useful Commands

### `grep` — search inside files

`grep` searches for a pattern inside a file and prints every line that contains it.

```bash
%%bash
grep "line two" project/results/analysis_notes.txt
```

In bioinformatics, `grep` is incredibly useful. For example, to find all the read names in a FASTQ file (which start with `@`):

```bash
%%bash
grep "^@" project/data/example.fastq | head -5
```

The `^` means "at the start of the line". So `^@` means "lines that start with @".

**🔍 Exercise 4:** Use `grep` to count how many reads are in `example.fastq`. Hint: every read name starts with `@SRR`. Combine `grep` with `wc -l`.

---

### Pipes `|` — connecting commands together

One of the most powerful features of the shell is the ability to chain commands together using a **pipe** `|`. The output of the first command becomes the input of the second.

You have already seen this above. Here's another example — count how many files are in a folder:

```bash
%%bash
ls sample_data/ | wc -l
```

`ls` lists the files, `|` passes that list to `wc -l`, which counts the lines (= number of files).

You can chain as many commands as you like:

```bash
%%bash
grep "^@" project/data/example.fastq | wc -l
```

This counts all read names — i.e. the number of reads — in one line.

---

### `echo` — print text to the screen

You have already seen `echo` used to write text into files. It simply prints whatever you give it:

```bash
%%bash
echo "Hello from the shell"
```

It is also useful for checking what a variable contains, which you will see in more advanced scripts.

---

### Getting help — `--help` and `man`

Every command has a help page. The quickest way to see it:

```bash
%%bash
ls --help 2>&1 | head -20
```

In a real Linux terminal (not Colab) you can also use `man ls` to open the full manual page. In Colab, `--help` is more reliable.

---

## Part 5 — Putting It All Together

Let's do a small realistic exercise that combines what we have learned.

We will:
1. Create a tidy project folder
2. Download a real FASTQ file
3. Inspect it using shell commands

```bash
%%bash
# 1. Set up folders
mkdir -p ngs_project/raw_data ngs_project/results

# 2. Download a small example FASTQ file
wget -q https://raw.githubusercontent.com/datacarpentry/shell-genomics/gh-pages/data/untrimmed_fastq/SRR097977.fastq \
    -O ngs_project/raw_data/reads.fastq

echo "Download complete"
```

```bash
%%bash
# 3. Check the file downloaded correctly
ls -lh ngs_project/raw_data/
```

```bash
%%bash
# 4. Look at the first read
head -4 ngs_project/raw_data/reads.fastq
```

```bash
%%bash
# 5. Count the total number of reads
grep "^@SRR" ngs_project/raw_data/reads.fastq | wc -l
```

**🔍 Final Exercise:** 
- How large is the file?
- How many reads does it contain?
- Look at the first read. Can you identify the 4 lines — name, sequence, separator, quality scores?
- What does the quality line look like? Are the characters all the same or do they vary?

---

## Summary

Here is a quick reference of everything covered in this tutorial:

| Command | What it does | Example |
|---------|-------------|---------|
| `pwd` | Show current directory | `pwd` |
| `ls` | List directory contents | `ls -lh` |
| `mkdir` | Create a directory | `mkdir -p data/raw` |
| `cd` | Change directory | `cd data/` |
| `touch` | Create an empty file | `touch notes.txt` |
| `cp` | Copy a file | `cp file.txt backup.txt` |
| `mv` | Move or rename a file | `mv old.txt new.txt` |
| `rm` | Delete a file | `rm -r folder/` |
| `cat` | Print file contents | `cat notes.txt` |
| `head` | Show first N lines | `head -4 reads.fastq` |
| `tail` | Show last N lines | `tail -10 log.txt` |
| `wc -l` | Count lines | `wc -l reads.fastq` |
| `grep` | Search for a pattern | `grep "^@" reads.fastq` |
| `\|` | Pipe output to next command | `grep "^@" reads.fastq \| wc -l` |
| `wget` | Download a file | `wget -q URL -O file.fastq` |
| `curl` | Query a URL or API | `curl -s URL` |
| `echo` | Print text | `echo "hello"` |
| `--help` | Show help for a command | `ls --help` |

---




*Exercises use data from the Data Carpentry Genomics curriculum.*
