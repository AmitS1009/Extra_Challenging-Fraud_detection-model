# Extra_Challenging-Fraud_detection-model

#### The challenge was too handle tremendously huge dataset , with extreme unbalance!!

# High-Performance Fraud Detection Model

## Project Overview

This project focuses on developing a high-performance machine learning model to detect fraudulent financial transactions. The primary goal is to build an effective classifier using a large-scale synthetic dataset of over 6.3 million transactions and to derive actionable insights to help a financial company improve its fraud prevention infrastructure. The project navigates the challenges of working with massive, highly imbalanced data and emphasizes robust evaluation techniques.

---

## 📂 Dataset

The model is trained on a synthetic dataset designed to mimic real-world financial transaction data, sourced from the project brief.

- [cite_start]**Size:** 6,362,620 rows and 10 initial columns[cite: 15].
- **Key Features:**
  - `step`: Represents a unit of time (1 step = 1 hour).
  - `type`: The type of transaction (e.g., `CASH_OUT`, `TRANSFER`).
  - `amount`: The transaction amount.
  - `oldbalanceOrg`, `newbalanceOrig`: Balances for the originator account.
  - `oldbalanceDest`, `newbalanceDest`: Balances for the recipient account.
  - `isFraud`: The target variable, indicating if a transaction is fraudulent.

---

## ⚙️ Project Workflow

The project followed a structured machine learning pipeline to ensure robustness and reproducibility.

1.  **Data Cleaning & Preprocessing:**
    - The large dataset was handled efficiently using the **Vaex** library to avoid memory issues.
    - Outliers and highly skewed distributions in financial columns were addressed using a **log transformation (`log1p`)**.
    - Multi-collinearity was analyzed, leading to the removal of redundant features.

2.  **Feature Engineering:**
    - The categorical `type` column was converted into numerical format using **one-hot encoding**.
    - Two powerful **anomaly-detection features** (`balanceDiffOrig` and `balanceDiffDest`) were created to capture inconsistencies in account balance updates, which proved to be a very strong signal for fraud.

3.  **Model Training:**
    - An **XGBoost Classifier** was chosen for its high performance and efficiency with tabular data.
    - The severe class imbalance (0.13% fraud rate) was handled by setting the `scale_pos_weight` parameter, forcing the model to pay close attention to fraudulent cases.
    - A **time-based train-test split** was used to simulate a real-world scenario where the model is trained on past data to predict future fraud.

4.  **Model Evaluation:**
    - The model's performance was evaluated using metrics appropriate for imbalanced classification, including **Precision, Recall, F1-Score, and AUPRC**.
    - The final model achieved a **Recall of ~95%**, successfully identifying the vast majority of fraudulent transactions.

---

## 💡 Key Insights & Model Performance

The model not only performs well but also provides clear insights into the nature of fraud in this dataset.

- **Key Fraud Predictors:** The model identified the following as the most important factors for detecting fraud:
    1.  **`balanceDiffOrig`**: Ledger inconsistencies in the sender's account.
    2.  **`oldbalanceOrg_log`**: High balances in the originating account.
    3.  **`step`**: The time of the transaction.
    4.  **`type_TRANSFER` & `type_CASH_OUT`**: The specific methods used to move money out.

- **Performance Metrics:**
  - **Recall:** ~95%
  - **Precision:** ~78%
  - **AUPRC (Area Under PR Curve):** ~0.96

---

## 🛠️ Technologies Used

- **Data Manipulation & Processing:** Vaex, Pandas
- **Machine Learning:** Scikit-learn, XGBoost
- **Data Visualization:** Matplotlib, Seaborn
- **Environment:** Jupyter Notebook, Conda

---

## 🚀 How to Run

1.  Clone the repository:
    ```bash
    git clone [https://github.com/AmitS1009/Extra_Challenging-Fraud_detection-model.git](https://github.com/AmitS1009/Extra_Challenging-Fraud_detection-model.git)
    ```
2.  Navigate to the project directory:
    ```bash
    cd Extra_Challenging-Fraud_detection-model
    ```
3.  Create and activate the Conda environment using the provided `environment.yml` file (recommended):
    ```bash
    conda env create -f environment.yml
    conda activate fraud_detection_env
    ```
4.  Launch Jupyter Notebook and open the main project notebook.
    ```bash
    jupyter notebook
    ```

---

## 📋 Actionable Recommendations

Based on the model's insights, the following prevention strategies are recommended:

1.  **Implement Dynamic Multi-Factor Authentication (MFA):** Use the model's real-time risk score to trigger MFA challenges for high-risk transactions.
2.  **Create Real-time Anomaly Alerts:** Immediately flag any transaction with a ledger inconsistency (i.e., a non-zero `balanceDiffOrig`).
3.  **Enhance Destination Account Profiling:** Increase the risk score for large transactions sent to new accounts or accounts with no prior history.
