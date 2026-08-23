# QuickCart_StockRisk_Classification_Minor_Project-6
# QuickCart Warehouse Inventory Stockout Risk Prediction

## Project Overview

This project focuses on predicting the risk of inventory stockouts for QuickCart, a quick-commerce dark-store operator. The objective is to classify each product at each store on each day into one of three risk categories: **Safe**, **At-Risk**, or **Imminent**.

The project uses a supervised machine learning classification approach on multi-table inventory, store, product, supplier, and event data. The analysis considers important operational factors such as sales velocity, available stock, reorder points, supplier reliability, lead times, product characteristics, and festival or promotional demand spikes.

## Business Problem

QuickCart operates multiple dark stores and manages inventory across different products, suppliers, and locations. For every SKU at every store, the inventory team must determine whether the available stock will last until the next supplier delivery.

Incorrect predictions can create two major business problems:

* **Underestimating stockout risk** can result in empty shelves, lost sales, and poor customer experience.
* **Overestimating stockout risk** can lead to unnecessary ordering, increased inventory costs, and inefficient use of warehouse space.

The target variable, `stockout_risk`, contains three classes:

* **Safe:** Sufficient stock is available beyond the replenishment waiting period.
* **At-Risk:** Stock availability is close to the replenishment requirement.
* **Imminent:** The product is expected to run out of stock before the next delivery arrives.

The dataset is imbalanced, with the Imminent class representing the smallest but most critical category.

## Dataset Description

The project uses five related datasets:

| Dataset                    | Description                                                                                        |
| -------------------------- | -------------------------------------------------------------------------------------------------- |
| `dim_stores.csv`           | Store information including city, store size, area, opening year, and baseline orders              |
| `dim_skus.csv`             | Product information including category, price, shelf life, perishability, popularity, and supplier |
| `dim_suppliers.csv`        | Supplier information including reliability score and lead-time characteristics                     |
| `dim_events.csv`           | Calendar information for regular days, festival periods, and promotional events                    |
| `fact_inventory_daily.csv` | Daily inventory and sales data used as the primary modeling dataset                                |

The modeling table contains **21,600 records**, representing combinations of stores, SKUs, and dates.

## Project Workflow

### 1. Data Loading and Understanding

The datasets were loaded and examined to understand their structure, data types, missing values, and relationships.

### 2. Data Cleaning

Data quality issues were addressed, including:

* Missing supplier reliability values.
* Inconsistent text formatting in city-related fields.
* Missing values in `lead_time_days_actual`.
* Conversion of categorical and numerical columns into suitable formats for machine learning.

The dataset specification also highlights that supplier reliability values may contain missing entries represented as `N/A`, requiring explicit handling during preprocessing.

### 3. Data Integration

The store, SKU, supplier, and event datasets were merged with the daily inventory dataset to create a complete dataset for analysis and model development.

### 4. Feature Engineering

Relevant features were prepared and engineered to improve model performance. Important inventory-related features include:

* Stock levels
* Sales velocity
* Days of cover
* Reorder point
* Reorder status
* Expected supplier lead time
* Supplier reliability
* Product category
* Perishability
* Festival and promotional indicators

Suggested inventory-specific engineered features include the gap between reorder point and closing stock and the ratio of available stock cover to expected lead time.

### 5. Train-Test Split

A time-based train-test split was used to preserve the chronological nature of the inventory data. This approach helps reduce information leakage that could occur if the same store-SKU combinations were randomly distributed between training and testing datasets.

The project specification recommends training on earlier dates and testing on later dates because the dataset represents repeated daily observations for the same store-SKU combinations.

### 6. Data Preprocessing

Categorical features were transformed into numerical representations, while missing numerical values were handled through appropriate imputation techniques. The preprocessing pipeline ensures that the machine learning models receive clean and model-compatible input data.

### 7. Machine Learning Models

Multiple classification models were evaluated to predict stockout risk. The modeling approach can include:

* Baseline Classifier
* Logistic Regression
* Random Forest Classifier
* Gradient Boosting Classifier

These models allow comparison between a simple baseline, an interpretable linear model, and more advanced models capable of capturing non-linear inventory patterns.

### 8. Model Evaluation

Models were evaluated using classification metrics such as:

* Accuracy
* Macro F1 Score
* Weighted F1 Score
* Precision
* Recall
* Classification Report
* Confusion Matrix

Special attention was given to the **Recall of the Imminent class**, since failing to identify a potential stockout can have a significant operational and financial impact.

## Key Insights

The project demonstrates that stockout risk is influenced by several operational factors, including inventory availability, sales velocity, supplier reliability, expected delivery time, and demand spikes caused by festivals or promotions.

The dataset also shows that festival periods can substantially increase stockout risk and that lower supplier reliability is associated with a higher rate of imminent stockouts. Perishable products also show a higher imminent stockout rate than non-perishable products.

## Results

The final model is selected based on its overall classification performance and its ability to correctly identify high-risk inventory situations.

The final prediction output includes the relevant business information, such as:

* Store ID
* SKU ID
* Date
* Actual Stockout Risk
* Predicted Stockout Risk

The model can help the inventory team identify products that require immediate attention and support more effective replenishment decisions.

## Business Impact

This project can support QuickCart's inventory planning process by helping the business:

* Identify potential stockouts before they occur.
* Prioritize products with imminent inventory risk.
* Improve replenishment planning.
* Reduce lost sales caused by unavailable products.
* Improve supplier and inventory management.
* Handle increased demand during festivals and promotional events.
* Balance product availability with inventory holding costs.

## Technologies Used

* Python
* Pandas
* NumPy
* Scikit-learn
* Matplotlib
* Seaborn
* Google Colab or Jupyter Notebook

## Project Structure

```text
QuickCart-Stockout-Risk-Prediction/
│
├── data/
│   ├── dim_stores.csv
│   ├── dim_skus.csv
│   ├── dim_suppliers.csv
│   ├── dim_events.csv
│   └── fact_inventory_daily.csv
│
├── notebooks/
│   └── QuickCart_Stockout_Risk_Prediction.ipynb
│
├── README.md
└── requirements.txt
```

## Conclusion

The QuickCart Stockout Risk Prediction project demonstrates the application of supervised machine learning to a real-world inventory management problem. By combining data from multiple sources and analyzing factors such as stock availability, sales patterns, supplier performance, and demand events, the project predicts whether a product is Safe, At-Risk, or Imminent for stockout.

The developed solution provides a data-driven approach to inventory monitoring and can support proactive replenishment decisions. By identifying high-risk products in advance, QuickCart can reduce stockouts, improve product availability, and make inventory operations more efficient.

## Dataset Summary

The project dataset includes 12 stores, 60 SKUs, 15 suppliers, 30 days of event data, and 21,600 daily inventory records. The target distribution consists primarily of Safe observations, followed by At-Risk and Imminent cases.
