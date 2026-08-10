# ShopNest E-Commerce Sales & Revenue Analytics (Power BI)

An end-to-end sales and revenue analytics project using Power BI, Power Query, and DAX to evaluate R$ 16.01M in revenue, customer purchasing behavior, order fulfillment logistics, and product category performance across 99k+ orders.

---

## 1. Project Title

**ShopNest E-Commerce Sales & Revenue Analytics**  
*An end-to-end e-commerce data analytics solution built in Power BI to analyze revenue growth, customer demographics, payment preferences, and logistics performance.*

---

## 2. Project Overview

This project analyzes the end-to-end sales performance of ShopNest, an e-commerce platform operating across Brazil. Using a relational dataset of over 99,000 transactions recorded between 2016 and 2018, the project investigates revenue growth, product category trends, geographical demand, payment methods, and delivery logistics. 

By modeling data in Power BI Desktop and leveraging DAX measures and Power Query transformations, this project converts raw transactional data into interactive visual dashboards. The analytical outcome identifies core revenue drivers, operational fulfillment bottlenecks, and high-value customer segments to support executive data-driven decision-making.

---

## 3. Business Problem

E-commerce businesses often face challenges in optimizing supply chain logistics, identifying top-performing sales regions, and understanding customer payment preferences. ShopNest required a comprehensive analysis to resolve the following key business questions:

- **Revenue Trajectory:** How has total revenue evolved year-over-year, and what are the seasonal quarterly trends?
- **Product Category Performance:** Which product categories generate the highest total revenue, and which underperform?
- **Geographical Concentration:** Which states and regions drive the majority of customer purchases?
- **Order Fulfillment & Logistics:** What proportion of orders are delivered on time versus delayed, and which months experience delivery bottlenecks?
- **Payment Method Preferences:** How do customers prefer to pay (Credit Card, Boleto, Voucher, Debit Card), and what is the transaction volume for each?
- **Customer Satisfaction:** How do customer review ratings correlate with product categories and operational delivery times?

---

## 4. Dataset

The analysis is based on the multi-table ShopNest E-Commerce relational dataset, comprising **99,441 orders** and **112,650 item records** collected from **September 2016 through October 2018**.

### Relational Schema Tables

| Table Name | Description | Key Fields | Record Count |
| :--- | :--- | :--- | :--- |
| **Customers** | Customer demographic and location details | `customer_id`, `customer_city`, `customer_state`, `customer_zip_code_prefix` | 99,441 |
| **Orders** | Master order header with purchase timestamps and delivery dates | `order_id`, `customer_id`, `order_status`, `order_purchase_timestamp`, `order_delivered_customer_date`, `order_estimated_delivery_date` | 99,441 |
| **Order Items** | Individual items per order including price and freight | `order_id`, `order_item_id`, `product_id`, `seller_id`, `price`, `freight_value` | 112,650 |
| **Order Payments** | Payment transaction values and payment methods | `order_id`, `payment_type`, `payment_installments`, `payment_value` | 103,886 |
| **Order Reviews** | Customer feedback ratings and creation dates | `review_id`, `order_id`, `review_score`, `review_comment_title` | 100,000 |
| **Products** | Product specifications and original Portuguese category names | `product_id`, `product_category_name`, `product_weight_g`, `product_photos_qty` | 32,951 |
| **Sellers** | Seller location metadata | `seller_id`, `seller_city`, `seller_state` | 3,095 |
| **Geolocation** | Geographic coordinates mapped by postal code | `geolocation_zip_code_prefix`, `geolocation_lat`, `geolocation_lng` | 1,000,163 |
| **Category Translation** | Crosswalk mapping Portuguese category names to English | `product_category_name`, `product_category_name_english` | 71 |

---

## 5. Tools & Technologies

| Category | Tool / Technology | Application in Project |
| :--- | :--- | :--- |
| **Business Intelligence** | Power BI Desktop | Data modeling, interactive dashboard design, visual reporting |
| **Data Transformation** | Power Query (M) | ETL pipeline, data type standardizations, table merges & custom columns |
| **Calculations & Analytics** | DAX (Data Analysis Expressions) | Calculated measures, custom aggregations, delivery status logic, KPIs |
| **Data Format & Storage** | CSV / Relational Datasets | Raw data storage and relational schema input |
| **Spreadsheets & Validation** | Microsoft Excel | Preliminary data checks, quick pivot validation |
| **Documentation & Reports** | Microsoft Word & PDF | Findings summary and project technical reporting |

---

## 6. Project Workflow

```
Raw CSV Datasets
       │
       ▼
Data Cleaning & Power Query ETL
       │
       ▼
Data Modeling & Star Schema Definition
       │
       ▼
DAX Measures & Business Logic Calculations
       │
       ▼
Exploratory Data Analysis & Visualizations
       │
       ▼
Interactive Power BI Dashboard Development
       │
       ▼
Business Insight Generation & Strategic Recommendations
```

### Stage Summary
1. **Raw Data Ingestion:** Imported 9 relational CSV tables into Power BI Desktop.
2. **Data Cleaning:** Standardized date/time fields, translated product categories from Portuguese to English, handled missing values, and created normalized keys.
3. **Data Modeling:** Established a Star Schema relational model connecting Orders, Customers, Products, Payments, and Reviews via 1-to-Many relationships.
4. **DAX Calculations:** Formulated DAX metrics for Total Sales, Total Revenue, On-Time Orders, Delayed Orders Count, and Average Review Score.
5. **Dashboard Visuals:** Developed interactive bar charts, pie charts, trend lines, and KPI cards across key business metrics.
6. **Insight & Action:** Extracted strategic findings to synthesize executive recommendations.

---

## 7. Data Cleaning & Preparation

To ensure high data integrity and accurate visual reporting, the following cleaning steps were executed within Power Query:

- **Category Language Translation:** Merged `product_category_name_translation` with `Nexusgoods_products_dataset` to replace Portuguese category names with clean English equivalents (e.g., `beleza_saude` → *Health & Beauty*, `relogios_presentes` → *Watches & Gifts*).
- **Date/Time Standardizations:** Parsed text timestamps in `Nexusgoods_orders_dataset` into standard Date/Time data types to enable time-intelligence slicing across Year, Quarter, and Month.
- **Logistics Logic Creation:** Calculated delivery timelines by comparing actual delivery date (`order_delivered_customer_date`) with promised estimated date (`order_estimated_delivery_date`). Formulated a flag for **On-Time** vs. **Delayed** orders.
- **Null Value Handling:** Filled missing text values in review titles/comments with `"No Comment"` and categorized missing product categories as `"Uncategorized"`.
- **Payment Method Cleanup:** Identified and filtered out 3 undefined payment entries while standardizing payment categories (*Credit Card*, *Boleto*, *Voucher*, *Debit Card*).

---

## 8. Data Analysis

### Power BI & DAX Calculations
Core metrics were established using optimized DAX expressions:

- **Total Revenue:** Sum of total customer payment values across all orders.
  ```dax
  Total Revenue = SUM(Nexusgoods_order_payments_dataset[payment_value])
  ```
- **Total Sales (Item Value):** Sum of product prices sold.
  ```dax
  Total Sales = SUM(Nexusgoods_order_items_dataset[price])
  ```
- **On-Time vs. Delayed Orders Count:** Dynamic metrics to measure fulfillment success.
  ```dax
  On-Time Orders = 
  CALCULATE(
      COUNT(Nexusgoods_orders_dataset[order_id]),
      Nexusgoods_orders_dataset[order_delivered_customer_date] <= Nexusgoods_orders_dataset[order_estimated_delivery_date],
      Nexusgoods_orders_dataset[order_status] = "delivered"
  )
  ```
  ```dax
  Delayed Orders = 
  CALCULATE(
      COUNT(Nexusgoods_orders_dataset[order_id]),
      Nexusgoods_orders_dataset[order_delivered_customer_date] > Nexusgoods_orders_dataset[order_estimated_delivery_date],
      Nexusgoods_orders_dataset[order_status] = "delivered"
  )
  ```
- **Average Customer Rating:** Aggregated review score across all evaluated orders.
  ```dax
  Average Rating = AVERAGE(Nexusgoods_order_reviews_dataset[review_score])
  ```

### Analytical Techniques
- **Time Trend Analysis:** Evaluated sales volume trajectories across years (2016–2018) and quarterly cycles to identify peak purchasing months.
- **Geographic Segmentation:** Analyzed regional revenue distribution across 27 Brazilian states to highlight top market concentrations.
- **Product Portfolio Evaluation:** Pareto ranking of product categories to isolate high-value revenue drivers vs. long-tail inventory.
- **Logistics Fulfillment Audit:** Monthly breakdown of on-time versus delayed delivery counts to locate operational bottlenecks.

---

## 9. Key KPIs

| Key Performance Indicator | Exact Metric Result | Business Context |
| :--- | :--- | :--- |
| **Total Revenue** | **R$ 16,008,872.12** | Gross payment revenue processed (including freight) |
| **Total Product Sales** | **R$ 13,591,643.70** | Total value of products purchased excluding freight |
| **Total Freight Value** | **R$ 2,251,909.54** | Total logistics shipping fees collected |
| **Total Orders Placed** | **99,441** | Completed master order transactions |
| **Total Items Sold** | **112,650** | Total individual product units sold |
| **Average Order Value (AOV)** | **R$ 160.99** | Average payment spend per order |
| **On-Time Delivery Rate** | **54.1%** (52,209 orders) | Orders delivered on or before estimated date |
| **Delayed Delivery Rate** | **45.9%** (44,261 orders) | Orders delivered after estimated date |
| **Average Customer Rating** | **4.07 / 5.00** | Overall satisfaction score across 100k reviews |
| **Top Product Category** | **Health & Beauty** (R$ 1.26M) | Leading revenue generating category |
| **Top Sales State** | **São Paulo (SP)** (R$ 6.00M) | Primary state market (36.1% of national revenue) |
| **Dominant Payment Type** | **Credit Card** (73.92%) | Primary customer payment choice (76,795 payments) |

---

## 10. Key Insights

### Insight 1 — Massive Multi-Year Revenue Growth
- **Finding:** Yearly revenue expanded rapidly from the platform's initial launch in 2016 through 2018.
- **Evidence:** Revenue grew from **R$ 59,362.34** in 2016 to **R$ 7,249,746.73** in 2017 (+12,112%) and reached **R$ 8,699,763.05** in 2018.
- **Business Meaning:** Demonstrates rapid market adoption and strong scaling of ShopNest's e-commerce operations.

### Insight 2 — Heavy Category Revenue Concentration
- **Finding:** A small subset of product categories generates the vast majority of total platform revenue.
- **Evidence:** The top 5 categories out of 71 generated over **R$ 5.40M** in product sales:
  1. *Health & Beauty* (`beleza_saude`): **R$ 1,258,681.34** (9,670 items)
  2. *Watches & Gifts* (`relogios_presentes`): **R$ 1,205,005.68** (5,991 items)
  3. *Bed, Bath & Table* (`cama_mesa_banho`): **R$ 1,036,988.68** (11,115 items)
  4. *Sports & Leisure* (`esporte_lazer`): **R$ 988,048.97** (8,641 items)
  5. *Computers & Accessories* (`informatica_acessorios`): **R$ 911,954.32** (7,827 items)
- **Business Meaning:** ShopNest relies heavily on lifestyle, home, and personal care categories for profitability.

### Insight 3 — Geographic Demand Centered in Southeastern Brazil
- **Finding:** Sales are heavily concentrated in southeastern state hubs.
- **Evidence:** São Paulo (**SP**) accounts for **R$ 5,998,226.96** (36.1% of total sales), followed by Rio de Janeiro (**RJ**) with **R$ 2,144,379.69** (15.38%), and Minas Gerais (**MG**) with **R$ 1,872,257.26** (10.27%). Together, these 3 states contribute **>61.7%** of total nationwide revenue.
- **Business Meaning:** Marketing campaigns and distribution hubs should prioritize the SP-RJ-MG corridor while optimizing regional logistics for outlying states.

### Insight 4 — Logistics Fulfillment Bottleneck with High Delay Rates
- **Finding:** Over 45% of delivered orders missed their initial delivery estimate, creating operational delivery pressure.
- **Evidence:** Out of 96,470 evaluated delivered orders, **52,209 (54.1%)** were on time while **44,261 (45.9%)** experienced delivery delays. Delays escalated during peak monthly purchasing volumes (e.g., late Q4 / early Q1).
- **Business Meaning:** Fulfillment delays represent the single largest operational risk to customer retention and brand reputation.

### Insight 5 — Overwhelming Preference for Digital Credit Card Payments
- **Finding:** Customers strongly prefer installment-enabled credit card payments over alternative payment methods.
- **Evidence:** Credit cards account for **76,795 payment transactions (73.92%)**, followed by Boleto bancário at **19,784 (19.04%)**, Vouchers at **5,775 (5.56%)**, and Debit Cards at **1,529 (1.47%)**.
- **Business Meaning:** Payment gateway optimization should focus on seamless credit card checkout and flexible installment plans.

### Insight 6 — High Overall Satisfaction Balanced by Delivery Friction
- **Finding:** Customer satisfaction remains positive overall, though lower ratings correlate directly with shipping delays.
- **Evidence:** The platform achieved an average review rating of **4.07 out of 5.00** across 100,000 reviews, with **57.4% (57,420)** 5-star ratings. However, **11.9% (11,858)** 1-star ratings stem predominantly from order delivery delays.
- **Business Meaning:** Improving delivery speed will directly reduce negative review volume and increase customer lifetime value.

---

## 11. Business Recommendations

### Recommendation 1 — Optimize Supply Chain & Carrier Partnerships in High-Delay Regions
- **Action:** Renegotiate carrier SLAs and establish regional fulfillment hubs near high-volume states (SP, RJ, MG).
- **Reason:** **45.9% of orders experienced delivery delays**, driving the majority of 1-star customer reviews.
- **Expected Impact:** Could improve on-time delivery rates to >80% and reduce 1-star negative reviews by an estimated 25–35%.

### Recommendation 2 — Expand Inventory Allocation for Top 5 Revenue Categories
- **Action:** Prioritize stock availability and seller onboarding in *Health & Beauty*, *Watches & Gifts*, and *Bed/Bath/Table*.
- **Reason:** Top 5 categories generate over **R$ 5.4M in sales**, representing the core revenue engine.
- **Expected Impact:** May prevent stockouts during peak promotional quarters and drive an estimated 10–15% sales lift.

### Recommendation 3 — Targeted Marketing & Regional Promotions in Southeastern States
- **Action:** Concentrate digital marketing spend and localized promotions in São Paulo, Rio de Janeiro, and Minas Gerais.
- **Reason:** **61.7% of total platform revenue** originates from these 3 southeastern states.
- **Expected Impact:** Could maximize ROI on customer acquisition cost (CAC) by targeting proven high-converting markets.

### Recommendation 4 — Enhance Flexible Payment Options & Credit Card Installment Incentives
- **Action:** Offer zero-interest installment options for high-value items paid via Credit Card and promote instant digital payment options to replace slow-clearing Boletos.
- **Reason:** **73.92% of payments occur via Credit Card**, highlighting customer demand for flexible digital payment options.
- **Expected Impact:** Could raise Average Order Value (AOV) above the current R$ 160.99 benchmark.

---

## 12. Dashboard / Visualization

The Power BI analysis is structured into clear analytical visual layouts captured in the project artifacts:

### 1. Top Categories by Total Sales
*Identifies revenue contributions across product categories, highlighting Health & Beauty, Watches, and Bed/Bath as top drivers.*  
![Top Categories by Sales](1.png)

### 2. Delayed Orders Count by Product Category
*Analyzes supply chain friction by evaluating delay volumes across product categories.*  
![Delayed Orders Count](2.png)

### 3. Monthly Comparison of On-Time and Delayed Orders
*Tracks fulfillment performance across months to pinpoint peak logistics pressure.*  
![On-Time vs Delayed Orders](3.png)

### 4. Payment Count by Payment Type Breakdown
*Illustrates payment method distribution (Credit Card 73.9%, Boleto 19.0%, Voucher 5.6%, Debit Card 1.5%).*  
![Payment Count by Type](4.png)

### 5. Product Rating & Customer Review Score Analysis
*Evaluates customer review score distribution (Average 4.07 / 5.0).*  
![Average Rating Analysis](5.png)

### 6. Sales Distribution by Customer State
*Displays geographic sales breakdown dominated by São Paulo (36.1%), Rio de Janeiro (15.4%), and Minas Gerais (10.3%).*  
![Sales by Customer State](6.png)

### 7. Quarterly & Yearly Sales Performance Trends
*Plots quarterly revenue curves showing late-year surge patterns.*  
![Sales by Quarter and Year](7.png)

### 8. Total Revenue Growth Trajectory (2016–2018)
*Displays multi-year overall revenue expansion from R$ 59.4K to R$ 8.70M.*  
![Total Revenue Growth](8.png)

---

## 13. Project Structure

```
ShopNest Sales and Revenue Analytics (Power BI)/
│
├── 1.png                              # Visual: Top Categories by Sales
├── 2.png                              # Visual: Delayed Orders Count by Category
├── 3.png                              # Visual: Monthly On-Time vs Delayed Orders
├── 4.png                              # Visual: Payment Count by Payment Type
├── 5.png                              # Visual: Average Rating by Product ID
├── 6.png                              # Visual: Sales by Customer State
├── 7.png                              # Visual: Sales by Quarter and Year
├── 8.png                              # Visual: Total Revenue Trajectory
│
├── NexusgoodsProject.pbix             # Core Power BI Dashboard Project File
├── IBM Project PowerBi.pdf            # PDF Export of Report Visuals
├── Shopnest Data Insight.docx         # Executive Insights & Summary Document
│
├── Shopnest Nexusgoods store dataset/ # Dataset Directory
│   ├── Nexusgoods_customers_dataset .csv
│   ├── Nexusgoods_orders_dataset.csv
│   ├── Nexusgoods_order_items_dataset.csv
│   ├── Nexusgoods_order_payments_dataset.csv
│   ├── Nexusgoods_order_reviews_dataset.csv
│   ├── Nexusgoods_products_dataset.csv
│   ├── Nexusgoods_sellers_dataset.csv
│   ├── Nexusgood_geolocation_dataset.csv
│   └── product_category_name_translation.csv
│
├── promptforreadme.txt                # Specification Prompt
└── README.md                          # Project Documentation
```

---

## 14. How to Run / Reproduce the Project

To explore the project locally:

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/your-username/shopnest-sales-analytics.git
   cd shopnest-sales-analytics
   ```
2. **Review Datasets:**  
   Navigate to the dataset directory to inspect raw CSV files (`Nexusgoods_orders_dataset.csv`, `Nexusgoods_order_payments_dataset.csv`, etc.).
3. **Open Power BI Dashboard:**  
   - Install **Power BI Desktop** (latest version).
   - Double-click `NexusgoodsProject.pbix` to launch the interactive dashboard.
4. **Interact with Filters & Slicers:**  
   - Use Year, Category, and State slicers to filter sales visuals dynamically.
   - Drill through category bar charts to view underlying order items.

---

## 15. Skills Demonstrated

### Technical Skills
- **Power BI Desktop:** Star schema data modeling, interactive page navigation, visual hierarchy design.
- **Power Query (ETL):** Data cleansing, type conversions, table merges, column creation, handling missing values.
- **DAX Calculations:** Measures using `SUM`, `CALCULATE`, `COUNT`, `AVERAGE`, and dynamic conditional logic.
- **Data Modeling:** Relational schema design connecting 9 tables via 1-to-many relationships.

### Analytical & Business Skills
- **KPI Tracking:** Translating transactional records into executive-level KPIs (Revenue, AOV, Delivery Rates).
- **Logistics Fulfillment Analytics:** Identifying delivery delay metrics to highlight supply chain operational risks.
- **Customer Segmentation & Behavior:** Analyzing payment preferences and state-wise geographical purchasing patterns.
- **Actionable Business Insights:** Converting empirical data findings into strategic executive recommendations.

---

## 16. Key Learnings

Completing this project provided practical experience in managing real-world e-commerce analytics:

- **Handling Multi-Table Relational Data:** Mastered constructing a clean star-schema model from 9 separate relational tables.
- **Solving Data Language Barriers:** Learned to apply translation crosswalks in Power Query to convert localized dataset attributes into standard English categories.
- **Creating Operational Metrics in DAX:** Formulated precise conditional DAX logic to isolate on-time versus delayed delivery orders based on timestamp comparison.
- **Balancing Technical Rigor with Executive Clarity:** Focused dashboard visual design on high-level business questions, ensuring recruiters and stakeholders can scan key findings within seconds.

---

## 17. Conclusion

The **ShopNest E-Commerce Sales & Revenue Analytics** project demonstrates a complete end-to-end data analytics workflow. By organizing 99k+ transactional records into a structured Power BI solution, the project uncovered key business truths: strong annual revenue expansion (R$ 16.01M total revenue), heavy category concentration in Health & Beauty and Gifts, geographic dominance in São Paulo (36.1%), and critical fulfillment delay challenges (45.9% delayed orders). 

This project showcases practical competence in Power BI dashboard engineering, Power Query data transformation, DAX metric creation, and business-focused insight generation suitable for a professional Data Analyst role.
