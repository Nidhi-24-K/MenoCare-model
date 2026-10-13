# MenoCare: Age-Span Prediction Modeling

MenoCare is a women's wellness project exploring whether health, symptom, lifestyle, and demographic data can help estimate an age-span category associated with menopause.

This repository focuses on the machine learning component of MenoCare: preparing the dataset, training classification models, comparing their performance, and identifying a suitable model for further evaluation before integration into the application.

## Project Objective

The objective is to predict one of eight age-span categories using selected features from the **Longitudinal Ageing Study in India (LASI), Wave 1**.

The broader MenoCare application aims to pair an estimated age-span window with relevant wellness information, including lifestyle guidance and recommendations informed by a user's reported health profile.

The model is intended to provide an estimate, not a clinical diagnosis.

## Dataset

The project uses data from the Longitudinal Ageing Study in India (LASI), a national study covering health, ageing, social circumstances, and well-being across India.

**Dataset and documentation:**
- [Official LASI data portal — International Institute for Population Sciences (IIPS)](https://www.iipsindia.ac.in/content/LASI-data)
- [LASI Wave 1 dataset catalogue — Government of India's Open Government Data Platform](https://www.data.gov.in/catalog/longitudinal-ageing-study-india-lasi)
- [LASI India report and publications](https://iipsindia.ac.in/content/lasi-publications)

The modeling workflow uses a combined LASI dataset prepared for analysis. The dataset files are not included in this repository; users should obtain the data through the relevant official access process.

## Target Variable

The target consists of eight age-span categories:

- 25–29
- 30–34
- 35–39
- 40–44
- 45–49
- 50–54
- 55–59
- 60–65

The current labels were constructed by calculating an approximate age at last menstrual period from the reported year of the last menstrual period and an approximate survey reference year, then grouping the resulting ages into categories.

These are **proxy labels, not clinically verified menopause-age records**. The quality and limitations of this target are important considerations when interpreting model performance.

## Input Features

The model uses selected variables from the prepared dataset, including:

- Age at first marriage
- Symptoms such as hot flashes, insomnia, severe fatigue, and prolonged depressive symptoms
- Lifestyle variables, including alcohol use and physical activity
- Health indicators, including heart disease and blood pressure
- Physical measurements, including height, weight, waist circumference, and hip circumference
- State

Variables directly used to construct the target are excluded from the model inputs to reduce target leakage.

## Models Evaluated

The notebook compares the following classification algorithms:

- Logistic Regression
- Decision Tree
- Random Forest
- Extra Trees
- Gradient Boosting
- XGBoost
- CatBoost

Multiple learning rates were evaluated for the boosting models.

## Initial Model Results

The following results were obtained from the current 80/20 train-test evaluation:

| Model | Accuracy | Balanced Accuracy | Macro F1 |
|---|---:|---:|---:|
| Random Forest | 42.11% | 19.44% | 20.55% |
| Extra Trees | 41.17% | 20.63% | 22.49% |
| XGBoost (learning rate 0.1) | 23.55% | 21.61% | 17.37% |
| CatBoost (learning rate 0.1) | 18.11% | 22.52% | 14.90% |

Extra Trees currently has the highest Macro F1, Random Forest has the highest accuracy, and CatBoost has the highest balanced accuracy among the tested configurations.

These results are preliminary and do not establish a final winning model. The differences between metrics highlight the need to evaluate performance across all eight categories, rather than relying on accuracy alone.

## Evaluation Approach

The current workflow includes:

- Data preparation and feature selection
- Handling missing values and categorical features
- An 80/20 stratified train-test split
- Comparison of multiple classification algorithms
- Evaluation using accuracy, balanced accuracy, and Macro F1
- A learning curve to investigate overfitting

The Extra Trees learning curve showed substantially higher training performance than validation performance, indicating overfitting.

The next stage will focus on stratified cross-validation, hyperparameter tuning, per-class recall, confusion-matrix analysis, and a final evaluation on the held-out test set.

## Limitations

- **Approximate target:** The target is derived from an approximate age-at-last-menstrual-period calculation and has not been clinically validated as menopause age.
- **Class imbalance:** Most records fall within the 40–49 age range, while some categories contain considerably fewer records.
- **Limited predictive performance:** Current results show that distinguishing all eight categories remains challenging.
- **Overfitting:** The learning curve indicates a substantial gap between training and validation performance.
- **Generalization:** Performance on the current dataset does not establish how well the model will perform for new users or other populations.
- **Non-causal relationships:** Feature importance and predictive associations do not establish that a particular health or lifestyle factor causes menopause to occur earlier or later.

## Intended Application

MenoCare aims to make women's wellness information more accessible by combining an estimated age-span window with relevant, evidence-informed guidance.

Potential future features include symptom education, general lifestyle suggestions, culturally relevant wellness practices, and recommendations to seek appropriate healthcare when needed.

The application may also explore exercise or fitness recommendations informed by a user's reported health needs. These recommendations would require their own evidence and safety checks; the prediction model itself does not diagnose osteoporosis, cardiovascular disease, or other medical conditions.

For background on menopause and its health implications, see the [World Health Organization's menopause fact sheet](https://www.who.int/news-room/fact-sheets/detail/menopause).

## Current Status

**In progress — initial model comparison completed.**

The next steps are to improve generalization, evaluate class-level performance, tune the leading candidate models, and assess prediction reliability before selecting a model for application integration.

## Disclaimer

This is an exploratory machine learning project, not a clinically validated prediction tool. Its outputs should not be used to diagnose menopause, predict an individual's exact menopause age, or make medical decisions.

Further validation is required before real-world deployment. Any future use should communicate uncertainty clearly and protect users' health information.
