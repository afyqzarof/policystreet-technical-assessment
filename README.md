# PolicyStreet Technical Assessment

Answers by Afyq Zarof
## Back End Test
### Task 1:


## SQL Questions

#### Note: SQL Function may vary based on different SQL Variants

1.

```sql
CREATE VIEW employees_salary_over_ten_thousand AS
SELECT employee_id, first_name, last_name, email, salary
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
SELECT CONCAT(locations.location_id,' ',locations.state_province) AS location,
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
SELECT em.employee_id, em.first_name, em.last_name, em.email
FROM employees AS em
INNER JOIN departments AS d
ON em.department_id = d.department_id
INNER JOIN office_locations AS ol
ON d.location_id = SUBSTRING(ol.location, 1, 4)
AND SUBSTRING(ol.LOCATION, 6) = 'California';

```

5. Using `office_locations` VIEW create in 3.

```sql
SELECT em.employee_id, em.first_name, em.last_name, em.email
FROM employees AS em
INNER JOIN departments AS d
ON em.department_id = d.department_id
INNER JOIN office_locations AS ol
ON d.location_id = SUBSTRING(ol.location, 1, 4)
WHERE loc.REGION LIKE 'A%';
```

6.

```sql
SELECT DISTINCT em.employee_id, em.first_name, em.last_name, em.email
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
SELECT DISTINCT em.employee_id, em.first_name, em.last_name, em.email, em.salary
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
SELECT em.employee_id, em.first_name, em.last_name, em.email, ol.country
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
SELECT pjc.previous_count, cjc.current_count, j.job_id, j.job_title
FROM previous_job_count AS pjc
OUTER JOIN current_job_count as cjc
ON pjc.job_id = cjc.job_id
INNER JOIN jobs AS j
ON cjc.job_id = j.job_id;
```
