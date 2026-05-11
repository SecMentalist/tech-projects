# 🧪 Hands‑On SQL Lab: Joining Tables for Security Investigations

## My Objective

In this lab, I practiced using **SQL joins** (`INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`) to connect data across multiple tables.  
The scenario simulates a real security investigation where I need to:

- Match employees to the machines they use  
- Find all machines and all employees (including unassigned ones)  
- Retrieve login attempt data for each employee  

All queries are written from my perspective as a security analyst working with a fictional database.

---

## 🗄️ Database Tables I Worked With

| Table | Columns |
|-------|---------|
| `machines` | `device_id`, `operating_system`, `purchase_date` |
| `employees` | `employee_id`, `username`, `first_name`, `last_name`, `device_id`, `department` |
| `log_in_attempts` | `login_id`, `username`, `login_date`, `login_time`, `success` |

**Linking columns used**:
- `machines.device_id` ↔ `employees.device_id`
- `employees.username` ↔ `log_in_attempts.username`

---

## 📋 My Hands‑On Tasks

### Task 1 – Match employees to their machines (INNER JOIN)

**The need**: Identify which employees are using which machines. Only employees with an assigned machine should appear.

**Query I completed** (filling in `X` and `Y` with `device_id`):

```sql
SELECT * 
FROM machines 
INNER JOIN employees ON machines.device_id = employees.device_id;

What I learned: INNER JOIN returns only rows where the joining column exists in both tables. No stray machines or unassigned employees appear.
Task 2 – Return more data (LEFT JOIN & RIGHT JOIN)

The need: I needed two different views:

    All machines – including those not yet assigned to any employee

    All employees – including those who don't have a machine

Part A – Left Join (all machines)

I replaced X with LEFT:
sql

SELECT * 
FROM machines 
LEFT JOIN employees ON machines.device_id = employees.device_id;

Result: Every machine appears. If an employee is assigned, their data shows up; otherwise, NULL for employee columns.
Part B – Right Join (all employees)

I switched to a RIGHT JOIN to keep all employees:
sql

SELECT * 
FROM machines 
RIGHT JOIN employees ON machines.device_id = employees.device_id;

Result: Every employee appears. Machine data is NULL for those without a device.

    My takeaway: LEFT and RIGHT joins are like two sides of the same coin – they let me decide which table stays complete.

Task 3 – Retrieve login attempt data (INNER JOIN)

The need: Get a list of all employees who have made login attempts, together with the attempt details.

I replaced X, Y, and Z in the template:
sql

SELECT * 
FROM employees 
INNER JOIN log_in_attempts ON employees.username = log_in_attempts.username;

Result: Only employees who have actually tried to log in appear (along with each login attempt). Employees with zero login attempts are excluded – that’s exactly what INNER JOIN does.
📊 Summary of Joins I Practiced
Join Type	My Query Example	What It Returns
INNER JOIN	machines INNER JOIN employees ON ...	Only rows where device_id exists in both tables
LEFT JOIN	machines LEFT JOIN employees ON ...	All rows from machines + matching rows from employees
RIGHT JOIN	machines RIGHT JOIN employees ON ...	All rows from employees + matching rows from machines
🧠 Key Takeaways from This Hands‑On Lab

    INNER JOIN is perfect for finding existing relationships (e.g., assigned machines).

    LEFT JOIN and RIGHT JOIN help me see everything from one table even if there’s no match in the other.

    The common linking column (device_id, username) is the glue – without it, the join makes no sense.

    In security investigations, joins let me combine login logs, machine inventory, and employee directories to get a complete picture.

✅ Conclusion

Through these three tasks, I gained hands‑on confidence with SQL joins. I can now:

    Match employees to their machines using INNER JOIN

    Use LEFT and RIGHT joins to preserve all rows from one side

    Connect login attempts back to employee records

These are essential skills for any security analyst working with relational databases.

This lab uses a fictional database modeled after the Chinook schema, designed for safe, hands‑on practice.
