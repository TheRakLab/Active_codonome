# Analysis Pipeline

This document describes the full computational pipeline implemented in `index.html`.  
It mirrors the logic of the original R scripts (`A-codon_count.R`, `B-Data_Processing.R`, `C-Analyzing_Visualizing.R`).

---

## Overview

```
Expression counts (.txt/.tsv/.csv)
        │
        ▼
  1. Gene ID matching
        │
        ▼
  2. CPM Normalization
        │
        ├──────────────────────────┐
        ▼                          ▼
  Codon count table          Per-gene metrics
  (species-specific)    ┌── codon proportion
        │               ├── RSCU bias
        │               └── codon counts
        │
        ▼
  3. Matrix Multiplication
     (codon × samples)
        │
        ▼
  4. Stop codon removal + re-normalization
        │
        ▼
  Output matrices (Usage, Proportion, Bias)
```

---

## Step 1 — Gene ID Matching

Expression genes are intersected with the codon count reference table.  
Only genes present in **both** datasets proceed to the analysis.

- Gene ID column is user-selectable (auto-detected from common names: `Gene`, `GeneSymbol`, `gene_id`, etc.)
- Unmatched genes are silently excluded

---

## Step 2 — CPM Normalization

Raw counts are normalized column-wise to account for differences in sequencing depth:

```
CPM_norm[gene][sample] = count[gene][sample] / colSum[sample]
```

This is equivalent to the R operation:
```r
countData <- countData / colSums(countData)
```

---

## Step 3 — Per-gene Codon Metrics

Three metrics are computed per gene from the codon count reference:

### 3a. Codon Proportion
```
proportion[gene][codon] = count[gene][codon] / sum_all_codons[gene]
```
Represents the relative frequency of each codon, independent of gene length.

### 3b. RSCU Bias
```
RSCU[gene][codon] = observed / expected
                  = count[gene][codon] / (total_AA_codons[gene] / n_synonyms)

bias[gene][codon] = RSCU - 1
```

Where:
- `total_AA_codons` = sum of all counts for codons encoding the same amino acid
- `n_synonyms` = number of codons in the synonym group
- `bias = 0` → equal usage among synonyms (no preference)
- `bias > 0` → codon used more than expected
- `bias < 0` → codon used less than expected (minimum = −1 when count = 0)

This mirrors the `calculate_codon_bias()` function from `func.R`.

---

## Step 4 — Matrix Multiplication

The core computation: combines codon metrics with expression data.

```
usageMatrix[codon][sample] = Σ_genes  codonCount[gene][codon] × CPM_norm[gene][sample]
propMatrix[codon][sample]  = Σ_genes  proportion[gene][codon]  × CPM_norm[gene][sample]
biasMatrix[codon][sample]  = Σ_genes  bias[gene][codon]         × CPM_norm[gene][sample]
```

**Interpretation:**  
Each value reflects the weighted average codon metric across all expressed genes, where each gene's contribution is proportional to its expression level.

In R this is:
```r
codon_usage_matrix <- CDS_codon_counts_matrix %*% countData_matrix
```

---

## Step 5 — Stop Codon Removal & Re-normalization

Stop codons (TGA, TAG, TAA) are removed from the output matrices.

For the **Usage** matrix, columns are re-normalized so each sample sums to 1:
```
usageNorm[codon][sample] = usageMatrix[codon][sample] / Σ_codons usageMatrix[codon][sample]
```

This allows meaningful comparison across samples without artifacts from stop codon variation.

---

## Step 6 — Replicate Averaging (optional)

When conditions are defined in the UI, an averaged matrix is computed:
```
avg[codon][condition] = mean( matrix[codon][s] for s in replicate_samples )
```

---

## FC Display Mode

In the Results tabs, values can be displayed as fold-change vs a reference:

| Matrix type | Formula | Notes |
|---|---|---|
| Usage / Proportion | `log₂(sample / reference)` | +1 = 2× higher than ref |
| Bias (RSCU−1) | `sample − reference` | Difference in RSCU bias |

Bias uses difference (not log ratio) because RSCU−1 is already a normalized, ratio-derived metric.

---

## PCA

PCA is computed from scratch in JavaScript:
1. Transpose matrix to samples × codons
2. Mean-center each feature (codon)
3. Compute covariance matrix
4. Extract PC1 and PC2 via **power iteration** + deflation
5. Project samples onto PC1 and PC2
6. Report variance explained (approximated from eigenvalues)

This mirrors `prcomp(df_t, scale. = TRUE)` from the R script.

---

## Prol/Diff Correlation

The Correlation tab plots each codon's expression-weighted value (Y axis) against `log₂(Prol/Diff ratio)` (X axis).

- X axis is fixed — the Prol/Diff reference vector from `human_prol_diff_vectors.csv`
- Y axis changes based on selected matrix and display mode
- Pearson r and R² are computed per sample
- A least-squares regression line is overlaid
