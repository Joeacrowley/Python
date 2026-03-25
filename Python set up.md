## Python for R Users — Getting Started

### Key Conceptual Differences

Python and R are both interpreted, dynamically typed languages suited to data analysis, but Python's OOP is **class-centric** (`object.method()`) while R's is **function-centric** (`method(object)`). R has multiple OOP systems (S3, S4, R6); Python has one consistent approach baked into the language.

Python also lacks built-in vectorisation — you need `numpy` or `pandas` for element-wise operations. Other quick differences to internalise:

- 0-indexed (R starts at 1)
- Indentation defines code blocks, not `{}` 
- Assignment is `=` only, no `<-`
- Packages use `import`, not `library()`

---

## Installation: Use Miniconda

Macs ship with a system Python — leave it alone and install your own separately. Python version and environment management is more complex than R's, which is why tooling exists to handle it.

**Recommended setup: Miniconda** — installs conda (environment + package manager) and Python, without the 3GB+ bloat of full Anaconda.

### Day-one setup

```bash
# Create an environment
conda create -n myenv python=3.12

# Activate it (always do this before installing anything)
conda activate myenv

# Install essentials
conda install -c conda-forge jupyterlab pandas numpy matplotlib

# Launch JupyterLab
jupyter lab
```

### Daily sesson set up

```bash

conda activate myenv
jupyter lab

# In Jupyter terminal
cd /Users/joecrowley/Python/Python
git remote set-url origin git@github.com:username/repo_name.git
git remote -v
pwd

```

### Installing new packages

Once your set up install new packages in the same way as above, but restart the Kernel in Jupyter Lab to get the new package. 

---

### Environments

Virtual environments are real folders on your hard drive — the "virtual" refers to logical isolation, not that it is on a remote server. Each environment contains its own Python interpreter and packages, so projects don't conflict. This is the equivalent of `renv` in R.

Always activate your environment before installing packages.

---

## Package Management and Security

Python packages are hosted on **PyPI**, which is largely unpoliced — anyone can publish, with no code review or compatibility testing. This is meaningfully less safe than CRAN, which manually reviews submissions and tests packages against each other.

Practical risks to be aware of: **typosquatting** (fake packages with near-identical names), abandoned packages taken over by bad actors. Stick to well-known packages with large download counts and active repositories. Conda's default channels are somewhat more curated than raw PyPI.

### Reproducing your environment

Two tools, two formats:

**conda** uses `.yml` (YAML), which captures the full environment in structured format — environment name, channels (where packages were sourced from), and all packages with exact versions including Python itself. Export and recreate with:

```bash
# Export, current environment
conda env export > environment.yml

# Recreate on another machine
conda env create -f environment.yml
```

**pip** uses `.txt`, a simpler flat list of packages and versions. It is not, however, more readabe. Export and recreate with:

```bash
# Export, current environment
pip freeze > requirements.txt

# Recreate
pip install -r requirements.txt
```

Since you're using conda, `.yml` is the right convention. The `>` operator in both commands is a shell redirect — it takes the output of the command and writes it to a file rather than printing it to the terminal.

### What goes on GitHub
 
You never store the environment itself on GitHub — it's a folder of binaries and installed packages, potentially gigabytes in size and machine-specific. Instead you store the `environment.yml` file, which is a lightweight text description of what the environment contains.
 
Someone cloning your repo then runs:
 
```bash
conda env create -f environment.yml
```
 
And conda rebuilds the environment fresh on their machine from that description.
 
---

## JupyterLab

JupyterLab is the closest equivalent to RStudio for Python. It runs in your browser (locally — nothing goes to the internet) and lets you write code in cells, see output inline, and mix in markdown text.

| Jupyter | R |
|---|---|
| Notebook (.ipynb) | R Markdown (.Rmd) |
| Cell | Code chunk |
| Kernel | R session |
| Shift+Enter | Cmd+Enter |

---

## The Shell / Terminal

The shell is a text interface to your operating system. On a Mac, opening Terminal gives you a shell (default: `zsh`). The terminal panel inside RStudio is exactly the same thing — just embedded in the IDE for convenience. Jupyer Lab also has its own Terminal. 

Most Python tooling, including conda, is operated via the shell.

---

## R Equivalents Quick Reference

| R | Python |
|---|---|
| `<-` | `=` |
| `c(1,2,3)` | `[1, 2, 3]` |
| `list()` | `dict()` |
| `data.frame()` | `pd.DataFrame()` |
| `library(dplyr)` | `import pandas as pd` |
| `%>%` pipe | `.` method chaining |
| `lm()` | `sklearn.linear_model.LinearRegression()` |
| RMarkdown | Jupyter notebook |
| `renv` | conda environments |
| CRAN | PyPI (less curated) |
