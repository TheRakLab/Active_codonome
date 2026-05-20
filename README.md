# 🧬 Codonome Analyzer

A browser-based tool for expression-weighted codon usage analysis. Upload an mRNA count matrix, select a species, and explore how codon usage patterns relate to gene expression — with built-in proliferation/differentiation reference vectors, PCA, and publication-ready figures.

**No installation. No server. Runs entirely in your browser.**

---

## 🚀 Quick Start

### Option A — GitHub Pages (recommended)
1. Fork this repository
2. Go to **Settings → Pages → Source → Deploy from branch: `main`, folder: `/ (root)`**
3. Open 'https://theraklab.github.io/Active_codonome/'
4. Done — codon tables load automatically when you select a species

### Option B — Local use
```bash
git clone https://github.com/TheRakLab/codonome-analyzer.git
cd codonome-analyzer
# Serve locally (required for data file fetching):
python3 -m http.server 8080
# Open http://localhost:8080 in your browser
```

> **Note:** Opening `index.html` directly as a `file://` URL will block the automatic codon table fetch (browser security). Use a local server as shown above, or click "Upload manually" when prompted.

---

## 📋 How to Use

### Step 1 — Upload Expression Data
- Upload a **tab-delimited** (`.txt` / `.tsv`) or comma-delimited (`.csv`) count matrix
- **Rows** = genes, **Columns** = samples
- First column should contain gene identifiers (Ensembl IDs or gene symbols)
- Raw integer counts are expected (normalization is done internally)

### Step 2 — Select Gene ID Column & Samples
- Choose which column holds gene identifiers
- Check/uncheck samples to include in the analysis
- Use **Auto-detect** or type condition names to group replicates (e.g. `ctrl`, `day3`, `day8`)

### Step 3 — Select Species
Codon count reference tables load automatically:

| Species | Reference | Genes |
|---|---|---|
| 🧬 *Homo sapiens* | Ensembl GRCh38 | 19,017 |
| 🐭 *Mus musculus* | NCBI RefSeq | 28,958 |
| 🐄 *Bos taurus* | ARS-UCD1.2 Ensembl | 20,524 |

### Step 4 — Analyze & Explore Results

| Tab | Content |
|---|---|
| **Usage** | Expression-weighted codon usage (normalized, no stop codons) |
| **Proportion** | Per-gene codon proportion × expression weight |
| **Bias (RSCU)** | RSCU−1 codon bias × expression weight |
| **PCA** | PCA of samples based on each matrix |
| **🎨 Heatmap Figure** | Publication-ready SVG heatmap, downloadable |
| **📈 Correlation** | Per-sample scatter: codon value vs log₂(Prol/Diff), with r and R² |
| **Downloads** | CSV + SVG figures, all as ZIP |

### FC Normalization
A control bar above all heatmap tabs lets you switch between:
- **Raw values** — absolute expression-weighted codon metrics
- **FC vs reference** — log₂(sample / reference) for Usage/Proportion, or Δ for Bias

Reference can be a single sample or the mean of selected samples.

---

## 📊 Analysis Pipeline

The pipeline mirrors the R scripts in `docs/pipeline/`:

```
1. Match gene IDs between expression matrix and codon count table
2. CPM normalize: each column ÷ column sum
3. Per-gene codon proportion: codon_count / gene_length
4. RSCU bias: (observed / expected) − 1
   where expected = total_AA_codons / n_synonyms
5. Matrix multiplication: codon × expression
   → Σ_genes [ codon_metric(g,c) × CPM_norm(g,s) ]
6. Remove stop codons (TGA, TAG, TAA) and re-normalize
```

---

## 🔬 Prol/Diff Reference Vectors

Codons are ordered in all heatmaps by their **Proliferation/Differentiation ratio** from Tamar Zeevi's dataset (human).

- **Pink / top** = proliferation-biased (ratio > 1)
- **Teal / bottom** = differentiation-biased (ratio < 1)
- The Correlation tab plots codon values against `log₂(Prol/Diff)` per sample

Reference file: `data/human_prol_diff_vectors.csv`

---

## 📁 Repository Structure

```
codonome-analyzer/
├── index.html                    # Main application (self-contained)
├── README.md                     # This file
├── data/
│   ├── human_codoncount.csv      # Human codon counts (19,017 genes)
│   ├── mouse_codoncount.csv      # Mouse codon counts (28,958 genes)
│   └── bovine_codoncount.csv     # Bovine codon counts (20,524 genes)
└── docs/
    ├── PIPELINE.md               # Detailed analysis pipeline
    ├── INPUT_FORMAT.md           # Input file format specifications
    └── OUTPUTS.md                # Output file descriptions
```

---

## 📥 Input File Format

See [`docs/INPUT_FORMAT.md`](docs/INPUT_FORMAT.md) for detailed specifications.

**Quick reference:**
```
GeneID    Sample1    Sample2    Sample3
ACTB      1245       983        1567
GAPDH     4521       3890       5102
MYC       234        189        441
```

Gene IDs should match those in the codon count reference (Ensembl gene symbols for human/bovine, gene symbols for mouse).

---

## 📤 Output Files

See [`docs/OUTPUTS.md`](docs/OUTPUTS.md) for full descriptions.

| File | Description |
|---|---|
| `codonUsage_NoStop_Normalized.csv` | Expression-weighted codon usage |
| `codonProportion_NoStop.csv` | Expression-weighted codon proportions |
| `codonBias_NoStop.csv` | Expression-weighted RSCU−1 bias |
| `codonUsage_raw.csv` | Raw usage including stop codons |
| `codonUsage_avg.csv` | Per-condition mean (if replicates defined) |
| `figures/heatmap.svg` | Heatmap figure |
| `figures/correlation_*.svg` | Per-sample correlation plots |

---

## 🛠️ Dependencies

All dependencies are loaded from CDN — no `npm install` needed:
- [PapaParse 5.4.1](https://www.papaparse.com/) — CSV parsing
- [JSZip 3.10.1](https://stuk.github.io/jszip/) — ZIP file generation
- [Google Fonts — Syne + JetBrains Mono](https://fonts.google.com/)

---

## 📖 Citation

If you use this tool in your research, please cite:

> Codonome Analyzer — expression-weighted codon usage analysis tool.  
> [Your Name], [Year]. https://github.com/YOUR-USERNAME/codonome-analyzer

Prol/Diff vectors from:
> Zeevi et al. (2011). Adaptation by alternative polyadenylation... *Cell* (cite appropriately based on your Tammy2 data source)

---

## 🤝 Contributing

Pull requests welcome. For major changes please open an issue first.

---

## 📄 License

MIT License — see [LICENSE](LICENSE) file.
# Active_codonome
calculate codonome- active codon usage based on mRNA sequencing
