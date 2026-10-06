-- DAY 3 ASSIGNMENT: DDL - CREATE & ALTER
-- Tables used: hr.employees and hr.departments


-- PART 1: PRACTICE QUESTIONS


-- Q1. Create table hr_emp_backup from hr.employees
CREATE TABLE hr_emp_backup AS
SELECT * FROM hr.employees;


-- Q2. Add column notes VARCHAR2(200) to hr_emp_backup.
ALTER TABLE hr_emp_backup
ADD notes VARCHAR2(200);


-- Q3. Rename column notes to remarks in hr_emp_backup.
ALTER TABLE hr_emp_backup
RENAME COLUMN notes TO remarks;


-- PART 2: SELF-PRACTICE


-- Q1. Create a table from hr.employees containing only
-- employee_id, first_name, last_name, salary and department_id.
CREATE TABLE emp_backup AS
SELECT employee_id, first_name, last_name, salary, department_id
FROM hr.employees;


-- Q2. Add effective_date column of type DATE.
ALTER TABLE emp_backup
ADD effective_date DATE;


-- Q3. Truncate the backup table so it becomes empty
-- but the structure remains.
TRUNCATE TABLE emp_backup;


-- PART 3: 20 MEDIUM QUESTIONS


-- M1. Create hr_dept_backup as a full copy of hr.departments.
CREATE TABLE hr_dept_backup AS
SELECT * FROM hr.departments;


-- M2. Add notes VARCHAR2(100) to hr_emp_backup.
-- NOTE: If you already renamed notes to remarks in Part 1,
-- this creates a new notes column.
ALTER TABLE hr_emp_backup
ADD notes VARCHAR2(100);


-- M3. Create emp_50 containing employees
-- from department 50 only.
CREATE TABLE emp_50 AS
SELECT *
FROM hr.employees
WHERE department_id = 50;


-- M4. Add updated_at DATE with default SYSDATE.
ALTER TABLE hr_emp_backup
ADD updated_at DATE DEFAULT SYSDATE;


-- M5. Create dept_names containing department_id
-- and department_name only.
CREATE TABLE dept_names AS
SELECT department_id, department_name
FROM hr.departments;


-- M6. Modify notes column to VARCHAR2(500).
ALTER TABLE hr_emp_backup
MODIFY notes VARCHAR2(500);


-- M7. Create empty emp_structure table
-- with same structure as hr.employees.
CREATE TABLE emp_structure AS
SELECT *
FROM hr.employees
WHERE 1 = 0;


-- M8. Rename hr_emp_backup to hr_employees_archive.
RENAME hr_emp_backup TO hr_employees_archive;


-- IMPORTANT:
-- From M8 onwards, hr_emp_backup is now named
-- hr_employees_archive.


-- M9. Add created_by and created_date columns.
ALTER TABLE hr_employees_archive
ADD (
    created_by VARCHAR2(50),
    created_date DATE
);


-- M10. Create high_earners containing employees
-- whose salary is greater than 10000.
CREATE TABLE high_earners AS
SELECT *
FROM hr.employees
WHERE salary > 10000;


-- M11. Drop notes column from backup table.
ALTER TABLE hr_employees_archive
DROP COLUMN notes;


-- M12. Create emp_salary_dept containing
-- employee_id, salary and department_id.
CREATE TABLE emp_salary_dept AS
SELECT employee_id, salary, department_id
FROM hr.employees;


-- M13. Truncate emp_50.
TRUNCATE TABLE emp_50;


-- M14. Rename remarks column to comments.
ALTER TABLE hr_employees_archive
RENAME COLUMN remarks TO comments;


-- M15. Create dept_emp_count with department_id
-- and literal 0 as emp_count.
CREATE TABLE dept_emp_count AS
SELECT department_id, 0 AS emp_count
FROM hr.departments;


-- M16. Add status VARCHAR2(20) with default ACTIVE.
ALTER TABLE hr_employees_archive
ADD status VARCHAR2(20) DEFAULT 'ACTIVE';


-- M17. Create emp_hire_2005 containing employees
-- hired in the year 2005.
CREATE TABLE emp_hire_2005 AS
SELECT *
FROM hr.employees
WHERE EXTRACT(YEAR FROM hire_date) = 2005;


-- M18. Modify status column to VARCHAR2(30).
ALTER TABLE hr_employees_archive
MODIFY status VARCHAR2(30);


-- M19. Create empty dept_template table
-- with same structure as hr.departments.
CREATE TABLE dept_template AS
SELECT *
FROM hr.departments
WHERE 1 = 0;


-- M20. Add audit_id NUMBER(10) to backup table.
ALTER TABLE hr_employees_archive
ADD audit_id NUMBER(10);


-- PART 4: 20 HARD QUESTIONS


-- H1. Create emp_dept_summary with one row per department,
-- including department_id, department_name and total salary.
CREATE TABLE emp_dept_summary AS
SELECT d.department_id,
       d.department_name,
       (SELECT SUM(e.salary)
        FROM hr.employees e
        WHERE e.department_id = d.department_id) AS total_sal
FROM hr.departments d;


-- H2. Create emp_backup_80 for department 80 with only
-- employee_id, first_name, last_name, salary and commission_pct.
CREATE TABLE emp_backup_80 AS
SELECT employee_id,
       first_name,
       last_name,
       salary,
       commission_pct
FROM hr.employees
WHERE department_id = 80;


-- H3. Add full_name column to backup table and populate it
-- using first_name and last_name.
ALTER TABLE hr_employees_archive
ADD full_name VARCHAR2(100);

UPDATE hr_employees_archive
SET full_name = first_name || ' ' || last_name;


-- H4. Create dept_with_mgr containing department information
-- and the department manager's full name.
CREATE TABLE dept_with_mgr AS
SELECT d.department_id,
       d.department_name,
       e.first_name || ' ' || e.last_name AS manager_name
FROM hr.departments d
LEFT JOIN hr.employees e
ON d.manager_id = e.employee_id;


-- H5. Create emp_job_salary containing job_id,
-- minimum salary, maximum salary and average salary.
CREATE TABLE emp_job_salary AS
SELECT job_id,
       MIN(salary) AS min_sal,
       MAX(salary) AS max_sal,
       AVG(salary) AS avg_sal
FROM hr.employees
GROUP BY job_id;


-- H6. Add a column with DEFAULT SYSDATE
-- and rename an existing column.
ALTER TABLE hr_employees_archive
ADD modified_date DATE DEFAULT SYSDATE;

ALTER TABLE hr_employees_archive
RENAME COLUMN comments TO employee_comments;


-- H7. Create emp_top_sal containing employees
-- whose salary is among the top 10 salary rows.
CREATE TABLE emp_top_sal AS
SELECT *
FROM hr.employees
WHERE salary IN (
    SELECT salary
    FROM hr.employees
    ORDER BY salary DESC
    FETCH FIRST 10 ROWS ONLY
);


-- H8. Create dept_emp_list with department_id,
-- department_name and employee_count.
CREATE TABLE dept_emp_list AS
SELECT d.department_id,
       d.department_name,
       COUNT(e.employee_id) AS employee_count
FROM hr.departments d
LEFT JOIN hr.employees e
ON e.department_id = d.department_id
GROUP BY d.department_id, d.department_name;


-- H9. Drop two columns from backup table in one statement.
-- We use created_by and created_date created in M9.
ALTER TABLE hr_employees_archive
DROP (created_by, created_date);


-- H10. Create table containing employees whose manager_id
-- and department_id are both NOT NULL.
CREATE TABLE emp_with_mgr_dept AS
SELECT *
FROM hr.employees
WHERE manager_id IS NOT NULL
AND department_id IS NOT NULL;


-- H11. Add salary_band column, update it using CASE,
-- then set DEFAULT Medium for new rows.
ALTER TABLE hr_employees_archive
ADD salary_band VARCHAR2(10);

UPDATE hr_employees_archive
SET salary_band =
    CASE
        WHEN salary < 5000 THEN 'Low'
        WHEN salary <= 10000 THEN 'Medium'
        ELSE 'High'
    END;

ALTER TABLE hr_employees_archive
MODIFY salary_band DEFAULT 'Medium';


-- H12. Create emp_duplicate_check showing how many employees
-- have the same first_name and last_name.
CREATE TABLE emp_duplicate_check AS
SELECT employee_id,
       first_name,
       last_name,
       COUNT(*) OVER (
           PARTITION BY first_name, last_name
       ) AS dup_count
FROM hr.employees;


-- H13. Create empty emp_import_staging table
-- with the same structure as hr.employees.
CREATE TABLE emp_import_staging AS
SELECT *
FROM hr.employees
WHERE 1 = 0;


-- H14. Change employee_id from NUMBER to VARCHAR2
-- using a new temporary column.
ALTER TABLE emp_import_staging
ADD employee_id_new VARCHAR2(20);

UPDATE emp_import_staging
SET employee_id_new = TO_CHAR(employee_id);

ALTER TABLE emp_import_staging
DROP COLUMN employee_id;

ALTER TABLE emp_import_staging
RENAME COLUMN employee_id_new TO employee_id;


-- H15. Create dept_location_1700 containing departments
-- where location_id = 1700.
CREATE TABLE dept_location_1700 AS
SELECT *
FROM hr.departments
WHERE location_id = 1700;


-- H16. Add version with default 1 and
-- last_modified with default SYSDATE.
ALTER TABLE hr_employees_archive
ADD (
    version NUMBER DEFAULT 1,
    last_modified DATE DEFAULT SYSDATE
);


-- H17. Create emp_salary_range containing employees
-- whose salary is between 5000 and 15000.
CREATE TABLE emp_salary_range AS
SELECT *
FROM hr.employees
WHERE salary BETWEEN 5000 AND 15000;


-- H18. Truncate a table, add a new column
-- and verify that the table contains 0 rows.
TRUNCATE TABLE emp_salary_range;

ALTER TABLE emp_salary_range
ADD remarks VARCHAR2(100);

SELECT COUNT(*) AS total_rows
FROM emp_salary_range;


-- H19. Create job_list with distinct job_id
-- and literal HR as category.
CREATE TABLE job_list AS
SELECT DISTINCT job_id,
       'HR' AS category
FROM hr.employees;


-- H20. Drop emp_structure only if it exists.
BEGIN
    EXECUTE IMMEDIATE 'DROP TABLE emp_structure';
EXCEPTION
    WHEN OTHERS THEN
        IF SQLCODE != -942 THEN
            RAISE;
        END IF;
END;
/
