


## CHAPTER: Functions in Python

**(The Most Flexible Feature in Programming)**

**Goal:** Move beyond "writing code" to "writing smart, reusable logic."


-----


## 1️⃣ Why are Python Functions Special?

In languages like C and Java, function arguments follow a strict, rigid rule. Python changes this:


 * Order matters deeply.
 * No default values (usually).
 * No keyword arguments.
 * To handle different inputs, you must write **Function Overloading** (writing the same function 5 times).


**The Old Way (Java/C++):**


```java
greet(name);
greet(name, age);
greet(name, age, city);
```


**The Python Way:**
Python asks a simple question:


> "Why should programmers repeat the same logic 5 times just because of extra parameters?"


Thus, Python gives us superpowers:


1. **Positional Arguments** (Standard).
2. **Keyword Arguments** (Order doesn't matter).
3. **Default Arguments** (Optional inputs).
4. **Returning Multiple Values**.


This allows **ONE** single function to serve **10** different use cases. Let’s master them.


-----


## PART A — Defining a Function

### 1️⃣ Simple Function (No Parameters)

A simple function is like a button on a calculator. You press it and an action happens without needing to provide inputs.


**Example 1: Greeting**


```python
def greet():
   print("Hello, welcome to Python class!")


# Call the function
greet()
```


**Line-by-line Analysis:**


 * `def`: Keyword to define a new function.
 * `greet`: The name of the function.
 * `()`: Empty parentheses = No inputs needed.
 * `:`: Starts the function body (indentation required).
 * `greet()`: The actual call that executes the code.


**Example 2: Date Display**


```python
def show_subject():
   print("Today's subject is: Advanced Python.")


show_subject()
```


**Common Mistakes:**


1. **Forgetting Parentheses:** Calling `greet` (which just shows the function object) instead of `greet()` (which runs it).
2. **Indentation Errors:** Writing the logic outside the function block.


-----


## PART B — Functions with Parameters

### 2️⃣ Positional Arguments

Arguments are mapped to parameters based on their position (1st to 1st, 2nd to 2nd). Order matters.


**Example 1: Student Details**


```python
def student(name, roll_no):
   print("Name:", name)
   print("Roll No:", roll_no)


# Correct Order
student("Raju", 45)
```


**Line-by-line Analysis:**

* `"Raju"` is at position 1 → maps to `name`.
* `45` is at position 2 → maps to `roll_no`.


**Example 2: Addition**


```python
def add(a, b):
   return a + b


print(add(10, 20))
```


**Common Mistake:**


 * **Wrong Order:** `student(45, "Raju")` will print "Name: 45", which is logically wrong.


-----


### 3️⃣ Keyword Arguments

Think of ordering food online where you specify what you want by name, not by order.
You don't say "1st give me Rice, 2nd give me Curry."
You say: **"Item=Biryani, Drink=Coke"**. The order doesn't matter because you used the names.


**Explanation 2 (Technical):**
Arguments are mapped by **parameter name**, ignoring the position completely.


**✔ Example 1: Ordering Food**


```python
def order(food, drink):
   print(f"Order: {food} with {drink}")


# Order doesn't matter here!
order(drink="Lime Soda", food="Biryani")
```


**✔ Example 2: Profile Creation**


```python
def create_profile(name, age):
   print(f"User: {name}, Age: {age}")


create_profile(age=22, name="Uday")
```


**COMMON MISTAKE:**


 * **Mixing Rules:** `order(food="Biryani", "Coke")`.
 * *Rule:* Once you start using keywords, you cannot go back to positional arguments in the same call.


-----


## PART C — DEFAULT ARGUMENTS


**(The KING feature of Python Functions)**


###  TYPE 4 — Default Parameters


**Explanation 1 (Intuition):**
Think of a Restaurant Thali.


 * It comes with **Default Chutney**.
 * If you don't ask for anything, you get the default.
 * If you specifically ask for "Pickle", the default is removed, and you get Pickle.


**Explanation 2 (Technical):**
Parameters are assigned a value during definition. If the user calls the function without that argument, the default value is used.


**✔ Example 1: Default Greeting**


```python
def greet(name="Student"):
   print("Hello,", name)


greet()         # Uses default "Student"
greet("Uday")   # Overrides default with "Uday"
```


**✔ Example 2: Power Calculation**


```python
def power(base, exp=2):
   return base ** exp


print(power(5))      # 5^2 = 25 (Default used)
print(power(5, 3))   # 5^3 = 125 (Default overridden)
```


**Common Mistakes:**


1. **Placement Error:** Placing a non-default argument after a default one.
     * `def wrong(a=5, b):` -\> **Error\!** Default args must be at the end.
2. **Mutable Defaults (The Interview Trap):**
     * `def add(item, list=[]):` -\> **Dangerous\!** The list persists across calls.
     * *Fix:* Use `None` as default and create the list inside.


-----


## PART D — FUNCTIONS THAT RETURN VALUES


###  TYPE 5 — Return Statements


**Explanation 1 (Intuition):**
Using `print` is like an ATM showing your balance on the screen.
Using `return` is like the ATM actually **giving you cash** to put in your pocket.


**Explanation 2 (Technical):**
The `return` keyword exits the function immediately and sends an object back to the caller.


**✔ Example 1: Sum**


```python
def sum(a, b):
   return a + b


result = sum(3, 7)  # Result captures the value 10
print(result)
```


**✔ Example 2: Even Check**


```python
def is_even(num):
   return num % 2 == 0  # Returns True or False
```


**COMMON MISTAKE:**


 * **Forgetting Return:** `def add(a,b): c = a+b`. This calculates `c` but throws it away. The function returns `None`.


-----


## PART E — TRUTHINESS & NONE


### TYPE 6 — Functions Returning `None`


**Explanation 1 (Intuition):**
If you ask a function for a result, but it has no `return` statement, Python hands you an "Empty Box". That box is `None`.


**Explanation 2 (Technical):**
In Python, all functions implicitly return `None` if no return statement is executed.


**✔ Example 1: Implicit None**


```python
def perform_task():
   print("Task done")


x = perform_task()
print(x)   # Output: None
```


**✔ Example 2: Conditional Return**


```python
def check_value(x):
   if x > 10:
       return "Big"
   # If x <= 10, it implicitly returns None
```


**COMMON MISTAKE:**


 * Trying to do math on a void function: `x = perform_task() + 10` -\> Error.


-----


## PART F — MIXING EVERYTHING


**(Positional + Keyword + Default)**


### TYPE 7 — The Mixing Rules


**Explanation 1 (Intuition):**
You order Biryani (Main Item) first. Then you specify "Spice=Medium" (Customization).
**Rule:** essentials first, specifics later.


**Explanation 2 (Technical):**


1. Positional arguments come first.
2. Keyword arguments come next.
3. Default arguments act as fallbacks.


**✔ Example 1: Billing System**


```python
def bill(amount, tax=5, tip=0):
   return amount + (amount * tax / 100) + tip


# Positional (100) + Keyword (tax) + Default (tip)
print(bill(100, tax=10))
```


**✔ Example 2: Profile**


```python
def profile(name, age, city="Hyderabad"):
   print(name, age, city)


profile("Uday", age=22)                 # Uses default city
profile("Ravi", city="Delhi", age=25)   # Overrides city, order of keys doesn't matter
```


**COMMON MISTAKE:**


 * **Keyword before Positional:** `profile(age=22, "Uday")` -\> **Error\!**


-----


## FINAL SUMMARY TABLE (Perfect for Exams)


| Function Feature | Description | Use Case |
| :--- | :--- | :--- |
| **Positional Args** | Strict order (Like C/Java) | Mandatory inputs (e.g., `add(a,b)`). |
| **Keyword Args** | Order-free (`key=value`) | Clarity when many inputs exist. |
| **Default Params** | `arg=val` in definition | Optional inputs / Configuration. |
| **Return Value** | Sends data back | Calculations / Logic. |
| **None** | Default return value | Procedures that just "do" things. |
| **Mixed Args** | Positional **\>** Keyword | Complex function calls. |


-----


##  WRAP-UP


Python functions give you superpowers that C and Java simply do not have out of the box:


1. **Cleaner Code:** No need to write 5 different functions for similar tasks.
2. **Flexibility:** Call functions however you like (Positional or Keyword).
3. **Safety:** Default arguments handle missing data gracefully.


This makes Python functions **Cleaner, Shorter, and More Powerful.**

