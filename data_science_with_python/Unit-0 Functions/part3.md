


# CHAPTER: Advanced Powers of Python Functions

**(Lambdas • Generators • Decorators • Scope • Recursion • Async)**

**Goal:** Treat functions not just as code blocks, but as powerful tools that can be passed, modified, and generated on the fly.


-----


## 1️⃣ LAMBDA FUNCTIONS (Anonymous One-Line Functions)

In real projects, you often need very small functions temporarily. Lambda functions let you define them right where you use them, without needing a name.


### Example 1 — Square of a number


```python
square = lambda x: x * x
print(square(5))
```


**Output:** `25`


**Line-by-Line:**


1. `lambda x:` → Takes one parameter `x`.
2. `x * x` → Returns the square automatically.
3. `square(5)` → Calls the lambda.


### Example 2 — Sorting by Age


```python
students = [("Anil", 22), ("Bharath", 19), ("Chitra", 24)]


# Sort based on the second element (Age)
students_sorted = sorted(students, key=lambda s: s[1])
print(students_sorted)
```


**Output:** `[('Bharath', 19), ('Anil', 22), ('Chitra', 24)]`


**Line-by-Line:**


1. `students` → A list of tuples.
2. `key=lambda s: s[1]` → For each student tuple `s`, extract index 1 (Age).
3. `sorted` uses these ages to arrange the list.


**Common Mistakes (Lambda):**


1. **Writing multiple statements:** Allowed only ONE expression.
2. **Using lambda for complex logic:** Use `def` instead.
3. **No `return` keyword:** Lambda returns the result automatically.


-----


## 2️⃣ GENERATORS — The Most Practical Python Feature

Generators help you produce large sequences without storing them all in memory. It's like cooking one dosa per order instead of cooking 100 dosas at once.


### Example 1 — Simple Counter Generator


```python
def counter(limit):
   i = 1
   while i <= limit:
       yield i    # Pause here and return i
       i += 1     # Resume from here next time


for num in counter(5):
   print(num)
```


**Output:**


```text
1
2
3
4
5
```


**Line-by-Line:**


1. `def counter(limit):` → Defines a generator function.
2. `yield i` → Returns value & pauses execution. State is saved.
3. `i += 1` → Resumes execution when the loop asks for the next number.


### Example 2 — Infinite Fibonacci Generator (Advanced)


```python
def fibonacci():
   a, b = 0, 1
   while True:
       yield a
       a, b = b, a+b
```


**Why Powerful?** This generates an **infinite** sequence without crashing memory. You can take as many as you need.


### Example 3 — Reading Large Files (Real World)


```python
def read_large(path):
   with open(path) as f:
       for line in f:
           yield line.strip()  # Return one line at a time
```


**Why Powerful?** Perfect for processing a **10GB log file**. It never loads the whole file into RAM.


**Common Mistakes (Generators):**


1. **Calling `next()` after exhaustion:** Raises `StopIteration`.
2. **Expecting restart:** A generator is "use once". You must recreate it to loop again.
3. **Using `return` instead of `yield`:** `return` kills the function; `yield` pauses it.


-----


## 3️⃣ GENERATOR COMPREHENSIONS


Just like List Comprehensions, but with parentheses `()`.


```python
gen = (x*x for x in range(5))


# Iterating
for val in gen:
   print(val)
```


**Output:** `0, 1, 4, 9, 16` (One by one).


**Key:**


 * `[]` → List (Immediate, All in Memory).
 * `()` → Generator (Lazy, One at a time).


-----


## 4️⃣ DECORATORS — Adding Features to Functions

Decorators wrap your function to add new behavior without changing the function's code.


### Example 1 — Logging Decorator


```python
def logger(func):
   def wrapper(*args, **kwargs):
       print(f"[LOG]: Running {func.__name__}")
       result = func(*args, **kwargs)
       print(f"[LOG]: Result = {result}")
       return result
   return wrapper


@logger
def add(a, b):
   return a + b


add(5, 7)
```


**Output:**


```text
[LOG]: Running add
[LOG]: Result = 12
```


**Line-by-Line:**


1. `@logger` takes `add` and passes it to `logger`.
2. `wrapper` prints the log message.
3. `func(*args)` executes the original `add`.
4. `wrapper` prints the result.


### Example 2 — Timer Decorator


```python
import time


def timer(func):
   def wrapper(*args, **kwargs):
       start = time.time()
       result = func(*args, **kwargs)
       print("Time taken:", time.time() - start)
       return result
   return wrapper


@timer
def slow():
   time.sleep(1)


slow()
```


**Output:** `Time taken: 1.00023`


**Common Mistakes (Decorators):**


1. **Forgetting `return wrapper`:** The decorator MUST return the wrapper function.
2. **Forgetting `*args, **kwargs`:** Breaks the ability to decorate functions with arguments.
3. **Confusing Order:** Decorators are applied from bottom to top.


-----


## 5️⃣ NAMESPACES & SCOPE


Python follows the **LEGB Rule**:


1. **L**ocal (Inside function)
2. **E**nclosing (Function inside function)
3. **G**lobal (Module level)
4. **B**uilt-in (Python keywords)


### Example 1 — Accessing Global


```python
animal = "Lion"


def show():
   print(animal)  # Reads global


show()
```


**Output:** `Lion`


###  Example 2 — Modifying Global


```python
count = 0


def increase():
   global count   # Must declare global to modify
   count += 1


increase()
print(count)
```


**Output:** `1`


**Common Mistakes (Scope):**


1. **UnboundLocalError:** Trying to modify a global variable without the `global` keyword.
2. **Shadowing:** Creating a local variable with the same name as a global one (confusing).


-----


## 6️⃣ DUNDER NAMES (`__init__`, `__str__`)

These connect your code to Python's internal machinery.


### Example — Accessing Metadata


```python
def greet():
   """This prints a greeting message"""
   print("Hello!")


print(greet.__name__)  # 'greet'
print(greet.__doc__)   # 'This prints a greeting message'
```


-----


## 7️⃣ RECURSION — Function Calls Itself


**(Solving Nested Problems)**


###  Example 1 — Factorial


```python
def fact(n):
   if n == 0:  # Base Case
       return 1
   return n * fact(n-1)  # Recursive Step


print(fact(5))
```


**Output:** `120`


### Example 2 — Flatten Nested Lists


```python
def flatten(lst):
   for item in lst:
       if isinstance(item, list):
           yield from flatten(item)  # Recursion with Generators!
       else:
           yield item


print(list(flatten([1, [2, 3], [4, [5]]])))
```


**Output:** `[1, 2, 3, 4, 5]`


**Common Mistakes (Recursion):**


1. **Missing Base Case:** Causes Infinite Recursion (`RecursionError`).
2. **Logic Error:** The problem must get smaller with each step.


-----


## 8️⃣ ASYNC & AWAIT

Async is used for tasks that involve waiting: API calls, Database queries, File downloads.


### Example 1 — Simple Async


```python
import asyncio


async def greet():
   print("Hello...")
   await asyncio.sleep(1)  # Non-blocking pause
   print("...World!")


# asyncio.run(greet())
```


### Example 2 — Parallel Execution (Advanced)


```python
async def fetch(server):
   print(f"Fetching from {server}...")
   await asyncio.sleep(2)
   print(f"{server} done!")


async def main():
   # Gather runs tasks concurrently
   await asyncio.gather(
       fetch("Server A"),
       fetch("Server B"),
       fetch("Server C")
   )


# asyncio.run(main())
```


**Output:**


 * All "Fetching..." messages appear instantly.
 * All "Done\!" messages appear after 2 seconds.
 * Total time: 2 seconds (not 6\!).


**Common Mistakes (Async):**


1. **Calling directly:** You must use `await` or `asyncio.run()`.
2. **Using `time.sleep()`:** This blocks everything. Use `asyncio.sleep()`.


-----


##  FINAL SUPER SUMMARY TABLE


| Concept | Keyword | Meaning |
| :--- | :--- | :--- |
| **Lambda** | `lambda` | One-line temporary function. |
| **Generator** | `yield` | Produces values lazily (Memory Efficient). |
| **Decorator** | `@` | Adds features to functions without changing code. |
| **Scope** | `global` | Where variables live (Local vs Global). |
| **Recursion** | N/A | Function calls itself to solve nested problems. |
| **Async** | `async/await` | Non-blocking execution for parallel tasks. |




