# 🧬 Codonome Analyzer

A browser-based tool for mRNA expression × codon usage analysis. No installation, no server, no file uploads required for species tables — runs entirely in your browser as a single HTML file.

## Live Demo

> If hosted via GitHub Pages: `https://TheRakLab.github.io/<repo-name>/`

## Features

- Upload expression count matrices (`.txt` / `.tsv` / `.csv`)
- **Built-in codon tables** for Human, Mouse, and Bovine — no upload needed
- Assign replicates and average per condition
- Computes codon **usage**, **proportion**, and **RSCU bias** (no-stop codons)
- **Fold-change normalization** — pick a reference sample or mean of multiple samples
- Results tabs: **Heatmap**, **PCA**, **Correlation plots**, **Summary**
- Export: individual CSVs, SVG figures, or a full ZIP bundle

## Built-in Species Tables

| Species | Gene IDs | Genes |
|---------|----------|-------|
| 🧬 *Homo sapiens* | Gene symbols (e.g. `A1BG`) | ~19,017 |
| 🐭 *Mus musculus* | Gene symbols (e.g. `Actb`) | ~20,554 |
| 🐄 *Bos taurus* | Ensembl IDs (e.g. `ENSBTAG…`) | ~20,524 |

The tables are gzip-compressed and base64-encoded directly inside `index.html`. The browser decompresses them on the fly using the native `DecompressionStream` API — no external files or network requests needed.

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

> Gene IDs in your expression matrix must match the species table (symbols for human/mouse, Ensembl IDs for bovine).

## How It Works

```
Upload count matrix
       ↓
Select gene ID column & samples
       ↓
Click species button → table loads instantly (built-in)
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

Works in any modern browser (Chrome, Firefox, Edge, Safari 16.4+). Requires `DecompressionStream` API support for built-in tables — available in all browsers since 2022.

## License

MIT
