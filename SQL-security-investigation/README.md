# 🧪 SQL Lab: Investigate Device & Login Activity

## 📌 Scenario (fictional)

I work as a security analyst. I need to retrieve employee device information and investigate user login activity for unusual behavior.

The organization database contains two tables:

- **`machines`** – device info (`device_id`, `operating_system`, `email_client`, `OS_patch_date`, `employee_id`)
- **`log_in_attempts`** – login activity (`event_id`, `username`, `login_date`, `login_time`, `country`, `ip_address`, `success`)

I used SQL queries in a MariaDB shell (with the `organization` database already open).

---

## ✅ What I Did

### Task 1 – Retrieve employee device data

#### 1.1 Get all device information
I ran:
```sql
SELECT * FROM machines;

This returned 200 rows with all columns. I could see device IDs, OS types, email clients, patch dates, and employee IDs.
1.2 Focus on device ID and email client

I selected only those two columns:
sql

SELECT device_id, email_client FROM machines;

The third row showed Email Client 2.
1.3 Get device ID, OS, and patch date

Query:
sql

SELECT device_id, operating_system, OS_patch_date FROM machines;

The first entry had a patch date of 2021-09-01.
Task 2 – Investigate login activity
2.1 Check login locations

I wanted to see if any login attempts came from unexpected countries (the company only operates in North America).
Query:
sql

SELECT event_id, country FROM log_in_attempts;

I scanned the results – no login attempts from Australia.
2.2 Check login times (unusual hours)

I retrieved usernames, dates, and times:
sql

SELECT username, login_date, login_time FROM log_in_attempts;

The fifth row returned username jrafael.
2.3 Get complete login data

I selected all columns to get a full picture:
sql

SELECT * FROM log_in_attempts;

I manually scanned 200 rows – a good learning exercise. Later I'll use WHERE to filter.
Task 3 – Order login attempts data
3.1 Sort by login date

I needed to see the earliest logins first:
sql

SELECT * FROM log_in_attempts ORDER BY login_date;

The first record showed username ivelasco on 2022-05-08.
3.2 Sort by date and then time

To get a perfect chronological list:
sql

SELECT * FROM log_in_attempts ORDER BY login_date, login_time;

The first record now had username bsand and login time 00:19:11.
🧠 What I Learned

    SELECT * returns all columns; specifying columns gives focused data.

    Login attempts from unexpected countries or odd hours are red flags.

    ORDER BY with multiple columns sorts by the first column first, then the second within matching values.
