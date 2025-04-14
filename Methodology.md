# 📊 Methodology

This section explains the step-by-step methodology used in the project, following the Google Data Analytics framework:
**Ask → Prepare → Process → Analyze → Share → Act**

---

## 1️⃣ ASK: Defining the Problem

### Why do this project?

The goal of this analysis is to improve and showcase skills in real-life e-commerce data analysis by answering the following business questions:

- What are the best-selling categories?
- How do discounts impact sales?
- What are the seasonal sales trends?
- Which customers bring the most revenue?
- What is the return rate, and how does it affect profits?
- Which payment and shipping methods are preferred?
- How does warehouse location impact order fulfillment?

### Purpose of the Analysis

To demonstrate practical data analysis skills in understanding e-commerce trends and customer behavior.

---

## 2️⃣ PREPARE: Data Collection

### What data is needed?

The dataset used is from Kaggle and simulates realistic e-commerce transactional data. It closely resembles the type of data I have worked with in my family's online store.

### Data Cleaning & Preparation

After loading the dataset, the following cleaning steps were performed using **SQLite**:

#### ✅ Handling Missing Values

**1. CustomerID (Critical Field)**

- Missing Values: 4,900 out of 496,700 rows (\~0.99%)
- Reason for Removal: CustomerID is essential for identifying and tracking unique customers, Ensured that sales trends and customer insights remain reliable, Keeping NULLs would skew customer-related metrics (e.g., customer count, repeat buyers).
Ensures accurate customer segmentation & sales analysis.
- Action: Removed all rows with NULL CustomerID.

**2. WarehouseLocation (Non-Critical Field)**

- Reason for Keeping: Warehouse location is useful but not critical to overall sales or customer behavior analysis.
- Action: Replaced NULLs with "Unknown" to retain data while preserving integrity.

---

## 3️⃣ PROCESS: Data Transformation & SQL Queries

The cleaned dataset was analyzed using SQL to extract meaningful insights.

### Key Queries and Transformations:

#### 1. Best-Selling Categories

- Counted the frequency of each category to calculate order rate per product category.

#### 2. Discounts Impact

- Created four fields: `DiscountQuantity`, `NoDiscountQuantity`, `DiscountSales`, and `NoDiscountSales`.

#### 3. Seasonal Sales Trends

- Aggregated total sales by year and month to visualize seasonal fluctuations.

#### 4. High-Value Customers

- Identified customers generating over \$10K in revenue.

#### 5. Return Rate & Profit Impact

- Used `CASE` statements to classify returns and calculate return rates and lost revenue.
- Analyzed return trends by month.

#### 6. Preferred Payment & Shipping Methods

- Counted the usage frequency of each payment and shipping method.

#### 7. Warehouse Fulfillment Efficiency

- Compared order volumes and return rates across warehouse locations.

---

## 4️⃣ ANALYZE & SHARE: Insights

### 1. Best-Selling Categories

- **Furniture** leads at 20.25% of orders.
- Categories like **Stationery**, **Electronics**, and **Accessories** follow closely.
- **Apparel** ranked lowest at 19.84%.

#### 🔍 Interpretation:

Subtle differences suggest category engagement is balanced. Furniture's lead could result from catalog size or promotions. Apparel might benefit from product strategy reviews.

---

### 2. How Do Discounts Affect Sales?

- Total Non-Discounted Sales: \$9.75M
- Total Discounted Sales: \$7.27M
- Average Non-Discounted (Yearly): \$1.86M
- Average Discounted (Yearly): \$1.39M

#### 🔍 Interpretation:

Non-discounted sales consistently outperformed. Discounts are not essential for revenue growth.

---

### 3. Seasonal Sales Trends

#### 📅 2020

- April had the most sales (\$859,159.49)
- July had the lowest sales (\$759,247.67)

#### 📅 2021

- September recorded the highest sales (\$875,789.69)
- February had the lowest (\$767,360.07)
- January and September showed slight increases compared to 2020

#### 📅 2022

- January recorded highest sales (\$864,492.48)
- September had the lowest (\$742,399.65)
- September sales dropped by approx. 15.23% from 2021

#### 📅 2023

- December recorded highest sales (\$881,874.56)
- February had the lowest again (\$756,675.76)

#### 📅 2024

- August had the highest sales (\$879,555.70)
- February had the lowest again for the third time

#### 📅 2025

- Data is incomplete — last three months are missing
- August had the most sales (\$875,412.59)
- September was the lowest due to missing data; second lowest was February

 Overall, monthly sales show seasonal trends with fluctuations in specific months like February often underperforming. The highest sales shift between months over the years, but August and September frequently emerge as strong performers. These insights can guide inventory planning, marketing focus, and operational decisions.&#xD;

#### 🔍 Interpretation:

February may require promotional focus or cost-saving adjustments during slower demand.

August and September could be leveraged for major campaigns, product launches, or inventory clearance.

---

### 4. Top Revenue Customers

- Customer #21733 generated \$11,806.36 (3 orders)
- Customer #52808 ranked 10th with \$10,063.92 (2 orders)
- Top 10 spenders placed only 2–4 orders each

#### 🔍 Interpretation:

Lack of repeat purchases may indicate market competition or the absence of a loyalty strategy.

**Recommendation:** Implement a customer retention or loyalty program.

---

### 5. Return Rate & Profit Impact

- Return Rate: 48.97%
- Total Orders: 44.80K
- Total Returns: 4,387
- Revenue Lost: \$5.44M
- Net Revenue: \$56.40M

#### 🔍 Interpretation:

Returns nearly halved overall sales. High return rates are concerning and may be linked to quality, delivery, or product fit issues. London had the lowest returns, while Paris led in volume.

---

### 6. Warehouse Location & Fulfillment

- High Return Warehouses: Paris (895), Amsterdam (891), Berlin (887)
- Lowest: London (852)

#### 🔍 Interpretation:

Return rates vary by warehouse. London’s performance may reflect stronger QA and logistics practices. Benchmarking it could reduce inefficiencies elsewhere.

---

## 5️⃣ Data Limitations & Considerations

- The dataset lacked strong diversity in warehouse performance and customer behavior.
- Most trends were subtle, requiring granular analysis and careful accuracy.
- External factors (e.g., shipping delays, product defects, customer reviews) were not included in the dataset, limiting deeper causal insights.

Despite these limitations, the project demanded precision and attention to small data points, emphasizing the importance of **detail-oriented analysis in real-world business data**.

---


