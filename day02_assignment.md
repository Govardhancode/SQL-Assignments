-- DAY 2 ASSIGNMENT: FILTERING & SORTING
-- Tables used: hr.employees and hr.departments


-- PART 1: PRACTICE QUESTIONS


-- Q1. List employees in department_id 50.
SELECT employee_id, first_name, last_name, department_id
FROM hr.employees
WHERE department_id = 50;


-- Q2. List employees whose salary is between 5000 and 10000 (inclusive).
SELECT employee_id, first_name, last_name, salary
FROM hr.employees
WHERE salary BETWEEN 5000 AND 10000;


-- Q3. List employees whose last name starts with 'K'.
SELECT employee_id, first_name, last_name
FROM hr.employees
WHERE last_name LIKE 'K%';


-- Q4. List the top 5 highest-paid employees.
SELECT employee_id, first_name, salary
FROM hr.employees
ORDER BY salary DESC
FETCH FIRST 5 ROWS ONLY;


-- PART 2: SELF-PRACTICE


-- Q1. List employees who have no commission_pct (NULL).
SELECT employee_id, first_name, last_name, commission_pct
FROM hr.employees
WHERE commission_pct IS NULL;


-- Q2. List employees whose job_id contains the string 'MAN'.
SELECT employee_id, first_name, last_name, job_id
FROM hr.employees
WHERE job_id LIKE '%MAN%';


-- Q3. List all employees ordered by hire_date descending (newest first).
SELECT employee_id, first_name, last_name, hire_date
FROM hr.employees
ORDER BY hire_date DESC;


-- PART 3: 20 MEDIUM QUESTIONS


-- M1. List employees in department_id 80 with salary greater than 8000.
SELECT employee_id, first_name, last_name, department_id, salary
FROM hr.employees
WHERE department_id = 80
AND salary > 8000;


-- M2. Find employees whose last_name ends with 'n'.
SELECT employee_id, first_name, last_name
FROM hr.employees
WHERE last_name LIKE '%n';


-- M3. List employees hired after January 1, 2005.
SELECT employee_id, first_name, last_name, hire_date
FROM hr.employees
WHERE hire_date > DATE '2005-01-01';


-- M4. Get employees whose job_id is either 'SA_REP' or 'SA_MAN'.
SELECT employee_id, first_name, last_name, job_id
FROM hr.employees
WHERE job_id IN ('SA_REP', 'SA_MAN');


-- M5. List employees with salary between 4000 and 7000 (inclusive).
SELECT employee_id, first_name, last_name, salary
FROM hr.employees
WHERE salary BETWEEN 4000 AND 7000;


-- M6. Find employees who have a manager.
SELECT employee_id, first_name, last_name, manager_id
FROM hr.employees
WHERE manager_id IS NOT NULL;


-- M7. List departments with department_id 10, 20, or 30.
SELECT department_id, department_name
FROM hr.departments
WHERE department_id IN (10, 20, 30);


-- M8. Get the top 3 employees by hire_date (oldest first).
SELECT employee_id, first_name, last_name, hire_date
FROM hr.employees
ORDER BY hire_date ASC
FETCH FIRST 3 ROWS ONLY;


-- M9. List employees in department 50, ordered by last_name ascending.
SELECT employee_id, first_name, last_name, department_id
FROM hr.employees
WHERE department_id = 50
ORDER BY last_name ASC;


-- M10. Find employees whose first_name starts with 'J'.
SELECT employee_id, first_name, last_name
FROM hr.employees
WHERE first_name LIKE 'J%';


-- M11. List employees with salary not in the range 5000 to 10000.
SELECT employee_id, first_name, last_name, salary
FROM hr.employees
WHERE salary NOT BETWEEN 5000 AND 10000;


-- M12. Get employees whose job_id contains 'CLERK'.
SELECT employee_id, first_name, last_name, job_id
FROM hr.employees
WHERE job_id LIKE '%CLERK%';


-- M13. List employees with commission_pct greater than 0.2.
SELECT employee_id, first_name, last_name, commission_pct
FROM hr.employees
WHERE commission_pct > 0.2;


-- M14. Find the 10 most recently hired employees.
SELECT employee_id, first_name, last_name, hire_date
FROM hr.employees
ORDER BY hire_date DESC
FETCH FIRST 10 ROWS ONLY;


-- M15. List employees in departments 50 or 60,
-- ordered by department_id then salary descending.
SELECT employee_id, first_name, last_name, department_id, salary
FROM hr.employees
WHERE department_id IN (50, 60)
ORDER BY department_id ASC, salary DESC;


-- M16. Get employees whose last_name has exactly 5 characters.
SELECT employee_id, first_name, last_name
FROM hr.employees
WHERE last_name LIKE '_____';


-- M17. List departments where manager_id is not null.
SELECT department_id, department_name, manager_id
FROM hr.departments
WHERE manager_id IS NOT NULL;


-- M18. Find employees with salary >= 10000,
-- ordered by salary ascending.
SELECT employee_id, first_name, last_name, salary
FROM hr.employees
WHERE salary >= 10000
ORDER BY salary ASC;


-- M19. List employees whose email ends with '.com'
-- or contains 'example'.
SELECT employee_id, first_name, last_name, email
FROM hr.employees
WHERE email LIKE '%.com'
OR email LIKE '%example%';


-- M20. Get distinct job_id values from employees in department 50.
SELECT DISTINCT job_id
FROM hr.employees
WHERE department_id = 50;


-- PART 4: 20 HARD QUESTIONS


-- H1. List employees in department 80 with salary > 7000
-- OR job_id = 'SA_MAN', ordered by salary descending.
SELECT employee_id, first_name, last_name,
       department_id, job_id, salary
FROM hr.employees
WHERE (department_id = 80 AND salary > 7000)
OR job_id = 'SA_MAN'
ORDER BY salary DESC;


-- H2. Find employees hired between Jan 1, 2000 and Dec 31, 2005.
SELECT employee_id, first_name, last_name, hire_date
FROM hr.employees
WHERE hire_date BETWEEN DATE '2000-01-01'
                    AND DATE '2005-12-31';


-- H3. List employees whose last_name is 4 characters
-- and starts with 'K'.
SELECT employee_id, first_name, last_name
FROM hr.employees
WHERE last_name LIKE 'K___';


-- H4. Get top 5 highest-paid employees in department 50 only.
SELECT employee_id, first_name, last_name, salary
FROM hr.employees
WHERE department_id = 50
ORDER BY salary DESC
FETCH FIRST 5 ROWS ONLY;


-- H5. List employees with no manager and salary > 5000.
SELECT employee_id, first_name, last_name, manager_id, salary
FROM hr.employees
WHERE manager_id IS NULL
AND salary > 5000;


-- H6. Find employees whose first_name has 'a'
-- as the second character.
SELECT employee_id, first_name, last_name
FROM hr.employees
WHERE first_name LIKE '_a%';


-- H7. List departments with department_id between 40 and 90.
SELECT department_id, department_name
FROM hr.departments
WHERE department_id BETWEEN 40 AND 90;


-- H8. Get employees with salary < 3000 or salary > 15000,
-- ordered by salary.
SELECT employee_id, first_name, last_name, salary
FROM hr.employees
WHERE salary < 3000
OR salary > 15000
ORDER BY salary ASC;


-- H9. List employees in department 60 with job_id 'IT_PROG',
-- or in department 100 with job_id like 'FI%'.
SELECT employee_id, first_name, last_name,
       department_id, job_id
FROM hr.employees
WHERE (department_id = 60 AND job_id = 'IT_PROG')
OR (department_id = 100 AND job_id LIKE 'FI%');


-- H10. Find employees whose hire_date is in the year 2003.
SELECT employee_id, first_name, last_name, hire_date
FROM hr.employees
WHERE EXTRACT(YEAR FROM hire_date) = 2003;


-- H11. List employees with commission_pct NULL
-- and job_id starting with 'SA'.
SELECT employee_id, first_name, last_name,
       commission_pct, job_id
FROM hr.employees
WHERE commission_pct IS NULL
AND job_id LIKE 'SA%';


-- H12. Get the 3 oldest employees (earliest hire_date)
-- in department 90.
SELECT employee_id, first_name, last_name, hire_date
FROM hr.employees
WHERE department_id = 90
ORDER BY hire_date ASC
FETCH FIRST 3 ROWS ONLY;


-- H13. List employees whose last_name does not
-- start with A, B, or C.
SELECT employee_id, first_name, last_name
FROM hr.employees
WHERE last_name NOT LIKE 'A%'
AND last_name NOT LIKE 'B%'
AND last_name NOT LIKE 'C%';


-- H14. Find employees with salary
-- in (5000, 6000, 7000, 8000).
SELECT employee_id, first_name, last_name, salary
FROM hr.employees
WHERE salary IN (5000, 6000, 7000, 8000);


-- H15. List employees ordered by department_id ascending,
-- then hire_date descending within each department.
SELECT employee_id, first_name, last_name,
       department_id, hire_date
FROM hr.employees
ORDER BY department_id ASC, hire_date DESC;


-- H16. Get employees whose first_name and last_name
-- start with the same letter.
SELECT employee_id, first_name, last_name
FROM hr.employees
WHERE SUBSTR(first_name, 1, 1) =
      SUBSTR(last_name, 1, 1);


-- H17. List employees with manager_id not null
-- and department_id in (50, 80, 100).
SELECT employee_id, first_name, last_name,
       manager_id, department_id
FROM hr.employees
WHERE manager_id IS NOT NULL
AND department_id IN (50, 80, 100);


-- H18. Find employees with salary between 3000 and 5000
-- and job_id containing 'REP'.
SELECT employee_id, first_name, last_name, salary, job_id
FROM hr.employees
WHERE salary BETWEEN 3000 AND 5000
AND job_id LIKE '%REP%';


-- H19. List departments ordered by department_name descending.
SELECT *
FROM hr.departments
ORDER BY department_name DESC;


-- H20. Get employees with hire_date not in 2004.
SELECT employee_id, first_name, last_name, hire_date
FROM hr.employees
WHERE EXTRACT(YEAR FROM hire_date) <> 2004;
