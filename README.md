# How reliable is genomic prediction within a rice breeding programme?

Code for the study *"How reliable is genomic prediction within a rice breeding programme? Across-family accuracy, explainable AI and conformal prediction intervals with 1k-RiCA SNP data."*

## Data

- **Source:** supplementary data of Arbelaez JD et al. (2019) 1k-RiCA (1K-Rice Custom Amplicon) a novel genotyping amplicon-based SNP assay for genetics and breeding applications in rice. *Rice* 12:55. https://doi.org/10.1186/s12284-019-0311-0
- **Population:** 353 elite *indica* breeding lines from 24 families (crosses).
- **Markers:** 965 SNPs in the source file; 941 kept after QC (call rate ≥ 0.90, MAF ≥ 0.05, heterozygosity ≤ 0.05). The 305 missing calls (0.1%) were imputed by kNN (k = 5).
- **Traits:** adjusted means for days to flowering (FLW), grain yield (GY, t/ha) and plant height (PH, cm).
- **File used by the notebook:** `1kRiCA_clean_data.xlsx`
  - Sheet `data`: one row per line, with columns `id`, `line_name`, `family`, `family_size`, `lofo_test_family`, `parents`, `FLW`, `GY`, `PH`, the QC flags `GY_outlier`, `flag_possible_mislabel` and `drop_in_sensitivity`, then 941 SNP columns coded as minor-allele dosage (0/1/2).
  - Sheet `snp_map`: SNP ID, chromosome, position (IRGSP-1.0), alleles, MAF and call rate.

## How to run

1. Open `notebooks/Rice_GP_Reliability_1kRiCA.ipynb` in Google Colab.
2. Upload `data/1kRiCA_clean_data.xlsx` through the Files panel.
3. Choose **Runtime → Run all**. The full run takes about 10 minutes on a free Colab CPU.
4. Cell 6 downloads every figure (PNG) and table (Excel/CSV).

| Cell | Analysis |
|---|---|
| 1 | GBLUP (REML), Bayesian ridge, random forest and XGBoost; random 5-fold CV vs leave-one-family-out (LOFO) |
| 2 | SHAP (exact TreeSHAP from XGBoost): beeswarm, SHAP heatmap by family, genome-wide importance, SHAP stability across families |
| 3 | Cross-conformal prediction intervals (80% and 90%) with random vs family-aware calibration |
| 4 | Circular UPGMA tree, raincloud plot, observed vs predicted, t-SNE, ridgeline plot, SHAP dependence, forest plot |
| 5 | 10 × 5-fold repeated CV, within-family accuracy, parent-disjoint LOFO, Wilcoxon tests (Holm-adjusted), sensitivity analyses, Ensembl gene lookup near top SHAP SNPs |
| 5b | Gene lookup near top SHAP SNPs with automatic retries |
| 7 | Candidate-gene figure: Ghd8 haplotype and sd1 region |
| 6 | Download outputs |

## Repository layout

```
notebooks/   Rice_GP_Reliability_1kRiCA.ipynb   all analysis code (Google Colab)
data/        1kRiCA_clean_data.xlsx             processed dataset used by the notebook
             Supplementary_Data_S1.xlsx         supplementary data of the paper (14 sheets)
figures/     main/  Fig1-Fig7 of the paper
             supplementary/  FigS1-FigS8
```

## Software

Python 3 with numpy, pandas, scipy, scikit-learn, xgboost, matplotlib, openpyxl and requests (all preinstalled on Google Colab). Random seeds are fixed in the code.

## Citation

If you use this code, please cite the paper (citation to be added on publication) and the data source above.

## License

MIT (code). Data remain under the terms of the original publication.
