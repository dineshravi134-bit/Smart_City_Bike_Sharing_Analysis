# 🚲 Smart City Bike-Sharing System Analysis

An end-to-end data analytics project transforming raw public bike-sharing telemetry into actionable urban mobility intelligence using **Power Query**, **Power BI**, and a normalized **Star Schema**.

## 📋 Project Overview

This project cleans, models, and visualizes **2,856 bike-sharing stations** spanning **25 municipal contracts** (comprising **57,203 total bike stands** and **19,646 available bicycles**). 

### **Main Objective**
* **Understand station distribution & operating status:** Monitor how bike stations are spread across cities, whether they are open or closed, and how many bikes and empty docks are available for commuters.
* **Support operational decisions:** Provide an interactive Power BI dashboard to help city planners spot bottlenecks (like empty stations or broken docks) and make quick decisions to keep the bike-sharing system running smoothly.

---

## 🛠️ Tools & Technologies

* **Power Query (Power BI):** Data cleaning, text standardization, geospatial coordinate splitting, and surrogate key generation (`Contract_ID`).
* **Power BI Desktop:** Relational Star Schema modeling, DAX KPI calculations, interactive dashboard reporting, and visual storytelling.

---

## 🗄️ Data Architecture (Star Schema)

To optimize query performance and eliminate redundancy, the dataset was normalized into a **Star Schema** with a single `1:*` relationship:

* **Fact Table (`Fact_Table` - 2,856 rows):** Contains station operational telemetry: `Number`, `Name`, `Address`, `Position` (`Latitude`, `Longitude`), `Contract_ID` (FK), `Status`, `Banking`, `Bonus`, `Bike Stands`, `Available Bike Stands`, `Available Bikes`, `Last Update`.
* **Dimension Table (`Contract_Table` - 25 rows):** Contains contract lookups: `Contract_ID` (PK), `Contract Name`.

---

## 📐 Key DAX Measures

The KPI summary cards and dashboard metrics are powered by the following custom DAX measures:

```dax
Total_Bike_Stations = COUNTROWS(Fact_Table)

Total_Bike_Stands = SUM(Fact_Table[Bike Stands])

Total_Bike = SUM(Fact_Table[Available Bikes])

Open_Stations = CALCULATE(COUNTROWS(Fact_Table), Fact_Table[status] = "OPEN")

Closed_Stations = CALCULATE(COUNTROWS(Fact_Table), Fact_Table[status] = "CLOSED")
```

---

## 🔍 Key Insights (4-Stage Analytical Framework)

### 1. Descriptive Analysis (*What happened?*)
* **Network Scale:** Analyzed **2,856 stations** across **25 contracts** with **57,203 bike stands** and **19,646 available bikes** (overall bike availability rate of **34.34%**).
* **Operational Availability:** **92.79%** of stations (**2,650**) are actively `OPEN`, while **7.21%** (**206 stations**) are `CLOSED`.
* **Top Hubs:** **Lyon** (448 stations), **Toulouse** (431 stations), and **Bruxelles-Capitale** (357 stations) account for **43.3%** of all stations.

### 2. Diagnostic Analysis (*Why did it happen?*)
* **Station Closures:** Contracts like **Vilnius** and **Cergy-Pontoise** suffer from high proportions of closed stations due to seasonal shutdowns or maintenance.
* **Disabled Docks:** **5,439 bike stands** are unaccounted for or disabled across the network—heavily concentrated in **Marseille** (1,173 disabled stands / 62.49% of capacity).
* **Stock Imbalances:** Open stations experience localized friction, with hundreds of stations sitting completely empty (0 bikes) or fully congested (0 empty docks).

### 3. Predictive Analysis (*What is likely to happen?*)
* Without proactive repairs, contracts with high disabled-stand ratios will continue to experience low rider utilization and declining commuter trust.
* High-traffic hubs will face recurring peak-hour shortages at empty stations unless fleet redistribution schedules are introduced.

### 4. Prescriptive Analysis (*What actions should be taken?*)
* **Prioritize Maintenance:** Dispatch field technicians to audit and repair locked or disabled stands in **Marseille** and **Lyon**.
* **Dynamic Van Rebalancing:** Run delivery trucks in the morning and evening to move bikes around—bringing bikes to empty stations and picking bikes up from full stations.
* **Expand Accessibility:** Increase card payment terminal coverage and expand bonus return credits to encourage riders to self-balance uphill or empty docks.
