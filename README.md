# Mental Health Risk Prediction Using NHIS Data

A machine learning project predicting psychological distress from the 2024 National Health Interview Survey (NHIS) dataset using demographic, behavioral, and social factors.

## Project Overview

**Research Question:** Can a machine learning model using baseline demographic, behavioral, and social factors predict future psychological distress (K6 ≥ 5) with ≥70% accuracy, and identify the most important modifiable risk factors for early intervention?

This project demonstrates that psychological distress can be predicted using readily accessible survey data without requiring medical tests or extensive family history, making it practical for early intervention screening programs.

## Dataset

**Source:** 2024 National Health Interview Survey (NHIS)  
**Provider:** CDC's National Center for Health Statistics (NCHS)

- **Sample Size:** 32,629 adult participants
- **Response Rate:** 47.9%
- **Features:** 630 variables covering:
  - Demographics (age, sex, race/ethnicity, education, marital status)
  - Socioeconomic factors (income, employment, poverty ratio)
  - Health behaviors (exercise, sleep, substance use)
  - Chronic conditions and healthcare access
  - Mental health indicators

**Target Variable:** K6 Psychological Distress Score (0-24 scale)
- Binary classification: At-risk (K6 ≥ 5) vs. Not at-risk (K6 < 5)
- Multi-class option: Exact K6 scores for severity profiling

**Dataset Links:**
- [Dataset Documentation](https://ftp.cdc.gov/pub/Health_Statistics/NCHS/Dataset_Documentation/NHIS/2024/)
- [Codebook](https://ftp.cdc.gov/pub/Health_Statistics/NCHS/Dataset_Documentation/NHIS/2024/adult-codebook.pdf)
- [Survey Description](https://ftp.cdc.gov/pub/Health_Statistics/NCHS/Dataset_Documentation/NHIS/2024/srvydesc-508.pdf)

## Key Results

- **Overall Accuracy:** 87%
- **At-Risk Recall:** 82% (prioritized to minimize false negatives)
- **ROC-AUC:** 0.927
- **At-Risk F1 Score:** 0.72

**Exceeded 70% accuracy target** while maintaining high recall for at-risk individuals.

## Technical Stack

**Languages & Libraries:**
- Python 3.12
- pandas, numpy
- scikit-learn
- matplotlib, seaborn
- XGBoost

**Machine Learning Pipeline:**
- Data preprocessing and imputation
- Feature engineering (ordinal/nominal encoding)
- Model training (Logistic Regression, Random Forest, Gradient Boosting, XGBoost)
- Hyperparameter tuning (RandomizedSearchCV)
- Performance evaluation and visualization

## Project Structure
```
├── data
  ├── adult24.csv                            # Raw NHIS dataset
  ├── nhis_variable_dictionary.json          # Custom variable name mapping
├── eda
  ├── Milestone-EDA.ipynb                    # Exploratory data analysis
├── modelling
  ├── Milestone-3-Machine-Learning.ipynb     # Model development & evaluation
  ├── Milestone_3__Modelling_Report.pdf      # Comprehensive project report
├── Appendix
  ├── Any extra research and file references
└── README.md                                # This file
```

## Workflow

### 1. Data Engineering & Preprocessing
- Built custom parser to convert 630 cryptic NHIS variable codes into human-readable names
- Dropped columns with >70% missing values (277 columns removed)
- Applied median imputation for remaining missing values (324 columns)
- Reconstructed K6 score from 6 component features (reverse-coded 0-4 scale)
- Removed data leakage from anxiety/depression features correlated with K6 components

### 2. Exploratory Data Analysis
- Analyzed class distribution: 6,817 at-risk (20.9%) vs. 24,832 not at-risk (79.1%)
- Visualized feature relationships across demographics, mental health, behavioral indicators, and chronic conditions
- Identified patterns in age groups, sex, education level, and marital status
- Assessed data imbalance and determined appropriate modeling strategies

### 3. Feature Engineering
- Manually classified 353 features into ordinal, nominal, and numeric categories
- Applied OrdinalEncoder to preserve survey response ordering
- Standardized features using StandardScaler
- Created age group categories and binary at-risk classification

### 4. Model Development
**Models Tested:**
- Logistic Regression (baseline)
- Random Forest Classifier
- Gradient Boosting Classifier
- **XGBoost Classifier (final model)** ✓

**Rationale for XGBoost:**
- Robust to mixed data types (ordinal/nominal/numeric)
- Handles class imbalance effectively
- Provides feature importance for interpretability
- Superior performance on validation metrics

### 5. Model Evaluation
- **Primary Metric:** Recall for at-risk class (minimizing false negatives in healthcare context)
- **Secondary Metrics:** Accuracy, balanced accuracy, ROC-AUC, precision, F1-score
- Applied RandomizedSearchCV for hyperparameter optimization
- Generated confusion matrices, ROC curves, and precision-recall curves

## Key Findings

1. **Psychological distress is predictable** from accessible demographic and behavioral survey data
2. **Tree-based models outperform** parametric approaches for mixed survey data types
3. **Class imbalance** (79% not at-risk) successfully managed without synthetic data generation
4. **Feature importance analysis** enables identification of modifiable risk factors for targeted interventions
5. **High recall (82%)** minimizes missed at-risk cases, critical for healthcare screening applications

## 💡 Impact & Applications

This work provides:
- **Early detection framework** for mental health risk screening programs
- **Interpretable predictions** using only survey-based data (no medical tests required)
- **Actionable insights** through feature importance rankings
- **Scalable methodology** applicable to other public health datasets

The model prioritizes recall to minimize missed at-risk individuals, making it suitable for large-scale screening where follow-up assessments can confirm positive predictions.

## Citation

**Dataset:**
National Center for Health Statistics. National Health Interview Survey, 2024 survey description. 2025. Available at: https://ftp.cdc.gov/pub/Health_Statistics/NCHS/Dataset_Documentation/NHIS/2024/srvydesc-508.pdf

## Author

**Surbhi Kapoor**  
Date: November 2025

---

## Notes

- The NHIS dataset represents a significant sample of the U.S. civilian noninstitutionalized population
- All analysis respects survey methodology and sampling weights for generalizability
- Feature interpretability prioritized to support clinical decision-making
- Ethical considerations addressed through focus on modifiable risk factors
