-- DAY 4 ASSIGNMENT: DML - INSERT, UPDATE, DELETE


-- PART 1: PRACTICE QUESTIONS


-- Q1. Insert one row into the backup table.
INSERT INTO hr_emp_backup
(employee_id, first_name, last_name, email,
 hire_date, job_id, salary, department_id)
VALUES
(999, 'John', 'Doe', 'JDOE',
 SYSDATE, 'SA_REP', 5000, 50);


-- Q2. Increase salary by 10% for employees in department 60.
UPDATE hr_emp_backup
SET salary = salary * 1.10
WHERE department_id = 60;


-- Q3. Delete rows where department_id is NULL.
DELETE FROM hr_emp_backup
WHERE department_id IS NULL;


-- PART 2: SELF-PRACTICE


-- Q1. Insert employees whose salary is greater than 10000.
INSERT INTO hr_emp_backup
SELECT *
FROM hr.employees
WHERE salary > 10000;


-- Q2. Update job_id for a specific employee.
UPDATE hr_emp_backup
SET job_id = 'IT_PROG'
WHERE employee_id = 100;


-- Q3. Delete the test employee with employee_id 999.
DELETE FROM hr_emp_backup
WHERE employee_id = 999;


-- PART 3: 20 MEDIUM QUESTIONS


-- M1. Insert employee 990 with first_name Test,
-- last_name User, salary 4000 and department 50.
INSERT INTO hr_emp_backup
(employee_id, first_name, last_name, salary, department_id)
VALUES
(990, 'Test', 'User', 4000, 50);


-- M2. Update salary to 6000 for employee_id 990.
UPDATE hr_emp_backup
SET salary = 6000
WHERE employee_id = 990;


-- M3. Delete employee_id 990.
DELETE FROM hr_emp_backup
WHERE employee_id = 990;


-- M4. Insert employees from department 80.
INSERT INTO hr_emp_backup
SELECT *
FROM hr.employees
WHERE department_id = 80;


-- M5. Update first_name to Updated for employee_id 100.
UPDATE hr_emp_backup
SET first_name = 'Updated'
WHERE employee_id = 100;


-- M6. Delete all employees from department 90.
DELETE FROM hr_emp_backup
WHERE department_id = 90;


-- M7. Insert two employees using separate INSERT statements.
INSERT INTO hr_emp_backup
(employee_id, first_name, last_name, salary, department_id)
VALUES
(991, 'Ram', 'Kumar', 4500, 50);

INSERT INTO hr_emp_backup
(employee_id, first_name, last_name, salary, department_id)
VALUES
(992, 'Sam', 'Roy', 5000, 60);


-- M8. Increase salary by 5% for department 50.
UPDATE hr_emp_backup
SET salary = salary * 1.05
WHERE department_id = 50;


-- M9. Delete employees whose salary is NULL.
DELETE FROM hr_emp_backup
WHERE salary IS NULL;


-- M10. Insert employees whose job_id is SA_REP.
INSERT INTO hr_emp_backup
SELECT *
FROM hr.employees
WHERE job_id = 'SA_REP';


-- M11. Update department_id to 60 for employee_id 105.
UPDATE hr_emp_backup
SET department_id = 60
WHERE employee_id = 105;


-- M12. Delete employee_id 999 if it exists.
DELETE FROM hr_emp_backup
WHERE employee_id = 999;


-- M13. Insert employee 993, Amy Lee,
-- salary 5500, department 60.
INSERT INTO hr_emp_backup
(employee_id, last_name, first_name, salary, department_id)
VALUES
(993, 'Lee', 'Amy', 5500, 60);


-- M14. Change last_name to Smith
-- where first_name is John.
UPDATE hr_emp_backup
SET last_name = 'Smith'
WHERE first_name = 'John';


-- M15. Delete employees hired before 2000.
DELETE FROM hr_emp_backup
WHERE hire_date < DATE '2000-01-01';


-- M16. Insert employees whose salary
-- is between 5000 and 7000.
INSERT INTO hr_emp_backup
SELECT *
FROM hr.employees
WHERE salary BETWEEN 5000 AND 7000;


-- M17. Change job_id to IT_PROG for employee_id 200.
UPDATE hr_emp_backup
SET job_id = 'IT_PROG'
WHERE employee_id = 200;


-- M18. Delete employees who have commission.
DELETE FROM hr_emp_backup
WHERE commission_pct IS NOT NULL;


-- M19. Insert a new employee using SYSDATE as hire_date.
INSERT INTO hr_emp_backup
(employee_id, first_name, last_name, email,
 hire_date, job_id, salary, department_id)
VALUES
(994, 'David', 'John', 'DJOHN',
 SYSDATE, 'IT_PROG', 6000, 60);


-- M20. Set salary to 10000 for employee
-- having the highest employee_id.
UPDATE hr_emp_backup
SET salary = 10000
WHERE employee_id =
      (SELECT MAX(employee_id)
       FROM hr_emp_backup);


-- PART 4: 20 HARD QUESTIONS


-- H1. MERGE hr.employees into hr_emp_backup.
-- If employee exists, update salary and hire_date.
-- If employee does not exist, insert it.
MERGE INTO hr_emp_backup t
USING hr.employees s
ON (t.employee_id = s.employee_id)

WHEN MATCHED THEN
UPDATE SET
    t.salary = s.salary,
    t.hire_date = s.hire_date

WHEN NOT MATCHED THEN
INSERT
(employee_id, first_name, last_name, email,
 phone_number, hire_date, job_id, salary,
 commission_pct, manager_id, department_id)
VALUES
(s.employee_id, s.first_name, s.last_name, s.email,
 s.phone_number, s.hire_date, s.job_id, s.salary,
 s.commission_pct, s.manager_id, s.department_id);


-- H2. Set salary equal to the original salary
-- from hr.employees for employees in department 60.
UPDATE hr_emp_backup e
SET e.salary =
    (SELECT h.salary
     FROM hr.employees h
     WHERE h.employee_id = e.employee_id)
WHERE e.employee_id IN
    (SELECT employee_id
     FROM hr.employees
     WHERE department_id = 60);


-- H3. Delete employees from backup
-- who do not exist in hr.employees.
DELETE FROM hr_emp_backup b
WHERE NOT EXISTS
(
    SELECT 1
    FROM hr.employees e
    WHERE e.employee_id = b.employee_id
);


-- H4. Insert employees only when employee_id
-- does not already exist in backup.
INSERT INTO hr_emp_backup
SELECT e.*
FROM hr.employees e
WHERE NOT EXISTS
(
    SELECT 1
    FROM hr_emp_backup b
    WHERE b.employee_id = e.employee_id
);


-- H5. Set salary to average salary
-- of the employee's department.
UPDATE hr_emp_backup b
SET salary =
(
    SELECT AVG(e.salary)
    FROM hr.employees e
    WHERE e.department_id = b.department_id
)
WHERE b.department_id IS NOT NULL;


-- H6. Delete employee having smallest employee_id.
DELETE FROM hr_emp_backup
WHERE employee_id =
      (SELECT MIN(employee_id)
       FROM hr_emp_backup);


-- H7. Insert one row per department with emp_count = 0.
-- First create a backup table for this question.
CREATE TABLE dept_count_backup
(
    department_id NUMBER,
    department_name VARCHAR2(100),
    emp_count NUMBER
);

INSERT INTO dept_count_backup
(department_id, department_name, emp_count)
SELECT department_id, department_name, 0
FROM hr.departments;

-- Update employee count for each department.
UPDATE dept_count_backup d
SET emp_count =
(
    SELECT COUNT(*)
    FROM hr.employees e
    WHERE e.department_id = d.department_id
);


-- H8. Update first_name and last_name from hr.employees
-- for employees in department 50.
UPDATE hr_emp_backup e
SET (first_name, last_name) =
(
    SELECT h.first_name, h.last_name
    FROM hr.employees h
    WHERE h.employee_id = e.employee_id
)
WHERE e.department_id = 50
AND EXISTS
(
    SELECT 1
    FROM hr.employees h
    WHERE h.employee_id = e.employee_id
);


-- H9. Delete backup employees whose salary
-- in hr.employees is less than 3000.
DELETE FROM hr_emp_backup
WHERE employee_id IN
(
    SELECT employee_id
    FROM hr.employees
    WHERE salary < 3000
);


-- H10. Insert employees from departments 10, 20 or 30
-- whose salary is greater than 5000.
INSERT INTO hr_emp_backup
SELECT *
FROM hr.employees
WHERE department_id IN (10, 20, 30)
AND salary > 5000;


-- H11. Set department_id to 50
-- where department_id is NULL.
UPDATE hr_emp_backup
SET department_id = 50
WHERE department_id IS NULL;


-- H12. Delete duplicate employees with the same
-- first_name and last_name, keeping the smallest employee_id.
DELETE FROM hr_emp_backup a
WHERE employee_id NOT IN
(
    SELECT MIN(employee_id)
    FROM hr_emp_backup
    GROUP BY first_name, last_name
)
AND EXISTS
(
    SELECT 1
    FROM hr_emp_backup b
    WHERE b.first_name = a.first_name
    AND b.last_name = a.last_name
    AND b.employee_id < a.employee_id
);


-- H13. MERGE department 80 employees.
-- Update salary if employee exists.
-- Insert employee if employee does not exist.
MERGE INTO hr_emp_backup t
USING
(
    SELECT *
    FROM hr.employees
    WHERE department_id = 80
) s
ON (t.employee_id = s.employee_id)

WHEN MATCHED THEN
UPDATE SET
    t.salary = s.salary

WHEN NOT MATCHED THEN
INSERT
(employee_id, first_name, last_name, email,
 phone_number, hire_date, job_id, salary,
 commission_pct, manager_id, department_id)
VALUES
(s.employee_id, s.first_name, s.last_name, s.email,
 s.phone_number, s.hire_date, s.job_id, s.salary,
 s.commission_pct, s.manager_id, s.department_id);


-- H14. Increase backup salary by 10%
-- for employees whose original salary is below
-- the company average.
UPDATE hr_emp_backup
SET salary = salary * 1.10
WHERE employee_id IN
(
    SELECT employee_id
    FROM hr.employees
    WHERE salary <
          (SELECT AVG(salary)
           FROM hr.employees)
);


-- H15. Delete employee with the earliest hire_date.
-- FETCH FIRST 1 ROW ONLY ensures only one row is deleted
-- even if multiple employees have the same earliest date.
DELETE FROM hr_emp_backup
WHERE employee_id =
(
    SELECT employee_id
    FROM hr_emp_backup
    ORDER BY hire_date ASC, employee_id ASC
    FETCH FIRST 1 ROW ONLY
);


-- H16. Insert employees who are managers.
INSERT INTO hr_emp_backup
SELECT *
FROM hr.employees
WHERE employee_id IN
(
    SELECT manager_id
    FROM hr.employees
    WHERE manager_id IS NOT NULL
);


-- H17. Convert all last_name values to uppercase.
UPDATE hr_emp_backup
SET last_name = UPPER(last_name);


-- H18. Delete the top 5 highest salary earners.
DELETE FROM hr_emp_backup
WHERE employee_id IN
(
    SELECT employee_id
    FROM
    (
        SELECT employee_id
        FROM hr_emp_backup
        ORDER BY salary DESC
        FETCH FIRST 5 ROWS ONLY
    )
);


-- H19. Insert employees whose job_id starts with SA
-- and commission_pct is NOT NULL.
INSERT INTO hr_emp_backup
SELECT *
FROM hr.employees
WHERE job_id LIKE 'SA%'
AND commission_pct IS NOT NULL;


-- H20. Set salary to maximum salary
-- of the employee's department.
UPDATE hr_emp_backup e
SET salary =
(
    SELECT MAX(h.salary)
    FROM hr.employees h
    WHERE h.department_id = e.department_id
)
WHERE e.department_id IS NOT NULL;


-- OPTIONAL: Save DML changes.
COMMIT;
