# Transparent AI for IoT Security (CICIoT 2023)

This project is directly inspired by the paper **"Enhancing IoT Security in 6G Environment With Transparent AI: Leveraging XGBoost, SHAP and LIME"**. It builds a robust and explainable machine learning model to detect malicious network traffic using the **CICIoT 2023** dataset.

Instead of a "black-box" approach, this project uses XGBoost combined with SHAP and LIME to clearly explain *why* the model flags certain traffic as an attack.

## 🛠️ The Pipeline

The code follows a clean, step-by-step pipeline from raw data to an explainable AI model:

1. **Data Loading:** Randomly samples and concatenates CSV files from the dataset folder.
2. **Data Cleaning:** Handles infinite values, drops missing rows (NaNs), forces features to numeric types, and encodes labels.
3. **Scaling:** Applies `StandardScaler` to normalize the data.
4. **Categorization & Balancing:** Maps the original 34 attack sub-classes into 8 main parent categories. It then uses a mix of **SMOTE** (oversampling) and **RandomUnderSampler** to perfectly balance all attack classes against the normal 'Benign' traffic.
5. **Binarization:** Converts the labels into a straightforward binary format: `0` for Benign and `1` for Attack.
6. **Modeling:** Trains an XGBoost classifier on the balanced, scaled dataset.
7. **Explainability (XAI):** Uses SHAP and LIME to visualize feature importance globally and locally (per-packet).

## 🧹 Want just the cleaned data?

If you only need the data preprocessing steps for your own models:
- **Run blocks/cells 1 to 4:** This loads the raw CSVs, cleans infinite/NaN values, ensures numeric types, and encodes the string labels.
- **Run blocks 5 to 11:** This splits the data, scales it, maps the categories, balances the dataset using SMOTE, and outputs the final binary dataset ready for training (`X_train_final`, `y_train_final`, `X_test_final`, `y_test_final`).

## 🚀 How to Run

1. Make sure you have the required libraries installed:
```bash
   pip install pandas numpy scikit-learn imbalanced-learn xgboost shap lime matplotlib seaborn
   
```
2. Place your dataset chunks in a folder named ./ciciot-dataset-partial/ in the same directory as the script.

3. Run the code sequentially.

## 📊 Model Evaluation & XAI

At the end of the pipeline, the code evaluates the XGBoost model using standard metrics (Accuracy, Recall, Precision, F1 Score) and generates:

- **XGBoost Feature Importance:** Highlights the top overall contributing features.
- **SHAP Summary Plots:** Shows how much each feature pushes the model’s output towards ‘Attack’ or ‘Normal’.
- **LIME Plots:** Explains individual predictions row-by-row, proving exactly why the model made a specific choice for a single network packet.