# Python Lab: Automating Login Attempt Analysis – My Learning Journey

## Scope

In this fictional lab, I acted as a security analyst responsible for writing Python code to automate the analysis of login attempts to a specific device. The scope included:

- Creating variables to store device ID, approved usernames, maximum login attempts, current login attempts, and login status.
- Checking and understanding data types (`str`, `list`, `int`, `bool`).
- Updating lists when new users are granted access.
- Using comparison operators to evaluate login attempts against limits.
- Experimenting with different values to observe Boolean outputs.

This document records each scenario (Tasks 1–10) along with the exact code I wrote and the observations I made.

---

## Scenario 1 – Assign and Display a Device ID

**Description:** The device has ID `"72e08x0"`. Only users on an allow list can access it. I assigned this value to a variable and printed it.

**My code:**
```python
device_id = "72e08x0"
print(device_id)
Output: 72e08x0

Scenario 2 – Check Data Type of Device ID
Description: I used type() to find what kind of data device_id holds.
My code:
python
device_id = "72e08x0"
device_id_type = type(device_id)
print(device_id_type)
Output: <class 'str'>
Observation: device_id is a string – perfect for text like IDs.

Scenario 3 – Create a List of Approved Usernames
Description: Approved usernames: "madebowa", "jnguyen", "tbecker", "nhersh", "redwards". I stored them in a list.
My code:
python
username_list = ["madebowa", "jnguyen", "tbecker", "nhersh", "redwards"]
print(username_list)
Output: ['madebowa', 'jnguyen', 'tbecker', 'nhersh', 'redwards']

Scenario 4 – Data Type of the Username List
Description: I verified the data type of the list variable.
My code:
python
username_list = ["madebowa", "jnguyen", "tbecker", "nhersh", "redwards"]
username_list_type = type(username_list)
print(username_list_type)
Output: <class 'list'>
Observation: Python recognizes it as a list, which can hold multiple values.

Scenario 5 – Update the List with a New User
Description: A new employee "lpope" now has access. I reassigned username_list to an updated list and printed both before and after.
My code:
python
username_list = ["madebowa", "jnguyen", "tbecker", "nhersh", "redwards"]
print(username_list)

username_list = ["madebowa", "jnguyen", "tbecker", "nhersh", "redwards", "lpope"]
print(username_list)
Output:
text
['madebowa', 'jnguyen', 'tbecker', 'nhersh', 'redwards']
['madebowa', 'jnguyen', 'tbecker', 'nhersh', 'redwards', 'lpope']
Observation: Lists are mutable – I can easily update them as access rules change.

Scenario 6 – Maximum Login Attempts (Integer)
Description: The system allows a maximum of 3 login attempts per user. I stored this as an integer.
My code:
python
max_logins = 3
max_logins_type = type(max_logins)
print(max_logins_type)
Output: <class 'int'>

Scenario 7 – Current Login Attempts (Integer)
Description: A user has made 2 login attempts so far. I stored this as another integer.
My code:
python
login_attempts = 2
login_attempts_type = type(login_attempts)
print(login_attempts_type)
Output: <class 'int'>

Scenario 8 – Compare Login Attempts to Maximum
Description: I compared login_attempts (2) with max_logins (3) using the <= operator to get a Boolean result – whether the current attempts are within the allowed limit.
My code:
python
max_logins = 3
login_attempts = 2
print(login_attempts <= max_logins)
Output: True
Observation: The output is True because 2 is less than or equal to 3. The data type of login_attempts is int (integer), as it stores the whole number 2. This comparison returns a Boolean value useful for access logic.

Scenario 9 – Test Different Login Attempt Values
Description: I reassigned login_attempts to various values (including values higher than max_logins) and used the <= comparison operator again.
My test code (example with 5):
python
max_logins = 3
login_attempts = 5
print(login_attempts <= max_logins)
Output: False
Additional tests I ran:
login_attempts = 2 → True
login_attempts = 3 → True
login_attempts = 4 → False
Observation: The comparison returns True only when login_attempts is less than or equal to max_logins. This is useful for automating lockout decisions.

Scenario 10 – Boolean Variable for Login Status
Description: I created a variable to represent whether a user is currently logged in, initially set to False.
My code:
python
login_status = False
login_status_type = type(login_status)
print(login_status_type)
Output: <class 'bool'>
Observation: Boolean variables hold only True or False, ideal for status flags.

Summary of Data Types Encountered
Data Type
Example Value
Use in Lab
str
"72e08x0"
Device ID
list
["madebowa", ...]
Approved usernames
int
3, 2
Max attempts, current attempts
bool
False
Login status

Key Takeaways
type() is essential to verify what kind of data a variable holds.
Strings store text (e.g., IDs).
Lists store ordered collections and can be updated (mutable).
Integers store whole numbers for counting.
Booleans are perfect for yes/no, true/false states.
Comparison operators (like <=) return Booleans, enabling automated logic.
Conclusion
Through these 10 scenarios, I learned how to create variables of different data types, check their types with type(), update lists, and use Boolean comparisons – all within a realistic security automation context. This lab builds a strong foundation for using Python in cybersecurity tasks.

