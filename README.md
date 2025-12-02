# Lecture Notes (Source)

[![Build Status](https://github.com/mendax1234/lecture-notes/actions/workflows/ci.yml/badge.svg)](https://github.com/mendax1234/lecture-notes/actions)
[![Live Site](https://img.shields.io/badge/Status-Online-success)](https://mendax1234.github.io/lecture-notes/)

> **Looking for the notes?** > Access the rendered site here: **[The Core Dump](https://mendax1234.github.io/lecture-notes/)**

---

## Development

This repository hosts the source code for my MkDocs documentation site.

### Dependencies

* Python 3.x
* MkDocs Material

### Local Setup

1. **Clone & Install**

   ```bash
   git clone https://github.com/mendax1234/lecture-notes.git
   cd lecture-notes
   
   # Recommended: Use a virtual environment
   python3 -m venv .venv
   source .venv/bin/activate
   
   pip install mkdocs mkdocs-material

2. **Run Locally**

   ```bash
   mkdocs serve
   ```

   Open `http://127.0.0.1:8000`.

3. Deploy

    ```bash
    mkdocs gh-deploy
    ```
