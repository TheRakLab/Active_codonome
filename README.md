# Codonome Analyzer

mRNA expression × codon usage analysis — runs entirely in the browser, no server required.

**[▶ Open the app](https://YOUR-USERNAME.github.io/YOUR-REPO/)**

---

## Repository layout

```
/
├── index.html          ← the app (single self-contained file)
├── .nojekyll           ← tells GitHub Pages not to process with Jekyll
├── README.md
└── data/
    ├── human_codoncount_uniqueGene.csv
    ├── mouse_NM_symbol_codoncount.csv
    └── BosTauCodonCount.csv
```

---

## Setup

### 1 — Add your codon reference CSVs

Place the three codon-count files in the `data/` folder:

| Species | Filename |
|---------|----------|
| *Homo sapiens* | `human_codoncount_uniqueGene.csv` |
| *Mus musculus* | `mouse_NM_symbol_codoncount.csv` |
| *Bos taurus* | `BosTauCodonCount.csv` |

Each CSV must have one row per gene, a gene-ID column, and 64 codon columns named by their triplet (e.g. `TTT`, `TTC`, …).

### 2 — Create `.nojekyll` directly on GitHub

This file cannot be uploaded via drag-and-drop (browsers hide dot-files). Create it on GitHub instead:

1. In your repo, click **Add file → Create new file**.
2. Name it exactly `.nojekyll` (no extension, just the dot and the word).
3. Leave the content blank and click **Commit new file**.

This tells GitHub Pages not to process the repo with Jekyll, which is required so the CSV files in `data/` are served correctly.

### 3 — Enable GitHub Pages

1. Go to **Settings → Pages**.
2. Under *Source*, choose **Deploy from a branch** → `main` (or `master`) → `/ (root)`.
3. Save. GitHub will give you a URL like `https://YOUR-USERNAME.github.io/YOUR-REPO/`.

---

## Usage

1. **Upload** your expression count matrix (CSV or TSV — rows = genes, columns = samples).
2. Pick the **Gene ID column** and select which samples to include.
3. Choose a **species** and click **"Load from repo"** — the codon CSV is fetched automatically from `data/`.  
   *(You can still upload a local CSV if you prefer.)*
4. Hit **Run Analysis** and explore Usage / Proportion / RSCU Bias / PCA / Heatmap / Correlation tabs.
5. Download individual CSVs or the full ZIP.

---

## Changing the data path

If you move the CSV files or rename the folder, update this one constant near the top of `index.html`:

```js
const DATA_PATH = 'data/';
```

---

## Dependencies (CDN, no install needed)

| Library | Version | Purpose |
|---------|---------|---------|
| [PapaParse](https://www.papaparse.com/) | 5.4.1 | CSV parsing |
| [JSZip](https://stuk.github.io/jszip/) | 3.10.1 | ZIP export |
| JetBrains Mono / Syne | — | Fonts (Google Fonts) |

All other logic is vanilla JS — no build step, no Node, no bundler.
