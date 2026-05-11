# SQL Lab: Investigating Login Attempts

## Objective

This hands-on lab demonstrates my ability to use **SQL** for security incident investigation.  
I queried a fictional `log_in_attempts` table to filter login events by date, time, and event ID.  
The goal: Hands‑on practice with writing precise SQL queries to extract actionable insights from database logs.

## Lab Environment

- **Database**: fictional security logs table `log_in_attempts`
- **Columns used**: `login_date`, `login_time`, `event_id`, `username`
- **Tools**: any standard SQL interface (e.g., MySQL, SQLite, PostgreSQL)

---

## Task 1 – Retrieve login attempts after a certain date

**Scenario**: A security incident occurred after `2022-05-09`. I need all login attempts made after that date.

### Query 1.1 – after a specific date

```sql
SELECT * 
FROM log_in_attempts 
WHERE login_date > '2022-05-09';

Result: 125 login attempts were made after 2022-05-09.
Query 1.2 – including the start date

To expand the range and also include 2022-05-09 itself:
sql

SELECT * 
FROM log_in_attempts 
WHERE login_date >= '2022-05-09';

Result: 165 login attempts on or after 2022-05-09.
Task 2 – Retrieve logins in a date range

Requirement: Narrow the search to logins between 2022-05-09 and 2022-05-11 (inclusive).
Query
sql

SELECT * 
FROM log_in_attempts 
WHERE login_date BETWEEN '2022-05-09' AND '2022-05-11';

Outcome: Returns all login attempts within that three‑day window.
Task 3 – Investigate logins at certain times

Goal: Find logins outside typical working hours (before 07:00:00) and then focus on the 06:00‑07:00 window.
Query 3.1 – before 07:00:00
sql

SELECT * 
FROM log_in_attempts 
WHERE login_time < '07:00:00';

Result: The fifth record returned has the username eraab.
Query 3.2 – between 06:00:00 and 07:00:00
sql

SELECT * 
FROM log_in_attempts 
WHERE login_time BETWEEN '06:00:00' AND '07:00:00';

Result: The first record shows a login time of 06:01:31.
Task 4 – Investigate logins by event ID

Goal: Filter based on numeric event_id and return only relevant columns.
Query 4.1 – event_id ≥ 100
sql

SELECT event_id, username, login_date 
FROM log_in_attempts 
WHERE event_id >= 100;

Result: The third record has the login date 2022-05-09.
Query 4.2 – event_id between 100 and 150 (inclusive)
sql

SELECT event_id, username, login_date
FROM log_in_attempts 
WHERE event_id BETWEEN 100 AND 150;

Result: The seventh record shows the username tmitchel.
Key SQL Skills Demonstrated
Skill	Example
Filtering with comparison operators	WHERE login_date > '2022-05-09'
Inclusive date ranges	WHERE login_date >= '2022-05-09'
Range filtering with BETWEEN	WHERE login_date BETWEEN ... AND ...
Time comparisons	WHERE login_time < '07:00:00'
Selecting specific columns	SELECT event_id, username, login_date
Numeric filtering (no quotes)	WHERE event_id >= 100
Conclusion

Through this lab I successfully:

    Retrieved login attempts after and on specific dates.

    Narrowed searches to a tight date range using BETWEEN.

    Investigated login times outside normal working hours.

    Filtered by numeric event IDs with precise column selection.

These queries are directly applicable to real‑world security investigations – proving my ability to use SQL for log analysis and incident response.
text
