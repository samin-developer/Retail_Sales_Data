# 🛒 Retail Sales Data — Task 2: Exploratory Data Analysis & Integrity Cleaning Pipeline

## 📌 Project Overview
This repository contains my submission for **Task 2: Exploratory Data Analysis (EDA) & Integrity Cleaning Pipeline**. The primary objective of this project was to take a large, raw, and uncurated industry dataset, diagnose its underlying issues, systematically clean it, and extract meaningful descriptive insights through data visualization.

**Dataset Context:** 
The dataset represents a **Retail Grocery / FMCG (Fast-Moving Consumer Goods) Sales and Inventory system**. It contains transaction-level data, customer demographics, product details, pricing strategies, and inventory management metrics. The data spans the years **2023 to 2024** and includes 19,321 rows across 40 columns.

**Key Data Points in the Dataset:**
- **Transaction Info:** Order ID, Date, Time, Payment Method.
- **Customer Info:** Age, Gender, Segment, Loyalty Status.
- **Product Info:** Category, Unit Price, Cost Price, Discounts, Profit.
- **Inventory Info:** Units Produced, Units Sold, Waste %, Shelf Life.
- **External Factors:** Weather, Temperature, Festivals, Seasons.

---

## 🎯 What I Did in This Task (Step-by-Step)

### 1. Data Loading & Initial Diagnosis
- Loaded the massive 19,321-row dataset into a Pandas DataFrame.
- Conducted a thorough data profiling to diagnose abnormalities using `df.info()`, `df.describe()`, and value counts.
- **Abnormalities Found:** Identified high missing values (e.g., 83% missing in `Festival`, 72% missing in `Promotion_Type`), heavily skewed numerical columns, and potential data-type mismatches (like Date and Time stored as strings).

### 2. Data Integrity & Cleaning Pipeline
I executed a systematic pipeline to clean the raw data:
- **Duplicate Removal:** Used business logic (subsetting by `Order_ID`, `Product`, and `Date`) instead of blindly dropping rows, because one order could contain multiple products. This preserved valid multi-item transactions.
- **Dtype Corrections:** Converted string formats to proper `datetime` objects for time-series analysis.
- **Outlier Treatment:** Applied the **IQR (Interquartile Range) Capping method** (k=1.5) on heavily skewed pricing columns (`Total_Bill`, `Profit`, `Unit_Price`, etc.) to prevent extreme values from distorting the analysis.
- **Semantic Missing Value Imputation:** Instead of using statistical modes, I used logical imputation. For example, missing `Festival` values were filled with `'No Festival'`, and missing `Promotion_Type` was filled with `'No Promotion'`. 
- **Business Bounds:** Enforced hard logical limits (e.g., Customer Rating between 1-5, Discount between 0-30%, Age between 16-80).

### 3. Descriptive Visualization & Pattern Extraction
I plotted multiple graphs using `matplotlib` and `seaborn` to uncover hidden trends:
- **Distributions:** Histograms to check the skewness of Total Bill and Profit.
- **Correlation Heatmap:** To find relationships between discounts, quantities, and profit margins.
- **Time-Series Analysis:** Heatmaps mapping Sales by Day of Week and Hour of the day to find peak business hours.
- **Categorical Analysis:** Boxplots comparing waste percentages across different product categories and seasonality impacts.

---

## 💡 Key Insights Discovered
1. **Top Revenue vs. Waste:** The **Dairy & Spreads** category generates the highest revenue but suffers from a massive ~33% average waste percentage.
2. **Peak Sales Window:** The store experiences the highest footfall and sales on **Saturdays between 6 PM and 8 PM**.
3. **Discount Impact:** Applying discounts does increase the average quantity sold per order by approximately **+12%**, but heavily impacts the profit margin.
4. **Loyalty Behavior:** Loyalty members have a slightly larger basket size and higher average ratings.
5. **Seasonality:** Monsoon and Autumn seasons show alignment with festival calendars, causing specific spikes in transactions.
6. **Loss Analysis:** Instead of dropping negative profit rows, I flagged them under a new `Is_Loss` column. We found that loss-making transactions are a normal part of the business, likely caused by extreme promotions or high-perishability waste.

---

## 🧠 What I Learned from This Task
- **Data Cleaning is Contextual:** I learned that blindly applying `.dropna()` or filling missing values with the mode is a mistake. Understanding the *business meaning* of the missing data (e.g., missing promotion means "no promotion") is crucial.
- **Outlier Handling:** Gained hands-on experience in distinguishing between statistical outliers (which can be capped) and valid business extremes (like a loss-making transaction, which should be kept but flagged).
- **Data Validation:** Learned how to validate data by checking formula consistency (e.g., ensuring `Selling_Price` actually equals `Unit_Price` minus the discount).
- **Visual Storytelling:** Learned how to use heatmaps and boxplots not just to draw charts, but to tell a story that a business manager can understand (e.g., "When should we schedule our staff?").

---

## 📁 Repository Structure
```text
Retail_Sales_Data/
│
├── EDA_Retail_Cleaning.ipynb      ← Main Jupyter Notebook (Full Code & Logic)
├── cleaned_retail_data.csv        ← Final Cleaned Dataset Output
├── category_performance.csv       ← Summary Table (Category KPIs)
├── monthly_trend.csv              ← Time-series Summary
├── README.md                      ← Project Documentation
│
└── plots/                         ← All 10 Saved Visualizations
    ├── 01_numeric_distributions.png
    ├── 02_categorical_distributions.png
    ├── 03_correlation_heatmap.png
    ├── 04_sales_heatmap.png
    ├── 05_monthly_trend.png
    ├── 06_category_performance.png
    ├── 07_discount_impact.png
    ├── 08_customer_insights.png
    ├── 09_weather_season.png
    └── 10_outlier_boxplots.png
