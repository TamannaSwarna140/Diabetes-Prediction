# Diabetes Risk Prediction on the Pima Indians Dataset

Predicts whether a patient tested positive for diabetes from eight clinical measurements, with careful handling of missing data,
repeated cross-validation, a wider set of metrics and a decision-threshold analysis.

> A learning project on a small public dataset. It is not a diagnostic tool and must not be used for medical decisions.

## Data
- Pima Indians Diabetes dataset (National Institute of Diabetes and Digestive and Kidney Diseases): 768 women aged 21 or older of
  Pima Indian heritage; 268 tested positive (34.9%)
- Features: pregnancies, glucose, blood pressure, skin thickness, insulin, BMI, diabetes pedigree function, age
- `data/diabetes.csv`

## Approach
1. Found that glucose, blood pressure, skin thickness, insulin and BMI contain impossible zeros (376 rows, 49.0%, affected),
   treated them as missing and imputed with the median inside each cross-validation fold
2. Showed that a single 80/20 split is unreliable: accuracy ranges from 70.1% to 82.5% across 100 random splits
3. Compared six models with 5-fold cross-validation repeated 10 times (accuracy, ROC-AUC, recall, precision, F1)
4. Evaluated the chosen model with out-of-fold predictions, ROC curve and confusion matrix
5. Analysed the sensitivity/specificity trade-off across decision thresholds
6. Inspected feature drivers with logistic coefficients and random forest importance

## Results (5-fold cross-validation, 10 repeats)
| Model | Accuracy | ROC-AUC | Recall | Precision |
|---|---|---|---|---|
| Logistic regression | 0.767 | 0.835 | 0.558 | 0.713 |
| Linear SVM | 0.764 | 0.835 | 0.545 | 0.712 |
| Random forest | 0.762 | 0.835 | 0.592 | 0.686 |
| KNN (k=15) | 0.759 | 0.824 | 0.571 | 0.690 |
| Gradient boosting | 0.757 | 0.828 | 0.601 | 0.670 |
| Decision tree (depth 4) | 0.734 | 0.785 | 0.646 | 0.622 |

- The always-negative baseline reaches 65.1% accuracy. Logistic regression, linear SVM and random forest are statistically
  indistinguishable; logistic regression is chosen for its interpretability.
- At the default 0.5 threshold, sensitivity is 0.57 and specificity 0.88. Lowering the threshold to 0.3 raises sensitivity to 0.79
  (specificity 0.70, precision 0.59).
- Glucose is the strongest predictor, followed by BMI, pregnancies, pedigree function and age.

![Model comparison](images/04_model_comparison.png)
![ROC and confusion matrix](images/05_roc_confusion.png)

## Limitations
- Small sample from a single population; results should not be assumed to transfer to other groups
- Research-study measurements, not routine screening data
- Median imputation with insulin missing for nearly half the patients
- No external validation and no hyperparameter tuning beyond fixed settings

## How to run
1. Place `diabetes.csv` in a `data/` folder next to the notebook (or in the same folder).
2. Open `diabetes_prediction.ipynb` in Google Colab or Jupyter and run all cells.

## Next steps
- Nested cross-validation with hyperparameter tuning and missing-value indicators
- Probability calibration and a threshold chosen from clinical costs
- External validation on a different diabetes dataset

## Project history
This project began as a university project in 2023 (single 80/20 split, accuracy only, zeros treated as real values). The original
notebook is kept unchanged in this repository as Version 1. In 2026 I rebuilt it with data-quality handling, repeated cross-validation,
additional metrics and a threshold analysis. The original accuracy of about 77% turned out to be consistent with the corrected
estimate, but the comparison between models and the handling of missing values were not reliable.
