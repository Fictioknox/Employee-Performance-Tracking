## Employee Performance Tracking Report

### Overview

This project analyzes the IBM HR Analytics dataset from Kaggle (`pavansubhasht/ibm-hr-analytics-attrition-dataset`), licensed under DbCL-1.0, to track employee performance and predict high performers.<br>
The dataset includes 1,470 records with fields like `EmployeeNumber`, `Department`, `JobRole`, `MonthlyIncome`, `PerformanceRating`, `YearsAtCompany`, `Attrition`, `Age`, `JobSatisfaction`, and `JobLevel`.<br>
The analysis uses SQL for querying, Python for orchestration, Excel for reporting, and Machine Learning for prediction, conducted in Google Colab with SQLite.<br>
The goal was to provide actionable HR insights while showcasing advanced SQL techniques and predictive modeling.

### Dataset Description

The dataset includes:<br>
- **EmployeeNumber**: Unique identifier (PRIMARY KEY).<br>
- **Department**: Employee department (NOT NULL).<br>
- **JobRole**: Specific role.<br>
- **MonthlyIncome**: Monthly salary.<br>
- **PerformanceRating**: Rating (1–4, target for high performer prediction).<br>
- **YearsAtCompany**: Tenure.<br>
- **Attrition**: Whether the employee left (Yes/No).<br>
- **Age**: Employee age.<br>
- **JobSatisfaction**: Satisfaction level (1–4).<br>
- **JobLevel**: Career level.<br>

A SQLite table was created with relational constraints for efficient querying.

### SQL Techniques Demonstrated

The project showcases:<br>
- **Basic SQL Syntax**: `INSERT` (new employee), `UPDATE` (ratings), `DELETE` (low satisfaction), `SELECT`.<br>
- **Basic Queries**: Retrieve (Query 1), Filter (Query 2), Manipulate (Query 3 with window functions).<br>
- **Database Concepts**: `PRIMARY KEY`, `NOT NULL`, grouping.<br>
- **Complex Queries**: Subqueries (Query 2), CTEs (Queries 4, 6), window functions (Queries 3, 6), nested queries (Query 5).<br>
- **Excel Integration**: Exported results to CSV for analysis (e.g., pivot tables, charts).<br>
- **Machine Learning**: Logistic Regression with class balancing.<br>
- **Python + SQL**: Python orchestrates SQL and ML in Google Colab.<br>

### Analysis and Insights

The project executed six SQL queries and a Machine Learning model to predict high performers (PerformanceRating ≥ 4). Results were exported to CSV for Excel analysis.

#### Query 1: Department Performance Stats

**Objective**: Assess department performance and income.<br>
**SQL Techniques**: `SELECT`, `COUNT`, `AVG`, `GROUP BY`, `ORDER BY`.<br>
**Results**:
```
               Department  employee_count  avg_income  avg_performance
0  Research & Development             769     6258.09         3.289987
1                   Sales             361     6945.55         3.288089
2         Human Resources              52     6696.23         3.250000
```
**Insights**:<br>
- R&D has the most employees (769) with an average performance of 3.29.<br>
- Sales has the highest average income (6,945.55) but slightly lower performance (3.29).<br>
- HR has fewer employees (52) and the lowest performance (3.25).<br>
**Business Implication**: Focus performance improvement initiatives on HR.

#### Query 2: Top Performers by Job Role with Subquery

**Objective**: Identify top performers compared to role averages.<br>
**SQL Techniques**: `SELECT`, subquery, `WHERE`, `ORDER BY`, `LIMIT`.<br>
**Results**:
```
                     JobRole  EmployeeNumber  PerformanceRating  MonthlyIncome  avg_role_rating
0  Healthcare Representative              36                  4        10248.0         3.342857
1  Healthcare Representative              83                  4        10096.0         3.342857
2  Healthcare Representative             117                  4         4152.0         3.342857
3  Healthcare Representative             119                  4        13503.0         3.342857
4  Healthcare Representative             165                  4        10312.0         3.342857
```
**Insights**:<br>
- Healthcare Representatives with rating 4 exceed their role average (3.34).<br>
- Income varies widely (4,152 to 13,503) among top performers.<br>
- Indicates strong performers in this role.<br>
**Business Implication**: Recognize high performers in Healthcare roles.

#### Query 3: Attrition Risk by Tenure with Window Function

**Objective**: Analyze attrition rates by tenure.<br>
**SQL Techniques**: `SELECT`, `SUM`, `CASE`, window function (`AVG OVER`), `GROUP BY`, `ORDER BY`, `LIMIT`.<br>
**Results**:
```
   YearsAtCompany  employee_count  attrition_count  attrition_rate  avg_rating
0              40               1                1          100.00         4.0
1              23               2                1           50.00         4.0
2              31               2                1           50.00         4.0
3              32               3                1           33.33         4.0
4               1             140               45           32.14         4.0
```
**Insights**:<br>
- Employees with 40 years tenure have a 100% attrition rate (small sample).<br>
- High attrition (32.14%) in the first year (140 employees, 45 left).<br>
- Performance ratings remain high (4.0) despite attrition.<br>
**Business Implication**: Improve onboarding to reduce first-year attrition.

#### Query 4: Departmental Income Disparity with CTE

**Objective**: Assess income inequality within departments.<br>
**SQL Techniques**: `WITH`, `DENSE_RANK`, `MAX`, `MIN`, `GROUP BY`.<br>
**Results**:
```
               Department  top_income  bottom_income  disparity_percent
0         Human Resources     19717.0         1555.0            1167.97
1  Research & Development     19999.0         1009.0            1882.06
2                   Sales     19847.0         1052.0            1786.60
```
**Insights**:<br>
- R&D has the largest disparity (1,882.06%), with incomes from 1,009 to 19,999.<br>
- HR has the smallest disparity (1,167.97%) but still significant.<br>
- Indicates potential inequity in compensation.<br>
**Business Implication**: Review salary structures for fairness.

#### Query 5: Job Satisfaction Trends by Age Group

**Objective**: Analyze satisfaction and attrition by age group.<br>
**SQL Techniques**: `SELECT`, nested query, `CASE`, `AVG`, `SUM`, `GROUP BY`, `ORDER BY`.<br>
**Results**:
```
  age_group  employee_count  avg_satisfaction  attrition_count
0  Under 30             258              3.16               67
1     30-40             553              3.16               62
2   Over 40             371              3.14               42
```
**Insights**:<br>
- Younger employees (Under 30) have high attrition (67 out of 258) despite good satisfaction (3.16).<br>
- The 30-40 group has the most employees (553) with matching satisfaction (3.16).<br>
- Older employees (Over 40) show slightly lower satisfaction (3.14).<br>
**Business Implication**: Target retention strategies for younger employees.

#### Query 6: Performance Consistency with CTE and Window Function

**Objective**: Evaluate departmental performance consistency.<br>
**SQL Techniques**: `WITH`, `ROW_NUMBER`, `AVG`, `MAX`, `SUM`, `GROUP BY`, `ORDER BY`.<br>
**Results**:
```
               Department  total_employees  avg_rating  top_rating  high_performers
0  Research & Development              769    3.289987           4              223
1                   Sales              361    3.288089           4              104
2         Human Resources               52    3.250000           4               13
```
**Insights**:<br>
- R&D has 223 high performers (rating ≥ 4) out of 769 employees.<br>
- Sales follows with 104 high performers out of 361.<br>
- HR has the lowest average rating (3.25) with 13 high performers.<br>
**Business Implication**: Leverage high performers in R&D for mentorship programs.

#### Machine Learning: High Performer Prediction

**Objective**: Predict employees with PerformanceRating ≥ 4 using Logistic Regression.<br>
**Features**: `MonthlyIncome`, `YearsAtCompany`, `Age`, `JobSatisfaction`, `JobLevel`.<br>
**Results**:
```
Accuracy: 0.78
                    precision    recall  f1-score   support
Not High Performer       0.88      0.80      0.84       168
    High Performer       0.60      0.74      0.66        69
          accuracy                           0.78       237
         macro avg       0.74      0.77      0.75       237
      weighted avg       0.80      0.78      0.79       237
```
**Insights**:<br>
- Accuracy of 0.78 indicates strong predictive power.<br>
- High performers (69 instances) have a recall of 0.74 and precision of 0.60, showing good identification with some false positives.<br>
- Not high performers (168 instances) are well-predicted (recall 0.80, precision 0.88).<br>
**Business Implication**: Use predictions to target high-potential employees for development programs.


### Conclusion

This Employee Performance Tracking project offers actionable insights:<br>
- **Retention**: High first-year attrition (32.14%) requires better onboarding.<br>
- **Compensation**: Address income disparities (e.g., R&D’s 1,882.06%).<br>
- **Performance**: Leverage R&D’s 223 high performers for training.<br>
- **Prediction**: Identify high performers (78% accuracy) for targeted development.<br>

Future improvements:<br>
- Enhance ML with features like `WorkLifeBalance` or `OverTime`.<br>
- Use Excel for visualizations (e.g., performance trends by department).<br>
- Explore ensemble models for better prediction accuracy.<br>

This project demonstrates advanced SQL, Python, and ML skills, making it a standout portfolio piece.
