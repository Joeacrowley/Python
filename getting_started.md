---
title: Getting Started
---

# Key Conceptual Differences

Python and R are both interpreted, dynamically typed languages suited to data analysis, but Python's OOP is **class-centric** (`object.method()`) while R's is **function-centric** (`method(object)`). R has multiple OOP systems (S3, S4, R6); Python has one consistent approach baked into the language.

Python also lacks built-in vectorisation — you need `numpy` or `pandas` for element-wise operations. Other quick differences to internalise:

- 0-indexed (R starts at 1)
- Indentation defines code blocks, not `{}` 
- Assignment is `=` only, no `<-`
- Packages use `import`, not `library()`

---

# Installation: Use Miniconda

Macs ship with a system Python — leave it alone and install your own separately. Python version and environment management is more complex than R's, which is why tooling exists to handle it.

In Python, tooling refers to the ecosystem of software tools used to support the development process. Here's a breakdown of the main categories.

**Package Management**

*pip* — the standard package installer  
*pipenv / poetry / uv* — dependency management and virtual environments [not explored]  
*conda* — package and environment manager, popular in data science  

**Recommended setup: Miniconda** — installs conda (environment + package manager) and Python, without the 3GB+ bloat of full Anaconda.

Note: far more work is done in Python (than in R) through the shell / Terminal. 

The shell is a text interface to your operating system. On a Mac, opening Terminal gives you a shell (default: `zsh`). The terminal panel inside RStudio is exactly the same thing — just embedded in the IDE for convenience. Jupyer Lab also has its own Terminal. 

Most Python tooling, including conda, is operated via the shell.

---

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

# Daily sesson set up

```bash

# Activate environment
conda activate myenv
jupyter lab

# Check current environment - rerun 'conda activate myenv' if needed
conda info --envs

# In Jupyter terminal, set remote and working directory
cd /Users/joecrowley/Python/Python
git remote set-url origin git@github.com:Joeacrowley/Python.git
git remote -v
pwd
```

If needed, also run this to authenticate your SSH key (so can push/pull). Should only be needed first time using that repo on your machine. 

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
ssh -T git@github.com
``` 

# First time for repo

This links the working directory to the existing remote, set your working directory first. 

```bash

# 1. Initialize a new Git repo in your current directory
cd /Users/joecrowley/Python/Python
git init

# 2. Add your GitHub remote
git remote add origin git@github.com:Joeacrowley/Python.git

# 3. Verify the remote was added
git remote -v

```

In this case you may also need:

```bash
git pull origin main --allow-unrelated-histories
```

This fetches and merges remote files (in thisc case I was missing .gitignore).

If cannot push/pull may also need to authenticate SSH key...

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
ssh -T git@github.com
``` 

# Installing new packages

Once your set up install new packages in the same way as above, but restart the Kernel in Jupyter Lab to get the new package. 

---

# Environments

Virtual environments are real folders on your hard drive — the "virtual" refers to logical isolation, not that it is on a remote server. Each environment contains its own Python interpreter and packages, so projects don't conflict. This is the equivalent of `renv` in R.

Always activate your environment before installing packages.

---

## Package Management and Security

Python packages are hosted on **PyPI**, which is largely unpoliced — anyone can publish, with no code review or compatibility testing. This is meaningfully less safe than CRAN, which manually reviews submissions and tests packages against each other.

Practical risks to be aware of: **typosquatting** (fake packages with near-identical names), abandoned packages taken over by bad actors. Stick to well-known packages with large download counts and active repositories. Conda's default channels are somewhat more curated than raw PyPI.

## Reproducing your environment

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

## What goes on GitHub
 
You never store the environment itself on GitHub — it's a folder of binaries and installed packages, potentially gigabytes in size and machine-specific. Instead you store the `environment.yml` file, which is a lightweight text description of what the environment contains.
 
Someone cloning your repo then runs:
 
```bash
conda env create -f environment.yml
```
 
And conda rebuilds the environment fresh on their machine from that description.
 
---




