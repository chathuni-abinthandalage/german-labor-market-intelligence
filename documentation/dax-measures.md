# DAX Measures

This document contains the 16 core DAX measures used in the **Germany Labour Market Intelligence** Power BI dashboard.

All measures are centralized in the `_Measures` table and organized into four display folders:

- National Vacancies
- National Unemployment
- Regional Vacancies
- Regional Unemployment

---

## 1. National Vacancies

### 1.1 Registered Vacancies

```DAX
Registered Vacancies =
SUM ( Fact_Vacancies_DE[Bestand Arbeitsstellen] )
```

Returns the total number of registered vacancies for the selected national reporting period.

### 1.2 Latest Registered Vacancies

```DAX
Latest Registered Vacancies =
VAR LatestDate =
    MAX ( Fact_Vacancies_DE[Date] )
RETURN
    CALCULATE (
        SUM ( Fact_Vacancies_DE[Bestand Arbeitsstellen] ),
        Fact_Vacancies_DE[Date] = LatestDate
    )
```

Returns the number of registered vacancies for the latest available reporting month in the national BA vacancy dataset.

### 1.3 YoY Vacancy Change %

```DAX
YoY Vacancy Change % =
VAR CurrentVacancies =
    [Registered Vacancies]
VAR PreviousYearVacancies =
    CALCULATE (
        [Registered Vacancies],
        DATEADD ( DimDate[Date], -1, YEAR )
    )
RETURN
    DIVIDE (
        CurrentVacancies - PreviousYearVacancies,
        PreviousYearVacancies
    )
```

Calculates the year-over-year percentage change in national registered vacancies compared with the same reporting period one year earlier.

### 1.4 Latest YoY Vacancy Change %

```DAX
Latest YoY Vacancy Change % =
VAR LatestDate =
    CALCULATE (
        MAX ( Fact_Vacancies_DE[Date] ),
        ALL ( Fact_Vacancies_DE[Date] )
    )
RETURN
    CALCULATE (
        [YoY Vacancy Change %],
        REMOVEFILTERS ( Fact_Vacancies_DE[Date] ),
        DimDate[Date] = LatestDate
    )
```

Returns the year-over-year percentage change in registered vacancies for the latest available national reporting month.

---

## 2. National Unemployment

### 2.1 Unemployed Persons

```DAX
Unemployed Persons =
SUM ( Fact_Unemployment_DE[Arbeitslose] )
```

Returns the total number of unemployed persons for the selected national reporting period.

### 2.2 Latest Unemployed Persons

```DAX
Latest Unemployed Persons =
VAR LatestDate =
    MAX ( Fact_Unemployment_DE[Date] )
RETURN
    CALCULATE (
        [Unemployed Persons],
        Fact_Unemployment_DE[Date] = LatestDate
    )
```

Returns the number of unemployed persons for the latest available reporting month in the national BA unemployment dataset.

### 2.3 YoY Unemployment Change %

```DAX
YoY Unemployment Change % =
VAR CurrentUnemployment =
    [Unemployed Persons]
VAR PreviousYearUnemployment =
    CALCULATE (
        [Unemployed Persons],
        DATEADD ( DimDate[Date], -1, YEAR )
    )
RETURN
    DIVIDE (
        CurrentUnemployment - PreviousYearUnemployment,
        PreviousYearUnemployment
    )
```

Calculates the year-over-year percentage change in national unemployment compared with the same reporting period one year earlier.

### 2.4 Latest YoY Unemployment Change %

```DAX
Latest YoY Unemployment Change % =
VAR LatestDate =
    CALCULATE (
        MAX ( Fact_Unemployment_DE[Date] ),
        ALL ( Fact_Unemployment_DE[Date] )
    )
RETURN
    CALCULATE (
        [YoY Unemployment Change %],
        REMOVEFILTERS ( Fact_Unemployment_DE[Date] ),
        DimDate[Date] = LatestDate
    )
```

Returns the year-over-year percentage change in unemployment for the latest available national reporting month.

---

## 3. Regional Vacancies

### 3.1 Regional Registered Vacancies

```DAX
Regional Registered Vacancies =
SUM ( Fact_Vacancies_Regions[Bestand Arbeitsstellen] )
```

Returns the number of registered vacancies for the selected Bundesland and reporting period.

### 3.2 Latest Regional Registered Vacancies

```DAX
Latest Regional Registered Vacancies =
VAR LatestDate =
    CALCULATE (
        MAX ( Fact_Vacancies_Regions[Date] ),
        ALL ( Fact_Vacancies_Regions[Date] )
    )
RETURN
    CALCULATE (
        [Regional Registered Vacancies],
        REMOVEFILTERS ( Fact_Vacancies_Regions[Date] ),
        DimDate[Date] = LatestDate
    )
```

Returns registered vacancies for the latest available reporting month for the selected Bundesland.

### 3.3 Regional YoY Vacancy Change %

```DAX
Regional YoY Vacancy Change % =
VAR CurrentVacancies =
    [Regional Registered Vacancies]
VAR PreviousYearVacancies =
    CALCULATE (
        [Regional Registered Vacancies],
        DATEADD ( DimDate[Date], -1, YEAR )
    )
RETURN
    DIVIDE (
        CurrentVacancies - PreviousYearVacancies,
        PreviousYearVacancies
    )
```

Calculates the year-over-year percentage change in registered vacancies for each Bundesland compared with the same reporting period one year earlier.

### 3.4 Latest Regional YoY Vacancy Change %

```DAX
Latest Regional YoY Vacancy Change % =
VAR LatestDate =
    CALCULATE (
        MAX ( Fact_Vacancies_Regions[Date] ),
        ALL ( Fact_Vacancies_Regions[Date] )
    )
RETURN
    CALCULATE (
        [Regional YoY Vacancy Change %],
        REMOVEFILTERS ( Fact_Vacancies_Regions[Date] ),
        DimDate[Date] = LatestDate
    )
```

Returns the year-over-year percentage change in registered vacancies for the latest available reporting month for each Bundesland.

---

## 4. Regional Unemployment

### 4.1 Regional Unemployed Persons

```DAX
Regional Unemployed Persons =
SUM ( Fact_Unemployment_Regions[Arbeitslose] )
```

Returns the number of unemployed persons for the selected Bundesland and reporting period.

### 4.2 Latest Regional Unemployed Persons

```DAX
Latest Regional Unemployed Persons =
VAR LatestDate =
    CALCULATE (
        MAX ( Fact_Unemployment_Regions[Date] ),
        ALL ( Fact_Unemployment_Regions[Date] )
    )
RETURN
    CALCULATE (
        [Regional Unemployed Persons],
        REMOVEFILTERS ( Fact_Unemployment_Regions[Date] ),
        DimDate[Date] = LatestDate
    )
```

Returns unemployed persons for the latest available reporting month for the selected Bundesland.

### 4.3 Regional YoY Unemployment Change %

```DAX
Regional YoY Unemployment Change % =
VAR CurrentUnemployment =
    [Regional Unemployed Persons]
VAR PreviousYearUnemployment =
    CALCULATE (
        [Regional Unemployed Persons],
        DATEADD ( DimDate[Date], -1, YEAR )
    )
RETURN
    DIVIDE (
        CurrentUnemployment - PreviousYearUnemployment,
        PreviousYearUnemployment
    )
```

Calculates the year-over-year percentage change in unemployment for each Bundesland compared with the same reporting period one year earlier.

### 4.4 Latest Regional YoY Unemployment Change %

```DAX
Latest Regional YoY Unemployment Change % =
VAR LatestDate =
    CALCULATE (
        MAX ( Fact_Unemployment_Regions[Date] ),
        ALL ( Fact_Unemployment_Regions[Date] )
    )
RETURN
    CALCULATE (
        [Regional YoY Unemployment Change %],
        REMOVEFILTERS ( Fact_Unemployment_Regions[Date] ),
        DimDate[Date] = LatestDate
    )
```

Returns the year-over-year percentage change in unemployment for the latest available reporting month for each Bundesland.

---

## Key DAX Concepts Demonstrated

The DAX layer demonstrates several core Power BI modelling and analytical concepts:

- `SUM` for aggregation of labour-market indicators
- `CALCULATE` for modifying filter context
- `VAR` for readable calculation logic
- `DATEADD` for year-over-year time intelligence
- `DIVIDE` for safe percentage calculations
- `ALL` for identifying the latest available reporting period
- `REMOVEFILTERS` for controlling date filter context
- A dedicated `DimDate` table for time intelligence
- `DimRegion`-driven regional filtering
- Dynamic latest-period measures rather than hard-coded reporting dates

## Measure Organization

The 16 measures are centralized in the disconnected `_Measures` table and grouped into four Power BI display folders:

| Display Folder | Measures |
|---|---:|
| National Vacancies | 4 |
| National Unemployment | 4 |
| Regional Vacancies | 4 |
| Regional Unemployment | 4 |
| **Total** | **16** |

This organization keeps the semantic model easier to navigate while separating analytical measures from the underlying fact and dimension tables.
