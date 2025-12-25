# Used Car Price Prediction

This project focuses on predicting the price of used cars using various features from a given dataset. The primary goal is to build a regression model that can accurately estimate a vehicle's market value, helping to address the information asymmetry often found in the used car market.

## Table of Contents
- [Overview](#overview)
- [Dataset](#dataset)
- [Methodology](#methodology)
  - [Data Cleaning and Preprocessing](#data-cleaning-and-preprocessing)
  - [Feature Engineering](#feature-engineering)
  - [Model Training](#model-training)
- [Results](#results)
- [How to Run](#how-to-run)

## Overview

The used car market can be challenging for buyers who may not have complete information about a vehicle's history and value. This project attempts to solve this by using machine learning to predict car prices based on their attributes. By analyzing historical data, we can create a model that provides a fair estimation of a car's price, empowering potential buyers with more knowledge.

The process involves extensive data cleaning, feature engineering to extract meaningful information, and training an XGBoost regression model to make the final predictions.

## Dataset

The data is provided in CSV files located in the `datasets/` directory:
- `train.csv`: The training data used to build the model.
- `test.csv`: The test data used for generating final predictions.
- `sample_submission.csv`: A sample submission file format.

The training data includes the following columns: `brand`, `model`, `model_year`, `milage`, `fuel_type`, `engine`, `transmission`, `ext_col`, `int_col`, `accident`, `clean_title`, and the target variable `price`.

## Methodology

### Data Cleaning and Preprocessing

1.  **Handling Missing Values**: Rows with missing data (`NaN`) were dropped to ensure the quality of the training data.
2.  **Correcting Typos**: The column `milage` was renamed to `mileage`.
3.  **Dropping Unnecessary Columns**: The `ext_col` (exterior color) and `int_col` (interior color) columns were dropped from the dataset.

### Feature Engineering

Several new features were created from existing columns to be used in the model:

1.  **Transmission Type**: The `transmission` column was categorized into broader groups: 'Automatic', 'Manual', 'CVT', 'Dual', and 'Other'. These categories were then label encoded.
2.  **Engine Features**: Regex was used on the `engine` column to extract:
    *   `horsepower`
    *   `engine_size` (in Liters)
    *   `type_of_fuel` (Gasoline, Flex Fuel, Electric)
3.  **Clean Title**: The `clean_title` column was converted to a binary feature (1 for "Yes", 0 for "No"). Missing values were imputed as "No".
4.  **Accident History**: The `accident` column was converted to a binary feature (1 for reported accidents, 0 for none).
5.  **Brand Category**: Car `brand` was classified into three tiers: 'Luxury', 'Premium', and 'Mass-Market', which were then label encoded. The original `brand` and `model` columns were dropped.
6.  **Car Age**: The `model_year` was used to calculate the `age` of the car (relative to 2024).

### Model Training

An **XGBoost** regression model was used for this task.
- The data was split into training (80%) and testing (20%) sets.
- The model was trained using the `reg:squarederror` objective.
- Early stopping was employed to prevent overfitting.
- The final trained model is used to predict prices on the test dataset.

## Results

The model's performance was evaluated using the Root Mean Squared Error (RMSE) metric on the validation set.

- **RMSE**: 66784.71

Feature importance analysis showed that `mileage`, `horsepower`, and `age` were among the most influential features in determining a car's price.

The final predictions on the test data are saved in `test_predictions.csv`.

## How to Run

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/your-username/regression-of-used-car-prices.git
    cd regression-of-used-car-prices
    ```

2.  **Set up a Python environment and install dependencies:**
    It is recommended to use a virtual environment.
    ```bash
    python3 -m venv env
    source env/bin/activate
    ```
    Install the required libraries:
    ```bash
    pip install pandas numpy matplotlib seaborn scikit-learn xgboost
    ```

3.  **Run the Jupyter Notebook:**
    Launch Jupyter and open the `carPriceReg.ipynb` notebook to see the full analysis and model training process.
    ```bash
    jupyter notebook carPriceReg.ipynb
    ```