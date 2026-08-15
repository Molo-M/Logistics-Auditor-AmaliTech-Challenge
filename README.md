# Logistics Auditor (AmaliTech Challenge)

A delivery performance audit of the **Olist Brazilian E-Commerce Dataset**, developed as part of the AmaliTech project challenge.

The purpose of this project is to investigate whether delivery delays are concentrated in specific regions and whether late deliveries are contributing to lower customer satisfaction. The analysis combines logistics performance, geographic information, and customer review scores to identify potential areas for operational improvement.

> **Project Challenge Instructions:** See [`Instructions.md`](Instructions.md) for the original project brief, user stories, acceptance criteria, and submission requirements.

---

## A. Executive Summary

The analysis shows that approximately **97% of orders reached the delivered stage**, meaning that the majority of orders successfully completed the fulfillment process. However, delivery performance varies considerably across Brazilian states, with several states in the North and Northeast showing substantially higher late-delivery percentages than many southern and central states. Customer satisfaction is also strongly associated with delivery performance: **on-time deliveries received an average review score of 4.29/5, compared with 2.57/5 for late deliveries**. The most severe delays, exceeding five days, had an average review score of only **1.79/5**, indicating that significant delivery delays are an important contributor to negative customer experiences.

---

## B. Project Links

| Deliverable                 | Link                                        |
| --------------------------- | ------------------------------------------- |
| **Jupyter Notebook**     | [View Notebook](Notebook_Logistics_Auditor.ipynb)         |
| **HTML Notebook Export**     | [View HTML Notebook Export](HTML-Notebook_Logistics_Auditor.html)         |
| **Dashboard**            | [View Dashboard](https://public.tableau.com/app/profile/molo.munyansanga/viz/OlistBrazilianE-CommerceLogisticsReview/Dashboard1)       |
| **Presentation**         | [View Presentation](PPT-Presentation.pdf) |
| **Project Instructions** | [View Instructions](Instructions.md)        |

> **Note:** The notebook, dashboard, and presentation have been made publicly accessible as required by the challenge. The notebook contains the complete analysis and visualizations, while the dashboard provides an interactive view of the delivery-performance findings.

---

## C. Technical Explanation

### Data Source

The project uses the **Olist Brazilian E-Commerce Dataset**, a relational e-commerce dataset containing information about orders, customers, reviews, products, geolocation, and product-category translations.

The primary datasets used in the analysis were:

* `olist_orders_dataset.csv`
* `olist_order_reviews_dataset.csv`
* `olist_customers_dataset.csv`
* `olist_products_dataset.csv`
* `olist_geolocation_dataset.csv`
* `product_category_name_translation.csv`

The raw dataset is **not included in this repository** due to its size. The notebook uses relative file paths so that the analysis can be reproduced when the dataset is placed in the appropriate project directory.

---

## Data Preparation & Cleaning

The analysis began by loading the relevant CSV files into pandas DataFrames.

### 1. Joining Orders, Customers & Reviews

The `customers` and `orders` datasets were joined using `customer_id`.

The order reviews were first aggregated by `order_id` because some orders contained multiple reviews. The mean review score was calculated for orders with multiple reviews, preventing the join from unintentionally duplicating order-level records.

The aggregated reviews were then joined to the customer-order dataset using `order_id`.

The resulting master dataset contains **99,441 orders**, with each `order_id` remaining unique after the joins.

### 2. Handling Delivery Status

Orders were inspected using the `order_status` field before calculating delivery performance.

Only orders with:

```text
order_status = delivered
```

were used for the delivery-delay analysis.

This resulted in **96,478 successfully delivered orders**. Orders that were canceled, unavailable, shipped, processing, invoiced, created, or approved were excluded from the delivery-delay calculations because they did not represent completed customer deliveries.

### 3. Calculating Delivery Performance

The difference between the estimated delivery date and actual customer delivery date was calculated using:

```text
Days_Difference =
order_estimated_delivery_date
-
order_delivered_customer_date
```

Orders were then classified into three delivery-performance categories:

| Status         | Definition                                     |
| -------------- | ---------------------------------------------- |
| **On Time**    | Delivered on or before the estimated date      |
| **Late**       | 1–5 days later than the estimated date         |
| **Super Late** | More than 5 days later than the estimated date |

### 4. Geographic Analysis

Late-delivery percentages were calculated for each Brazilian state using:

```text
Late Percentage =
Late Orders / Total Delivered Orders × 100
```

State-level geographic coordinates were derived from the Olist geolocation dataset and used to visualize delivery performance geographically.

The analysis indicates that several states in the **North and Northeast** have comparatively high late-delivery percentages.

Because the dataset does not provide distribution-center coordinates, São Paulo was used as a geographic reference point when considering the potential relationship between geographic remoteness and delivery performance. This should therefore be interpreted as an exploratory observation rather than proof that distance from a distribution center causes delays.

### 5. Customer Sentiment Analysis

Average review scores were compared across delivery-performance categories.

The results were:

| Delivery Status | Average Review Score |
| --------------- | -------------------: |
| **On Time**     |         **4.29 / 5** |
| **Late**        |         **2.57 / 5** |
| **Super Late**  |         **1.79 / 5** |

This shows a substantial difference in customer satisfaction between on-time and late deliveries, with severe delays associated with the lowest average review scores.

---

## Candidate's Choice

### Order Status Distribution

For the **Candidate's Choice** challenge, I added an **Order Status Distribution pie chart** to the Jupyter Notebook.

The chart shows the proportion of all orders that successfully reached the `delivered` stage compared with orders that remained in other statuses such as canceled, unavailable, shipped, processing, invoiced, created, or approved.

The analysis found that approximately **97% of orders reached the delivered stage**, while approximately **3% remained in other fulfillment statuses**.

### Why This Matters

This additional analysis provides useful context before evaluating final-mile delivery performance.

Without first looking at overall order status, it would be easy to focus exclusively on late deliveries and overlook problems occurring earlier in the fulfillment process.

The pie chart allows stakeholders to distinguish between:

* **Problems before successful delivery**, and
* **Problems occurring during the final-mile delivery process.**

### Why It Is Not Included in the Dashboard

The pie chart was intentionally **not included in the final dashboard**.

It was primarily designed as an introductory analytical visualization in the Jupyter Notebook to establish the overall fulfillment picture before moving into the main delivery-performance analysis.

The dashboard focuses instead on the more actionable insights surrounding **delivery delays, geographic performance, and customer sentiment**.

---

## Key Findings

### 1. Most orders were successfully delivered

Approximately **97% of orders reached the delivered stage**, providing a strong overall completion rate.

### 2. Delivery performance varies considerably by region

Late deliveries are not evenly distributed across Brazil. Several states in the North and Northeast have substantially higher late-delivery percentages.

The highest observed late-delivery percentages included:

* **Alagoas (AL): 23.9%**
* **Maranhão (MA): 19.7%**
* **Piauí (PI): 16.0%**
* **Ceará (CE): 15.3%**
* **Sergipe (SE): 15.2%**

### 3. Late deliveries are associated with lower customer satisfaction

On-time deliveries received an average review score of **4.29/5**, while late deliveries received **2.57/5**.

The effect becomes even more pronounced for severe delays: orders that were more than five days late received an average score of only **1.79/5**.

### 4. Regional delivery performance should be investigated

The concentration of late deliveries in certain regions suggests that the problem is **not evenly distributed nationwide**.

High-risk regions should therefore be investigated further to determine whether factors such as logistics infrastructure, delivery routes, regional fulfillment capacity, or delivery-date estimation practices are contributing to the observed delays.

---

## Recommendations

Based on the analysis, the following actions could be considered:

1. **Prioritize high-risk regions:**
   Investigate operational processes in states with the highest late-delivery percentages.

2. **Review estimated delivery dates:**
   Evaluate whether delivery estimates are overly optimistic in regions where delays occur frequently.

3. **Monitor severe delays separately:**
   Track orders that are more than five days late as a distinct KPI because of their particularly strong relationship with poor customer reviews.

4. **Investigate regional logistics factors:**
   Further analysis could examine distribution-center locations, shipping routes, product characteristics, and carrier performance to identify the underlying causes of regional delays.


---

## Author

**Molo Munyansanga**

[GitHub](https://github.com/Molo-M) • [LinkedIn](https://www.linkedin.com/in/molomunyansanga/)
