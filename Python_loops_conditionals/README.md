# Lab: Python loops conditionals

## Overview

This lab documents my hands‑on experience learning **Python loops** (`for` and `while`) and **conditional statements** (`if`/`else`). I worked through 8 tasks that simulate real‑world scenarios: network connection messages, allow‑list IP address checks, and employee ID generation. Each task helped me understand how to control the flow of a program, repeat actions efficiently, and make decisions inside loops.

---

## Task 1 – Basic `for` loop with `range()`

I wrote a `for` loop that prints `"Connection could not be established."` three times. Using `range(3)` gave me exactly three iterations.

```python
for i in range(3):
    print("Connection could not be established.")

What I learned: range(n) produces a sequence of n numbers; the loop body runs n times.
Task 2 – Using a variable with range()

I stored the number of attempts in a variable connection_attempts and passed it to range(). This makes the loop flexible – I can change the value in one place.
python

connection_attempts = 3
for i in range(connection_attempts):
    print("Connection could not be established.")

What I learned: Variables can control loop repetition; code becomes easier to modify.
Task 3 – while loop with a counter

I rewrote the same logic using a while loop. I manually initialised connection_attempts = 0, checked the condition connection_attempts < 3, and incremented the counter inside the loop.
python

connection_attempts = 0
while connection_attempts < 3:
    print("Connection could not be established.")
    connection_attempts = connection_attempts + 1

What I learned: while loops give more control but require manual counter management. They are useful when the number of iterations is not known in advance.
Task 4 – Iterating over a list

I looped through a list of IP addresses (ip_addresses) and printed each one.
python

ip_addresses = ["192.168.142.245", "192.168.109.50", ...]
for i in ip_addresses:
    print(i)

What I learned: A for loop can directly iterate over the elements of a list, not just numbers.
Task 5 – Adding conditional logic inside a loop

Inside the loop, I checked whether each IP address belongs to an allow_list. I used if i in allow_list: and printed different messages.
python

for i in ip_addresses:
    if i in allow_list:
        print("IP address is allowed")
    else:
        print("IP address is not allowed")

What I learned: The in operator checks membership. Loops combined with if/else allow decision‑making for each element.
Task 6 – Using break to exit a loop early

For security reasons, I had to stop the loop as soon as a disallowed IP address was found. I used break and extended the error message.
python

for i in ip_addresses:
    if i in allow_list:
        print("IP address is allowed")
    else:
        print("IP address is not allowed. Further investigation of login activity required")
        break

What I learned: break immediately terminates the loop. This is valuable when continuing could be unsafe or unnecessary.
Task 7 – Generating employee IDs with a while loop

I needed to create unique IDs divisible by 5, between 5000 and 5150 (inclusive). A while loop with a step of 5 worked perfectly.
python

i = 5000
while i <= 5150:
    print(i)
    i = i + 5

What I learned: while loops can generate sequences with custom steps. I must update the loop variable to avoid infinite loops.
Task 8 – Adding an alert inside the loop

When i reached 5100 (meaning only 10 IDs remain), I displayed a special message. I placed the conditional after printing the ID so that every ID is still shown.
python

while i <= 5150:
    print(i)
    if i == 5100:
        print("Only 10 valid employee ids remaining")
    i = i + 5

What I learned: Code placement inside a loop matters. General actions go before special‑case conditions.
Conclusion

This lab gave me hands‑on experience with the fundamental building blocks of Python iteration and conditionals. I can now:

    Choose between for and while based on whether I know the number of iterations in advance.

    Use range() with constants or variables to control repetition.

    Loop through lists and apply if/else logic per element.

    Stop a loop early with break when a critical condition is met.

    Generate sequences with custom steps using while.

    Place conditional alerts correctly without disrupting normal output.

These skills are directly applicable to real‑world security tasks: checking login attempts, filtering allowed IP addresses, generating IDs, and triggering alerts. I feel more confident writing Python code that automates repetitive decisions.

Lab completed as part of my Python learning journey.
