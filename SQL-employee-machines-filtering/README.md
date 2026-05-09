# 🧪 SQL Lab: Employee & Machine Data Filtering

## 📌 Scenario (fictional)

I work as a **cybersecurity analyst**. I need to get specific information about employees, their machines, and the departments they’re in. My team needs this data to perform various tasks, such as running updates, posting a privacy notice in certain departments, and sending an alert to an employee with an issue on a machine.

I am responsible for finding the required information by querying a database. I’ll add filters to my queries to locate the information more quickly.

Here’s how I’ll do this task:

1. List all organization machines and their operating systems.
2. List all machines with the operating system OS 2.
3. List all the employees in the Finance and Sales departments.
4. Obtain information about machines.

I’m ready to add filters to SQL queries.

> **Note:** In this lab I worked with the fictional `organization` database in a MariaDB shell. 

---

## ✅ What I Did

### Task 1 – List all organization machines

I needed a list of all organization machines and their operating systems from the `machines` table.

**Query I ran:**
```sql
SELECT device_id, operating_system 
FROM machines;

Result:
The output listed only the selected columns from all 200 rows.
Example output snippet:
text

+--------------+------------------+
| device_id    | operating_system |
+--------------+------------------+
| a184b775c707 | OS 1             |
| a192b174c940 | OS 2             |
| a305b818c708 | OS 3             |
| a317b635c465 | OS 1             |
...                              |
+--------------+------------------+
200 rows in set (0.028 sec)

Answer: The machines table returned 200 rows.
Task 2 – Retrieve a list of the machines with OS 2

My team needed a list of all machines with the 'OS 2' operating system because these machines need an update. I used a WHERE filter.

Query I ran:
sql

SELECT device_id, operating_system 
FROM machines 
WHERE operating_system = 'OS 2';

Result:
Output snippet:
text

+--------------+------------------+
| a821b452c176 | OS 2             |
| b157c491d493 | OS 2             |
| b264c773d977 | OS 2             |
...                              |
+--------------+------------------+
80 rows in set (0.264 sec)

Answer: There are 80 machines that use the OS 2 operating system.
Task 3 – List employees in specific departments

I needed to retrieve a list of all employees in the Finance and Sales departments to get their office numbers (a privacy notice would be posted to these offices).
3.1 Filter for Finance department

Query I ran:
sql

SELECT * 
FROM employees 
WHERE department = 'Finance';

Result:
Output showed all employees in the Finance department.

Answer: The employee_id of the first row returned is 1003.
3.2 Filter for Sales department

I modified the query to return employees in the Sales department.

Query I ran:
sql

SELECT * 
FROM employees
WHERE department = 'Sales';

Result:
Output showed all employees in the Sales department.

Answer: There are 33 employees who work in the Sales department.
Task 4 – Identify employee machines (South building issue)
4.1 Find employee using the machine in 'South-109'

A machine in 'South-109' has an issue. I needed to determine which employee uses that computer to send an alert.

Query I ran:
sql

SELECT *
FROM employees
WHERE office = 'South-109';

Answer: The user ID of the employee with the computer issue is jlansky.
4.2 Find all employees in the South building

My team later determined that all machines in the South building have an issue. Offices are named like 'South-109' (building name, hyphen, office number). I modified the query to use the LIKE operator with the % wildcard.

Query I ran:
sql

SELECT *
FROM employees
WHERE office LIKE 'South%';

How LIKE works:

    'South%' finds everything that begins with "South" (e.g., South-109, South-210).

    % is a "fill-in-the-blanks" tool – it represents any number of characters.

Answer: The first employee listed in the South building belongs to the Finance department.
🧠 What I Learned

    SELECT with specific columns returns only the data I need.

    WHERE filters rows based on a condition.

    LIKE '%pattern%' allows flexible string matching.

    Always use single quotes for string values and end queries with a semicolon.

Lab completed using MariaDB + organization database (fictional).
