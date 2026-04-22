# Energy Efficiency Score vs Actual Emissions: Discrepancy Analysis

## Overview

This project investigates the gap between **ENERGY STAR scores** and actual **greenhouse gas (GHG) emissions** in buildings.
The goal is to validate data reliability and detect anomalies such as potential “greenwashing” using machine learning.

By predicting expected emissions from physical and operational building features, we identify buildings whose reported efficiency scores do not align with their real environmental impact.

---

## Objectives

* Build a high-accuracy model to predict expected GHG emissions
* Quantify discrepancies between predicted and actual emissions
* Detect anomalous buildings with inconsistent energy efficiency signals
* Provide a data-driven validation framework for sustainability metrics

---

## Dataset

* Source: NYC Open Data
* Datasets:

  * NYC Building Energy and Water Data Disclosure (LL84)
  * PLUTO (building physical characteristics)

---

## Data Description

### Building Identification

* Property Id
* BBL (Borough, Block, Lot)

### Physical Features

* Primary Property Type
* Gross Floor Area (ft²)
* Year Built
* Number of Buildings

### Energy & Environmental Features

* ENERGY STAR Score
* Site EUI (kBtu/ft²)
* Natural Gas Use (therms)
* Electricity Use (kWh)
* Total GHG Emissions (Metric Tons CO2e)

---

## Methodology

### 1. Data Integration

* Merge LL84 and PLUTO datasets using BBL
* Perform unit normalization and missing value handling

### 2. Exploratory Data Analysis

* Analyze distribution of emissions across ENERGY STAR score ranges
* Identify suspicious patterns among high-score buildings

### 3. Emissions Prediction Model

* Models:

  * XGBoost
  * LightGBM
* Inputs:

  * Physical characteristics
  * Energy usage data
* Output:

  * Expected GHG emissions

### 4. Residual-Based Anomaly Detection

* Compute:

  * Residual = Actual Emissions − Predicted Emissions
* Apply statistical methods (e.g., Z-score) to identify outliers

### 5. Dissonance Index

* Combine:

  * ENERGY STAR Score
  * Emission residuals
* Identify buildings with:

  * High efficiency score but unusually high emissions

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
│   ├── merge_datasets.py
│   ├── eda.py
│   ├── train_model.py
│   ├── anomaly_detection.py
│   └── explainability.py
├── results/
└── README.md
```

---

## Training Pipeline

1. Data collection and integration
2. Data cleaning and feature engineering
3. Model training for emissions prediction
4. Residual calculation and anomaly detection
5. Interpretation and reporting

---

## Expected Results

* Identification of buildings with inconsistent efficiency metrics
* Detection of potential greenwashing cases
* Improved understanding of factors driving emissions

---

## Key Contributions

* Data-driven validation of energy efficiency scores
* Residual-based anomaly detection framework
* Integration of physical and operational building data

---

## Analysis and Interpretability

* SHAP analysis for feature importance
* Borough-specific insights into energy usage patterns
* Identification of key drivers of emission discrepancies

---

## Future Work

* Real-time monitoring system for building emissions
* Extension to other cities and regulatory environments
* Integration with policy enforcement systems

---

## Conclusion

This project goes beyond simple prediction by uncovering inconsistencies between reported efficiency metrics and actual emissions.
It provides a practical framework for detecting unreliable data and supporting evidence-based environmental policy decisions.
