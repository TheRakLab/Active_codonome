# Input File Format

## Expression Count Matrix

### Format
- **File types:** `.txt`, `.tsv` (tab-delimited) or `.csv` (comma-delimited)
- **Structure:** rows = genes, columns = samples
- **First row:** header with sample names
- **First column:** gene identifiers (can be any column — selectable in the UI)
- **Values:** raw integer counts (the tool handles normalization internally)

### Example — Tab-delimited
```
GeneID	ctrl_rep1	ctrl_rep2	treatment_d3_rep1	treatment_d3_rep2
ACTB	1245	983	1567	1423
GAPDH	4521	3890	5102	4876
MYC	234	189	441	398
BRCA1	89	102	156	143
```

### Example — CSV
```
Gene,ctrl_1,ctrl_2,day3_1,day3_2
ACTB,1245,983,1567,1423
GAPDH,4521,3890,5102,4876
MYC,234,189,441,398
```

---

## Gene ID Requirements

Gene IDs in the expression file must match those in the codon count reference table for the selected species.

| Species | Expected ID format | Example |
|---|---|---|
| Human | HGNC gene symbol | `ACTB`, `MYC`, `BRCA1` |
| Mouse | Gene symbol | `Actb`, `Myc`, `Brca1` |
| Bovine | Ensembl gene ID | `ENSBTAG00000000005` |

> **Tip:** If your gene IDs don't match automatically, try selecting a different column in the "Gene ID column" dropdown. The tool shows a preview of the first 5 values to help you identify the right column.

---

## Multi-condition / Replicate Experiments

The tool supports assigning samples to conditions after upload:

1. After uploading, scroll down to **"Assign conditions / replicates"**
2. Type a condition name next to each sample (e.g. `ctrl`, `diff_d3`, `diff_d8`)
3. Samples sharing the same condition name are treated as replicates
4. Enable **"Show averaged results per condition"** to display mean values per condition
5. Averaged CSVs are included in the ZIP download

**Auto-detect** strips trailing numbers/underscores from sample names to guess condition labels  
(e.g. `sampleD0_rep1`, `sampleD0_rep2` → condition `sampleD0`).

---

## Codon Count Tables

Codon count tables are included in the repository (`data/` folder) and load automatically when a species is selected. No manual upload is needed.

If auto-loading fails (e.g. when opening the HTML file directly without a server), a fallback upload button appears.

### Table format
- CSV with one row per gene
- 64 codon columns (AAA, AAC, … TTT)
- One gene ID column

| Column | Human | Mouse | Bovine |
|---|---|---|---|
| Gene ID | `Gene` (last col) | `GeneSymbol` (last col) | First unnamed col |
| Codon values | average of transcripts | average of transcripts | average of transcripts |

---

## Common Issues

**"Matched genes: 0"**  
→ Gene IDs in your expression file don't match the codon table. Check that you're using gene symbols (human/mouse) or Ensembl IDs (bovine), and that the correct "Gene ID column" is selected.

**"No file chosen" / upload button doesn't respond**  
→ File input should work in all modern browsers. If it doesn't, try a different browser or use a local server (`python3 -m http.server 8080`).

**Auto-load failed**  
→ Open the tool via a local server or GitHub Pages instead of directly from the file system. See [README.md](../README.md) for setup instructions.
