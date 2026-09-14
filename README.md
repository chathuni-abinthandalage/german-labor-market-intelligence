# 🇩🇪 German Labour Market Intelligence

An interactive Power BI project analyzing registered vacancies and unemployment trends across Germany and its 16 Bundesländer using official labour-market data from the Federal Employment Agency (Bundesagentur für Arbeit).

## 🎯 Project Objective

The project investigates how Germany's labour market has evolved across time and regions, with a particular focus on:

- Registered vacancies
- Unemployment
- Year-over-year labour-market changes
- Regional differences across all 16 Bundesländer

The goal is to transform official German labour-market time-series data into an interactive decision-support dashboard that enables users to identify national trends, compare regional labour-market conditions, and investigate changing vacancy and unemployment dynamics.

## 📊 Data Source & Scope

**Source:** Statistik der Bundesagentur für Arbeit (Federal Employment Agency)

The project uses official monthly labour-market time-series data retrieved from the Bundesagentur für Arbeit statistical API.

### Data coverage

- **Geography:** Germany and all 16 Bundesländer
- **Frequency:** Monthly
- **Period:** 2007–2026
- **Latest reporting period in the current dashboard:** August 2026
- **Core indicators analyzed:** Registered vacancies and unemployed persons
- **Additional source fields:** Seasonally adjusted series, vacancy inflows, and underemployment

### Terminology

The vacancy indicator is based on the Bundesagentur für Arbeit measure **"gemeldete Arbeitsstellen"**. In this project, it is referred to as **registered vacancies**.

Registered vacancies should not be interpreted as the total number of job openings in Germany, as the statistic covers vacancies registered with the Federal Employment Agency.

### Data acquisition

Data is retrieved directly from the official Bundesagentur für Arbeit API and transformed in Power Query. Separate API requests are used for Germany-wide and Bundesland-level time series, with reusable Power Query functions used to retrieve and combine regional data.

## 🏗️ Technical Architecture

The project follows an end-to-end BI workflow:

**Bundesagentur für Arbeit API → Power Query ETL → Dimensional Data Model → DAX Measures → Power BI Dashboard → Labour Market Insights**

### Data Transformation

Power Query is used to:

- Connect directly to official CSV API endpoints
- Parse and clean API responses
- Remove metadata rows and promote headers
- Convert German reporting-month values into proper date fields
- Apply appropriate numeric data types
- Retrieve data for all 16 Bundesländer using reusable Power Query functions
- Combine regional responses into standardized fact tables

### Data Model

The Power BI model uses a dimensional structure with:

**Dimension tables**
- `DimDate` — central calendar dimension used for time intelligence
- `DimRegion` — contains the 16 Bundesländer

**Fact tables**
- `Fact_Vacancies_DE` — national registered-vacancy time series
- `Fact_Vacancies_Regions` — Bundesland-level registered-vacancy time series
- `Fact_Unemployment_DE` — national unemployment time series
- `Fact_Unemployment_Regions` — Bundesland-level unemployment time series

**Measure table**
- `_Measures` — centralized DAX measures organized into National Vacancies, National Unemployment, Regional Vacancies, and Regional Unemployment folders

Relationships follow a **one-to-many dimension-to-fact structure with single-direction filtering**. The model avoids direct fact-to-fact relationships.

### DAX

The analytical layer includes measures for:

- Registered vacancies
- Unemployed persons
- Latest reporting-period KPIs
- Year-over-year vacancy change
- Year-over-year unemployment change
- National and Bundesland-level calculations

Time-intelligence calculations use the dedicated `DimDate` table rather than embedding reporting periods directly into individual calculations.

## 📈 Dashboard Pages

### 1. Executive Overview

Provides a national view of Germany's labour market, including:

- Latest registered vacancies
- Year-over-year vacancy change
- Latest number of unemployed persons
- Year-over-year unemployment change
- Long-term vacancy and unemployment trends
- Year-over-year growth trends
- Key labour-market insights

### 2. Regional Analysis

Compares labour-market conditions across all 16 Bundesländer through:

- Registered vacancies by Bundesland
- Unemployed persons by Bundesland
- Year-over-year vacancy change
- Year-over-year unemployment change
- Interactive Bundesland filtering
- Regional labour-market dynamics scatter analysis

The scatter analysis compares the latest year-over-year change in registered vacancies with the latest year-over-year change in unemployment for each Bundesland. Reference lines at 0% divide the visualization into four quadrants to support interpretation of differing regional labour-market dynamics.

## 🔎 Key Findings

As of **August 2026**:

- Germany recorded approximately **656,000 registered vacancies**, around **4.0% higher** than in August 2025.
- Approximately **3.06 million people were unemployed**, around **1.2% higher** than in August 2025.
- Registered vacancies remain below the earlier peaks visible in the historical series despite the latest positive year-over-year change.
- Unemployment has risen from its post-pandemic low and is currently around 3 million.
- Regional developments vary substantially across the 16 Bundesländer, demonstrating that national aggregates can mask important differences in local labour-market dynamics.

### Interpretation

The simultaneous year-over-year increase in both registered vacancies and unemployment presents a **mixed labour-market signal**. The dashboard is designed to support further investigation of these developments rather than infer a causal relationship between vacancies and unemployment.

Regional comparisons should also be interpreted carefully: absolute vacancy and unemployment levels are influenced by differences in population and economic size between Bundesländer.

## 🛠️ Tools & Skills Demonstrated

- **Power BI Desktop** — dashboard development and interactive reporting
- **Power Query (M)** — API connection, data cleaning, transformation, reusable functions, and regional data consolidation
- **DAX** — KPI measures, dynamic latest-period calculations, year-over-year analysis, and time intelligence
- **Dimensional Modelling** — date and region dimensions, fact tables, one-to-many relationships, and single-direction filtering
- **Data Visualization** — executive KPI reporting, time-series analysis, regional comparisons, and scatter analysis
- **Economic Analysis** — interpretation of labour-market trends while distinguishing descriptive evidence from causal conclusions
- **Data Documentation** — documented measures, data definitions, methodology, and limitations

## ⚠️ Limitations

- Registered vacancies represent vacancies reported to the Federal Employment Agency and therefore should not be interpreted as all vacancies available in the German economy.
- The current dashboard focuses primarily on registered vacancies and unemployment. Other labour-market dimensions such as employment, wages, industry composition, and occupational shortages are not yet incorporated.
- Current year-over-year comparisons are based on the reported monthly series. Seasonally adjusted source fields are available but are not the primary series used in the current dashboard.
- Absolute comparisons between Bundesländer are influenced by differences in population and economic size.
- The analysis is descriptive. Relationships observed between vacancy and unemployment developments should not be interpreted as causal effects.

## 🚀 Future Development

Potential extensions include:

- Employment indicators
- Industry and occupational analysis
- Wage and earnings data
- Population-adjusted regional indicators
- Python-based preprocessing and validation
- SQL-based analytical data preparation
- Additional economic indicators from official German statistical sources
