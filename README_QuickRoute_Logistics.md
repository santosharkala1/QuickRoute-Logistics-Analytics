# QuickRoute Logistics — Last-Mile Delivery Operations Analytics

**End-to-end Power BI portfolio project | Data cleaning · Data modeling · DAX · Interactive reporting**

## Project overview

QuickRoute Logistics is a fictional last-mile delivery company. This Power BI project analyzes order demand, customer ordering behavior, delivery reliability, driver and vehicle workloads, and operational delivery problems. The objective is to help operations managers understand performance and prioritize improvements.

**Analysis period:** January 2022–December 2024 (based on order dates)  
**Tools:** Power BI Desktop, Power Query, DAX  
**Data:** Six related CSV tables—`customers`, `orders`, `deliveries`, `drivers`, `vehicles`, and `date_table`.

## Business objectives

1. **Delivery demand:** Understand changes in order volume, service preferences, priority levels, and delivery-zone demand.
2. **Customer behavior:** Compare customer segments, repeat ordering, order value, and geographic activity.
3. **Delivery performance:** Monitor delivery outcomes, record-level success, order fulfillment, duration, and zone performance.
4. **Driver and vehicle performance:** Compare delivery workload, outcomes, driver ratings, and vehicle-type performance.
5. **Delivery problems:** Investigate failures, rescheduling, repeated records, explicit attempt numbers, and concentration of problems by zone or service type.
6. **Executive summary:** Present headline KPIs, key findings, and recommended operational actions.

## Data preparation and modeling

- Inspected the CSV tables in **Power Query** for data types, missing values, duplicates, and inconsistent text categories.
- Standardized categorical values, including customer city names, and addressed duplicate customer IDs during cleaning.
- Preserved missing actual-delivery dates for records not marked Delivered rather than fabricating delivery dates.
- Built a star-like relational model with one-to-many relationships:
  - `customers[customer_id]` → `orders[customer_id]`
  - `date_table[Date]` → `orders[order_date]`
  - `orders[order_id]` → `deliveries[order_id]`
  - `drivers[driver_id]` → `deliveries[driver_id]`
  - `vehicles[vehicle_id]` → `deliveries[vehicle_id]`
- Created DAX measures for demand, revenue, customer activity, delivery outcomes, operational problems, and resource workload.

> **Date-modeling note:** Time trends involving delivery records are grouped by **order date** through the active Date Table relationship; they are not automatically trends by actual delivery-completion date.

## Executive KPIs

| Metric | Full-dataset value |
|---|---:|
| Total orders | 3,000 |
| Total order value | 7,522,414.78 (source currency unspecified) |
| Active customers | 400 |
| Delivery records | 3,500 |
| Delivered records | 2,581 |
| Delivery record success rate | 73.74% |
| Orders with at least one Delivered record / all orders | 57.77% |
| Failed delivery records | 295 |
| Rescheduled delivery records | 318 |
| Failed-or-rescheduled record rate | 17.51% |
| Average duration for Delivered records | 46.92 minutes |
| Drivers with delivery records | 75 |
| Vehicles used | 50 |

**Metric distinction:** Delivery record success rate uses *delivery records* as the denominator. Order fulfillment rate uses *unique orders* as the denominator. They measure different outcomes and should not be conflated.

## Key findings

- **Growing demand:** Annual orders increased from **608 in 2022** to **1,163 in 2023** and **1,229 in 2024**.
- **Customer mix:** Individual customers generated **2,050 orders (68.33%)** and Business customers generated **950 (31.67%)**.
- **Delivery reliability:** **2,581 of 3,500** records were marked Delivered; **613** were Failed or Rescheduled combined.
- **Service problem rates:** Economy (**19.48%**) and Same Day (**19.35%**) had the highest observed failed-or-rescheduled record rates among service types.
- **Operational hotspots:** `ZONE0016` had **52** Failed/Rescheduled records, the largest combined count; `ZONE0020` had the lowest observed delivered-record success rate (**68.33%**).
- **Data coverage:** **925 of 3,000 orders** had no matching delivery record in the provided data. This is a tracking or coverage question to investigate, not evidence that those orders failed.
- **Attempt tracking:** **949 orders** had multiple delivery records, while only **165 records** explicitly had `delivery_attempt > 1`; these are distinct measures and suggest a need to examine attempt-recording conventions.

## Report pages

1. **Delivery Demand Analysis** — order volume, order value, service type, priority, zones, and monthly trends.
2. **Customer Behaviour Analysis** — customer types, repeat-order activity, highest-value individual customers, and top cities.
3. **Delivery Performance Analysis** — delivery-status distribution, success rate by zone, durations, and delivery outcomes by order month.
4. **Driver & Vehicle Performance Analysis** — driver workload, success-rate comparisons, vehicle-type workload, and reliability.
5. **Delivery Problems Analysis** — outcomes by attempt number, problem zones, monthly problems, and service-type problem rates.
6. **Executive Summary** — headline KPIs, key business findings, and management priorities.

An internal **KPI Validation** page was also used to check the DAX results, separate from the six exported business-facing pages.

## Recommendations

1. **Improve order-to-delivery tracking:** Investigate orders without matching delivery records and validate delivery-attempt conventions.
2. **Prioritize problem zones:** Review failure and rescheduling drivers in `ZONE0016` and lower-success-rate zones such as `ZONE0020`.
3. **Optimize operations:** Compare driver assignments, vehicle suitability, workloads, and delivery durations before changing route plans or staffing.
4. **Monitor both order- and record-level performance:** Track delivery success and order fulfillment as separate KPIs.

## Repository structure

```text
QuickRoute-Logistics-Analytics/
├── README.md
├── QuickRoute_Logistics_Delivery_Analytics.pbix
├── QuickRoute_Logistics_Delivery_Analytics.pdf
└── Screenshots/
    ├── Power_Query_Applied_Steps.png
    ├── Data_Model_Relationships.png
    └── DAX_Measures_Display_Folders.png
```

Screenshots and report files document the project work. Source CSV files should be shared only if the dataset owner or course terms permit redistribution.

## How to explore

- **Without Power BI:** Open `QuickRoute_Logistics_Delivery_Analytics.pdf` to review the six dashboards.
- **With Power BI Desktop:** Open the `.pbix` file to inspect interactive report pages, the relationships, Power Query transformations, and DAX measures.

## Scope and limitations

- This is a portfolio analysis of a fictional business and the supplied course dataset; findings should not be interpreted as actual QuickRoute Logistics results.
- The data does not establish the root cause of differences by delivery zone, service type, driver, or vehicle. Recommendations are investigative priorities.
- Some names are duplicated across different customer IDs. High-value-customer charts use a unique ID-aware label to avoid aggregating unrelated customers under one name.
- The PDF is a static export; slicer interaction is available in the `.pbix` report.
