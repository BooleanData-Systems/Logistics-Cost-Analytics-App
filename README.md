# Logistics Cost Analytics Accelerator

AI-powered logistics cost intelligence platform built as a Snowflake Native App with Cortex AI.

## Overview

End-to-end logistics cost visibility: Cost Per Shipment, Cost Per Mile, Fuel Efficiency, Margin Per Route, Cost Variance, and monthly trends — all powered by Snowflake Cortex AI for RAG-based Q&A.

### Key Features

- **8 Analytics Tabs** — Cost Overview, Cost Breakdown, Route Analysis, Fuel & Surcharges, Cost Variance, Optimization, Trends, AI Predictions
- **20+ KPIs** — Total logistics cost, cost per shipment, cost per mile, fuel efficiency, margin per route, OTIF rate, cost efficiency score
- **Dynamic Filters** — Carrier, transport mode, year, month — all cross-tab
- **AI Chat** — Ask questions about your logistics cost data using Snowflake Cortex (mistral-large2)
- **Cost Variance** — Actual vs budget analysis with over/under budget detection
- **Optimization Scoring** — Route and carrier cost efficiency ranking
- **Tier Classification** — EXCELLENT / GOOD / AVERAGE / NEEDS IMPROVEMENT across all KPIs

## Setup

### Required Table References

After installing the app, bind 16 tables/views from your data layer via the Snowsight Permissions tab.

#### 1. Cost By Carrier (Required)

| Column | Type | Description |
|--------|------|-------------|
| CARRIER_ID | VARCHAR | Carrier identifier |
| CARRIER_NAME | VARCHAR | Carrier display name |
| TRANSPORT_MODE | VARCHAR | ROAD / RAIL / AIR / SEA / INTERMODAL |
| SHIPMENT_COUNT | INT | Number of shipments |
| TOTAL_COST | FLOAT | Total logistics cost |
| AVG_COST | FLOAT | Average cost per shipment |
| AVG_COST_PER_MILE | FLOAT | Average cost per mile |
| AVG_MARGIN_PERCENT | FLOAT | Average margin percentage |

#### 2. Cost By Mode (Required)

| Column | Type | Description |
|--------|------|-------------|
| TRANSPORT_MODE | VARCHAR | Transport mode |
| SHIPMENT_COUNT | INT | Number of shipments |
| TOTAL_FREIGHT | FLOAT | Total freight cost |
| TOTAL_FUEL_SURCHARGE | FLOAT | Total fuel surcharge |
| TOTAL_ACCESSORIAL | FLOAT | Total accessorial charges |
| TOTAL_WAREHOUSING | FLOAT | Total warehousing cost |
| TOTAL_LAST_MILE | FLOAT | Total last-mile cost |
| TOTAL_COST | FLOAT | Sum of all cost components |

#### 3. Cost By Route (Required)

| Column | Type | Description |
|--------|------|-------------|
| ORIGIN_REGION | VARCHAR | Origin region |
| DESTINATION_REGION | VARCHAR | Destination region |
| TRANSPORT_MODE | VARCHAR | Transport mode |
| SHIPMENT_COUNT | INT | Number of shipments |
| TOTAL_COST | FLOAT | Total cost for route |
| AVG_COST | FLOAT | Average cost |
| AVG_COST_PER_MILE | FLOAT | Cost per mile |
| AVG_TRANSIT_DAYS | FLOAT | Average transit days |
| AVG_MARGIN_PERCENT | FLOAT | Route margin % |

#### 4. Margin Per Route (Required)

| Column | Type | Description |
|--------|------|-------------|
| ORIGIN_REGION | VARCHAR | Origin region |
| DESTINATION_REGION | VARCHAR | Destination region |
| CARRIER_NAME | VARCHAR | Carrier name |
| SHIPMENT_COUNT | INT | Shipment count |
| TOTAL_REVENUE | FLOAT | Total revenue |
| TOTAL_COST | FLOAT | Total cost |
| AVG_MARGIN_PERCENT | FLOAT | Average margin % |
| MARGIN_TIER | VARCHAR | HIGH_MARGIN / MODERATE_MARGIN / LOW_MARGIN / LOSS_MAKING |

#### 5. Cost Per Mile (Required)

| Column | Type | Description |
|--------|------|-------------|
| CARRIER_NAME | VARCHAR | Carrier name |
| TRANSPORT_MODE | VARCHAR | Transport mode |
| SHIPMENT_COUNT | INT | Shipments |
| TOTAL_MILES | FLOAT | Total miles |
| AVG_COST_PER_MILE | FLOAT | Cost per mile |

#### 6. Fuel Efficiency (Required)

| Column | Type | Description |
|--------|------|-------------|
| CARRIER_NAME | VARCHAR | Carrier name |
| TRANSPORT_MODE | VARCHAR | Transport mode |
| AVG_FUEL_EFFICIENCY_PERCENT | FLOAT | Fuel efficiency % |
| MILES_PER_LITRE | FLOAT | Miles per litre |
| TOTAL_FUEL_LITRES | FLOAT | Total fuel consumed |
| FUEL_COST_SHARE_PERCENT | FLOAT | Fuel as % of total cost |

#### 7. Fuel Surcharge Trend (Required)

| Column | Type | Description |
|--------|------|-------------|
| MONTH | DATE | Month |
| SHIPMENT_COUNT | INT | Shipments |
| TOTAL_FUEL_SURCHARGE | FLOAT | Total fuel surcharge |
| AVG_FUEL_SURCHARGE | FLOAT | Avg surcharge per shipment |
| FUEL_SURCHARGE_SHARE_PERCENT | FLOAT | Share of total cost |

#### 8. Cost Variance (Required)

| Column | Type | Description |
|--------|------|-------------|
| CARRIER_NAME | VARCHAR | Carrier name |
| TRANSPORT_MODE | VARCHAR | Transport mode |
| ACTUAL_COST | FLOAT | Actual cost |
| BUDGET_COST | FLOAT | Budget cost |
| COST_VARIANCE | FLOAT | Actual - Budget |
| VARIANCE_PERCENT | FLOAT | Variance % |
| VARIANCE_STATUS | VARCHAR | OVER_BUDGET / WITHIN_TOLERANCE / UNDER_BUDGET |

#### 9. Cost Optimization (Required)

| Column | Type | Description |
|--------|------|-------------|
| CARRIER_NAME | VARCHAR | Carrier name |
| ORIGIN_REGION | VARCHAR | Origin |
| DESTINATION_REGION | VARCHAR | Destination |
| SHIPMENT_COUNT | INT | Shipments |
| AVG_COST_PER_MILE | FLOAT | Cost per mile |
| AVG_MARGIN_PERCENT | FLOAT | Margin % |
| OTIF_RATE | FLOAT | On-time in-full rate |
| COST_EFFICIENCY_SCORE | FLOAT | Composite efficiency score |

#### 10. Cost Per KG (Required)

| Column | Type | Description |
|--------|------|-------------|
| CARRIER_NAME | VARCHAR | Carrier name |
| TRANSPORT_MODE | VARCHAR | Transport mode |
| COST_PER_KG | FLOAT | Cost per kilogram |

#### 11. Total Logistics Cost (Required)

| Column | Type | Description |
|--------|------|-------------|
| MONTH | DATE | Month |
| TOTAL_FREIGHT_COST | FLOAT | Freight cost |
| TOTAL_FUEL_SURCHARGE | FLOAT | Fuel surcharge |
| TOTAL_ACCESSORIAL | FLOAT | Accessorial |
| TOTAL_WAREHOUSING_COST | FLOAT | Warehousing |
| TOTAL_LAST_MILE_COST | FLOAT | Last mile |
| TOTAL_LOGISTICS_COST | FLOAT | Sum of all |
| TOTAL_SHIPMENTS | INT | Shipment count |

#### 12. Cost Per Shipment (Required)

| Column | Type | Description |
|--------|------|-------------|
| CARRIER_NAME | VARCHAR | Carrier name |
| SERVICE_TYPE | VARCHAR | STANDARD / EXPRESS / PREMIUM / ECONOMY |
| SHIPMENT_COUNT | INT | Shipments |
| TOTAL_COST | FLOAT | Total cost |
| AVG_COST_PER_SHIPMENT | FLOAT | Cost per shipment |

#### 13. Pipeline Status (Required)

| Column | Type | Description |
|--------|------|-------------|
| (varies) | — | Pipeline configuration status columns |

#### 14. Shipment Cost Trends (Required)

| Column | Type | Description |
|--------|------|-------------|
| MONTH | DATE | Month |
| CARRIER_NAME | VARCHAR | Carrier |
| TRANSPORT_MODE | VARCHAR | Mode |
| SHIPMENT_COUNT | INT | Count |
| TOTAL_COST | FLOAT | Total cost |

#### 15. Fact Shipments (Required)

| Column | Type | Description |
|--------|------|-------------|
| SHIP_DATE | DATE | Shipment date |
| CARRIER_ID | VARCHAR | Carrier ID |
| ORIGIN_REGION | VARCHAR | Origin |
| DESTINATION_REGION | VARCHAR | Destination |
| TRANSPORT_MODE | VARCHAR | Mode |
| TOTAL_COST | FLOAT | Total cost |
| COST_PER_MILE | FLOAT | Cost per mile |
| REVENUE | FLOAT | Revenue |
| MARGIN_PERCENT | FLOAT | Margin % |
| FUEL_EFFICIENCY_PERCENT | FLOAT | Fuel efficiency |

#### 16. Carrier Dimension (Required)

| Column | Type | Description |
|--------|------|-------------|
| CARRIER_ID | VARCHAR | Carrier identifier |
| CARRIER_NAME | VARCHAR | Carrier name |
| IS_CURRENT | BOOLEAN | SCD2 current flag |

### Enable Cortex AI (Required)

Grant the Cortex user role via the **Permissions** tab in Snowsight.

## Privileges

| Privilege | Required | Purpose |
|-----------|----------|---------|
| Cortex User Role | Yes | Granted via Permissions tab for AI Q&A |

## AI Features

All AI features use **Snowflake Cortex** (`COMPLETE` with `mistral-large2`). RAG-powered Q&A is grounded entirely in your logistics cost data. No external data is accessed.

## Data Privacy

- Data never leaves your account
- No data is copied, stored, or transmitted externally
- AI processing occurs entirely within Snowflake Cortex
- Only SELECT privileges requested

## Application Architecture

The installed application contains 3 schemas:

| Schema | Purpose |
|--------|---------|
| APP_DATA | Reference-backed views over your bound tables |
| APP_SCHEMA | Streamlit dashboard |
| CONFIG | Reference callback and view setup |

The consumer is responsible for providing the 16 pre-aggregated KPI tables/views listed above. The application provides visualization, filtering, tier classification, and AI-powered Q&A over that data.

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 6.0.0 | 2026-04 | Initial Marketplace release — 8 tabs, 20+ KPIs, Cortex AI |
