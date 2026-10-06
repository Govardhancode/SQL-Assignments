-- DAY 5 ASSIGNMENT: DCL AND TCL
-- Use hr_emp_backup for UPDATE, DELETE and INSERT practice.


-- PART 1: PRACTICE QUESTIONS


-- Q1. Update employee 100, create a savepoint,
-- update employee 101, rollback to savepoint, then commit.
UPDATE hr_emp_backup
SET salary = salary * 1.05
WHERE employee_id = 100;

SAVEPOINT after_first;

UPDATE hr_emp_backup
SET salary = salary * 1.10
WHERE employee_id = 101;

ROLLBACK TO SAVEPOINT after_first;

COMMIT;

-- Answer:
-- Only employee_id 100's salary change is permanent.
-- Employee_id 101's change is rolled back.


-- Q2. Explain COMMIT vs ROLLBACK session visibility.

-- Answer:
-- COMMIT makes the changes permanent and visible
-- to other sessions.
-- ROLLBACK cancels uncommitted changes.
-- Other sessions cannot see uncommitted changes.


-- PART 2: SELF-PRACTICE


-- Q1. Perform two UPDATEs and then one ROLLBACK.
UPDATE hr_emp_backup
SET salary = salary + 500
WHERE employee_id = 100;

UPDATE hr_emp_backup
SET salary = salary + 500
WHERE employee_id = 101;

ROLLBACK;

-- Answer:
-- Both UPDATEs are undone because there was
-- no COMMIT between them.


-- Q2. What privilege is required to query hr.employees?

-- Answer:
-- SELECT privilege is required.

GRANT SELECT ON hr.employees TO user1;

-- This command must be executed by HR/table owner
-- or another user with sufficient privileges.


-- PART 3: 20 MEDIUM QUESTIONS


-- M1. Update one row, COMMIT and verify.
UPDATE hr_emp_backup
SET salary = 6000
WHERE employee_id = 100;

COMMIT;

SELECT *
FROM hr_emp_backup
WHERE employee_id = 100;


-- M2. Update two rows and ROLLBACK.
UPDATE hr_emp_backup
SET salary = 7000
WHERE employee_id = 100;

UPDATE hr_emp_backup
SET salary = 8000
WHERE employee_id = 101;

ROLLBACK;

SELECT *
FROM hr_emp_backup
WHERE employee_id IN (100, 101);

-- Both changes are undone.


-- M3. Update one employee, create savepoint,
-- update another employee and rollback to savepoint.
UPDATE hr_emp_backup
SET salary = salary + 500
WHERE employee_id = 100;

SAVEPOINT sp1;

UPDATE hr_emp_backup
SET salary = salary + 500
WHERE employee_id = 101;

ROLLBACK TO SAVEPOINT sp1;

-- Answer:
-- Employee 100's update remains in the transaction.
-- Employee 101's update is undone.


-- M4. Grant SELECT on hr.employees
-- to role hr_select_role.
CREATE ROLE hr_select_role;

GRANT SELECT ON hr.employees
TO hr_select_role;


-- M5. Revoke SELECT on hr.departments from a user.
REVOKE SELECT ON hr.departments
FROM some_user;


-- M6. Update 100, savepoint, update 101,
-- rollback to savepoint and commit.
UPDATE hr_emp_backup
SET salary = salary * 1.05
WHERE employee_id = 100;

SAVEPOINT sp1;

UPDATE hr_emp_backup
SET salary = salary * 1.05
WHERE employee_id = 101;

ROLLBACK TO SAVEPOINT sp1;

COMMIT;

-- Answer:
-- Only employee 100 gets the new salary permanently.


-- M7. Grant INSERT and UPDATE on backup table to a role.
CREATE ROLE backup_writer;

GRANT INSERT, UPDATE ON hr_emp_backup
TO backup_writer;


-- M8. Update 3 rows, check SQL%ROWCOUNT,
-- then rollback.
BEGIN
    UPDATE hr_emp_backup
    SET salary = salary + 100
    WHERE employee_id IN (100, 101, 102);

    DBMS_OUTPUT.PUT_LINE(
        'Rows updated: ' || SQL%ROWCOUNT
    );

    ROLLBACK;
END;
/

-- Answer:
-- SQL%ROWCOUNT shows the number of rows updated.
-- ROLLBACK then undoes those updates.


-- M9. Create hr_report role and grant SELECT
-- on employees and departments.
CREATE ROLE hr_report;

GRANT SELECT ON hr.employees
TO hr_report;

GRANT SELECT ON hr.departments
TO hr_report;


-- M10. DELETE without COMMIT.
DELETE FROM hr_emp_backup
WHERE employee_id = 100;

-- Answer:
-- Same session: deleted row is no longer visible.
-- Other session: row remains visible until COMMIT.
-- ROLLBACK can restore the row.


-- M11. Use two savepoints and three updates.
UPDATE hr_emp_backup
SET salary = salary + 100
WHERE employee_id = 100;

SAVEPOINT a;

UPDATE hr_emp_backup
SET salary = salary + 100
WHERE employee_id = 101;

SAVEPOINT b;

UPDATE hr_emp_backup
SET salary = salary + 100
WHERE employee_id = 102;

ROLLBACK TO SAVEPOINT a;

COMMIT;

-- Answer:
-- Only employee 100's update is committed.
-- Updates to 101 and 102 are undone.


-- M12. Grant SELECT to user and then revoke it.
GRANT SELECT ON hr.employees
TO user1;

REVOKE SELECT ON hr.employees
FROM user1;


-- M13. Run two UPDATEs and COMMIT.
UPDATE hr_emp_backup
SET salary = salary + 100
WHERE department_id = 50;

UPDATE hr_emp_backup
SET salary = salary + 200
WHERE department_id = 60;

COMMIT;

-- Answer:
-- All rows affected by both UPDATE statements
-- are committed together.


-- M14. Create role with SELECT on departments only.
CREATE ROLE dept_reader;

GRANT SELECT ON hr.departments
TO dept_reader;


-- M15. Update, verify using SELECT, then ROLLBACK.
UPDATE hr_emp_backup
SET salary = 9000
WHERE employee_id = 100;

SELECT employee_id, salary
FROM hr_emp_backup
WHERE employee_id = 100;

ROLLBACK;

-- Answer:
-- ROLLBACK is useful when verification shows
-- that the change should not be saved.


-- M16. Use two savepoints and rollback to sp1.
UPDATE hr_emp_backup
SET salary = salary + 100
WHERE employee_id = 100;

SAVEPOINT sp1;

UPDATE hr_emp_backup
SET salary = salary + 100
WHERE employee_id = 101;

SAVEPOINT sp2;

ROLLBACK TO SAVEPOINT sp1;

-- Answer:
-- Employee 101's update is undone.
-- Employee 100's update remains.


-- M17. Privileges required to create a table
-- and insert into hr.employees.

GRANT CREATE TABLE TO user1;

GRANT INSERT ON hr.employees
TO user1;

-- Answer:
-- CREATE TABLE = system privilege.
-- INSERT on hr.employees = object privilege.
-- Tablespace quota may also be required.


-- M18. First UPDATE + COMMIT,
-- second UPDATE + ROLLBACK.
UPDATE hr_emp_backup
SET salary = salary + 100
WHERE employee_id = 100;

COMMIT;

UPDATE hr_emp_backup
SET salary = salary + 100
WHERE employee_id = 101;

ROLLBACK;

-- Answer:
-- Employee 100's update remains permanent.
-- Employee 101's update is undone.


-- M19. Grant hr_reader role to app_user.
GRANT hr_reader TO app_user;

-- Answer:
-- app_user gets the privileges granted
-- to the hr_reader role.


-- M20. Delete 5 rows and rollback.
DELETE FROM hr_emp_backup
WHERE employee_id IN (100, 101, 102, 103, 104);

ROLLBACK;

SELECT *
FROM hr_emp_backup
WHERE employee_id IN (100, 101, 102, 103, 104);

-- Answer:
-- The deleted rows are restored.


-- PART 4: 20 HARD QUESTIONS


-- H1. Update 10 rows and check SQL%ROWCOUNT.
-- Commit only when exactly 10 rows are updated.
BEGIN
    UPDATE hr_emp_backup
    SET salary = salary + 100
    WHERE employee_id IN
          (100,101,102,103,104,
           105,106,107,108,109);

    IF SQL%ROWCOUNT != 10 THEN
        ROLLBACK;
    ELSE
        COMMIT;
    END IF;
END;
/


-- H2. Three updates with two savepoints.
UPDATE hr_emp_backup
SET salary = salary + 100
WHERE employee_id = 100;

SAVEPOINT sp1;

UPDATE hr_emp_backup
SET salary = salary + 100
WHERE employee_id = 101;

SAVEPOINT sp2;

UPDATE hr_emp_backup
SET salary = salary + 100
WHERE employee_id = 102;

ROLLBACK TO SAVEPOINT sp1;

COMMIT;

-- Answer:
-- Only the first UPDATE is permanent.
-- Updates 2 and 3 are undone.


-- H3. Grant SELECT, INSERT and UPDATE to role,
-- then revoke UPDATE.
CREATE ROLE hr_hrw;

GRANT SELECT, INSERT, UPDATE
ON hr.employees
TO hr_hrw;

REVOKE UPDATE
ON hr.employees
FROM hr_hrw;

-- hr_hrw keeps SELECT and INSERT.


-- H4. Update departments 50, 60 and 70
-- using a savepoint.
UPDATE hr_emp_backup
SET salary = salary * 1.05
WHERE department_id = 50;

SAVEPOINT sp1;

UPDATE hr_emp_backup
SET salary = salary * 1.05
WHERE department_id = 60;

ROLLBACK TO SAVEPOINT sp1;

UPDATE hr_emp_backup
SET salary = salary * 1.05
WHERE department_id = 70;

COMMIT;

-- Answer:
-- Departments 50 and 70 are updated.
-- Department 60 update is undone.


-- H5. Session A updates a row without COMMIT.
-- Session B tries to update the same row.

-- Answer:
-- Session B waits because Session A holds
-- a lock on that row.
-- After Session A COMMITs or ROLLBACKs,
-- Session B can continue.


-- H6. Create role with SELECT on both tables
-- and grant it to two users.
CREATE ROLE hr_reader;

GRANT SELECT ON hr.employees
TO hr_reader;

GRANT SELECT ON hr.departments
TO hr_reader;

GRANT hr_reader TO user1;

GRANT hr_reader TO user2;


-- H7. UPDATE, savepoint, DELETE,
-- rollback to savepoint and commit.
UPDATE hr_emp_backup
SET salary = salary + 100
WHERE employee_id = 100;

SAVEPOINT sp1;

DELETE FROM hr_emp_backup
WHERE employee_id = 101;

ROLLBACK TO SAVEPOINT sp1;

COMMIT;

-- Answer:
-- Employee 101 is NOT deleted.
-- Only employee 100's update is committed.


-- H8. What privilege is required for
-- SELECT * FROM hr.employees?

-- Answer:
-- SELECT object privilege.

GRANT SELECT ON hr.employees
TO user1;


-- H9. Insert one row, savepoint,
-- insert another, rollback and commit.
INSERT INTO hr_emp_backup
(employee_id, first_name, last_name)
VALUES
(991, 'John', 'One');

SAVEPOINT sp1;

INSERT INTO hr_emp_backup
(employee_id, first_name, last_name)
VALUES
(992, 'John', 'Two');

ROLLBACK TO SAVEPOINT sp1;

COMMIT;

-- Answer:
-- Only employee 991 remains.
-- Employee 992 is rolled back.


-- H10. Grant role to user and then revoke role.
CREATE ROLE emp_reader;

GRANT SELECT ON hr.employees
TO emp_reader;

GRANT emp_reader TO user1;

REVOKE emp_reader FROM user1;

-- Answer:
-- user1 can no longer query hr.employees
-- through emp_reader.


-- H11. Update 3 rows and rollback only
-- the third update.
UPDATE hr_emp_backup
SET salary = salary + 100
WHERE employee_id = 100;

UPDATE hr_emp_backup
SET salary = salary + 100
WHERE employee_id = 101;

SAVEPOINT s;

UPDATE hr_emp_backup
SET salary = salary + 100
WHERE employee_id = 102;

ROLLBACK TO SAVEPOINT s;

COMMIT;

-- Answer:
-- Updates for 100 and 101 are committed.
-- Update for 102 is undone.


-- H12. Revoke SELECT from a role.
REVOKE SELECT ON hr.employees
FROM hr_reader;

-- Answer:
-- Users relying on hr_reader lose SELECT
-- access through that role.


-- H13. Delete department 50, savepoint,
-- delete department 60, rollback and commit.
DELETE FROM hr_emp_backup
WHERE department_id = 50;

SAVEPOINT sp1;

DELETE FROM hr_emp_backup
WHERE department_id = 60;

ROLLBACK TO SAVEPOINT sp1;

COMMIT;

-- Answer:
-- Department 50 rows are deleted permanently.
-- Department 60 deletion is undone.


-- H14. Role chaining.
CREATE ROLE role_a;

GRANT SELECT ON hr.employees
TO role_a;

GRANT role_a TO role_b;

GRANT role_b TO user_c;

-- Answer:
-- user_c receives access through the role chain,
-- subject to Oracle role configuration/privileges.


-- H15. Five updates with savepoints.
UPDATE hr_emp_backup
SET salary = salary + 10
WHERE employee_id = 100;

SAVEPOINT s1;

UPDATE hr_emp_backup
SET salary = salary + 10
WHERE employee_id = 101;

SAVEPOINT s2;

UPDATE hr_emp_backup
SET salary = salary + 10
WHERE employee_id = 102;

SAVEPOINT s3;

UPDATE hr_emp_backup
SET salary = salary + 10
WHERE employee_id = 103;

SAVEPOINT s4;

UPDATE hr_emp_backup
SET salary = salary + 10
WHERE employee_id = 104;

SAVEPOINT s5;

ROLLBACK TO SAVEPOINT s2;

COMMIT;

-- Answer:
-- First two UPDATEs remain.
-- Updates 3, 4 and 5 are undone.


-- H16. Revoke INSERT from a role.
REVOKE INSERT ON hr.employees
FROM hr_hrw;

-- Answer:
-- Users who receive INSERT through hr_hrw
-- lose that INSERT privilege.


-- H17. Update without COMMIT and check
-- the same rows from another session.
UPDATE hr_emp_backup
SET salary = salary + 100
WHERE employee_id = 100;

-- Answer:
-- Current session sees the new value.
-- Another session sees the old committed value.
-- After COMMIT, new queries in the other session
-- can see the new value.


-- H18. Savepoint before a risky UPDATE.
DECLARE
    v_rows NUMBER;
BEGIN
    SAVEPOINT before_update;

    UPDATE hr_emp_backup
    SET salary = salary * 1.10
    WHERE department_id = 50;

    v_rows := SQL%ROWCOUNT;

    IF v_rows > 0 THEN
        COMMIT;
    ELSE
        ROLLBACK TO SAVEPOINT before_update;
    END IF;
END;
/


-- H19. Grant SELECT to user X.
GRANT SELECT ON hr.employees
TO user_x;

-- Conceptual Answer:
-- User X can create a view if X also has the
-- necessary CREATE VIEW privilege and direct
-- privileges required by Oracle.
-- X cannot grant user Y direct SELECT on
-- hr.employees unless X has GRANT OPTION.


-- H20. Insert row 1, savepoint A,
-- insert row 2, savepoint B,
-- delete row 1, rollback to B, commit.

INSERT INTO hr_emp_backup
(employee_id, first_name, last_name)
VALUES
(991, 'Row', 'One');

SAVEPOINT a;

INSERT INTO hr_emp_backup
(employee_id, first_name, last_name)
VALUES
(992, 'Row', 'Two');

SAVEPOINT b;

DELETE FROM hr_emp_backup
WHERE employee_id = 991;

ROLLBACK TO SAVEPOINT b;

COMMIT;

-- Answer:
-- Both employee 991 and 992 remain.
-- The DELETE of employee 991 was rolled back.
