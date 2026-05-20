# Output Files

All outputs are downloadable from the **Downloads** tab, either individually or as a ZIP archive.

---

## CSV Files

### `codonUsage_NoStop_Normalized.csv`
- **What:** Expression-weighted codon usage, stop codons removed, re-normalized
- **Rows:** 61 sense codons (sorted by Prol/Diff ratio, high → low)
- **Columns:** one per sample
- **Values:** proportion of each codon in the effective transcriptome of that sample (sums to 1 per column)
- **Formula:** `Σ_genes [ codon_count(g,c) × CPM_norm(g,s) ]`, then normalized

### `codonProportion_NoStop.csv`
- **What:** Expression-weighted codon proportion, stop codons removed
- **Values:** per-gene codon frequency weighted by expression
- **Formula:** `Σ_genes [ (count(g,c) / gene_length(g)) × CPM_norm(g,s) ]`

### `codonBias_NoStop.csv`
- **What:** Expression-weighted RSCU−1 bias, stop codons removed
- **Values:** weighted mean codon bias across expressed genes
- **Range:** approximately −1 to +(n_synonyms − 1)
- **Interpretation:** positive = codon used more than expected; negative = less than expected

### `codonUsage_raw.csv`
- **What:** Same as Usage but including all 64 codons (stop codons included, not re-normalized)

### `codonUsage_avg.csv` / `codonProportion_avg.csv` / `codonBias_avg.csv`
- **What:** Per-condition mean matrices, generated when replicates are defined
- **Columns:** one per condition (mean of all replicate samples in that condition)
- Only included in the ZIP if conditions have been assigned

---

## Figure Files

### `figures/heatmap.svg`
- SVG heatmap generated from the **🎨 Heatmap Figure** tab
- Rows: codons sorted by Prol/Diff ratio (proliferation-biased top, differentiation-biased bottom)
- Columns: samples (+ condition means if enabled)
- Left annotation: mini bar showing Prol/Diff ratio per codon
- Color scale: diverging (green = high, pink = low) for FC/Bias; blue scale for raw Usage
- Fully scalable vector format, suitable for publication

**Customize before downloading:**
- Switch matrix (Usage / Proportion / Bias)
- Switch display mode (Raw / log₂FC)
- Choose reference sample or condition mean
- Adjust color scale range (±N)

### `figures/correlation_SAMPLENAME.svg`
- One plot per sample from the **📈 Correlation** tab
- **X axis:** `log₂(Prol/Diff ratio)` — fixed reference vector per codon
- **Y axis:** codon value for that sample (raw or log₂FC)
- **Dots:** pink = proliferation-biased codons; teal = differentiation-biased
- **Line:** least-squares regression
- **Stats:** Pearson r and R² displayed in corner

---

## ZIP Archive (`codonome_results.zip`)

Structure:
```
codonome_results.zip
├── csv/
│   ├── codonUsage_NoStop_Normalized.csv
│   ├── codonProportion_NoStop.csv
│   ├── codonBias_NoStop.csv
│   ├── codonUsage_raw.csv
│   ├── codonUsage_avg.csv          (if conditions defined)
│   ├── codonProportion_avg.csv     (if conditions defined)
│   └── codonBias_avg.csv           (if conditions defined)
└── figures/
    ├── heatmap.svg                 (if Heatmap Figure tab was opened)
    ├── correlation_sample1.svg
    ├── correlation_sample2.svg
    └── …
```

> **Note:** SVG figures are only included in the ZIP if their respective tabs were opened before downloading. Open the Heatmap Figure and Correlation tabs first to ensure they are rendered.

---

## Using Outputs in R

```r
# Load outputs back into R
usage <- read.csv("codonUsage_NoStop_Normalized.csv", row.names=1)
bias  <- read.csv("codonBias_NoStop.csv", row.names=1)

# The row order matches the Prol/Diff sort order
# Rows: codons, Columns: samples

# Compute fold change manually
ctrl_mean  <- rowMeans(usage[, grep("ctrl", colnames(usage))])
treat_mean <- rowMeans(usage[, grep("treatment", colnames(usage))])
log2FC     <- log2(treat_mean / ctrl_mean)
```
