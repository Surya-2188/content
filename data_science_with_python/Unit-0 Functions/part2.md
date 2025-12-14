


## CHAPTER: Advanced Python Functions

**(Flexible Arguments, Packing, Unpacking & Real Engineering Use Cases)**

**Goal:** Learn how to write flexible, professional-grade functions that power real-world libraries like Pandas and Django.


-----


## 1️⃣ Why Flexible Arguments Matter

In languages like C and Java, functions are rigid. If a function needs 2 parameters, you must call it with exactly 2. This is problematic for real projects where requirements change daily.


 * If a function needs 2 parameters → You **must** call it with exactly 2.
 * If tomorrow you want 3 parameters → You must modify the function or write a new one (Overloading).


**This is difficult for real projects where requirements change daily.**


### Real-World Example: Pizza Ordering System


 * **Day 1:** `order_pizza(size)` -\> Just the base.
 * **Day 5:** `order_pizza(size, topping1)` -\> Students want cheese.
 * **Day 7:** `order_pizza(size, t1, t2, t3, t4...)` -\> Customers want a loaded pizza.


If Python behaved like C, you would die writing 100 different versions of `order_pizza`.


**Python's Answer:**


> "What if a function could accept **ANY** number of inputs... even ones you don't know at coding time?"


This is where `*args`, `**kwargs`, `/`, and `*` come into play. These features make Python the **MOST flexible language** for real-world APIs.


-----


## 2️⃣ *args — Any Number of Positional Arguments


*args acts like a basket that holds unlimited items.


### Example 1: Pizza Toppings


```python
def add_toppings(*args):
   # args becomes a TUPLE containing all arguments
   print("You added:", args)


# Call with 1 argument
add_toppings("cheese")


# Call with 3 arguments
add_toppings("cheese", "corn", "olives")
```


**Output:**


```text
You added: ('cheese',)
You added: ('cheese', 'corn', 'olives')
```


### Example 2: Student Scores (Summing Numbers)


```python
def total(*marks):
   return sum(marks)


print(total(10, 20, 30))  # 60
print(total(90, 80))      # 170
```


**Common Mistakes:**

1. **Treating `args` as a list:** It is a **tuple** (Immutable). You cannot write `args[0] = 5`.
2. **Confusing Packing vs. Unpacking:**
     * `total([10, 20])` -\> Error\! It receives one list.
     * `total(*[10, 20])` -\> Correct\! It unpacks the list into individual arguments.


-----


## 3️⃣ **kwargs — Any Number of Keyword Arguments

**kwargs acts like a labeled suitcase. Build a student registration function where fields change based on the student type.


### Example 1: Dynamic Student Registration


```python
def register(**kwargs):
   # kwargs becomes a DICTIONARY
   print("Student Data:", kwargs)


register(name="Uday", age=22)
register(name="Kiran", dept="CSE", hostel="H3")
```


**Output:**


```text
{'name': 'Uday', 'age': 22}
{'name': 'Kiran', 'dept': 'CSE', 'hostel': 'H3'}
```


### Example 2: Printing Invoice


```python
def invoice(**details):
   for key, value in details.items():
       print(f"{key}: {value}")


invoice(id=101, amount=5000, tax=18, mode="online")
```


**Common Mistakes:**

1. **Passing Positional Arguments:** `register("Uday")` -\> Error. `**kwargs` only accepts `key=value`.
2. **Forgetting to Unpack Dicts:**
     * `data = {'name': 'Arun'}`
     * `register(data)` -\> Error
     * `register(**data)` -\> Correct


-----


## 4️⃣ Combining Parameters


When using multiple types, the order **MUST** be:


1. Standard Positional
2. `*args`
3. Standard Keyword / Keyword-only
4. `**kwargs`


### Example: Real API-Style Function


```python
def make_pizza(size, *toppings, crust="thin", **extra):
   print(f"Size: {size}")
   print(f"Toppings: {toppings}")
   print(f"Crust: {crust}")
   print(f"Extras: {extra}")


make_pizza(12, "cheese", "corn", "olives", crust="pan", sauce="spicy", dip="garlic")
```


**Output:**


```text
Size: 12
Toppings: ('cheese', 'corn', 'olives')
Crust: pan
Extras: {'sauce': 'spicy', 'dip': 'garlic'}
```


This structure is exactly how libraries like **Pandas** and **Matplotlib** are written.


-----


##  5️⃣ POSITIONAL-ONLY PARAMETERS (`/`)


**(Modern Python Feature)**


**Requirement:** Force a function to accept parameters **only by position**, preventing keyword usage. Useful for mathematical functions.


### Example: Area of Circle


```python
def area(radius, /):
   return 3.14 * radius * radius


print(area(10))        # ✔ Valid
# print(area(radius=10)) # ❌ Invalid!
```


-----


##  6️⃣ KEYWORD-ONLY PARAMETERS (`*`)


**(For Clarity)**


**Requirement:** Force specific parameters to be named explicitly. This prevents confusion in functions with many arguments.


### Example: Payment Processing


```python
def pay(amount, *, mode="online"):
   print(f"Paid {amount} via {mode}")


pay(500, mode="cash")  # ✔ Valid
# pay(500, "cash")       # ❌ Invalid! 'mode' must be named.
```


-----


##  7️⃣ MUTABLE vs IMMUTABLE ARGUMENTS


**(The Most Dangerous Python Rule)**


###  The Concept


 * **Immutable (Int, Float, Str, Tuple):** Changes inside the function **DO NOT** reflect outside.
 * **Mutable (List, Dict, Set):** Changes inside the function **DO** reflect outside.


### Example 1: Modifying a List (Mutable)


```python
def change(lst):
   lst.append(100)


x = [1, 2, 3]
change(x)
print(x)  # [1, 2, 3, 100] -> CHANGED!
```


### Example 2: Modifying a String (Immutable)


```python
def modify(s):
   s = s + "!!!"


t = "hello"
modify(t)
print(t)  # "hello" -> UNCHANGED!
```


### CRITICAL MISTAKE: MUTABLE DEFAULT ARGUMENTS


This is the \#1 bug beginners introduce.


**Wrong Way:**


```python
def add(item, bucket=[]):  # The list is created ONCE and reused forever
   bucket.append(item)
   return bucket


print(add("Apple"))  # ['Apple']
print(add("Banana")) # ['Apple', 'Banana'] -> WAITS! Why is Apple here?
```


**Correct Way:**


```python
def add(item, bucket=None):
   if bucket is None:
       bucket = []  # Create a NEW list every time
   bucket.append(item)
   return bucket
```


-----


## FINAL SUMMARY TABLE (Cheat Sheet)


| Symbol | Name | Purpose | Example |
| :--- | :--- | :--- | :--- |
| `*args` | Arbitrary Args | Passing unlimited positional inputs | `sum(*numbers)` |
| `**kwargs` | Arbitrary Keywords | Passing unlimited key-value inputs | `register(**data)` |
| `/` | Positional-Only | Arguments before this **cannot** be keywords | `area(10, /)` |
| `*` | Keyword-Only | Arguments after this **must** be keywords | `sort(data, *, reverse=True)` |
| `*` (Call) | Unpacking | Expanding a list/tuple into args | `func(*list)` |
| `**` (Call) | Unpacking | Expanding a dict into kwargs | `func(**dict)` |


-----


##  WRAP-UP


This chapter is the heart of Python's flexibility.
These concepts separate a **Beginner** (who writes rigid code) from a **Professional** (who writes flexible, reusable APIs).


1. **`*args` & `**kwargs`**: Allow your functions to handle the unknown.
2. **`/` & `*`**: Allow you to enforce strict API rules.
3. **Mutable Defaults**: A trap you now know how to avoid.




