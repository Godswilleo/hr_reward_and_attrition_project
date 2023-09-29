# HR REWARD AND ATTRITION
Today is January 2rd 2023. The management of the company is planning to reward staff who have put in lots of efforts to achieve good results for the company as show on their KPI scores.

# Project Overview
The scope of this project is of two dimension. One covers a reward being set up to appreciate staff members for the past year while the other part explores and seeks to establish insights pertaining to employee attrition in the company.

### Data Set
THe data set provided is csv file. This is the csv named **employee_attrition_train.csv**

**Kpi_scores.csv** is then created from the provided csv file using the following sql query.

```-- Selects the database to use
USE hr_attrition;

/* Deletes the table if a table already exists which have same name as
specified for the new table to be created */
DROP 
  TABLE IF EXISTS kpi_scores;

/* Creates a new Table using the fields and records retrieved from a CTE as 
the field and records of the new table */
CREATE TABLE kpi_scores AS(
  WITH CTE_kpi_score AS(
    SELECT 
      staff_id, 
      Quarter1_KPI_Score, 
      Quarter2_KPI_Score, 
      Quarter3_KPI_Score, 
      Quarter4_KPI_Score, 
      ROUND(
        (30 / 100)* quarter1_kpi_score, 
        0
      ) as computed_quarter1_score, 
      ROUND(
        (40 / 100)* quarter2_kpi_score, 
        0
      ) as computed_quarter2_score, 
      ROUND(
        (15 / 100)* quarter3_kpi_score, 
        0
      ) AS computed_quarter3_score, 
      ROUND(
        (15 / 100)* quarter4_kpi_score, 
        0
      ) AS computed_quarter4_score 
    FROM 
      employee_attrition 
    WHERE 
      attrition = 'No'
  ) 
  
  -- Retrieves fields from the CTE and generates a calculated field from the retrieved field
  SELECT 
    *, 
    (
      computed_quarter1_score + computed_quarter2_score + computed_quarter3_score + computed_quarter4_score
    ) AS total_kpi_scores 
  FROM 
    CTE_kpi_score
);
```



## Section 1: REWARD
The reward is basically incentivizing staff based on staff KPI performance for the past year. 

All monetory rewards are expected to commence from January salary. Salaries are normally paid on the 25th day of each month.

In computing the total_kpi_scores for the year, out of total possible score of 100, the weight of each quarter are as follows
quarter1 = 30, quarter2 = 40, quarter3 = 15, quarter4 = 15. This is because due to the nature of the business, quarters 3 and 4 are considered peak periods where business is expected to boom and employees are expected to score well in their KPI, while quarters 1 and 2 are  considered off peak periods when employees are not expected to really do well with their KPI

In light of this management decided to set up the following;

1. Reward those with total annual KPI of 70% and above with a 10% monthly salary increment

2. Reward those with total annual KPI of between 60% to 69% with a 5% monthly salary increment and also an award plaque

3. Give 1% salary increment to people who whose total annual KPI score is below 60%

4. Reward staff with the highest total annual KPI with a one-off bonus of 30% of their monthly salary

5. Reward staff with KPI score of 50% and above in quarter2 with a 10% one-off bonus

6. Payroll needs payment schedule for January salary payments generated with all of the above factored in.

This is to be delivered to HR to help collate deserving staff the the annual awards

## Generating the data for the rewards as required
1. Get the staff with the highest KPI score for the entire year
2. Get all the staff who got up to 50% of their KPI in the off-peak period (Second quarter)
3. Get for each department, the staff with the highest KPI score
4. Get all the staff who scored more than average KPI score for the entire year

## Section 2: Attrition
1. Get attrition rate
2. How effective training has being for the past one year
3. What department is more affected by attrition
4. Get staff that are due for promotion considering the fact that company policy state that every staff should get promotion at least once in three years.
5. Possible causes of attrition


Section 3:
Create a dashboard for monitoring rate of attrition
2. Number of attrition
3. Number of staff due for promotion