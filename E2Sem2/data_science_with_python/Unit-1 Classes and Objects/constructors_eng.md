

#  LECTURE: Constructors (`__init__`) in Python





## 1️⃣ THE PROBLEM — Manual Labor is Boring



Remember our Student example? We created a class, but it was empty. To give a student a name and an ID, we had to write code manually for every single student.


If we have 3 students, it’s fine. If we have 100 students, we are writing 200 lines of boring code just to set names."


### The "Bad" Way (Without a Constructor)


```python
class Student:
   # Empty class (pass means "do nothing")
   pass


# --- EXECUTION ---
# 1. Create the object (Empty)
s1 = Student()


# 2. Manually attach data (Tedious & Risky)
s1.name = "Raju"
s1.student_id = 1001


# 3. Repeat for next student
s2 = Student()
s2.name = "Rani"
# Oops! I forgot to set s2.student_id.
# Now s2 is a "half-baked" object.


print(s1.name, s1.student_id)
# print(s2.student_id) -> CRASH! AttributeError
```


**The Issues:**


1. **Repetitive:** We repeat `.name = ...` every time.
2. **Unsafe:** If we forget one line (like `s2.student_id`), the program crashes later.


**The Question:**
"Can Python run a setup function **automatically** the moment we create an object?"


**The Answer:**
"Yes. This is called the **Constructor**."


-----


## 2️⃣ WHAT IS A CONSTRUCTOR IN PYTHON?





In Python, the constructor is a special method named **`__init__`**.


**Key Rules:**


1. It **must** be named `__init__` (two underscores before, two after).
2. It runs **automatically** when you type `Student()`.
3. Its first parameter is **always** `self`.
4. It **never** returns a value.


-----


##  TYPE 1: The Default Constructor (Safe Defaults)




"Let's say we want every student to start with safe default values so our program never crashes."


### Program 1: Default Values


```python
class Student:
   # The Constructor
   def __init__(self):
       # We set "Safe" default values here
       self.name = "Unknown"
       self.student_id = -1
       print("🛠️ A new Student object was just created!")


   def show_identity(self):
       print("Name:", self.name, "| ID:", self.student_id)


# --- EXECUTION ---
s1 = Student()   # Python calls __init__ automatically here
s1.show_identity()


s2 = Student()   # Python calls __init__ again here
s2.show_identity()
```


###  Line-by-Line Explanation


 * `def __init__(self):`
     * Defines the setup function. It takes no extra data, just `self` (the object itself).
 * `self.name = "Unknown"`
     * Attaches the attribute `name` to the object and sets it to "Unknown".
 * `s1 = Student()`
     * Creates the object **AND** immediately runs `__init__`.
     * Now, `s1` is guaranteed to have a name and ID.


-----


##  TYPE 2: The Parameterized Constructor (The Standard Way)





"Defaults are nice, but usually, we know the student's name when we create them. We want to pass the data **into** the object immediately."



### Program 2: Passing Data


```python
class Student:
   # We ask for data strictly at the time of creation
   def __init__(self, name, student_id):
       self.name = name             # LHS: Object's data | RHS: Incoming variable
       self.student_id = student_id
       print(f"✅ Created Student: {self.name}")


   def show_identity(self):
       print("Name:", self.name, "| ID:", self.student_id)


# --- EXECUTION ---
# We MUST pass arguments now, or it will error
s1 = Student("Raju", 1001)
s2 = Student("Rani", 1002)


s1.show_identity()
s2.show_identity()
```


### Line-by-Line Explanation


 * `def __init__(self, name, student_id):`
     * The constructor now demands two pieces of information.
 * `self.name = name`
     * **Crucial Line:** It takes the temporary value `name` (passed from outside) and saves it permanently into `self.name` (the object's memory).
 * `s1 = Student("Raju", 1001)`
     * Creates the object.
     * Passes "Raju" into `name`.
     * Passes 1001 into `student_id`.
     * Runs the setup logic.


-----


##  TYPE 3: Flexibility (Python's Superpower)





**The Java vs. Python Difference:**


 * In **Java**, if you want a constructor that takes just a Name, and another that takes Name + ID, you write **two** constructors (Overloading).
 * In **Python**, you cannot write two `__init__` methods. The second one will delete the first one.
 * **Solution:** We use **Default Arguments**.


### Program 3: The "All-in-One" Constructor


```python
class Student:
   # One constructor to rule them all
   # If values are not passed, use the defaults
   def __init__(self, name="Unknown", student_id=-1):
       self.name = name
       self.student_id = student_id


   def show_identity(self):
       print(f"Name: {self.name} | ID: {self.student_id}")


# --- EXECUTION ---
# Scenario 1: No data given (Uses defaults)
s1 = Student()


# Scenario 2: Only Name given (ID uses default)
s2 = Student("Raju")


# Scenario 3: Both given (Overrides defaults)
s3 = Student("Rani", 1002)


s1.show_identity()  # Unknown, -1
s2.show_identity()  # Raju, -1
s3.show_identity()  # Rani, 1002
```


**Why is this cool?**
We achieved the same flexibility as Java Overloading, but with only **one** function\!


-----


##  COMMON MISTAKES 





**❌ 1. Writing Multiple `__init__` Methods**


```python
class Student:
   def __init__(self): ...
   def __init__(self, name): ... # ❌ This deletes the previous one!
```


 * **Fix:** Use Default Arguments (Type 3 above).


**❌ 2. Forgetting `self` in the arguments**


```python
def __init__(name, id): # ❌ Crash!
```


 * **Reason:** Python tries to pass the object as the first argument automatically. If you don't catch it with `self`, the arguments misalign.


**❌ 3. Shadowing Variables (The "Nothing Happened" Error)**


```python
def __init__(self, name, id):
   name = name  # ❌ Creates a local variable, throws it away
   # self.name is never set!
```


 * **Fix:** `self.name = name`. You must attach it to `self`.


**❌ 4. Returning a Value**


```python
def __init__(self):
   return "Done" # ❌ TypeError
```


 * **Reason:** Constructors are for *setup*, not for calculation results.


-----


##  FINAL SUMMARY





 * **`__init__`** is Python's Constructor.
 * It runs **automatically** on object creation.
 * **Always** initialize your attributes here (`self.variable = value`).
 * Use **Default Arguments** (`name="Unknown"`) to make your class flexible.


