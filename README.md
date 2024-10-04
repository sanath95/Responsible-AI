# Responsible AI - Predicting Healthcare Test Results

---

## Introduction

The concept of **Responsible AI** refers to artificial intelligence systems that prioritize fairness, transparency, security, privacy, and accountability while ensuring they benefit humanity. These systems should be free from bias, explainable, safe, and aligned with ethical standards.

In this notebook, we develop a **Responsible AI** system to predict healthcare test results using a [healthcare dataset](https://www.kaggle.com/datasets/prasad22/healthcare-dataset?resource=download). By applying the principles of Value-Based Engineering, our goal is to create an AI system that upholds human and social values while minimizing negative impacts.

### Principles of Responsible AI

Key principles guiding this AI system include:
- Fairness and justice
- Transparency and explainability
- Security and safety
- Privacy protection
- Responsibility and accountability
- Avoidance of AI hallucinations
- Ensuring human oversight, societal benefit, respect for human rights, and prevention of harm

### EU AI Act: Risk Classification of AI Systems

Under the EU AI Act, healthcare AI systems fall under the **High Risk** category (Annex 3). This system adheres to the following high-risk requirements outlined in Article 8 of the EU AI Act:
- Risk management (Art. 9)
- Data and data governance (Art. 10)
- Technical documentation (Art. 11)
- Transparency (Art. 13)
- Human oversight (Art. 14)

---

## Datasheet

### Dataset

[Healthcare Dataset](https://www.kaggle.com/datasets/prasad22/healthcare-dataset?resource=download)

This synthetic healthcare dataset is designed to simulate real-world healthcare data, providing a safe platform for data science, machine learning, and analysis practice without the risk of personal information being leaked. However, for the purposes of this project, we will approach it as if it contains real-world data and demonstrate risk management techniques. This will include strategies to protect members' personal information from potential adversaries, ensuring privacy and security.

### Overview

- Number of Samples: 55500
- Number of Features: 14
    - Numeric: 3 features
    - Categorical: 11 features
- Target Variable: "Test Results" 
    - Categorical: Normal, Abnormal, and Inconclusive
- Null values: 0

### Features

| Column Name        | Description	                                                                       | Data Type | Example         |
|--------------------|-------------------------------------------------------------------------------------|-----------|-----------------|
| Name               | Name of the patient associated with the healthcare record                           | string    | Bobby JacksOn   |
| Age                | Age of the patient at the time of admission(in years)                               | int       | 30              |
| Gender             | Gender of the patient                                                               | string    | Female          |
| Blood Type         | Blood type of the patient                                                           | string    | A+              |
| Medical Condition  | Primary medical condition or diagnosis associated with the patient                  | string    | Diabetes        |
| Date of Admission  | Date on which the patient was admitted to the healthcare facility                   | string    | 2024-01-31      |
| Doctor             | Name of the doctor responsible for the patient's care during their admission        | string    | Matthew Smith   |
| Hospital           | Healthcare facility or hospital where the patient was admitted                      | string    | Sons and Miller |
| Insurance Provider | Insurance provider of the patient                                                   | string    | Aetna           |
| Billing amount     | Amount of money billed for the patient's healthcare services during their admission | float     | 18856.281306    |
| Room Number        | Room number where the patient was accommodated during their admission               | int       | 328             |
| Admission Type     | Type of admission, reflecting the circumstances of the admission                    | string    | Emergency       |
| Discharge Date     | Date on which the patient was discharged from the healthcare facility               | string    | 2024-02-02      |
| Medication         | Medication prescribed or administered to the patient during their admission         | string    | Aspirin         |
| Test Results       | The results of a medical test conducted during the patient's admission              | string    | Normal          |

### Data Quality & Considerations

- *No missing Values:* The dataset does not contain any missing values.
- *Balanced Data:* The target variable (test results) is balanced with approximately equal number of records for each class.
- *Bias & Fairness Considerations:* Gender and age disparities could influence healthcare outcomes, necessitating fairness analysis.

### Use Cases:

- Practice data cleaning, transformation, visualization, and analysis techniques.
- Develop and test healthcare predictive models by framing it as a multi-class classification problem, focusing on "Test Results."
- Apply risk management strategies and data governance practices.
- Experiment with accountability tools, privacy-enhancing methods, and fairness measures for AI.
- Build a "responsible AI" system aimed at enhancing human health, safety, and protecting fundamental rights.

---

## Exploratory Data Analysis

### Categorical Features
- There are 11 categorical features in the dataset.
- Among these, 'Name,' 'Date of Admission,' 'Doctor,' 'Hospital,' and 'Discharge Date' have a large number of unique categories.
- A chi-square test against the target variable (Test Results) shows that the following features are statistically significant:

|Feature|p-value|
|---|---|
|Name|5.4e-112|
|Date of Admission|	1.5e-16|
|Doctor|	5.1e-121|
|Hospital|	1.4e-107|
|Discharge Date|	3.4e-11|

- The remaining categorical variables are evenly distributed across all classes of Test Results.

### Numeric Features

- The dataset contains 3 numeric features.
- These features show very weak correlations with the target variable.
- Histograms reveal that the distribution of these numeric features is quite similar across all Test Result classes.

### Data Leakage

- The "Discharge Date" feature could lead to data leakage, as it contains information that would not be available at the time of prediction. Therefore, this feature should be excluded from model training.

---

## Model Card
This model card provides an overview of two models trained to predict test results using the [healthcare dataset](https://www.kaggle.com/datasets/prasad22/healthcare-dataset?resource=download).

Since the dataset contains many categorical features with numerous unique categories, [CatBoost](https://catboost.ai/) and [XGBoost](https://xgboost.readthedocs.io/en/stable/) are chosen due to their native support for handling categorical data efficiently.

### Model Details
- Developed by: Sanath Haritsa
- Model:
  1. [CatBoost](https://catboost.ai/)
  2. [XGBoost](https://xgboost.readthedocs.io/en/stable/)
- License: Apache 2.0
- Model Description: These models are designed to predict healthcare test results based on a person's personal data. They were developed using privacy-preserving techniques, and fairness measures have been calculated to ensure ethical use.
- Additional Resources: 
  - [XGBoost with categorical features](https://xgboost.readthedocs.io/en/stable/tutorials/categorical.html)
  - [CatBoost with categorical features](https://datascience.stackexchange.com/a/109055)

### Training

#### Training Data
The models were trained on a subset of the healthcare dataset.

Since accuracy is not the primary objective of this project, only two key hyperparameters were used during training:
- iterations=100
- learning_rate=0.03

#### Training Procedure
- "Inconclusive" test results were removed from the dataset to convert the task into a binary classification problem.
- The following features were used for model training:
  - Name
  - Date of Admission
  - Doctor
  - Hospital
  - Gender
- Categorical columns were cast as ``category`` types to ensure proper processing by the models.

#### Privacy-Enhancing Methods
- **Pseudonymization:** Personal medical information, such as medical conditions and medications, was excluded from the training data. This ensures that personal data cannot be linked to a specific individual without additional information.
- **Anonymization:** Personal data was transformed to a form that is unreadable by unauthorized users, providing additional privacy protection.

### Evaluation Results

|Model|Accuracy|
|---|---|
|Cat Boost|57.68%|
|XG Boost|51.99%|

### Intended Use

#### *Direct Use*
The models are primarily intended for educational and research purposes to predict healthcare test results based on personal information. They should not be used for purposes outside the intended scope as outlined in the Misuse and Out-of-Scope Use sections.

#### *Downstream Use*
The models may also be used for downstream tasks such as:
- Research to examine model limitations and biases, with the aim of advancing scientific understanding.
- Improving model performance through data engineering enhancements.
Downstream uses should avoid those mentioned in the Misuse and Out-of-Scope Use sections.

### Misuse
The model should not be used to conduct membership inference attacks, including attempts to deduce a person's medical condition or medication details.

### Out-of-Scope Use
The models were not designed to provide factual or accurate representations of individuals or events. Therefore, any attempt to use these models to predict such information falls outside the scope of their intended capabilities.

### Limitations
Due to the models being trained on synthetic data, their accuracy is relatively low (57%), making them unsuitable for real-world applications.

### Fairness Considerations
A comprehensive fairness analysis will be performed to assess the influence of 'Gender' on model predictions and to address any existing biases within the models.

### Environmental Impact
The models were trained on Google Colab, utilizing cloud-based CPUs. The environmental impact of this process is minimal due to Google's commitment to using renewable energy in its data centers. This translates to near-zero carbon emissions during training, with an estimated energy consumption of around 0.15 kWh per hour if running on standard grid electricity.

---

## Fairness

**Fairness** in machine learning refers to the idea that models and algorithms should treat individuals or groups equally and without bias, especially in sensitive areas such as race, gender, age, socioeconomic status, or other protected characteristics. The goal is to ensure that the decisions made by machine learning systems do not result in unfair advantages or disadvantages for any specific group of people.

In this case, "Male" and "Female" are considered the protected and unprotected groups under the "Gender" feature. Below are various fairness definitions and their corresponding calculations for both the CatBoost and XGBoost models.

### Definitions of fairness

#### Definitions Based on Predicted Outcome
1. Statistical Parity
  
  Satisfied if protected and unprotected groups have equal probability of being assigned to the positive predicted class.

2. Conditional Statistical Parity
  
  Satisfied if protected and unprotected groups have equal probability of being assigned to the positive predicted class, controlling for a set of legitimate factors L.

#### Definitions Based on Predicted and Actual Outcomes
1. Predictive Parity
  
  Satisfied if protected and unprotected groups have equal PPV - the probability of a subject with positive predictive value to truly belong to the positive class.

2. False positive error rate balance

   Satisfied if protected and unprotected groups have equal FDR – subject in the negative class to have a positive predictive value.

3. False negative error rate balance

  Satisfied if protected and unprotected groups have equal FNR – the probability of a subject in a positive class to have a negative predictive value.

4. Equalized odds

  Satisfied if protected and unprotected groups have equal TPR and FPR.

5. Conditional use accuracy equality

  Satisfied if equal PPV and NPV – the probability of subjects with positive predictive value to truly belong to the positive class and the probability of subjects with negative predictive value to truly belong to the negative class.

6. Overall accuracy equality

  Satisfied if equal prediction accuracy for positive and negative classes.

7. Treatment equality

  Satisfied if protected and unprotected groups have an equal ratio of false negatives and false positives.

#### Definitions Based on Predicted Probabilities and Actual Outcomes:

1. Well-calibration

  Satisfied if for any predicted probability score S, subjects in both protected and unprotected groups should not only have an equal probability to truly belong to the positive class, but this probability should be equal to S

2. Balance of classes (positive & negative)
  
  Satisfied if: subjects constituting positive/negative class from both protected and unprotected groups have equal average predicted probability score S

|Fairness Definition|CatBoost|XGBoost|
|---|---|---|
|Statistical Parity|&#x2718;|&#x2713;|
|Conditional Statistical Parity|&#x2713;|&#x2713;|
|Predictive Parity|&#x2718;|&#x2713;|
|False positive error rate balance|&#x2718;|&#x2713;|
|False negative error rate balance|&#x2718;|&#x2713;|
|Equalized odds|&#x2718;|&#x2713;|
|Conditional use accuracy equality|&#x2718;|&#x2718;|

> Protected and unprotected groups are said to have equal values if their difference is <1%.

---

## Explainabile AI

Explainable Artificial Intelligence (XAI) encompasses a range of techniques and methods designed to help human users understand and trust the outcomes generated by machine learning algorithms.

Why should users trust an AI system that often functions like a black box? 

This section aims to shed light on the reasoning behind the XGBoost model predictions, offering insights into its decision-making process using *TreeShap*.

Target audience: Developers, and researchers.

### SHAP

[SHAP (SHapley Additive exPlanations)](https://shap.readthedocs.io/en/latest/) is a game-theory-based approach to interpret the output of machine learning models. It assigns each feature a Shapley value, which represents its contribution to the model’s prediction by considering all possible combinations of feature inputs. SHAP provides a unified measure of feature importance, ensuring consistent and interpretable explanations for complex models.

#### TreeShap

[TreeSHAP](https://shap.readthedocs.io/en/latest/tabular_examples.html#tree-based-models) is an explainability method based on SHAP specifically designed for tree-based machine learning models like decision trees, random forests, and gradient-boosted trees. It provides a consistent and accurate way to attribute a model's output to individual features, helping users understand how each feature contributes to the predictions by computing the Shapley values in polynomial time, making it efficient for large datasets and complex models.

1. Global Explanations

Global explanations provide insights into the overall behavior of a machine learning model, showing how the model makes predictions across the entire dataset. These explanations help understand general trends and the importance of features on a broader scale.

|Feature|Mean Shap Value|
|---|---|
|Date of Admission|    0.27|
|Hospital|              0.03|
|Gender|            0.02|
|Doctor|                0.01|
|Name|               0.01|

2. Local Explanations

Local explanations, on the other hand, focus on individual predictions, explaining why the model made a specific decision for a particular instance. This allows users to understand model behavior on a case-by-case basis, offering personalized insights into specific outcomes.

Example 1:

- Predicted class: 1
- Actual class: 1

![Example 1](./assets/ex1.png)

|Feature|Shap Value|
|---|---|
|Date of Admission|    0.485991|
|Gender|              -0.002991|
|Hospital|            -0.001530|
|Name|                -0.001119|
|Doctor|               0.000508|

> ``Date of Admission`` has a strong positive impact on this prediction.

Example 2:

- Predicted Class: 0
- Actual Class: 0

![Example 2](./assets/ex2.png)

|Feature|Shap Value|
|---|---|
|Hospital            |-0.003880|
|Name                |-0.002154|
|Gender              | 0.000706|
|Doctor              |-0.000511|
|Date of Admission   | 0.000315|

> ``Hospital`` has a strong negative impact on this prediction.

---

## Conclusions

### 1. Model Performance
- The CatBoost model achieved an accuracy of 57.68%, while the XGBoost model scored 51.99% on the healthcare test result prediction task.
- These relatively low accuracies are expected given that the models were trained on a synthetic dataset and were not fine-tuned for performance. The focus of this project was more on responsible AI practices than model accuracy.

### 2. Fairness Analysis
- XGBoost demonstrated better fairness across multiple metrics compared to CatBoost. It satisfied Statistical Parity, Predictive Parity, False Positive Error Rate Balance, False Negative Error Rate Balance, and Equalized Odds. These metrics suggest that XGBoost treats protected and unprotected groups (Male and Female) more equally in terms of its predictions.
- CatBoost, on the other hand, failed to meet most fairness criteria, indicating potential bias in its predictions for different gender groups.

### 3. Explainability with SHAP (TreeShap)
These insights into the model’s decision-making process provide transparency and allow users to understand which factors most influence predictions.
- Global Explanations: The most important feature across the dataset was the Date of Admission, contributing significantly to the prediction of healthcare test results. Other features, such as Hospital and Gender, also had minor contributions.
- Local Explanations: Local SHAP values revealed that specific predictions were heavily influenced by features such as Date of Admission, which played a critical role in certain test result classifications. For example, one instance with a strong positive SHAP value for "Date of Admission" was key in classifying a test result as "Normal."

### 4. Privacy and Security
- The implementation of pseudonymization and anonymization techniques ensured that the training data did not include sensitive personal information, such as medical conditions or medications. By excluding these features, we reduced the risk of re-identifying patients or compromising their privacy. This approach aligns with the principles of **data minimization** and **privacy by design**, essential components of responsible AI development, especially in healthcare.

### 5. Limitations and Risk Management
- Synthetic Data: The dataset used for training is synthetic and not representative of real-world healthcare data. As a result, the models' predictive capabilities may not generalize well to actual healthcare scenarios, limiting their real-world applicability.
- Low Accuracy: Both models exhibit relatively low accuracy—CatBoost at 57.68% and XGBoost at 51.99%. This is significantly below what is acceptable for healthcare AI applications, where accuracy can directly impact patient outcomes. These models are currently only suitable for research purposes or educational use.
- Data Leakage: The "Discharge Date" feature is identified as a potential source of data leakage, as this information may not be available at the time of prediction. Including this feature could lead to misleading model performance and inaccurate results.

### 6. Responsible AI Principles
- This project adheres to key principles of Responsible AI as outlined in the introduction, including transparency, fairness, and privacy protection. The models were designed with minimal risk of AI hallucinations and included privacy-enhancing methods to mitigate risks associated with high-risk healthcare AI systems.
- Furthermore, the project aligns with the EU AI Act, ensuring that risk management, data governance, and transparency practices were embedded into the development process.
- Accountability tools like the Model Card and Datasheet have been used to thoroughly document the model's intended use, ethical considerations, and performance metrics, helping to guide responsible deployment and inform end-users.

---
