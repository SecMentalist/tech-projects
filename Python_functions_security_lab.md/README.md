# Python Functions Lab: Security Alert & Username Formatting

## Introduction

As a security analyst, automating repetitive tasks with Python is essential. Functions allow me to write reusable blocks of code, making my scripts more efficient and maintainable. In this lab, I practiced defining and calling my own functions, working with loops, and using string concatenation to transform data.

## Scenario

Writing functions in Python is a useful skill in my work as a security analyst. In this lab, I defined and called a function that displays an alert about a potential security issue. I also worked with a list of employee usernames, creating a function that converts the list into one string.

---

## Task 1: Analyze a Function Definition

I examined a user-defined function named `alert()`. This function prints a security message.

```python
def alert():
    print("Potential security issue. Investigate further.")

Observation: When called, it outputs a single alert line.
Task 2: Call the Alert Function

I called the alert() function and observed the output.
python

def alert():
    print("Potential security issue. Investigate further.")

alert()

Output:
Potential security issue. Investigate further.

Advantages of using a function:

    Reusability – call it many times without rewriting code.

    Modularity – keeps the alert logic separate and easy to update.

Task 3: Modify the Function with a Loop

I updated alert() to repeat the message three times using a for loop.
python

def alert(): 
    for i in range(3):
        print("Potential security issue. Investigate further.")

alert()

Output:
text

Potential security issue. Investigate further.
Potential security issue. Investigate further.
Potential security issue. Investigate further.

Difference from Task 2: The previous version printed once; this version prints three times.
Task 4: Start Defining a New Function

I began writing a function called list_to_string() that will convert a list of usernames into a single string.
python

def list_to_string():
    pass

Task 5: Loop Through a List Inside a Function

I added a list of approved usernames and a loop to print each element.
python

def list_to_string():
    username_list = ["elarson", "bmoreno", "tshah", "sgilmore", "eraab", "gesparza", "alevitsk", "wjaffrey"]
    for i in username_list:
        print(i)

list_to_string()

Output: Each username printed on a separate line.
Task 6: String Concatenation to Build One String

I used string concatenation (+) to combine all usernames into a single string stored in sum_variable.
python

def list_to_string():
    username_list = ["elarson", "bmoreno", "tshah", "sgilmore", "eraab", "gesparza", "alevitsk", "wjaffrey"]
    sum_variable = ""
    for i in username_list:
        sum_variable = sum_variable + i
    print(sum_variable)

list_to_string()

Output: elarsonbmorenotshahsgilmoreeraabgesparzaalevitskwjaffrey
(All usernames run together – no spaces or separators)
Task 7: Improve Readability with Separators

I added a comma and a space (", ") after each username to make the output easier to read.
python

def list_to_string():
    username_list = ["elarson", "bmoreno", "tshah", "sgilmore", "eraab", "gesparza", "alevitsk", "wjaffrey"]
    sum_variable = ""
    for i in username_list:
        sum_variable = sum_variable + i + ", "
    print(sum_variable)

list_to_string()

Output: elarson, bmoreno, tshah, sgilmore, eraab, gesparza, alevitsk, wjaffrey,

Note: There is an extra ", " after the last username, which could be removed with more logic – but this shows the power of concatenation clearly.
Key Takeaways

    Functions let me reuse code and keep my security scripts organized.

    Loops inside functions automate repetitive actions (e.g., printing an alert multiple times).

    String concatenation transforms a list into a single string, which is useful for logging or saving data to a file.

This lab helped me build practical Python skills that I can apply directly to security automation tasks.
