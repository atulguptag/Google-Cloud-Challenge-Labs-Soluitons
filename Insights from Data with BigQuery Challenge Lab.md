## Lab Name - Insights from Data with BigQuery: Challenge Lab
## Lab Link - https://www.cloudskillsboost.google/focuses/11988?parent=catalog


Open Public Dataset.

In the GCP Console go to `Navigation Menu` > `BigQuery`.

Click on `+ ADD DATA`.

Then Explore `Public Datasets` from the left pane.

Search `covid19_open_data` and Select it.

## Task 1. Total confirmed cases

```cmd
SELECT
  SUM(cumulative_confirmed) AS total_cases_worldwide
FROM
  `bigquery-public-data.covid19_open_data.covid19_open_data`
WHERE
  date = "2020-06-25"
```


## Task 2. Worst affected areas

```cmd
with deaths_by_states as (
SELECT subregion1_name as state, sum(cumulative_deceased) as death_count 
FROM `bigquery-public-data.covid19_open_data.covid19_open_data` where country_name="United States of America" and date='2020-06-25' and subregion1_name is NOT NULL 
group by subregion1_name ) 
select count(*) as count_of_states from deaths_by_states where death_count > Enter_states
```


## Task 3. Identifying hotspots

```cmd
SELECT * FROM ( 
SELECT subregion1_name as state, sum(cumulative_confirmed) as total_confirmed_cases 
FROM `bigquery-public-data.covid19_open_data.covid19_open_data` WHERE country_code="US" AND date='2020-06-25' AND subregion1_name is NOT NULL 
GROUP BY subregion1_name ORDER BY total_confirmed_cases DESC ) WHERE total_confirmed_cases > Enter_confirmed_cases_number
```


## Task 4.Fatality Ratio

```cmd
SELECT SUM(cumulative_confirmed) AS total_confirmed_cases, SUM(cumulative_deceased) AS total_deaths, (SUM(cumulative_deceased)/SUM(cumulative_confirmed))*100 AS case_fatality_ratio
FROM `bigquery-public-data.covid19_open_data.covid19_open_data`
WHERE country_name="Italy" AND date BETWEEN "2020-04-01" AND "2020-04-30"
```


## Task 5.Identifying specific day

```cmd
SELECT
 date
FROM
  `bigquery-public-data.covid19_open_data.covid19_open_data`
WHERE
 country_name = 'Italy'
 AND cumulative_deceased > Enter_Total_Number
ORDER BY date
LIMIT 1
```


## Task 6: Finding days with zero net new cases

```cmd
WITH india_cases_by_date AS (
  SELECT
    date,
    SUM(cumulative_confirmed) AS cases
  FROM
    `bigquery-public-data.covid19_open_data.covid19_open_data`
  WHERE
    country_name="India"
    AND date between '2020-02-21' and '2020-03-14'
  GROUP BY
    date
  ORDER BY
    date ASC
 )

, india_previous_day_comparison AS
(SELECT
  date,
  cases,
  LAG(cases) OVER(ORDER BY date) AS previous_day,
  cases - LAG(cases) OVER(ORDER BY date) AS net_new_cases
FROM india_cases_by_date
)
SELECT
  COUNT(date)
FROM
  india_previous_day_comparison
WHERE
  net_new_cases = 0
```


## Task 7.Doubling rate

```cmd
  WITH us_cases_by_date AS (
    SELECT
      date,
      SUM( cumulative_confirmed ) AS cases
    FROM
      `bigquery-public-data.covid19_open_data.covid19_open_data`
    WHERE
      country_name="United States of America"
      AND date between '2020-03-22' and '2020-04-20'
    GROUP BY
      date
    ORDER BY
      date ASC
  )

  , us_previous_day_comparison AS
  (SELECT
    date,
    cases,
    LAG(cases) OVER(ORDER BY date) AS previous_day,
    cases - LAG(cases) OVER(ORDER BY date) AS net_new_cases,
    (cases - LAG(cases) OVER(ORDER BY date))*100/LAG(cases) OVER(ORDER BY date) AS percentage_increase
  FROM us_cases_by_date
  )
  SELECT
    Date,
    cases AS Confirmed_Cases_On_Day,
    previous_day AS Confirmed_Cases_Previous_Day,
    percentage_increase AS Percentage_Increase_In_Cases
  FROM
    us_previous_day_comparison
  WHERE
    percentage_increase > Enter_Percentage_Here
```


## Task 8.Recovery rate

```cmd
WITH cases_by_country AS (
  SELECT
    country_name AS country,
    SUM(cumulative_confirmed) AS cases,
    SUM(cumulative_recovered) AS recovered_cases
  FROM
    `bigquery-public-data.covid19_open_data.covid19_open_data`
  WHERE
    date="2020-05-10"
  GROUP BY
    country_name
)

, recovered_rate AS (
  SELECT
    country, cases, recovered_cases,
    (recovered_cases * 100)/cases AS recovery_rate
  FROM
    cases_by_country
)

SELECT country, cases AS confirmed_cases, recovered_cases, recovery_rate
FROM
   recovered_rate
WHERE
   cases > 50000
ORDER BY recovery_rate DESC
LIMIT Enter_Limit_Here
```


## Task 9.CDGR — Cumulative Daily Growth Rate

```cmd
WITH
  france_cases AS (
  SELECT
    date,
    SUM(cumulative_confirmed) AS total_cases
  FROM
    `bigquery-public-data.covid19_open_data.covid19_open_data`
  WHERE
    country_name="France"
    AND date IN ('2020-01-24',
      '2020-06-25')
  GROUP BY
    date
  ORDER BY
    date)
, summary as (
SELECT
  total_cases AS first_day_cases,
  LEAD(total_cases) OVER(ORDER BY date) AS last_day_cases,
  DATE_DIFF(LEAD(date) OVER(ORDER BY date),date, day) AS days_diff
FROM
  france_cases
LIMIT 1
)

select first_day_cases, last_day_cases, days_diff, POWER(last_day_cases/first_day_cases,1/days_diff)-1 as cdgr
from summary
```


## Task 10.Create a Datastudio report

```cmd
SELECT
  date, SUM(cumulative_confirmed) AS country_cases,
  SUM(cumulative_deceased) AS country_deaths
FROM
  `bigquery-public-data.covid19_open_data.covid19_open_data`
WHERE
  date BETWEEN 'Date range'
  AND 'Date range'
  AND country_name='United States of America'
GROUP BY date
```

# Congratulations🎉! You're done with this challenge lab.