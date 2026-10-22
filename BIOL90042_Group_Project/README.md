# Parkinson’s disease: analysis guide

This project combines population genetics, RNA-seq, drug screening and clinical gait data. `Final_Code.qmd` contains the analysis code, figures, statistical tests and written interpretations.

## Start here

- **Read the results:** open `Final_Code.html` after rendering. Follow the five main section headings, then the numbered questions within each section.
- **Find the code:** open `Final_Code.qmd`. The tables below map each question to its code and outputs.
- **Read the overall conclusion:** go to **Bring it all together questions → Question 5**.

## File layout

Keep this README beside `Final_Code.qmd`. The code uses the following layout; generated files appear after rendering:

``` text
project/
├── README.md
├── Final_Code.qmd          # Analysis code and written answers
├── Final_Code.html         # Generated report
├── references.bib          # References cited in the report
├── data/                   # Input datasets
│   ├── prs.Rda
│   ├── rnaseq.Rda
│   ├── rnaseq_gene_info.Rda
│   ├── drugscreen.Rda
│   └── gait.Rda
├── result/                 # Genetics, RNA-seq, drug-screen and PRS–MDS exports
└── *.png / *.gif            # Clinical figures and animations saved beside the QMD
```

The output folder is named **`result`**, singular. Clinical exports currently go to the working directory rather than `result/`.

## Run the analysis

1.  Open the project in RStudio and ensure the QMD executes from the folder containing `data/` and `references.bib`.
2.  Install the required R packages: `tidyverse`, `car`, `edgeR`, `limma`, `RNAseq123`, `pheatmap`, `ggrepel`, `writexl`, `gganimate`, `gifski`, `paletteer` and `knitr`. The RNA-seq packages `edgeR`, `limma` and `RNAseq123` are available through Bioconductor.
3.  Create `result/` if it does not exist. In the R console, run `dir.create("result", showWarnings = FALSE)`.
4.  Click **Render** in RStudio, or run `quarto render Final_Code.qmd` from the project folder.

Run sections from top to bottom: later sections reuse objects created earlier. Loading `prs.Rda` alone does not calculate `prs_scores`; Population Genetics Question 1 creates that object.

## Code-to-results map

Question numbers restart in each section. Line numbers refer to the current `Final_Code.qmd`; search for the headings or object names if the file changes.

### Population Genetics

| Question / code location | What it produces | Where to look |
|----|----|----|
| Q1, line 19: `prs_scores` | Participant PRS values and highest/lowest scores | Printed tables and interpretation under Q1 |
| Q2, line 58 | PRS distributions and descriptive statistics by disease status | Q2; `result/PRS_Distribution.png` and `result/PRS_Boxplot.png` |
| Q3, line 107: `linear_model`, `t.test()` | Assumption checks and case–control PRS comparison | Q3 test output; `result/PRS_PRS_QQ.png` and `result/PRS_Variance_Plot.png` |

### RNA Sequencing

Data preparation starts at line 167: filtering, TMM normalisation, MDS, voom and differential-expression modelling.

| Question / code location | What it produces | Where to look |
|----|----|----|
| Q1, line 317: `top_tibble_updated`, `vol_plot_data_fct` | Differential-expression results and volcano plot | DE tables in the preparation steps; Q1; `result/rnaseq_PD volcano.png` |
| Q2, line 396: `de_go_summary` | GO annotations grouped by category | Q2 table; `result/rna_Q2_gene_ontology.xlsx` |
| Q3, line 445: `top_gene_expr`, `heatmap` | Clustering of samples using the top 30 genes | Q3 interpretation; `result/rna_seq_heatmap.png`; initial MDS plot in Step 4 |

### Drug Screening

Data preparation starts at line 488 and creates `plate_long`.

| Question / code location | What it produces | Where to look |
|----|----|----|
| Q1, line 557: `p_heatmap`, `p_layout` | Plate luminescence, well layout and spatial checks | Q1; `result/drug_q1_heatmap.png` and `result/drug_q1_layout.png` |
| Q2, line 637: `results`, `hits` | Assumption checks, Welch tests and BH-adjusted hit selection | Q2 tables; `result/drug_q2_qqplot.png`, `result/drug_q2_residual_boxplot.png` and `result/drug_q2_hits.png` |
| Q3, line 725: `hits_info` | Hit-compound targets and mechanisms of action | Printed target table and explanation under Q3 |

### Clinical Trial

Data preparation starts at line 753 and creates `gait_long`. **RMS** summarises deviation from upright; **RMSSD** summarises changes between consecutive gait angles.

| Question / code location | What it produces | Where to look |
|----|----|----|
| Q1, line 790: `participant_stats`, `participant_summary` | Participant characteristics, RMS and descriptive gait statistics | Q1 code and interpretations; `participant_histograms.png`, gait boxplots and orientation GIFs beside the QMD |
| Q1, line 2314: `participant_smoothness` | Participant RMSSD and second-to-second angle changes | RMSSD subsections; `participant_angle_changes_over_time.png` and `participant_gait_rmssd.png` |
| Q2, line 2595: `primary_test`, `rmssd_test` | Treatment comparisons for RMS and RMSSD, assumption checks and sensitivity analyses | Q2 test output; `rms_qq_plots.png`, `rmssd_qq_plots.png` and the corresponding `*_treatment_assumptions.png` files |
| Q3, line 2913: `model_rms`, `model_rmssd` | Treatment effects adjusted for exercise and age, diagnostics, sensitivity checks and interactions | Q3; `rms_results`, `rmssd_results`; `rms_adjusted_model_diagnostics.png` and `rmssd_adjusted_model_diagnostics.png` |

### Bring it all together questions

| Question / code location | What it produces | Where to look |
|----|----|----|
| Q1, line 3411: `donorid_list`, `donorid_data` | Combined donor list and overlap across genetics, gait and RNA-seq | Printed tables and counts under Q1. Drug-screen data have no donor IDs, so four-arm overlap cannot be established here. |
| Q2, line 3463: `plot_MDS()` | RNA-seq MDS coloured by PRS category | Q2; `result/MDS_PRS.png` |
| Q3, line 3510: `drug_rna_go_overlap` | Drug-hit targets matched to RNA-seq results and GO annotations | Q3 table and interpretation |
| Q4, line 3565: `gait_prs`, `prs_gait_model` | Matched PRS–RMS data and linear regression | Q4 participant count, data table, model summary and interpretation |
| Q5, line 3600 | Overall findings and future research | Final paragraph of the report |

## Finding tables and model output

The HTML report contains plots, printed results and interpretations. Most statistical tables and models are not exported as separate files. After running the relevant chunks in RStudio, enter an object name in the console to inspect it, for example `participant_summary`, `rms_results` or `drug_rna_go_overlap`. Use `summary(prs_gait_model)` for the PRS–gait regression output.
