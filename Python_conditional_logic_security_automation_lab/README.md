# Python Lab: Conditional Logic for Security Automation – My Learning Journey

## Scope

In this fictional lab, I acted as a security analyst writing Python code to automate two key tasks:

1. Checking whether a user’s operating system requires an update (OS 2 is up‑to‑date; OS 1 and OS 3 need updates).
2. Verifying login attempts against an approved user list and organization hours.

The lab focused on **conditional statements** (`if`, `elif`, `else`), **logical operators** (`and`, `or`), the `in` operator, and Boolean variables. This document records all 10 tasks with my code and observations.

---

## Task 1 – Basic `if` Condition

**Scenario:** Display `"no update needed"` when the running system is `"OS 2"`.

**My code:**
```python
system = "OS 2"
if system == "OS 2":
    print("no update needed")

Output: no update needed

Observation: The if statement runs only when the condition is True.
Task 2 – Testing Different Values

Scenario: Change system to "OS 1", "OS 2", or "OS 3" and observe.

My code:
python

system = "OS 1"   # also tried "OS 2", "OS 3"
if system == "OS 2":
    print("no update needed")

Results:

    "OS 2" → prints "no update needed"

    "OS 1" or "OS 3" → nothing printed

Observation: Without else or elif, no message appears for non‑OS‑2 systems.
Task 3 – Adding else

Scenario: Provide an alternative message when an update is needed.

My code:
python

system = "OS 1"
if system == "OS 2":
    print("no update needed")
else:
    print("update needed")

Output: update needed

Observation: else catches all other cases, but it does not distinguish OS 1 from OS 3 or invalid inputs.
Task 4 – Using elif for Multiple Conditions

Scenario: Treat OS 1 and OS 3 separately (both need updates), but ignore other values.

My code:
python

system = "OS 3"
if system == "OS 2":
    print("no update needed")
elif system == "OS 1":
    print("update needed")
elif system == "OS 3":
    print("update needed")

Results:

    "OS 2" → "no update needed"

    "OS 1" or "OS 3" → "update needed"

    "OS 4" (or any other string) → no output

Observation: elif allows precise control, but the two elif blocks repeat the same message.
Task 5 – Conciseness with or

Scenario: Combine the two elif statements into one using a logical operator.

My code:
python

system = "OS 1"
if system == "OS 2":
    print("no update needed")
elif system == "OS 1" or system == "OS 3":
    print("update needed")

Results: Works correctly for OS 1, OS 2, OS 3, and other values.

Observation: Using or makes the code cleaner and avoids repetition. (Note: and would be wrong because a variable cannot equal two values at once.)
Task 6 – Checking Against Two Approved Users

Scenario: Two approved users ("elarson", "bmoreno"). Display access granted or denied.

My code:
python

approved_user1 = "elarson"
approved_user2 = "bmoreno"
username = "bmoreno"

if username == approved_user1 or username == approved_user2:
    print("This user has access to this device.")
else:
    print("This user does not have access to this device.")

Output: This user has access to this device.

Observation: Using or with two individual variables works, but it becomes messy with more users.
Task 7 – Using in with a List

Scenario: Five approved users stored in an approved_list. Use in to check membership.

My code:
python

approved_list = ["elarson", "bmoreno", "tshah", "sgilmore", "eraab"]
username = "bmoreno"

if username in approved_list:
    print("This user has access to this device.")
else:
    print("This user does not have access to this device.")

Results:

    "bmoreno" → "This user has access to this device."

    "unknown" → "This user does not have access to this device."

Observation: The in operator is concise and scales easily to many approved users.
Task 8 – Checking Boolean Variable (Organization Hours)

Scenario: organization_hours is True (during hours) or False (outside). Display appropriate message.

My code:
python

organization_hours = True
if organization_hours:
    print("Login attempt made during organization hours.")
else:
    print("Login attempt made outside of organization hours.")

Output: Login attempt made during organization hours.

Observation: A Boolean variable can be used directly as the condition – no need to write == True.
Task 9 – Combining Both Checks (Independent)

Scenario: Run both checks separately – approval status and organization hours.

My code:
python

approved_list = ["elarson", "bmoreno", "tshah", "sgilmore", "eraab"]
username = "bmoreno"

if username in approved_list:
    print("This user has access to this device.")
else:
    print("This user does not have access to this device.")

organization_hours = True
if organization_hours == True:
    print("Login attempt made during organization hours.")
else:
    print("Login attempt made outside of organization hours.")

Output:
text

This user has access to this device.
Login attempt made during organization hours.

Observation: The two conditions are independent – both messages can appear. This is useful but not concise when a single combined message is desired.
Task 10 – Combining Both Checks with and

Scenario: Display a single message only when both conditions are met (approved user and during organization hours). Otherwise, a generic denial message.

My code:
python

approved_list = ["elarson", "bmoreno", "tshah", "sgilmore", "eraab"]
username = "bmoreno"
organization_hours = True

if username in approved_list and organization_hours == True:
    print("Login attempt made by an approved user during organization hours.")
else:
    print("Username not approved or login attempt made outside of organization hours.")

Output: Login attempt made by an approved user during organization hours.

Observation: Using and combines both requirements into one if. The else covers all failure cases. This is much cleaner for reporting a single outcome.
Conclusion

Through these 10 tasks, I learned:

    if/elif/else let me control program flow based on conditions.

    Logical operators and and or combine conditions for precise logic.

    The in operator simplifies checking membership in lists.

    Boolean variables can stand alone as conditions.

    Combining multiple checks into one if statement makes code more concise and readable.

These skills directly apply to real security automation: verifying approved users, checking access hours, and determining update requirements. I am building a solid foundation for using Python in cybersecurity tasks.

This lab was completed as part of a Python learning exercise. All scenarios are fictional.
