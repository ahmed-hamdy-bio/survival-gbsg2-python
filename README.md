# Survival analysis of breast cancer recurrence in Python

Kaplan-Meier curves, Cox regression and survival prediction models in Python, from a clinical dataset to
hazard ratios, assumption checks and cross-validated prediction.

**Question:** which patient and tumour characteristics are associated with recurrence-free survival in
node-positive breast cancer, and does hormonal therapy improve it?

**Data:** the German Breast Cancer Study Group 2 (GBSG2) trial (Schumacher et al. 1994), distributed with
`scikit-survival`: 686 patients, 8 baseline variables, time to recurrence or death (or censoring).
It is a public example dataset, so the project demonstrates the workflow, not a new clinical finding.

## Pipeline
1. Cohort description and median follow-up (reverse Kaplan-Meier)
2. Kaplan-Meier curves by hormonal therapy, with at-risk table and log-rank test (`lifelines`)
3. Multivariable Cox proportional hazards model with hazard ratios and 95% CIs
4. Proportional hazards assumption test
5. Prediction: Cox regression vs random survival forest, 5-fold stratified cross-validation, concordance index (`scikit-survival`)

## Run it
```bash
pip install -r requirements.txt
jupyter nbconvert --to html --execute analysis.ipynb
```
The data comes with `scikit-survival`, so nothing needs to be downloaded. The command writes `analysis.html`,
plus tables in `results/` and figures in `figures/`. You can also open `analysis.ipynb` in Jupyter or VS Code.

## Results
- **Cohort:** 686 patients, 299 recurrence or death events (43.6%), median follow-up 4.5 years.
- **Hormonal therapy:** recurrence-free survival was better with hormonal therapy (log-rank p = 0.003). In the multivariable Cox model the hazard ratio was 0.71 (95% CI 0.55 to 0.92, p = 0.008).
- **Other predictors:** more positive lymph nodes increased the hazard (HR 1.05 per node, 95% CI 1.04 to 1.07, p < 0.001); higher progesterone receptor level lowered it (HR 0.87 per doubling, 95% CI 0.82 to 0.92, p < 0.001). Age, menopausal status, tumour size and estrogen receptor were not significant; tumour grade was borderline (HR 1.21 per grade, p = 0.09).
- **Proportional hazards:** no variable violated the assumption at p < 0.05; age (p = 0.061) and tumour grade (p = 0.066) were borderline.
- **Prediction:** 5-fold cross-validated concordance index was 0.68 (SD 0.04) for Cox regression and 0.69 (SD 0.03) for a random survival forest. Discrimination is modest and similar for both models.

## Limitations
- One trial of 686 patients; results may not transfer to other populations.
- Hormonal therapy was randomised, but the other variables are observational, so their hazard ratios are associations, not causes.
- Calibration and external validation were not assessed.
- The event combines recurrence and death, as defined in the dataset.
