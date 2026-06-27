---
title: Jupyter Lab
format:
  html:
    toc: true
    toc-depth: 4
    toc-location: right
---

JupyterLab is the closest equivalent to RStudio for Python. It runs in your browser (locally — nothing goes to the internet) and lets you write code in cells, see output inline, and mix in markdown text.

| Jupyter | R |
|---|---|
| Notebook (.ipynb) | R Markdown (.Rmd) |
| Cell | Code chunk |
| Kernel | R session |
| Shift+Enter | Cmd+Enter |

---

# Code Cells

The **Code** cell is the default cell type in a Jupyter notebook, and is where
you write and execute Python (or other language) commands. Output appears directly
below the cell after execution.

**Running cells**

- **Shift+Enter** — run the cell and move to the next
- **Ctrl+Enter** — run the cell and stay on it

**Auto-display behaviour**

Only the **last expression** in a cell is auto-displayed without `print()`:

```python
x = 5
x        # this will display 5
```

```python
x = 5
y = 10
y        # only y is auto-displayed
```

**Edit mode vs Command mode**

- **Enter** — enter edit mode (type inside a cell)
- **Escape** — enter command mode (navigate between cells)

**Useful command mode shortcuts**

| Key | Action |
|---|---|
| A | Insert cell above |
| B | Insert cell below |
| D, D | Delete cell |
| M | Convert to Markdown |
| R | Convert to Raw |
| Z | Undo |
| L | Toggle line numbers |
| O | Toggle output |
| Shift+O | Toggle output scrolling |
| C | Copy cell |
| X | Cut cell |
| V | Paste cell below |
| Shift+M | Merge selected cells |
| Shift+Up/Down | Select multiple cells |
 
**Useful edit mode shortcuts (Enter first)**
 
| Key | Action |
|---|---|
| Tab | Code completion / indent |
| Shift+Tab | Show tooltip / docstring |
| Cmd+Z | Undo |
| Cmd+/ | Toggle comment |
| Ctrl+D | Delete whole line |

**Shift+Tab** is particularly handy — place your cursor inside a function's and press it to see the function's arguments and documentation leaving the cell.

**Shared state across cells

Variables defined in one cell are available in all others — but only if that cell has been **run**. The order you run cells matters, not the order they appear on the page. This can cause confusion if cells are run out of order.

# Adding Markdown cells

**Why use markdown cells?**

In a `.ipynb` notebook used within a Quarto website, markdown cells let you add
headings, explanatory text, and structure between code cells. Headings render as
proper section headings on the page and appear in the table of contents (if `toc: true`
is set in your `_quarto.yml`).

**Heading hierarchy**

Use standard markdown heading syntax in markdown cells:

```markdown
## Section Heading
### Sub-section
#### Sub-sub-section
```

Follow the same conventions as any other Quarto page — if the page title is set via
front matter, start body headings at `##`. Avoid skipping levels.

## How to insert a markdown cell

- Click the **cell type dropdown** in the toolbar (which shows "Code") and select **Markdown**  
- Or press **Escape** to enter command mode, then press **M**

## Rendering and editing

- Press **Shift+Enter** to render a markdown cell
- **Double-click** a rendered cell to return it to editable text
- Press **Shift+Enter** again to re-render

# Raw cells

Jupyter notebooks have three cell types: **Code**, **Markdown**, and **Raw**.
Raw cells are neither executed nor rendered — their content is passed through
exactly as written. They are invisible in the final output unless a specific
pipeline is designed to read them.

**How to insert a Raw cell**

Use the same cell type dropdown where you find Code and Markdown, and select **Raw**.

## Common uses

### 1. YAML front matter (most common in Quarto)

In a Quarto `.ipynb` file, YAML front matter must go in a Raw cell at the top of
the notebook, so it is not rendered as markdown or executed as code:

```
[Raw cell]      ---
                title: "My Notebook"
                format:
                  html:
                    toc: true
                ---
```

### 2. Raw LaTeX

When converting a notebook to PDF via nbconvert, Raw cells can contain LaTeX
commands that are passed directly to the LaTeX renderer:

```latex
\newpage
\begin{equation}
  E = mc^2
\end{equation}
```

### 3. Raw HTML snippets

Embedding HTML that you don't want Quarto or Jupyter to escape or process,
such as custom `<div>` tags or inline styles:

```html
<div class="custom-box">
  Some custom content
</div>
```

### 4. Temporarily disabling cells

Raw cells are sometimes used as a quick way to "comment out" a markdown or code
cell without deleting it — since Raw cells are ignored by both the renderer and
executor. For code cells, commenting out with `#` is more conventional, but
converting to Raw is a handy shortcut for markdown cells.

## Summary

| Cell type | Executed? | Rendered? |
|---|---|---|
| Code | Yes | Output shown |
| Markdown | No | Rendered as HTML |
| Raw | No | Passed through as-is |

In day-to-day Quarto use, the main reason to reach for a Raw cell is the
**YAML front matter** at the top of a notebook.

# File naming reminder

Avoid spaces and mid-word capital letters in notebook filenames, e.g. use
`python_notes.ipynb` rather than `Python_Notes.ipynb`, to prevent broken links
in Quarto websites. 

# Managing notebooks on GitHub

Managing `.ipynb` Files in Git

**Why Jupyter Notebooks are Messy in Git**

`.ipynb` files are JSON under the hood and store more than just code:

- **Cell outputs** are embedded in the JSON — even if code didn't change, the file changes every run
- **Execution counters** increment every run (`"execution_count": 42`)
- **Metadata noise** — kernel info, cell IDs shift between runs/environments
- **Diffs are unreadable** — walls of JSON instead of clean code changes

## Adding `.ipynb` to `.gitignore`

```bash
echo "*.ipynb" >> .gitignore
```

Or open and edit manually:

```bash
nano .gitignore
```

Common patterns for Python projects:
```
# Jupyter Notebooks
*.ipynb

# Python cache
__pycache__/
*.pyc

# Environment files
.env
venv/

# Mac system files
.DS_Store
```

Commit the updated `.gitignore`:

```bash
git add .gitignore
git commit -m "Update .gitignore"
git push
```

> **Note:** `.gitignore` only ignores untracked files. If a file is already committed, run `git rm --cached filename` to untrack it.

---

## Keeping `.ipynb` Files but with Clean Diffs

For a notes repo where you want to commit notebooks but keep diffs meaningful, use **`nbstripout`**.

It strips all outputs and execution counts before committing, so diffs only show actual code changes.

Note this means that the outputs are **never** shown in files store on GitHub.

**Install nbstripout**

```bash
pip install nbstripout
nbstripout --install
```

*Verify — check `.git/config`*

```bash
cat .git/config
```

You should see:

```
[filter "nbstripout"]
    clean = "/path/to/python" -m nbstripout
    smudge = cat
    required = true
[diff "ipynb"]
    textconv = "/path/to/python" -m nbstripout -t
```

This means nbstripout installed via the **Git filter** mechanism — the preferred approach.

**Last step, create `.gitattributes`**

This file tells Git which files the filter from nbstripout applies to. Here we set to .ipynb files.

```bash
echo '*.ipynb filter=nbstripout diff=ipynb' > .gitattributes
```

Verify:
```bash
cat .gitattributes
```

Commit it:
```bash
git add .gitattributes
git commit -m "Add nbstripout git attributes"
git push
```

## What nbstripout does

| | |
|---|---|
| ❌ Removes | Cell outputs |
| ❌ Removes | Execution counts |
| ❌ Removes | Noisy metadata |
| ✅ Keeps | All code and markdown cells |
| ✅ Keeps | Notebook structure |
| ✅ Keeps | Local outputs intact (only stripped on commit) |