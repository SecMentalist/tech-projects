# 🧪 SQL Lab: Logical Operators for Security Investigations (AND, OR, NOT)

## 📌 Objective

This hands‑on lab demonstrates how to use **SQL logical operators** (`AND`, `OR`, `NOT`) to filter security‑related data from a fictional organization database.  
The goal is to practice writing precise queries that help a security team:

- Investigate failed login attempts after hours
- Find login activity on specific dates
- Exclude logins from a certain country
- Filter employee records by department and office location

All queries are run against two tables: `log_in_attempts` and `employees`.

---

## 🗄️ Lab Environment

- **Database**: `organization` (MariaDB / MySQL)
- **Tables**:
  - `log_in_attempts` – columns: `login_time`, `success`, `login_date`, `country`
  - `employees` – columns: `department`, `office`, `username`
- **Tools**: MariaDB shell (or any SQL interface)

> Note: MySQL stores Boolean `TRUE` as `1`, `FALSE` as `0`.

---

## 📋 Tasks & Queries

### Task 1 – Retrieve after‑hours failed login attempts

**Scenario**: Find all unsuccessful login attempts that occurred after 18:00 (business hours end at 18:00).

**Query**:
```sql
SELECT *
FROM log_in_attempts
WHERE login_time > '18:00' AND success = 0;

Result: 19 failed login attempts after 18:00.

✅ Operator used: AND
Task 2 – Retrieve login attempts on specific dates

Scenario: Get all login attempts on 2022-05-09 (suspicious event) and the day before (2022-05-08).

Query:
sql

SELECT * 
FROM log_in_attempts 
WHERE login_date = '2022-05-09' OR login_date = '2022-05-08';

Result: 75 login attempts on those two days.

✅ Operator used: OR
Task 3 – Retrieve login attempts outside of Mexico

Scenario: Find logins that did not originate in Mexico. The country field contains 'MEX' or 'MEXICO'.

Query:
sql

SELECT * 
FROM log_in_attempts
WHERE NOT country LIKE 'MEX%';

Result: 144 login attempts made outside Mexico.

✅ Operators used: NOT + LIKE (pattern matching)
Task 4 – Retrieve employees in Marketing (East building)

Scenario: Get all employees from the 'Marketing' department whose office is in the East building (e.g., 'East-170', 'East-320').

Query:
sql

SELECT *
FROM employees
WHERE department = 'Marketing' AND office LIKE 'East%';

Result: The first username returned is elarson.

✅ Operators used: AND + LIKE
Task 5 – Retrieve employees in Finance or Sales

Scenario: Get all employees who work in either the 'Finance' department or the 'Sales' department.

Query:
sql

SELECT *
FROM employees
WHERE department = 'Finance' OR department = 'Sales';

Result: The first username in Sales department is lrodriqu.

✅ Operator used: OR
Task 6 – Retrieve all employees not in IT

Scenario: The Information Technology department has already received updates. Find all employees who are not in that department.

Query:
sql

SELECT *
FROM employees
WHERE NOT department = 'Information Technology';

Result: 161 employees are not in IT.

✅ Operator used: NOT
📊 Summary of SQL Techniques
Task	Operators / Patterns	Purpose
1	AND	Combine two conditions (time + failure)
2	OR	Match either of two dates
3	NOT + LIKE	Exclude rows matching a pattern
4	AND + LIKE	Department + office building filter
5	OR	Multiple department values
6	NOT	Exclude a single department
🧠 Key Takeaways

    AND narrows results – all conditions must be true.

    OR broadens results – any condition can be true.

    NOT excludes results – useful with LIKE or = .

    Boolean values (TRUE/FALSE) are stored as 1/0 in MySQL.

    Pattern matching with LIKE and % wildcard helps find partial matches (e.g., 'East%').

✅ Conclusion

This lab provided hands‑on practice with logical operators in SQL. I successfully filtered login attempts and employee records using AND, OR, and NOT. These skills are directly applicable to real‑world security investigations – querying authentication logs, isolating suspicious activity, and preparing employee data for system updates.

Fictional database for educational purposes.
