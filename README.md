# NSCLC Gene Expression Analysis & Cross-Dataset Validation

Differential gene expression, pathway enrichment, and independent cross-validation of NSCLC tumor vs. normal lung tissue across two GEO microarray datasets — Python, GEO2R, Enrichr

## Aim

Identify genes significantly altered between NSCLC tumor and normal lung tissue, determine their biological function, and **cross-validate findings against a second, independent patient cohort** to confirm results aren't specific to one dataset.

## Datasets

| | GSE18842 (Primary) | GSE19188 (Validation) |
|---|---|---|
| Samples | 46 tumor vs. 45 normal | 91 tumor vs. 65 normal |
| Platform | Affymetrix GPL570 | Affymetrix GPL570 |
| Notes | — | Includes large cell carcinoma samples |

## Methods

1. Differential expression via GEO2R (limma) on both series independently
2. Filtering: adj. p-value < 0.05, |log2FC| > 1.5
3. Cross-dataset overlap analysis (Venn diagrams) between GSE18842 and GSE19188
4. Pathway enrichment (Enrichr: KEGG, MSigDB Hallmark, GO BP) on the *validated* (overlapping) gene sets
5. Validation of a 12-gene clinical NSCLC biomarker panel across both datasets

**Methods note:** GSE19188's initial GEO2R export had a reversed tumor/normal contrast direction (caught via biological anchor genes — proliferation markers like `TOP2A`/`CDK1` should be up in tumor, normal-lung markers like `AGER`/`SFTPC` should be down). Fixed by relabeling files and flipping logFC signs.

## Key Findings

**Cross-dataset overlap:**

| Direction | Overlap | % of GSE19188 replicated |
|---|---|---|
| Upregulated | 328 genes | 91.6% |
| Downregulated | 719 genes | 84.0% |

**Biomarker panel:** 11/12 clinical NSCLC biomarkers validated in the correct direction in both datasets (`CEACAM5` present but below significance threshold in GSE19188).

**Pathway enrichment on validated gene sets:**
- **Upregulated (328 genes):** Cell cycle (KEGG), E2F Targets (Hallmark), Mitotic Sister Chromatid Segregation (GO) — a consistent proliferation signature
- **Downregulated (719 genes):** Cell adhesion molecules (KEGG), TNF-alpha Signaling via NF-kB (Hallmark), Regulation of Angiogenesis (GO) — consistent with immune evasion and loss of tissue structure

The strong replication (>84–91% of GSE19188's signal recovered in GSE18842) across two independent cohorts on the same platform indicates these findings reflect real NSCLC biology rather than dataset-specific noise.

## Results

| | |
|---|---|
| <img width="400" alt="Upregulated Venn Diagram" src="https://github.com/user-attachments/assets/e92009a1-af40-4ee3-9474-7dc75b6c4840" /> | <img width="400" alt="Downregulated Venn Diagram" src="https://github.com/user-attachments/assets/bdc3e224-5347-4993-8020-1e4fb6ac91ee" /> |
| Upregulated gene overlap | Downregulated gene overlap |

Full enrichment tables and gene-level overlap lists are in `results/`.

## Limitations

- Microarray, not RNA-seq — lower sensitivity than modern sequencing
- GSE19188 includes large cell carcinoma samples, unlike GSE18842 — a plausible source of the non-overlapping genes
- Computational only — no wet-lab validation
- Biomarker panel is a curated 12-gene list, not exhaustive

## Repository Structure

```
├── scripts/
│   ├── volcano_plot.py
│   ├── heatmap.py
│   ├── biomarker_validation.py
│   ├── filter_GSE19188_correct.py
│   ├── compare_upregulated_final.py
│   ├── compare_downregulated_run.py
│   └── pathway_enrichment_overlap.py
├── results/
│   ├── volcano_plot.png / heatmap.png
│   ├── upregulated_venn_diagram.png / downregulated_venn_diagram.png
│   ├── overlap & non-overlap gene CSVs
│   └── enrichment_*.csv (KEGG, Hallmark, GO BP × 2 gene sets)
└── report/
    └── Lung_Cancer_Full_Analysis_Report.pdf
```

## Tools & Libraries

`Python` · `pandas` · `matplotlib` · `matplotlib-venn` · `numpy` · `requests` · `openpyxl` · GEO2R · Enrichr

## Data Availability

Raw data not included due to size. Download from GEO — [GSE18842](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE18842), [GSE19188](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE19188) — and run GEO2R to reproduce.

---

*Independent portfolio project — 3rd Year Biotechnology, HBTU, Kanpur*
