# Financial Transactions EDA, ML & Power BI Dashboard

This project analyzes and cleans a messy financial transaction dataset to
extract meaningful business insights and evaluate whether transaction
success or failure can be predicted using machine learning.

The project covers:

- Data cleaning and preprocessing
- Exploratory Data Analysis (EDA)
- Revenue analysis
- Transaction status analysis
- Payment method analysis
- Machine learning classification
- Interactive Power BI dashboard
- Business insight generation

---

## Project Objective

The main objective of this project is to transform a messy financial
transaction dataset into a clean and analysis-ready dataset, identify
important business patterns, and evaluate the predictive capability of
machine learning models.

The analysis focuses on:

- Identifying and fixing data quality issues
- Handling missing and invalid values
- Standardizing inconsistent product names
- Cleaning transaction identifiers and dates
- Calculating transaction revenue
- Analyzing completed, failed, and pending transactions
- Understanding revenue distribution across products and payment methods
- Testing machine learning models for transaction outcome prediction
- Building an interactive Power BI dashboard

---

## Dataset

The dataset contains financial transaction information including:

- Transaction ID
- Transaction Date
- Customer ID
- Product Name
- Quantity
- Price
- Payment Method
- Transaction Status

The original dataset contained several data quality issues such as
missing values, invalid dates, inconsistent transaction IDs, and
incomplete product names.

---

## Project Structure

```text
Financial_Transactions_EDA/
│
├── Power-BI/
│   └── Financial_Transactions_Dashboard.pbix
│
├── data/
│   └── clean_financial_transactions.csv
│
├── notebook/
│   └── Financial_Transactions_EDA_Analysis.ipynb
│
└── README.md
```

---

# 1. Data Cleaning

The original dataset contained several inconsistencies that needed to be
resolved before performing analysis.

### Transaction ID Cleaning

The `Transaction_ID` column contained inconsistent formatting and
missing/inconsistent values.

The cleaning process included:

- Removing unnecessary spaces
- Standardizing transaction ID formatting
- Maintaining unique transaction identifiers
- Handling missing or inconsistent IDs in a controlled manner

---

### Transaction Date Cleaning

The `Transaction_Date` column contained invalid date values.

Invalid dates were converted to missing datetime values using
`errors='coerce'`.

Missing dates were then filled by sampling from the remaining valid dates.

This approach was used instead of filling all missing dates with a single
median date. Using a single median date had previously concentrated a large
number of transactions on one day and created artificial July and Tuesday
revenue spikes in the dashboard.

---

### Product Name Cleaning

The `Product_Name` column contained incomplete and inconsistent values such
as:

- `cof`
- `smar`
- `lap`
- and other truncated product names

These values could cause the same product to appear as multiple different
categories.

The cleaning process included:

- Converting names to lowercase
- Removing unnecessary spaces
- Mapping incomplete names to their correct product names
- Converting the final product names back to title case

This allowed products to be grouped correctly during analysis.

---

# 2. Revenue Calculation

A new `Revenue` feature was created to measure the value of each
transaction.

The formula used was:

```text
Revenue = Quantity × Price
```

This feature was then used for revenue analysis across:

- Products
- Transaction status
- Payment methods
- Time periods

---

# 3. Completed Revenue Analysis

Revenue was also analyzed specifically for completed transactions.

Failed and pending transactions were separated from completed transactions
so that realized revenue could be analyzed independently from potential
revenue.

The analysis showed approximately:

| Transaction Status | Revenue |
|---------------------|---------|
| Completed | ~5.59 Billion |
| Pending | ~1.87 Billion |
| Failed | ~3.81 Billion |
| Total | ~11.28 Billion |

Only completed transactions were treated as realized revenue in the
business analysis.

---

# 4. Exploratory Data Analysis

Several areas were explored during EDA.

## Product Revenue

Revenue was analyzed across major product categories.

The approximate completed revenue contribution was:

- Tablet: ~114.97 Cr
- Headphones: ~112.64 Cr
- Coffee Machine: ~111.38 Cr
- Smartphone: ~111.29 Cr
- Laptop: ~109.49 Cr

Revenue was relatively balanced across the major product categories,
with tablets generating the highest revenue among them.

---

## Transaction Status Analysis

The transaction status distribution showed:

- Completed: ~49.6%
- Failed: ~33.8%
- Pending: ~16.6%

This shows a noticeable difference between potential revenue and
realized revenue.

Completed transactions represent revenue successfully received by the
business, while failed and pending transactions represent unsuccessful
or unresolved transaction outcomes.

---

## Payment Method Analysis

Transaction outcomes were also analyzed by payment method.

The observed completion and failure patterns were:

| Payment Method | Completed | Failed |
|----------------|-----------|--------|
| Credit Card | ~66.3% | ~33.7% |
| Cash | ~66.6% | ~33.4% |
| PayPal | ~66.7% | ~33.3% |

The success-to-failure pattern is relatively similar across the payment
methods.

Failed transaction counts were approximately:

- Credit Card: 14,494
- PayPal: 14,239
- Cash: 4,741

Based on this dataset, transaction failures do not appear to be strongly
different across payment methods.

However, the dataset does not contain information such as payment gateway
response codes, system errors, network failures, device information, or
checkout behavior, so the underlying cause of failures cannot be
determined from this dataset alone.

---

# 5. Machine Learning

Machine learning was used to test whether transaction success or failure
could be predicted from the available features.

For the classification task:

- Completed transactions were encoded as `0`
- Failed transactions were encoded as `1`
- Pending transactions were excluded from the classification dataset

Non-predictive identifiers were removed, including:

- `Transaction_ID`
- `Customer_ID`
- `Transaction_Date`

Categorical variables were converted into numerical features using
one-hot encoding.

---

## Models Tested

The following classification algorithms were evaluated:

- Logistic Regression
- K-Nearest Neighbors (KNN)
- Naive Bayes
- Decision Tree
- Random Forest
- Linear Support Vector Machine (SVM)

The dataset was split into:

```text
80% Training Data
20% Testing Data
```

Models were evaluated using:

- Accuracy
- F1 Score

---

# 6. Machine Learning Results

The machine learning experiments showed that most models achieved an
accuracy of approximately 54%–60%.

Some models, including Logistic Regression and Linear SVM, produced very
low F1 scores for identifying failed transactions.

This indicates that the available dataset does not contain enough
predictive information to reliably classify transaction outcomes.

Important factors that are missing from the dataset include:

- Payment gateway response codes
- Network or system errors
- Customer session behavior
- Device information
- Checkout process information
- Other transaction-level failure signals

Therefore, the dataset is more suitable for **exploratory data analysis
and business insight generation** than for building a strong transaction
failure prediction system.

The ML experiment nevertheless demonstrates the complete workflow of
preparing transaction data, training multiple classification models, and
evaluating their performance.

---

# 7. Power BI Dashboard

An interactive Power BI dashboard was created using the cleaned
transaction data.

The dashboard contains three pages.

## Page 1 — Financial Transactions Overview

This page provides an overall view of the financial transaction data.

It includes:

- Completed revenue by product
- Revenue by transaction status
- Revenue by year
- Key revenue and transaction metrics

---

## Page 2 — Revenue by Payment Method & Transaction Status

This page focuses on payment methods and transaction outcomes.

It includes:

- Completed revenue by payment method
- Pending revenue by payment method
- Failed revenue by payment method
- Comparison of Completed, Failed, and Pending transactions

This page helps analyze how revenue and transaction outcomes are
distributed across different payment methods.

---

## Page 3 — Weekly and Monthly Revenue Trend

This page focuses on time-based revenue analysis.

It includes:

- Monthly completed revenue
- Completed revenue by day of week
- Total Completed Revenue KPI
- Transaction count KPI
- Customer count KPI

The date cleaning process was also validated through the time-based
visualizations to ensure that artificial date spikes were not introduced
during preprocessing.

---

# 8. Key Business Insights

The analysis produced several important observations:

### 1. Data Quality Has a Direct Impact on Analysis

Inconsistent product names and invalid dates can create misleading
visualizations and incorrect business conclusions.

Proper data cleaning was therefore performed before creating the final
dashboard.

### 2. Revenue Is Distributed Across Products

The major product categories contribute relatively similar amounts of
completed revenue, with tablets slightly leading the other categories.

### 3. Realized Revenue Is Lower Than Potential Revenue

Only approximately 49.6% of the total revenue is associated with
completed transactions, while failed and pending transactions account
for a significant portion of the dataset.

### 4. Payment Methods Show Similar Transaction Patterns

Credit Card, Cash, and PayPal show broadly similar completion and failure
rates in this dataset.

### 5. Machine Learning Performance Is Limited

The tested models were not able to reliably predict transaction failures,
mainly because the dataset lacks important operational and transaction
failure features.

---

# 9. Technologies Used

### Programming & Data Analysis

- Python
- Pandas
- NumPy

### Data Visualization

- Matplotlib
- Seaborn
- Power BI

### Machine Learning

- Scikit-learn
- Logistic Regression
- KNN
- Naive Bayes
- Decision Tree
- Random Forest
- Linear SVM

### Development Tools

- Jupyter Notebook
- Git
- GitHub

---

# 10. How to Run

### Clone the Repository

```bash
git clone https://github.com/satyamkarn100-ctrl/Financial_Transactions_EDA.git
```

### Navigate to the Project

```bash
cd Financial_Transactions_EDA
```

### Install Required Libraries

```bash
pip install pandas numpy matplotlib seaborn scikit-learn
```

### Run the Notebook

Open:

```text
notebook/Financial_Transactions_EDA_Analysis.ipynb
```

and run the notebook cells.

### Power BI Dashboard

Open the Power BI file located inside:

```text
Power-BI/
```

using Power BI Desktop.

---

# 11. Project Workflow

```text
Raw Financial Transaction Data
            │
            ▼
      Data Exploration
            │
            ▼
       Data Cleaning
            │
            ├── Transaction ID Cleaning
            ├── Date Cleaning
            ├── Product Name Cleaning
            └── Missing Value Handling
            │
            ▼
       Feature Creation
            │
            └── Revenue = Quantity × Price
            │
            ▼
            EDA
            │
            ├── Product Analysis
            ├── Revenue Analysis
            ├── Transaction Status
            └── Payment Method Analysis
            │
            ├───────────────────┐
            ▼                   ▼
    Machine Learning        Power BI
            │                   │
            ▼                   ▼
    Model Evaluation      Interactive Dashboard
```

---

# 12. Conclusion

This project demonstrates an end-to-end workflow for working with a
messy financial transaction dataset.

The project covers data cleaning, exploratory data analysis, feature
engineering, machine learning experimentation, and Power BI dashboard
development.

The analysis shows that data quality is critical for reliable business
insights. It also demonstrates that machine learning performance depends
heavily on the quality and relevance of the available features.

While the dataset did not provide enough information to build a strong
transaction failure prediction model, it was useful for identifying
revenue patterns, transaction status distributions, product performance,
payment method patterns, and time-based trends.

---

## Author

**Satyam Karn**

GitHub: [satyamkarn100-ctrl](https://github.com/satyamkarn100-ctrl)
```

**Bhai ek correction:** tumhare GitHub screenshot ke hisaab se repo ka actual structure `Power-BI/`, `data/`, `notebook/` hai, isliye maine README mein wahi rakha hai. Aur README ko **ML ko overclaim nahi kar raha**—clear hai ki model performance weak thi aur dataset EDA/business analysis ke liye zyada useful nikla. Ye portfolio ke liye honest presentation hai.

Available next action: :chatgpt-content-reference{index="0"}
