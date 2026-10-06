-- DAY 1 SQL ASSIGNMENT - ORACLE FREE SQL

-- Q1. List employee_id, first_name and last_name.
SELECT employee_id, first_name, last_name
FROM hr.employees;

-- Q2. Display full name as a single column.
SELECT first_name || ' ' || last_name AS full_name
FROM hr.employees;

-- Q3. Calculate annual salary as salary * 12.
SELECT employee_id, first_name, last_name, salary,
       salary * 12 AS annual_salary
FROM hr.employees;

-- SELF PRACTICE 1. List all distinct job_id values.
SELECT DISTINCT job_id
FROM hr.employees;

-- SELF PRACTICE 2. Show commission_pct and salary for first 10 rows.
SELECT commission_pct, salary
FROM hr.employees
FETCH FIRST 10 ROWS ONLY;

-- SELF PRACTICE 3. Display employee_id and literal HR as department.
SELECT employee_id, 'HR' AS department
FROM hr.employees;

-- M1. Display full_name as First, Last.
SELECT employee_id, first_name, last_name,
       first_name || ', ' || last_name AS full_name
FROM hr.employees;

-- M2. Show 10% of salary as bonus_10_pct.
SELECT employee_id, salary,
       salary * 0.10 AS bonus_10_pct
FROM hr.employees;

-- M3. Display Employee as record_type.
SELECT employee_id, hire_date,
       'Employee' AS record_type
FROM hr.employees;

-- M4. Display @company.com as email_domain.
SELECT email, '@company.com' AS email_domain
FROM hr.employees;

-- M5. Replace NULL commission with 0.
SELECT employee_id, salary, commission_pct,
       NVL(commission_pct, 0) AS effective_commission
FROM hr.employees;

-- M6. Display employee initials.
SELECT first_name, last_name,
       SUBSTR(first_name,1,1) || SUBSTR(last_name,1,1) AS initials
FROM hr.employees;

-- M7. Display annual salary and annual salary with 10% bonus.
SELECT employee_id, salary,
       salary * 12 AS annual_salary,
       salary * 12 * 1.1 AS annual_plus_bonus
FROM hr.employees;

-- M8. Display all department columns explicitly.
SELECT department_id, department_name, manager_id, location_id
FROM hr.departments;

-- M9. Display employee description as Emp#employee_id.
SELECT employee_id,
       'Emp#' || TO_CHAR(employee_id) AS description
FROM hr.employees;

-- M10. Display Standard as salary_band.
SELECT job_id, salary, 'Standard' AS salary_band
FROM hr.employees;

-- M11. Display name as Last, First.
SELECT employee_id, first_name, last_name,
       last_name || ', ' || first_name AS display_name
FROM hr.employees;

-- M12. Display department_id and 1 as sort_order.
SELECT department_id, 1 AS sort_order
FROM hr.departments;

-- M13. Calculate monthly_net after 15% tax.
SELECT salary, salary * 0.85 AS monthly_net
FROM hr.employees;

-- M14. Display NULL commission as 0.
SELECT employee_id, commission_pct,
       NVL(commission_pct, 0) AS commission_display
FROM hr.employees;

-- M15. Calculate compensation including commission.
SELECT first_name, last_name, salary,
       salary * (1 + NVL(commission_pct,0)) AS compensation
FROM hr.employees;

-- M16. Display HQ as region.
SELECT department_name, 'HQ' AS region
FROM hr.departments;

-- M17. Display Years of service as years_label.
SELECT employee_id, hire_date,
       'Years of service' AS years_label
FROM hr.employees;

-- M18. Calculate double salary.
SELECT employee_id, salary,
       salary * 2 AS double_salary
FROM hr.employees;

-- M19. Display whether employee has manager.
SELECT manager_id,
       NVL2(manager_id, 'Yes', 'No') AS has_manager
FROM hr.employees;

-- M20. Display first 3 characters of department name as dept_code.
SELECT department_id, department_name,
       SUBSTR(department_name,1,3) AS dept_code
FROM hr.departments;

-- H1. Categorize salary as High, Medium or Low.
SELECT employee_id, first_name, last_name, salary,
       CASE
           WHEN salary >= 10000 THEN 'High'
           WHEN salary >= 5000 THEN 'Medium'
           ELSE 'Low'
       END AS salary_rank_label
FROM hr.employees;

-- H2. Calculate total compensation and round to 2 decimals.
SELECT employee_id, salary, commission_pct,
       ROUND(salary * (1 + NVL(commission_pct,0)),2) AS total_comp
FROM hr.employees;

-- H3. Display uppercase email and email length.
SELECT employee_id, email,
       UPPER(email) AS email_upper,
       LENGTH(email) AS email_length
FROM hr.employees;

-- H4. Display length of department name.
SELECT department_id, department_name,
       LENGTH(department_name) AS name_length
FROM hr.departments;

-- H5. Display reverse name as last_name + first_name.
SELECT employee_id, first_name, last_name,
       last_name || first_name AS reverse_name
FROM hr.employees;

-- H6. Display HR.EMPLOYEES as data_source.
SELECT employee_id, hire_date,
       'HR.EMPLOYEES' AS data_source
FROM hr.employees;

-- H7. Calculate employee salary percentage of total salary.
SELECT job_id, salary,
       ROUND(
           salary * 100 / (SELECT SUM(salary) FROM hr.employees),
           2
       ) AS salary_percentage
FROM hr.employees;

-- H8. Display formal employee name.
SELECT employee_id, first_name, last_name,
       'Mr. ' || first_name || ' ' || last_name AS formal_name
FROM hr.employees;

-- H9. Calculate annual salary with 5% raise.
SELECT employee_id, salary,
       salary * 12 * 1.05 AS annual_with_raise
FROM hr.employees;

-- H10. Display department_id and name separated by hyphen.
SELECT department_id, department_name,
       TO_CHAR(department_id) || '-' || department_name AS id_name
FROM hr.departments;

-- H11. Display Commissioned or Non-commissioned.
SELECT employee_id, commission_pct,
       NVL2(commission_pct,'Commissioned','Non-commissioned')
       AS commission_category
FROM hr.employees;

-- H12. Display literal string salary * 12.
SELECT employee_id, first_name, last_name, salary,
       'salary * 12' AS salary_expression
FROM hr.employees;

-- H13. Display job_id and salary together.
SELECT employee_id, job_id,
       job_id || ':' || TO_CHAR(salary) AS job_salary_label
FROM hr.employees;

-- H14. Categorize employee tax bracket.
SELECT employee_id, salary,
       CASE
           WHEN salary >= 10000 THEN '20%'
           WHEN salary >= 5000 THEN '15%'
           ELSE '10%'
       END AS tax_bracket
FROM hr.employees;

-- H15. Display department information.
SELECT department_id, department_name,
       'Department ' || TO_CHAR(department_id) ||
       ' - ' || department_name AS dept_info
FROM hr.departments;

-- H16. Display full name as Last First.
SELECT employee_id, first_name, last_name,
       last_name || ' ' || first_name AS full_name_reversed
FROM hr.employees;

-- H17. Calculate effective salary based on commission.
SELECT employee_id, salary, commission_pct,
       NVL2(commission_pct,
            salary * (1 + commission_pct),
            salary) AS effective_salary
FROM hr.employees;

-- H18. Extract hire year from hire_date.
SELECT employee_id, hire_date,
       EXTRACT(YEAR FROM hire_date) AS hire_year
FROM hr.employees;

-- H19. Count words in department_name.
SELECT department_name,
       LENGTH(department_name)
       - LENGTH(REPLACE(department_name,' ',''))
       + 1 AS word_count
FROM hr.departments;

-- H20. Display employee ID with employee full name.
SELECT employee_id, first_name, last_name,
       '[' || TO_CHAR(employee_id) || '] ' ||
       first_name || ' ' || last_name AS name_with_id
FROM hr.employees;
