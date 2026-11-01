# New York apple counting validation

Analyzed 919 complete image pairs in 5 cultivar/accession groups.

Frame-summed reference count: 11,674; predicted count: 10,746.

Pooled MAE = 1.1687, RMSE = 6.8060, and mean signed bias = -1.0098 apples/image.

R² = 0.5418; concordance correlation = 0.6800; WAPE = 9.20%.

Exact count agreement = 44.07%; within ±1 apple = 78.67%; within ±2 = 89.77%.


## Cultivar results

- Rootbag Porter's Perfection: n=191; MAE 2.921; RMSE 14.747; bias -2.723; R² 0.155.

- Fuji: n=190; MAE 1.211; RMSE 1.658; bias -0.905; R² 0.947.

- Honeycrisp: n=162; MAE 0.593; RMSE 1.024; bias -0.519; R² 0.950.

- Snapdragon: n=195; MAE 0.605; RMSE 1.086; bias -0.482; R² 0.942.

- Spijon-pomiferous: n=181; MAE 0.398; RMSE 0.780; bias -0.320; R² 0.948.


## Data review (no values changed or removed)

1 records meet a review trigger. Check data_review_flags.csv against original images/count records.

- rgb_videoblock70/image18.jpg, Excel row 29: predicted 21, reference 222; ground truth > Q3 + 3*IQR within folder; absolute count error >= 10.


## Interpretation and discussion boundaries

- Primary comparisons are paired per-image counts. Video frames can show the same apples repeatedly; totals are not unique orchard yields.

- Positive bias is overcounting; negative bias is undercounting. RMSE emphasizes large errors; compare it with MAE and median absolute error.

- R² is 1 − SSE/SST using predictions directly, not squared Pearson correlation. It can be negative. Pearson/Spearman association alone does not establish agreement.

- WAPE is 100 × sum absolute errors / sum reference counts. Predicted-to-real percentage is a count ratio, not detection recall or accuracy.

- Bland–Altman limits are descriptive bias ± 1.96 sample SD, not confidence intervals; skewness, heteroscedasticity and dependent frames limit their interpretation.

- No p-values or inferential confidence intervals are reported: tree, video and orchard sampling-unit identifiers are unavailable. Frames must not be treated as independent orchard replicates.

- Cultivar comparisons are descriptive and may be confounded by orchard conditions, occlusion, lighting, image density and repeated viewpoints.

- Count agreement cannot establish precision, recall, F1, mAP, Dice or IoU. Those require matched object-level or pixel-level references.

- These data can support external counting validation of a synthetic-trained model, conditional on confirmed training/test separation. They do not independently prove the training provenance or eliminate the need for field validation.

- Use the claim “no task-specific field imaging or manual annotation for training” only if the full training and annotation pipeline supports it, including any pretrained components.

- The supplied cultivar/accession names are retained. Rootbag is part of the supplied label; no botanical species or horticultural synonym has been inferred.


## Figure guide

01 parity; 02 cultivar errors; 03 frame-summed totals; 04 absolute-error ECDF; 05 Bland–Altman; 06 sequence diagnostics; 07 count tolerance; 08 reference-density errors.

All plots retain all observations and are saved as 600-dpi PNG plus vector PDF and SVG. No automatic outlier exclusion or correction is performed.


## Metric definitions

error = predicted − reference; MAE = mean(abs(error)); RMSE = sqrt(mean(error²)); bias = mean(error).

CCC = 2 cov(reference,predicted) / [var(reference)+var(predicted)+(mean(reference)−mean(predicted))²], with population moments.

NRMSE(mean) = 100 × RMSE / mean(reference). Undefined quantities are NaN, never replaced by zero.

Pooled metrics use all image pairs; macro averages weight each cultivar equally. Mean cultivar RMSE is not pooled RMSE.