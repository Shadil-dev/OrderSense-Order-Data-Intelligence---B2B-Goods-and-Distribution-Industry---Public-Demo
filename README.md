# 📦 OrderSense: Order Data Intelligence & Forecast System

> **Predictive Reordering for B2B & Recurring Goods Trade Industries**
>
> OrderSense is an intelligent, full-stack order intelligence and demand forecasting system. It predicts precise reorder dates and quantities for recurring business accounts, enabling distributors, wholesalers, and B2B merchants to automate replenishment, eliminate stockouts, and boost customer lifetime value.
>
> 🚀 **[Live Demo System](https://order-sense-reorder-intelligence-git-hub-public-demo-42ijm73y8.vercel.app/)**

---

## ✨ Key Capabilities & Features

*   **Smart Reorder Predictions:** Predicts future reorder dates and quantities for recurring accounts by integrating directly with Zoho Inventory and analyzing per-product transaction histories.
*   **Custom Forecasting Models:** Combines historical purchase cycles with demand trend patterns and confidence scores, mapped through normalized linear regression and exponential moving averages.
*   **Automated Data Sync Engine:** Daily scheduled batch synchronization processes via Spring Scheduler ingest real-time sales transactions, customer activity status, and catalog changes from Zoho Inventory.
*   **Robust REST API:** Secure, authenticated REST API endpoints filterable by customer status, specific product categories, and prediction horizon metrics.
*   **Enterprise-Grade Administration:** Secure workspace operations with custom authorization checks, password modification routines, and admin-only user account management features.

---

## 📊 Data Views & Interactive Insights

OrderSense turns raw transactional history into actionable visual reports through interactive dashboards:

### 1. Reorder Alerts & Forecasting Grid
*   **Urgency Mapping:** Customer-product pairs are grouped into visual urgency cohorts: *Overdue* (overdue for a reorder), *Urgent* (due within 5 days), *Soon* (due within 15 days), and *Safe* (due beyond 15 days).
*   **Detailed Metrics:** Lists the last order date, predicted reorder date, average order gap, confidence levels, and projected volumes.
*   **Advanced Sorting/Filtering:** Real-time search and filters for customer status and urgency tiers. Sort by upcoming order date, average quantity, or prediction confidence.

### 2. Customer Consumption Profile (Per-Product Consumption)
*   **Interactive Consumption Donut Charts:** Renders an interactive SVG donut chart for each B2B customer showing their monthly volume distribution across different products.
*   **Per-Customer Breakdown:** Shows active product counts, total unit volume projected for the selected month, and average order cycle.
*   **Interactive Legend:** Lists product-specific consumption details, including average gap days, typical order quantities, trend directions (📈 Increasing, 📉 Decreasing, ➖ Stable), and percentage share of customer budget/volume.

### 3. Overall Market Consumption Profile
*   **Product-First Analysis:** Aggregates consumption metrics across the entire portfolio.
*   **Customer Share Breakdown:** Select any product to see an interactive SVG donut chart showing the exact share of that product consumed by different business accounts, highlighting your highest-volume buyers.

### 4. Bulk Purchase Order (PO) Forecast
*   **Monthly Demand Projections:** Automatically projects the total required stock replenishment for any given month based on aggregate B2B customer consumption rates.
*   **Product-centric View:** Lists total required volume and estimated cost per product, including preferred vendor/manufacturer and cost price.
*   **Vendor-centric View:** Groups all forecasted purchases by manufacturer/preferred vendor. Displays total items count and estimated total cost. This allows procurement teams to prepare bulk replenishment purchase orders in seconds.

---

## 🛠️ The Technology Stack


OrderSense is built with a decoupled modern architecture:

| Tier | Technologies Used |
| :--- | :--- |
| **Backend** | Java 17, Spring Boot, Spring Data JPA, Spring Scheduler, Spring Mail |
| **Database** | PostgreSQL |
| **Frontend** | React, TypeScript, Vite, Material UI (MUI) |
| **Integrations** | Zoho Inventory REST API, OAuth 2.0 |

---

## 🧠 Deep Dive: The Prediction Model

The core predictive intelligence in OrderSense uses a hybrid model combining statistical averages, weighting, standard deviations, and linear regression:

### 1. Purchase Cycle Calculations
*   **Average Gap (`avgGap`):** Computes the basic mean time (in days) between historical orders for each customer-product pair.
*   **Exponentially Weighted Mean Gap (`weightedGap`):** Applies exponential weights to recent orders (recent purchase behaviors carry heavier significance).
*   **Combined Predicted Gap:** Calculated as `(avgGap * 0.4) + (weightedGap * 0.6)`.

### 2. Quantity Forecasting
*   **Average Quantity (`avgQty`):** Computes historical mean order quantities.
*   **Exponentially Weighted Quantity (`weightedQty`):** Values recent purchase volumes more heavily.
*   **Combined Predicted Quantity:** Calculated as `(avgQty * 0.3) + (weightedQty * 0.7)`.

### 3. Demand Trend & Normalized Linear Regression
To identify if a customer's demand for a product is rising, falling, or stable:
*   Applies a linear regression slope calculation over order quantities.
*   The slope is normalized against the average quantity: `normalizedSlope = slope / avgY`.
*   **Increases (`INCREASING`)** if `normalizedSlope > 0.05` (5% growth trend).
*   **Decreases (`DECREASING`)** if `normalizedSlope < -0.05`.
*   Otherwise marked as **`STABLE`**.

### 4. Confidence Scores
*   Computes the Coefficient of Variation ($CV = \sigma / \mu$) for both purchase gaps and quantities to establish a standard deviation metric.
*   Confidence is mapped as `1.0 - CV` (clamped between `0.0` and `1.0`). A lower variance results in a higher confidence score.

---

## 📂 Repository Structure

```
├── src/                    # Backend - Spring Boot codebase
│   └── main/java/com/reorderpredictor/
│       ├── config/         # App configurations (Security, Zoho, Mail, Cors)
│       ├── controller/     # REST Endpoints (Auth, Prediction, Admin)
│       ├── dto/            # Data Transfer Objects
│       ├── model/          # Database Entities (Order, Prediction, Product, User)
│       ├── repository/     # JPA Repositories
│       ├── service/        # Business Logic & Engine (Prediction, Zoho Sync, Scheduled Tasks)
│       └── util/           # Helper Utilities
├── frontend/               # Frontend - Vite + React + TypeScript App
│   ├── src/                # Components, Pages, State management & UI layout
│   └── package.json        # Frontend Dependencies
└── pom.xml                 # Maven Configuration
```

---

## 💼 Business Implementations & Support

Are you looking to integrate a custom order prediction and intelligence system for your B2B business? 

OrderSense can be tailored and deployed to sync with your ERP, CRM, or inventory management software (Zoho, QuickBooks, Salesforce, NetSuite, etc.) to automate recurring orders and forecast sales pipelines.

📧 **Get in touch:** [shadil.cse@gmail.com](mailto:shadil.cse@gmail.com)
