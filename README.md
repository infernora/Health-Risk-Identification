## Introduction

A health risk is the chance or likelihood that something will harm or otherwise affect your health
(Wein, 2017). Early identification of health risks provides a critical role in healthcare as it
provides ample time for treatment and overall efficiency. This study utilizes socio-economic and
clinical measurements to determine if a patient is “high risk” or “low risk.” The objective of this
study is not to achieve a high accuracy, but to ensure that the predicted high risk individuals
closely align with their actual health indicators such as blood pressure, sugar, pulse and more.

## Exploratory Data Analysis (EDA)

The dataset was initially loaded using pandas. Initial exploration showed that the dataset
consisted of 29999 records with 34 columns. There was a mixture of both numeric, categorical
and binary values. A stark feature was data sparsity especially in columns such as height, weight,
bmi, spo2, muac and more. Another feature was non uniform categorical values where columns
like result_stat_bmi had values like Mild high, moderate high, severe high and high.
Firstly, to address privacy risk in healthcare research, we removed all personally identifiable
information (PII) (Narasimha Chaitanya Samineni, 2023) columns such as name, mother’s name,
father’s name, union name, birthday and more. Socioeconomic columns such as is_poor
contained only 0 values so that was dropped as well. Data sparsity was handled by adding
“missing” columns such as bmi_missing and spo2_missing. This was done because missing
values in medical data are often due to selective screening rather than data inconsistency. This
means that oftentimes, missingness itself is an informative feature. For example, a person that
appears to be fit did not have his/her bmi calculated. Finally categorical values were brought
under new umbrella terms where all values indicating a high bp were added to a new column
called bp_high_levels. The same was done for different levels of sugar, pulse, muac, spo2 and
bmi.

## Health Risk Label Construction

Since the dataset did not have an explicit health risk flag, one was constructed using the relevant
columns. Clinically abnormal values such as high and low blood pressure, bmi, blood sugar,
oxygen saturation, malnutrition indicators and history of heart conditions were all taken into
account to create a health risk label. An individual was labelled high risk (1) if any of the above
conditions were present and low risk (0) otherwise. This was inspired by real-life healthcare
diagnostics where any single abnormality can pose a serious threat to an individual.

## Preprocessing Pipelines

In order to prevent data leakage, all columns that were used to create the target labels were
carefully removed before model training. Missing numerical features were imputed using median
values because median imputation is generally better for skewed distribution and were then
passed through a standard scaler to ensure fair contribution. On the other hand, categorical
features were imputed using mode imputation to preserve dominant population characteristics
and one-hot encoding was done to convert them into model interpretable values.

## Modelling and Evaluation

After preprocessing, the dataset was split into train and test sets using stratified sampling to
preserve class distribution. Several models were first considered: Complex models like neural
networks, linear models like logistic regression and tree based models like decision trees. Given
the task requirement, complex models were not chosen as accuracy is secondary.
Since the target outcome was a binary classification, logistic regression was first chosen because
of its ease of interpretability. It is also robust to class imbalance and has a lower risk of
memorizing noise. Another model used decision trees as they can handle non linear
relationships, and has built in feature selection capabilities. Recall was used as the primary
metric as it ensures that truly high risk individuals were not missed. F-1 score was used to
balance both precision and recall.

For the logistic regression model, Precision is 0.80 which means that when the model predicts 1,
it is correct 80% of the time. Recall (class 1) is only 0.20 which means out of all actual 1’s, the
model only detects 20% indicating very low sensitivity for the positive class. ROC-AUC is 0.686
meaning that the model is somewhat better than random (0.5), but not great. So, logistic
regression favors the majority class (0). It is poor at identifying the minority class (1). This is
typical with imbalanced datasets.


Fig 1: Confusion Matrix for Regression Model. This shows a high number of false positives.
For the decision tree model, Precision (class 1) was 0.98 showing that almost all positive
predictions are correct. Recall (class 1) was 0.85 indicating much better sensitivity. ROC-AUC
was 0.964 showing excellent ability to discriminate between classes. So, the decision tree
handles class imbalance much better. It performs well for both classes and is overall a much
stronger model. The decision tree is clearly superior here. Logistic regression is biased toward
the majority class and misses most of the minority class.


Fig 2: Confusion Matrix for Random Forest Model. This shows a high number of true positives
and negatives.
Finally, predicted high-risk cases are manually validated by inspecting their underlying health
indicators (e.g., abnormal blood pressure, extreme BMI, sugar irregularities). This step ensures
that the model’s predictions are clinically consistent rather than statistically coincidental.

## Conclusion
Health risk identifiers are crucial for early treatment and prevention of diseases. This study
underwent a thorough process to create health risk labels and care was taken to prevent any data
leakage. Model training was done using logistic regression and random forest. The recall value
of the random forest came out far superior than the regression model. This opens up scope of
further research of creating health risk severity scores, using other models and using more dense
datasets.
