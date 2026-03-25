## Setting Up Quarto for Jupyter Notebooks

### Why Quarto

Quarto was chosen over Jupyter Book and nbviewer because it:
- Executes notebooks at build time — Git stays clean via `nbstripout` (see Python set up), outputs still appear on the site
- Has excellent VS Code integration
- Produces clean, well-indexed, searchable websites from a set of linked notebooks
- Publishes to GitHub Pages in a single command

---

### Installation

Download and install from https://quarto.org/docs/get-started/ (Mac `.pkg` installer).

Verify:
```bash
quarto --version
```

Ensure Jupyter is installed in your environment:
```bash
conda activate myenv
which jupyter
```

---

### Project Structure

Here 'Python' is the overall working directory, '_quarto.yml' describes the website structure Quarto will create, 'index.md' is the landing page (must always use the name 'index'), 'Python_notes.ipynb' is the first of many subject pages (can also be other file types, e.g. .md), and the folder '_site/' is the place where Quarto stores everything it will use to produce the website. 

```
Python/
├── _quarto.yml        ← site config
├── index.md           ← landing page
├── Python_notes.ipynb
└── _site/             ← built output (gitignored)
```

---

### Configuration

Setting upthe `_quarto.yml` file. It should look something like this. 

```yaml
project:
  type: website
  output-dir: _site

website:
  title: "Joe's Python Notes"
  sidebar:
    contents:
      - index.md
      - Python_notes.ipynb

execute:
  enabled: true
  error: true

format:
  html:
    theme: cosmo
    toc: true
```

- `execute.enabled: true` — Quarto re-executes notebooks at build time so outputs appear on the site
- `execute.error: true` — rendering continues past errors, displaying them inline

When editing, save with Ctrl+X → Y → Enter. See prompts at bottom of screen.

Setting up the `index.md` file: 

```markdown
# Joe's Python Notes

Welcome to my Python notes site.
```

---

## Render and Publish

Add `_site/` to `.gitignore`:
```bash
echo "_site/" >> .gitignore
```

Render locally:
```bash
quarto render
```

View at the '_site/index.html' directly in your browser.

Publish to GitHub Pages:

```bash
quarto publish gh-pages
```

Quarto creates and manages a `gh-pages` branch automatically, a new branch within your repo — never edit it directly.

| Branch | Contains |
|---|---|
| `main` | Source `.ipynb` files, clean code |
| `gh-pages` | Built HTML site, managed by Quarto |

Site will be live at:
```
https://joeacrowley.github.io/Python/
```

Can be found at: GitHub repository > Settings > Pages

> **Note:** Make the GitHub repo public and enable GitHub Pages under Settings → Pages → Branch: `gh-pages`.