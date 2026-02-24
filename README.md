# ✈️ AeroPredict --- Flight Delay Prediction Pipeline

## 📌 Overview

**AeroPredict** is a Machine Learning project focused on predicting
flight arrival delays using structured aviation data.\
The objective is to identify patterns that increase the probability of
delays and evaluate classification models using proper validation
techniques.

This project was developed following Machine Learning Engineering best
practices, including:

-   Structured data preprocessing pipelines
-   Cross-validation with stratification
-   Model comparison
-   Performance metrics beyond accuracy
-   Interpretability analysis
-   Critical discussion of limitations

------------------------------------------------------------------------

## 📂 Dataset

Dataset: *Flight Delays and Cancellations (flights.csv)*

The dataset includes operational flight information such as:

-   Departure and arrival times
-   Distance
-   Airline
-   Taxi times
-   Weather and operational delay indicators
-   Cancellation and diversion flags

Target variable created:

IS_DELAYED = 1 if ARRIVAL_DELAY \> 15 minutes else 0

This threshold aligns with industry standards for defining significant
delays.

------------------------------------------------------------------------

## 🧹 Data Preprocessing

### 1. Cleaning

-   Removal of cancelled and diverted flights
-   Handling of missing values
-   Creation of binary target variable

### 2. Feature Engineering

-   Categorical encoding (OneHotEncoder)
-   Numerical scaling (StandardScaler)
-   Structured transformation using ColumnTransformer

All preprocessing steps were embedded into a Scikit-learn Pipeline to
prevent data leakage.

------------------------------------------------------------------------

## 🤖 Models Implemented

### Logistic Regression

-   Baseline linear classifier
-   Evaluated with class balancing considerations

### Random Forest Classifier

-   Non-linear ensemble model
-   Captures complex interactions between features

------------------------------------------------------------------------

## 🔁 Model Validation Strategy

-   Train/Test split (stratified)
-   Stratified K-Fold Cross-Validation (k=5)
-   Evaluation metrics:
    -   Accuracy
    -   Recall (delay class)
    -   ROC-AUC
    -   Confusion Matrix
    -   ROC Curve

Cross-validation ensures model robustness and reduces variance in
evaluation.

------------------------------------------------------------------------

## 📊 Model Performance

  Model                 Accuracy   Recall (Delay)   ROC-AUC
  --------------------- ---------- ---------------- ---------
  Logistic Regression   \~0.59     \~0.62           \~0.63
  Random Forest         \~0.65     \~0.52           \~0.64

### Interpretation

-   Logistic Regression achieved higher recall for delayed flights.
-   Random Forest achieved higher overall accuracy.
-   Both models show moderate predictive capability, indicating that
    flight delays are influenced by partially unobserved external
    variables.

------------------------------------------------------------------------

## 📈 Feature Importance (Random Forest)

The Random Forest model provided feature importance scores, highlighting
which operational variables most influence delay probability.

This contributes to interpretability and allows stakeholders to
understand delay drivers.

------------------------------------------------------------------------

## 📉 Limitations

1.  External weather APIs were not integrated.
2.  Air traffic control dynamics are partially captured.
3.  Dataset limited to a specific year.
4.  Flight delays have stochastic components not fully predictable.

These constraints explain why the ROC-AUC does not approach high
predictive thresholds.

------------------------------------------------------------------------

## 🏗 Engineering Practices Applied

-   Modular ML pipeline
-   Data leakage prevention
-   Stratified cross-validation
-   Performance metric selection aligned with business objective
-   Interpretability analysis
-   Reproducible workflow

------------------------------------------------------------------------

## 🚀 Business Perspective

Predicting flight delays can:

-   Improve operational planning
-   Reduce cascading delays
-   Support airline resource allocation
-   Enhance passenger experience

Even moderate predictive power can provide operational advantages when
deployed at scale.

------------------------------------------------------------------------

## 🧠 Conclusion

AeroPredict demonstrates a structured Machine Learning Engineering
workflow applied to real-world aviation data.

The project goes beyond simple model training by including:

-   Statistical validation
-   Performance trade-off analysis
-   Interpretability
-   Critical evaluation of limitations

This reflects an engineering-oriented approach rather than a purely
academic exercise.

------------------------------------------------------------------------

## 🛠 Tech Stack

-   Python
-   Pandas
-   NumPy
-   Scikit-learn
-   Matplotlib
-   Jupyter Notebook

------------------------------------------------------------------------

## 📎 Repository Structure

AeroPredict/ │ ├── notebooks/ ├── data/ ├── README.md └──
requirements.txt

------------------------------------------------------------------------

## 👤 Author

Developed as part of a Machine Learning Engineering academic challenge.

------------------------------------------------------------------------

# ✨ Final Note

This project reflects applied Machine Learning practices with emphasis
on engineering discipline, evaluation rigor, and analytical maturity.
