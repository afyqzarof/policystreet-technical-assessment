# PolicyStreet Technical Assessment

Answers by Afyq Zarof

## Back End Test

### Task 1:

#### Monthly Instalment Formula, $A$

$$A = Pr\frac{(r+1)^n}{(r+1)^{n} -1}$$

Where: <br>
$A$ is monthly instalment <br>
$P$ is the principal amount borrowed <br>
$r$ is the monthly interest rate <br>
$n$ is the number of payments

1. What is the monthly repayment if the customer applies for a RM 80,000 personal loan over a period of 5 years?

Here we have:<br>
$P = 80,000$ <br>
$r = \frac{6.5}{100}\frac{1}{12} = 0.00542$ <br>
$n = 5 \times 12 = 60$ <br>

Subbing in the numbers gives us:<br>
$A = 80000(0.00542)\frac{1.00542^{60}}{1.00542^{60} -1}$ <br>
$A = 433.6\frac{1.383}{0.383}$ <br>
$A = 433.6(3.61)$<br>
$A = 1565.30$ <br>

Therefore monthly installment is RM 1,565.30

### Task 2:

See `check-digit.ipynb` for code

1. check digit for `"726358263582647"` is `1`
2. It seems that the occurences are quite evenly distributed within the 101 to 889 range

![bar chart](barchart.png)


- The highest occurrence of 90 being 4 and 7
- The lowest occurrence of 86 being 8 and 9
## SQL Questions

#### Note: SQL Function may vary based on different SQL Variants

1.

```sql
CREATE VIEW employees_salary_over_ten_thousand AS
SELECT
  employee_id,
  first_name,
  last_name,
  email,
  salary
FROM employees
WHERE salary > 10000;
```

2.

```sql
CREATE VIEW employees_salary_over_hundred_thousand AS
SELECT *
FROM employees
WHERE salary > 100000;
```

3.

```sql
CREATE VIEW office_locations AS
SELECT
  CONCAT(locations.location_id,' ',locations.state_province) AS location,
  countries.country_name AS country,
  regions.region_name AS region
FROM locations
INNER JOIN countries
ON locations.country_id = country.country_id
INNER JOIN regions
ON country.region_id = regions.region_id
WHERE locations.state_province IS NOT NULL;
```

4. Using `office_locations` VIEW create in 3.

```sql
SELECT
  em.employee_id,
  em.first_name,
  em.last_name,
  em.email
FROM employees AS em
INNER JOIN departments AS d
ON em.department_id = d.department_id
INNER JOIN office_locations AS ol
ON d.location_id = SUBSTRING(ol.location, 1, 4)
AND SUBSTRING(ol.LOCATION, 6) = 'California';

```

5. Using `office_locations` VIEW create in 3.

```sql
SELECT
  em.employee_id,
  em.first_name,
  em.last_name,
  em.email
FROM employees AS em
INNER JOIN departments AS d
ON em.department_id = d.department_id
INNER JOIN office_locations AS ol
ON d.location_id = SUBSTRING(ol.location, 1, 4)
WHERE loc.REGION LIKE 'A%';
```

6.

```sql
SELECT DISTINCT
  em.employee_id,
  em.first_name,
  em.last_name,
  em.email
FROM employees AS em
INNER JOIN job_history as jh
ON em.employee_id = jh.employee_id
INNER JOIN jobs as j
ON jh.job_id = j.job_id
WHERE j.job_title = 'SALES REPRESENTATIVE'
AND jh.end_date < DATE('now');
```

7.

```sql
SELECT DISTINCT
  em.employee_id,
  em.first_name,
  em.last_name,
  em.email,
  em.salary
FROM employees AS em
INNER JOIN departments as d
ON em.department_id = d.department_id
AND d.department_name = 'IT';
```

8.

```sql
SELECT
  CONCAT(em1.first_name, ' ', em1.last_name) AS employee_full_name,
  CONCAT(em2.first_name, ' ', em2.last_name) AS manager_full_name
FROM employees as em1
INNER JOIN employees as em2
ON em1.manager_id = em2.employee_id;
```

9. Using `office_locations` VIEW create in 3.

```sql
SELECT
  em.employee_id,
  em.first_name,
  em.last_name,
  em.email,
  ol.country
FROM employees AS em
INNER JOIN departments AS d
ON em.department_id = d.department_id
INNER JOIN office_locations AS ol
ON d.location_id = SUBSTRING(ol.location, 1, 4)
GROUP BY ol.country;
```

10. Will require creating two views first

Create `previous_job_count` VIEW

```sql
CREATE VIEW previous_job_count AS
SELECT COUNT(employee_id) AS previous_count, job_id
FROM job_history
WHERE end_date < DATE('now')
GROUP BY job_id;
```

Create `current_job_count` VIEW

```sql
CREATE VIEW current_job_count AS
SELECT COUNT(employee_id) AS current_count, job_id
FROM job_history
WHERE end_date >= DATE('now')
GROUP BY job_id;
```

Run query

```sql
SELECT
  pjc.previous_count,
  cjc.current_count,
  j.job_id,
  j.job_title
FROM previous_job_count AS pjc
OUTER JOIN current_job_count as cjc
ON pjc.job_id = cjc.job_id
INNER JOIN jobs AS j
ON cjc.job_id = j.job_id;
```
