# Systematic-Bias-Detection-in-AES
## Description
This project examines the systematic bias in LLMs automated essay scoring in the IELTS writing, using the “IELTS-writing-task-2-evaluation” and “IELTS evaluations” datasets posted by Chillies via HuggingFace. Please see this link: (https://huggingface.co/chillies/datasets). While the former is used for training the internal models, the latter is used for external validation to test for generalization. The IsANLP library by Elena Chistova is also utilized. Please see this link: (https://github.com/tchewik/isanlp_rst).

## Research Summary
The increasing use of large language models (LLMs) for IELTS automated essay scoring (AES) raises concerns about systematic bias on their judgments, especially given their training in predominantly native-English writing. While prior work examines overall scoring accuracy, this study investigates which linguistic features, such as lexical, syntactic, and discourse-level, drive subgroup-level bias by examining their contribution to consistent over- or under-scoring of subgroups within band scores. The bias magnitude of each feature are computed using the mean absolute discrepancies between human and LLM scores within each subgroup before being ranked accordingly. Predictive models (Random Forest, Logistic Regression, SVM, XGBoost) are then trained without the identified top features to assess whether bias remains predictable. After cross-validation, the best model is tuned and evaluated on an external dataset. The results show that readability metrics are the strongest drivers of systematic bias among the top features; however, when removing these leading metrics, band and the remaining linguistic features still offer a competitive predictive power according to SHAP analysis. Among the models, XGBoost performs best (F1 = 0.9028; ROC-AUC = 0.9247), with high probability accurately identifying bias-prone essays and strong generalizability.

## Methodology Summary
### 1. Extract & Engineer linguistic features: 
Syntactic, Lexical, Rhetorical Structure Theory features, Interaction features.

### 2. Bias Labeling by group-based analysis: 
- I apply tertile binning to each linguistic feature, creating three subgroups: Low, Medium, High.

- Each essay is assigned to these subgroups separately for each feature based on its feature value.

- For each subgroup, I compute the mean discrepancy between human scores and LLM scores. Then, I calculate bias magnitude for each linguistic feature by difference between the maximum and minimum group-level mean discrepancies.

- Using a predefined bias magnitude threshold, I determine which feature–subgroups are consistently receiving bias scoring by LLMs, and then examine the bias direction (over or underscore) based on their values.

- For each score band, I select the top three biased feature-subgroups. These are used to create a binary bias label for each essay.

### 3. Predictive model building:
I build a predictive model following this procedure:

- Split data into train, validation, and test sets.

- Evaluate & compare models using Accuracy, F1-score, and ROC-AUC.

- Use the validation set for cross-validation and model selection.

- Perform hyperparameter tuning on the validation set.

- Compare tuned vs. initial models on the test set to select the best model.

- Conduct SHAP explainability analysis and error analysis on the final model.

- Perform external validation on a new dataset using the same pipeline and compare results to the internal dataset.

### Note:
Please note that for all the code snippets to work, you need to have all the csv file in your folder. Enjoy!
