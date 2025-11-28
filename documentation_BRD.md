# BUSINESS REQUIREMENTS DOCUMENT (BRD)
## HR Workforce & Attrition Analytics Dashboard

## 1. Purpose
This BRD defines the requirements for developing a Power BI dashboard that analyzes employee attrition, workforce demographics, and risk segments. The goal is to provide HR teams with actionable insights to reduce churn and support data-driven workforce planning.

## 2. Stakeholders
- Executive Sponsor
- HR Manager
- HR Business Partners
- Department Heads
- Data Analyst (Dashboard Developer)

## 3. Business Goals & Objectives
### Goals:
- Improve visibility into workforce health.
- Identify high-risk attrition groups.
- Enable data-driven HR decision making.

### Objectives:
- Deliver accurate attrition KPIs.
- Provide segment-wise attrition breakdown (age, salary, job role).
- Highlight early-tenure employee churn.
- Support retention strategy with evidence.

## 4. Key KPIs
- Total Employees
- Attrition Count
- Attrition Rate
- Average Age
- Average Monthly Salary
- Average Tenure at Company
- Attrition by: Age Group, Salary Slab, Job Role, Education Field, Gender, Tenure

## 5. Data Source
- File: HR_Analytics.csv (CSV)
- Records: ~1400
- Key fields include:
  Age, AgeGroup, Attrition, AttritionCount, Department, Gender, MonthlyIncome, JobRole, Education, EducationField, TotalWorkingYears, YearsAtCompany, YearsInCurrentRole, NumCompaniesWorked.

## 6. Data Preparation (Power Query)
- Removed nulls and duplicates
- Standardized text format
- Validated numeric fields
- Created Age Groups
- Created Salary Slabs
- Applied correct data types

## 7. DAX Measures

Total Employees:
TotalEmployees = COUNTROWS(HR_Analytics)

Attrition Count:
AttritionCount =
CALCULATE(
    COUNTROWS(HR_Analytics),
    HR_Analytics[Attrition] = "Yes"
)

Attrition Rate:
AttritionRate = DIVIDE([AttritionCount], [TotalEmployees])

Average Age:
AvgAge = AVERAGE(HR_Analytics[Age])

Average Salary:
AvgSalary = AVERAGE(HR_Analytics[MonthlyIncome])

Average Tenure:
AvgTenure = AVERAGE(HR_Analytics[YearsAtCompany])

Tenure-Based Attrition:
AttritionByTenure =
CALCULATE(
    [AttritionCount],
    ALLEXCEPT(HR_Analytics, HR_Analytics[YearsAtCompany])
)

## 8. Dashboard Requirements

### Required Visuals:
1. Donut Chart – Attrition by Education Field
2. Bar Chart – Attrition by Age Group
3. Bar Chart – Attrition by Salary Slab
4. Bar Chart – Attrition by Gender
5. Bar Chart – Attrition by Job Role
6. Line Chart – Attrition by Years of Service
7. Area Chart – Education Demographics by Department
8. KPI Cards – Total Employees, Attrition, Attrition Rate, Avg Age, Avg Salary, Avg Tenure

### Interactivity:
- Department slicer: HR, Sales, R&D
- Cross-filtering enabled across visuals
- All visuals must respond to all slicers

### Layout Rules:
- Consistent theme & color palette
- Clear spacing
- HR-friendly readability

## 9. Business Rules
- Attrition counted only when Attrition = "Yes".
- Salary slabs derived from MonthlyIncome.
- Age groups based on standard HR brackets.
- KPIs must be calculated strictly via DAX.
- Dataset refresh must not break dependencies.

## 10. Acceptance Criteria
- All visuals update instantly with filters
- DAX measures return accurate results
- Dashboard identifies top 5 high-risk segments
- No missing or incorrect values post-cleaning
- No visual or refresh errors

## 11. Key Insights (From Dashboard)
- Highest attrition in 26–35 age group
- Salary slab “Up to 5K” shows the most exits
- Maximum churn in early tenure (1–2 years)
- Life Sciences & Medical backgrounds dominate attrition
- High-risk roles: Lab Technician, Sales Executive
- Male attrition count higher than female

## 12. Technologies Used
Power BI, Power Query, DAX, CSV Dataset

# DATA MODEL DOCUMENTATION

## 13. Data Model Purpose
To structure HR employee data in an analysis-ready format enabling accurate KPI calculations, segmentation, and interactive reporting in Power BI.

## 14. Data Model Type
- Single denormalized **flat table (star-like)** model  
- One main fact table: **HR_Analytics**  
- All attributes act as embedded dimensions  

This approach ensures high performance, simplicity, and clean DAX logic.

## 15. Repository Structure
/data/hr_data.csv
/images/dashboard_screenshot.png
/pbix/HR_Insights_And_Trends.pbix
/docs/BRD_HR_Analytics.md

## END OF DOCUMENT
