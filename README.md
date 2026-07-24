# IBM HR Analytics Employee Attrition - Power BI

**GitHub Repository:**  
https://github.com/karthic180/HR-Analytics-PowerBI

## Project Overview
This Power BI dashboard anaylse employee attrition using the IBM HR Analytics dataset. It demonstrates data transformation, DAX calculations, interactive reporting, and dashboard design.

## Dataset
IBM HR Analytics Employee Attrition & Performance ([Kaggle](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-Dataset))

## Tools
- Power BI Desktop
- Power Query
- DAX
- GitHub

## Features
- Executive Summary Dashboard
- Attrition Analysis Dashboard
- Interactive Slicers
- KPI Cards
- DAX Measures
- Employee Insights

## Skills Demonstrated
- Data Cleaning
- Data Modeling
- DAX
- Report Design
- Interactive Visualizations

## Dataset

IBM HR Analytics Employee Attrition & Performance

Kaggle:
https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset

## DAX

-- =========================================================================
-- CALCULATED TABLES (Data Modeling & Dimensions)
-- =========================================================================

-- 1. Date Dimension Table (Ensuring a contiguous range for Time Intelligence)
Dim_Date = 
ADDCOLUMNS(
    CALENDAR(DATE(2020, 1, 1), DATE(2025, 12, 31)),
    "Year", YEAR([Date]),
    "Month Number", MONTH([Date]),
    "Month Name", FORMAT([Date], "MMMM"),
    "Quarter", "Q" & FORMAT([Date], "Q"),
    "Year-Month", FORMAT([Date], "YYYY-MM")
)

-- 2. Age Group Sort Table (To enforce correct demographic sequencing)
Dim_AgeSort = 
DATATABLE(
    "Age Group", STRING,
    "Sort Order", INTEGER,
    {
        {"18-25", 1},
        {"26-35", 2},
        {"36-45", 3},
        {"46-55", 4},
        {"56+", 5}
    }
)

-- 3. Calculated Summary Table: Departmental Attrition Performance
Summary_Dept_Attrition = 
SUMMARIZE(
    Fact_Employees,
    Dim_Department[DepartmentName],
    "Total Headcount", COUNTROWS(Fact_Employees),
    "Employees Left", CALCULATE(COUNTROWS(Fact_Employees), Fact_Employees[Attrition] = "Yes"),
    "Average Monthly Income", AVERAGE(Fact_Employees[MonthlyIncome])
)

-- 4. Calculated Table: Employee Risk Tier Segmentation
Dim_Employee_Risk_Tier = 
ADDCOLUMNS(
    VALUES(Fact_Employees[EmployeeID]),
    "Income Bracket", 
    IF(
        RELATED(Fact_Employees[MonthlyIncome]) >= 8000, 
        "High Earner", 
        "Standard Earner"
    ),
    "Tenure Category",
    IF(
        RELATED(Fact_Employees[YearsAtCompany]) <= 2, 
        "Early Tenure Risk", 
        "Established"
    )
)


-- =========================================================================
-- CORE ENTERPRISE CALCULATED MEASURES
-- =========================================================================

-- Total Employee Headcount
Total_Employees = COUNTROWS(Fact_Employees)

-- Total Employees Left (Absolute Volume)
Employees_Left = 
CALCULATE(
    [Total_Employees],
    Fact_Employees[Attrition] = "Yes"
)

-- Overall Attrition Rate (%)
Attrition_Rate_Pct = 
DIVIDE(
    [Employees_Left],
    [Total_Employees],
    0
)

-- Previous Year Attrition Rate (Time Intelligence)
Attrition_Rate_PY = 
CALCULATE(
    [Attrition_Rate_Pct],
    SAMEPERIODLASTYEAR('Dim_Date'[Date])
)

-- Year-over-Year (YoY) Attrition Rate Change
Attrition_Rate_YoY_Diff = 
[Attrition_Rate_Pct] - [Attrition_Rate_PY]

-- Average Monthly Income Across Active Population
Average_Monthly_Income = AVERAGE(Fact_Employees[MonthlyIncome])

-- Dynamic High-Risk Role Highlighter (For Conditional Formatting/Colors)
Highlight_High_Risk_Role = 
IF(
    [Attrition_Rate_Pct] > 0.25, 
    "#D9534F", -- Accent color for high risk (e.g., Sales Representatives ~40%)
    "#4A90E2"  -- Standard corporate neutral blue
)
