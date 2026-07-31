# Beejones Knowledge Base

This site is an assembly of knowledge assembled by **Beejones**.  
It is built using **MkDocs** and the **Material for MkDocs** theme, with automatic navigation based on the project’s folder structure.

---

## 📁 Project Structure

```
project-root/
│
├── mkdocs.yml
├── requirements.txt
├── .venv/
└── energy/
    ├── battery/
    │   └── Graphene-Supercapacitors.md
    └── solar/
...
```

All `.md` files inside the project are automatically included in the documentation site.

---

## ⚙️ Setup Instructions

### 1. Create a virtual environment

```bash
python3 -m venv .venv
```

### 2. Activate the virtual environment

**macOS / Linux**
```bash
source .venv/bin/activate
```

**Windows**
```bash
.venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

---

## ▶️ Local Development

To preview the site locally:

```bash
mkdocs serve
```

The site will be available at:

```
http://127.0.0.1:8000
```

This automatically rebuilds the site when you edit Markdown files.

---

## 🏗️ Build the Static Site

To generate the static HTML output:

```bash
mkdocs build
```

This creates a `site/` directory containing the full static website.

---

## 🚀 Deployment (GitHub Pages)

MkDocs includes a built‑in deployment command:

```bash
mkdocs gh-deploy
```

This will:

- Build the site  
- Push the output to the `gh-pages` branch  
- Publish it automatically via GitHub Pages  

Your site will be available at:

```
https://<your-username>.github.io/<your-repo>/
```

If GitHub Pages is not yet enabled:

1. Go to **Repository → Settings → Pages**  
2. Set **Source** to **GitHub Actions** or **gh-pages branch**  
3. Save  

---

## 📦 Updating Dependencies

To freeze the exact versions used:

```bash
pip freeze > requirements.txt
```

This ensures reproducible builds across machines.

---
