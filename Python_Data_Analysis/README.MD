# Data Exploration & Model Building for House Prices (Python)

## Overview
This repository focuses on exploring a house prices dataset and building various linear regression models for predicting house prices.

## Pipeline Summary

### Data Loading & Cleaning
- Utilizes pandas for loading the dataset.
- Addresses missing values and eliminates unnecessary columns.

### Exploratory Data Analysis (EDA)
- Summarizes data through descriptive statistics.
- Utilizes visualizations to analyze features such as "waterfront" and "sqft_living."

### Model Building
- Constructs multiple linear regression models with different features.
- Includes models for features like "long," "sqft_living," and combinations of multiple features.

### Model Evaluation & Refinement
- Evaluates models using Ridge regression with L2 regularization (alpha=0.1).
- Explores non-linear relationships through polynomial transformation and conducts further evaluation.

## Additional Notes
- Leverages libraries including pandas, NumPy, scikit-learn, and Seaborn.
- Serves as a useful starting point for creating and assessing models for house price prediction.
