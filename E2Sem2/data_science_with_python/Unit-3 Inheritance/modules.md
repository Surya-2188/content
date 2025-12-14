


# 📘 CHAPTER: PYTHON MODULES


**(Code Reuse, Organization & Libraries)**





##  1️⃣ INTRODUCTION: WHAT IS A MODULE?


**Simple Definition:**
A Python module is simply any **`.py` file** that contains reusable code (functions, variables, classes).


If you save a file as `demo.py`, it automatically becomes a module named `demo`. You can then use the code inside `demo.py` in any other file.


**Examples:**
| File Name | Becomes Module Name |
| :--- | :--- |
| `math.py` | `math` module |
| `os.py` | `os` module |
| `mymath.py` | `mymath` module |


**Key Rule:** Every `.py` file is automatically a module.


-----


##  2️⃣ WHY MODULES? (INTUITION USING C PROGRAMMING)


*Let's connect this to what you already know from First Year Engineering.*


###  The Problem in C Programming


In C, you write functions like `add()` or `multiply()` inside a `.c` file.


 * `int main()` calls `add()`.
 * This works fine **inside one file**.


**The Big Question:** What if you want to use that `add()` function in a completely different project?


In C, this is painful. You need:


1. **Header Files (`.h`):** To declare function prototypes.
2. **Linking:** You must link multiple object files together.
3. **Complex Compilation:** `gcc main.c math.c -o app`.


###  The Python Solution (Simplicity)


Python removes all this complexity. It asks one simple question:


> *"Do you want to use code from another file? Just **import** it."*


 * **No** header files.
 * **No** linking steps.
 * **No** compilation commands.


Just write:


```python
import mymath
```


**The Philosophy:** "Write code once → Use it anywhere."


-----


##  3️⃣ WHAT MODULES ACHIEVE


Modules solve the biggest problems in software development:


1. **Code Reuse:** You define the logic for "Area of Circle" once. You use it in 100 different projects.
2. **Organization:** Instead of one giant 5000-line file (which is impossible to debug), you split it into 10 clean files of 500 lines each.
3. **Teamwork:**
     * Developer A works on `database.py`.
     * Developer B works on `ui.py`.
     * They work simultaneously without conflict.
4. **Namespace Management:** Two files can both have a function named `calculate()`. Modules keep them separate (`math.calculate()` vs `physics.calculate()`).


-----


##  4️⃣ TYPES OF MODULES IN PYTHON


Python has two main categories of modules:


###  1. Built-in / System Modules


These come pre-installed with Python. You don't need to download anything.


| Module | Purpose |
| :--- | :--- |
| `math` | Mathematical functions (`sqrt`, `sin`, `pi`) |
| `sys` | System-specific parameters (`sys.path`, `sys.version`) |
| `os` | Operating system interactions (file paths, folders) |
| `random` | Generating random numbers |
| `time` | Time access and conversions |


**Example:**


```python
import math
print(math.sqrt(16))  # Output: 4.0
```


###  2. User-Defined Modules


These are the `.py` files **you** create.


**Step 1: Create `mymath.py`**


```python
# Save this code in a file named mymath.py
pi = 3.14159


def area_circle(r):
   return pi * r * r
```


**Step 2: Use it in `main.py`**


```python
import mymath  # Import your own file


print(mymath.pi)
print(mymath.area_circle(5))
```


-----


## 5️⃣ HOW IMPORT WORKS INTERNALLY 


[Image of Python import process flow chart]


When you write `import mymath`, Python does not just magically find the code. It performs a specific **3-Step Process**:


### ✔ STEP 1 — Search


Python looks for the file `mymath.py` in a specific order:


1. **Current Directory:** The folder where your main script is running.
2. **PYTHONPATH:** An environment variable listing custom folders.
3. **Standard Library:** Where `math`, `os` are installed.
4. **Site-packages:** Where external libraries (like `numpy`) are installed.


### ✔ STEP 2 — Compile (Optional)


Python converts your `.py` source code into **Bytecode** (`.pyc`).


 * It stores this in a folder named `__pycache__`.
 * **Why?** Next time you run the program, Python skips compilation and runs faster.


### ✔ STEP 3 — Execute


Python **runs** the entire module code exactly **once**.


 * **Warning:** If your module has a `print("Hello")` statement at the top level, it will print immediately when you import it\!


-----


## 6️⃣ MODULE CACHE (`sys.modules`)


This is a very important concept.
Python imports a module only **ONCE per session**.


 * When you import `mymath`, Python loads it into memory.
 * If you write `import mymath` again in the same program, Python ignores it. It uses the copy already in memory (Cache).


**The Problem:** If you edit `mymath.py` while your program is running, Python won't see the changes.
**The Solution:** Use the `reload` function.


```python
import importlib
importlib.reload(mymath)  # Forces Python to read the file again
```


-----


##  7️⃣ ALL IMPORT VARIANTS


You must know the different ways to import and when to use them.


### 1\. `import module`


Imports the whole module. You must use the module name as a prefix.


 * **Best for:** Code clarity. You know exactly where `math.sqrt` came from.


<!-- end list -->


```python
import math
print(math.sqrt(25))
```


### 2\. `from module import name`


Imports specific functions directly. No prefix needed.


 * **Best for:** Convenience if you use a function frequently.


<!-- end list -->


```python
from math import sqrt
print(sqrt(25))
```


### 3\. `from module import *` (NOT RECOMMENDED)


Imports **everything**.


 * **Risk:** It pollutes your namespace. If `math` has a function `log()` and `logging` module has `log()`, one will overwrite the other. **Avoid this.**


### 4\. `import module as alias`


Renames the module. Essential for data science.


```python
import math as m
import numpy as np
import pandas as pd


print(m.pi)
```


-----


##  8️⃣ PRACTICAL DEMO


**Step 1: Create the Module (`mymath.py`)**


```python
pi = 3.14159


def square(x):
   return x * x


def cube(x):
   return x * x * x
```


**Step 2: Import and Use (`main.py`)**


```python
import mymath


print("Value of Pi:", mymath.pi)
print("Square of 4:", mymath.square(4))
print("Cube of 3:", mymath.cube(3))
```


**Step 3: Module Reuse (One module using another)**
Create `geometry.py`:


```python
import mymath  # Reusing code from mymath


def circle_area(radius):
   return mymath.pi * mymath.square(radius)
```


**Step 4: Main Logic**


```python
import geometry
print("Area of circle:", geometry.circle_area(10))
```


-----


##  9️⃣ SYSTEM MODULES CHEATSHEET


*(Must-know functions for exams)*


| Module | Useful Function | Usage |
| :--- | :--- | :--- |
| **sys** | `sys.path` | Shows the list of folders Python searches. |
| **sys** | `sys.version` | Shows the Python version running. |
| **os** | `os.getcwd()` | Gets current working directory. |
| **os** | `os.mkdir('new')`| Creates a new folder. |
| **time** | `time.time()` | Gets current timestamp (for measuring speed). |
| **time** | `time.sleep(2)` | Pauses the program for 2 seconds. |
| **random** | `random.randint(1,10)` | Generates random number between 1 and 10. |
| **random** | `random.choice([A,B])` | Picks a random item from a list. |


-----


##  🔟 COMMON MISTAKES STUDENTS MAKE


*(The "Watch Out" List)*


 **Mistake 1: Adding `.py` in import**


 * **Wrong:** `import mymath.py`
 * **Right:** `import mymath`


 **Mistake 2: Assuming module re-runs every time**


 * **Fact:** It runs only once. If you change code, restart the kernel or use `reload()`.


 **Mistake 3: Using `from module import *` everywhere**


 * **Fact:** Makes debugging hard because you don't know where a function came from.


 **Mistake 4: Writing executable code at top-level**


 * **Fact:** Any `print()` call outside a function block runs immediately on import.
 * **Fix:** Use `if __name__ == "__main__":` block for test code.


 **Mistake 5: Module in wrong folder**


 * **Fact:** Python only searches specific paths. Keep your `.py` file in the same folder as your main script.


-----


##   SUMMARY 


1. **Module** = A `.py` file containing reusable code.
2. **Solves:** Code reuse, organization, and namespace management.
3. **Two Types:** Built-in (`os`, `sys`, `math`) and User-defined.
4. **Importing:** Loads the module code **ONCE** (Cache).
5. **Search Path:** Current Dir → PYTHONPATH → Standard Library.
6. **Reload:** Use `importlib.reload()` if you edit a module while running.
7. **Best Practice:** Avoid `import *`; be specific with imports.

