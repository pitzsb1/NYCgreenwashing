# Evaluating the Reliability of Energy Ratings and Detecting Hidden Inefficiencies in Buildings

## Overview

This project investigates the **reliability of building energy efficiency ratings** using the London EPC dataset.

Rather than treating energy ratings as ground truth, we critically examine whether these ratings are **consistent, trustworthy, and aligned with underlying building characteristics**.

By combining supervised learning with consistency analysis, we use machine learning not only to predict ratings, but to **identify hidden inefficiencies and potential inconsistencies in the evaluation system itself**.

---

## Objectives

* Build a predictive model for building energy efficiency ratings (A–G)
* Evaluate the **consistency of ratings across the same buildings over time**
* Detect **hidden inefficiencies** where predicted and actual ratings diverge
* Analyze whether the rating system reflects true building characteristics

---

## Dataset

* Source: UK Government Open Data (EPC)
* Dataset:

  * Domestic Energy Performance Certificates (London subset)

---

## Data Description

### Building Identification

* LMK_KEY (unique certificate ID)
* UPRN (Unique Property Reference Number)

---

### Physical Features

* Property Type
* Built Form
* Total Floor Area
* Construction Age Band

---

### Energy & Environmental Features

* Current Energy Rating (A–G)
* Current Energy Efficiency Score
* CO2 Emissions (Current)
* Main Heating Description
* Tenure

---

## Methodology

### 1. Data Preparation

* Filter London buildings using local authority codes (E09)
* Select relevant physical and energy-related features
* Handle missing values and categorical encoding

---

### 2. Exploratory Data Analysis

* Analyze distribution of energy ratings
* Examine relationships between building characteristics and efficiency
* Identify patterns across property types and construction periods

---

### 3. Energy Rating Prediction Model

* Models:

  * CatBoost
  * XGBoost
* Inputs:

  * Physical and energy-related features
* Output:

  * Predicted Energy Rating (A–G)

---

### 4. Consistency Analysis

* Group records by UPRN (same building)
* Measure rating variability across time

```id="zv3u0y"
rating_variation = df.groupby("UPRN")["CURRENT_ENERGY_RATING"].nunique()
```

* Identify buildings with inconsistent rating history

---

### 5. Hidden Inefficiency Detection

* Compare:

  * Model-predicted rating vs actual rating
* Identify:

  * Overestimated efficiency (actual > predicted)
  * Underestimated efficiency (actual < predicted)

---

### 6. Residual-Based Analysis

* Treat misclassification patterns as signals of inconsistency
* Analyze systematic deviations in predictions

---

## Project Structure

```id="6e8w2v"
.
├── data/
│   ├── raw/
│   ├── processed/
├── notebooks/
├── src/
│   ├── preprocessing.py
│   ├── eda.py
│   ├── train_model.py
│   ├── consistency_analysis.py
│   ├── inefficiency_detection.py
│   └── explainability.py
├── results/
└── README.md
```

---

## Training Pipeline

1. Data collection and filtering (London subset)
2. Data cleaning and feature selection
3. Model training for energy rating prediction
4. Consistency analysis across buildings
5. Detection of hidden inefficiencies
6. Interpretation and reporting

---

## Expected Results

* Identification of buildings with inconsistent rating histories
* Detection of hidden inefficiencies in energy evaluation
* Insights into discrepancies between predicted and assigned ratings

---

## Key Contributions

* Reframing supervised learning as a **tool for data validation**
* Consistency-based analysis of real-world evaluation systems
* Detection of structural inconsistencies in energy rating assignments

---

## Analysis and Interpretability

* Feature importance analysis (e.g., SHAP)
* Investigation of factors influencing rating predictions
* Case studies of inconsistent or anomalous buildings

---

## Future Work

* Incorporation of temporal modeling for rating evolution
* Extension to non-domestic buildings
* Integration with policy and regulatory evaluation frameworks

---

## Conclusion

This project moves beyond traditional prediction tasks by questioning the reliability of energy efficiency ratings themselves.

By using machine learning as a validation tool, we uncover inconsistencies and hidden inefficiencies, offering a more critical and data-driven perspective on building energy evaluation systems.
