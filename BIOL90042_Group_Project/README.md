# Clinical Trial Analysis

Compares L-DOPA with a new compound using 16 participants’ gait recordings, with exercise and age considered in adjusted analyses.

**Input and setup:** Place `data/gait.Rda` relative to the analysis working directory; it must contain `gait_data`. Use R and Quarto with `tidyverse`, `gganimate`, `gifski`, `paletteer`, and `knitr`. Render the analysis `.qmd` to HTML, running chunks from top to bottom in a fresh session.

| Code section | Purpose and key objects |
|---|---|
| Data preparation | Label participants, order measurement columns, and reshape readings into `gait_long`. |
| Question 1: summaries | Check missing values and group counts; calculate `participant_stats` and display `participant_summary`. |
| Question 1: visualisations | Produce histograms, orientation animations, participant/group boxplots, and RMS plots. `outlier_summary` flags readings using participant-specific 1.5 × IQR boundaries. |
| Question 1: angular changes | Calculate RMSSD in `participant_smoothness`; plot participant scores and absolute second-to-second changes. |
| Question 2: treatment comparisons | Check assumptions and run Welch’s tests: `primary_test` for RMS and `rmssd_test` for RMSSD. Sensitivity analyses exclude the highest L-DOPA score. |
| Question 3: adjusted comparisons | Fit `model_rms` and `model_rmssd` using treatment, exercise, and age; inspect diagnostics and produce coefficient tables. |
| Question 3: robustness | Exclude d204 and d216 separately; compare additive models with models containing all pairwise interactions. |

**Outcomes:** RMS is the primary measure of deviation from upright; RMSSD is the secondary measure of second-to-second angular variability. Both are in degrees. Inferential analyses use one score per participant, not individual seconds as independent observations.

**Outputs:** Tables and plots appear in the HTML report. Explicit save commands write PNG figures and GIF animations to the working directory. Keep full-precision analysis objects; rounded tables are for presentation only.
