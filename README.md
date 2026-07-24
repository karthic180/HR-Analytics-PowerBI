# IBM HR Analytics Employee Attrition - Power BI

**GitHub Repository:**  
https://github.com/karthic180/HR-Analytics-PowerBI

---

## Project Overview

This repository contains a professional, enterprise-grade Power BI dashboard analysing employee attrition using the IBM HR Analytics Employee Attrition & Performance dataset. The project demonstrates enterprise-level data modelling, advanced DAX calculations, Power Query ETL processes and executive dashboard design. It showcases how business intelligence can transform HR data into actionable insights for workforce planning and strategic decision-making.

---

## Dataset Source

- **Dataset Name:** IBM HR Analytics Employee Attrition & Performance
- **Source:** https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset

---

## Tools & Technologies

- **Power BI Desktop** – Dashboard development and interactive reporting
- **Power Query** – Data cleansing, transformation and ETL
- **DAX (Data Analysis Expressions)** – Measures, calculated columns and calculated tables
- **Star Schema Data Model** – Optimised relationships for analytical performance
- **GitHub** – Version control and project documentation

---

## Key Features

- Executive KPI Dashboard
- Employee Attrition Analysis
- Department & Job Role Performance
- Interactive Slicers & Drill-down Analysis
- Dynamic DAX Measures
- Conditional Formatting
- Business-focused Dashboard Design
- Executive Reporting

---

## Business Questions Answered

This dashboard enables HR managers, executives and business leaders to answer key workforce questions.

### 1. What is the current employee attrition rate?

**Answer**

The dashboard calculates the overall employee attrition rate by comparing employees who left against the total workforce, providing a clear measure of organisational retention.

---

### 2. Which departments experience the highest employee turnover?

**Answer**

Department-level analysis identifies areas with the greatest employee turnover, allowing HR teams to prioritise retention initiatives.

---

### 3. Which job roles are at the highest risk of attrition?

**Answer**

The report highlights high-risk job roles, enabling management to investigate possible causes such as workload, compensation or career progression.

---

### 4. Which age groups leave the organisation most frequently?

**Answer**

Demographic analysis identifies age groups with the highest employee turnover to support workforce planning.

---

### 5. Does gender influence employee attrition?

**Answer**

Interactive comparisons allow HR teams to identify potential workforce diversity and equality trends.

---

### 6. How does salary relate to employee attrition?

**Answer**

Monthly income analysis helps determine whether employees in lower salary bands have higher attrition rates.

---

### 7. Are newer employees more likely to leave?

**Answer**

Tenure analysis identifies whether recently hired employees represent the greatest retention risk.

---

### 8. Which education fields have the highest attrition?

**Answer**

The dashboard compares turnover across educational backgrounds to identify workforce trends.

---

### 9. Does business travel affect employee retention?

**Answer**

Business travel frequency is analysed to determine whether travel requirements contribute to employee turnover.

---

### 10. Which marital status groups have the highest attrition?

**Answer**

The report compares attrition across marital status categories to identify demographic patterns.

---

### 11. What is the average monthly income by department?

**Answer**

Average salary comparisons help management evaluate compensation consistency across departments.

---

### 12. Which employee groups should HR prioritise?

**Answer**

By combining department, job role, age, tenure and salary analysis, the dashboard identifies the highest-risk employee segments for targeted retention strategies.

---

## Business Value

This Power BI solution helps organisations:

- Monitor workforce health through executive KPIs.
- Measure employee attrition across departments.
- Identify high-risk job roles.
- Improve employee retention strategies.
- Reduce recruitment and onboarding costs.
- Support evidence-based HR decision making.
- Analyse workforce demographics.
- Deliver interactive executive reporting.
- Enable strategic workforce planning.

---

## Key Performance Indicators (KPIs)

| KPI | Business Purpose |
|------|------------------|
| Total Employees | Current workforce size |
| Employees Left | Total employees who left |
| Active Employees | Current workforce |
| Attrition Rate (%) | Overall employee turnover |
| Average Monthly Income | Average employee salary |
| Average Age | Workforce demographic profile |
| Average Years at Company | Employee tenure |
| Attrition by Department | Department comparison |
| Attrition by Job Role | Role comparison |
| Attrition by Gender | Diversity analysis |
| Attrition by Age Group | Demographic analysis |
| Attrition by Education Field | Education comparison |
| Attrition by Business Travel | Travel analysis |
| Attrition by Marital Status | Demographic comparison |

---

## Skills & Competencies Demonstrated

- Star Schema Data Modelling
- Power Query ETL Pipelines
- Advanced DAX Measures
- Calculated Tables & Columns
- Interactive Dashboard Design
- KPI Development
- HR Analytics
- Business Intelligence Reporting
- Executive Dashboard Design
- Data Visualisation Best Practices

---

## Dashboard Documentation

**Power BI Report (PDF)**

https://github.com/karthic180/HR-Analytics-PowerBI/blob/main/IBM%20HR%20Analytics%20Employee%20Attrition.pdf

---

## Sample DAX Code

```dax
-- =========================================================================
-- CALCULATED TABLES (Data Modelling)
-- =========================================================================

Dim_Date =
ADDCOLUMNS(
    CALENDAR(DATE(2020,1,1), DATE(2025,12,31)),
    "Year", YEAR([Date]),
    "Month Number", MONTH([Date]),
    "Month Name", FORMAT([Date], "MMMM"),
    "Quarter", "Q" & FORMAT([Date], "Q"),
    "Year-Month", FORMAT([Date], "YYYY-MM")
)

Dim_AgeSort =
DATATABLE(
    "Age Group", STRING,
    "Sort Order", INTEGER,
    {
        {"18-25",1},
        {"26-35",2},
        {"36-45",3},
        {"46-55",4},
        {"56+",5}
    }
)

Summary_Dept_Attrition =
SUMMARIZE(
    Fact_Employees,
    Dim_Department[DepartmentName],
    "Total Headcount", COUNTROWS(Fact_Employees),
    "Employees Left",
        CALCULATE(
            COUNTROWS(Fact_Employees),
            Fact_Employees[Attrition] = "Yes"
        ),
    "Average Monthly Income",
        AVERAGE(Fact_Employees[MonthlyIncome])
)

-- =========================================================================
-- CORE MEASURES
-- =========================================================================

Total_Employees =
COUNTROWS(Fact_Employees)

Employees_Left =
CALCULATE(
    [Total_Employees],
    Fact_Employees[Attrition] = "Yes"
)

Attrition_Rate_Pct =
DIVIDE(
    [Employees_Left],
    [Total_Employees],
    0
)

Average_Monthly_Income =
AVERAGE(Fact_Employees[MonthlyIncome])

Highlight_High_Risk_Role =
IF(
    [Attrition_Rate_Pct] > 0.25,
    "#D9534F",
    "#4A90E2"
)
```

## Author

**Karthic Sinnadurai**

- GitHub: https://github.com/karthic180
- LinkedIn: https://linkedin.com/in/karthic2
