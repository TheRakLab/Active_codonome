# 🧬 Codonome Analyzer

A browser-based tool for mRNA expression × codon usage analysis. No installation required — runs entirely in your browser as a single HTML file.

## Live Demo

> If hosted via GitHub Pages: `https://<your-username>.github.io/<repo-name>/`

## Features

- Upload expression count matrices (`.txt` / `.tsv` / `.csv`)
- Supports **Human**, **Mouse**, and **Bovine** codon tables (or upload your own)
- Computes codon **usage**, **proportion**, and **RSCU bias** (no-stop codons)
- Assign replicates and average per condition
- **Fold-change normalization** — pick a reference sample or mean of multiple samples
- Results tabs: **Heatmap**, **PCA**, **Correlation plots**, **Summary**
- Export: individual CSVs, SVG figures, or a full ZIP bundle

## Usage

### Option A — Open locally
Download `index.html` and open it in any modern browser. No server needed.

### Option B — GitHub Pages (share online)
1. Fork or clone this repo
2. Go to **Settings → Pages**
3. Set source to `main` branch, `/ (root)`
4. Your app will be live at `https://<your-username>.github.io/<repo-name>/`

## Input Format

| File | Format | Notes |
|------|--------|-------|
| Expression matrix | TSV / CSV / TXT | Genes as rows, samples as columns; one column must be gene IDs |
| Codon table | CSV | One row per gene; must include a gene ID column and one column per codon |

## How It Works

```
Upload count matrix
       ↓
Select gene ID column & samples
       ↓
Pick species (or upload custom codon table)
       ↓
Run analysis
       ↓
Explore heatmap / PCA / correlations
       ↓
Download CSVs or ZIP
```

## Dependencies

All loaded from CDN — no build step required:

- [PapaParse](https://www.papaparse.com/) — CSV parsing
- [JSZip](https://stuk.github.io/jszip/) — ZIP export
- [JetBrains Mono](https://fonts.google.com/specimen/JetBrains+Mono) + [Syne](https://fonts.google.com/specimen/Syne) — fonts

## Browser Support

Works in any modern browser (Chrome, Firefox, Edge, Safari). Large datasets (>50k genes) may be slow on older hardware.

## License

MIT
