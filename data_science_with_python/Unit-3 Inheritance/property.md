

#  CHAPTER: PROPERTIES IN PYTHON


**(The "Pythonic" Way of Encapsulation)**


**Goal:** Understand how to combine the safety of Encapsulation with the clean syntax of direct variable access.


-----


##  1️⃣ RECAP: THE EVOLUTION OF ORGANIZATION


*Let's connect this to what you already know.*


### In C Language:


 * You write functions.
 * To reuse code across files, you need **Header Files**, **Include Directives**, and **Linking**.
 * It works, but it is complex.


### In Python (Modules):


 * Python simplifies this: Every `.py` file is a module.
 * **"Write once → Import anywhere."**
 * This solves **Code Organization**.


**But now, we face a new problem inside our Classes: Data Organization.**


-----


##  2️⃣ THE PROBLEM: GETTERS & SETTERS ARE "CLUNKY"


After learning Encapsulation, we know we should not access private data directly.


**The "Old" (Java-style) Way:**


```python
# Setting value
s.set_cgpa(9.2)


# Getting value
print(s.get_cgpa())
```


**Why Python Developers dislike this:**


1. It looks verbose and "ugly".
2. It feels like a function call, not a variable access.
3. Python prefers simplicity: `s.cgpa = 9.2`.


**The Challenge:**


> *"How can we use simple syntax (`s.cgpa = 9.2`) but still have the security of Encapsulation (Validation) behind the scenes?"*


**The Solution:** **Python Properties**.


-----


##  3️⃣ THE PYTHONIC SOLUTION: PROPERTIES


Properties allow us to define methods that **behave like variables**.


Here is the final, modern Python class using properties.


```python
class Student:


   def __init__(self, name, cgpa):
       self.name = name
       # Private variable (The actual data storage)
       self.__cgpa = cgpa


   # ---------------------------------------------
   # 1. THE GETTER (Read Access)
   # ---------------------------------------------
   @property
   def cgpa(self):
       print(">> Getter called")
       return self.__cgpa


   # ---------------------------------------------
   # 2. THE SETTER (Write Access with Validation)
   # ---------------------------------------------
   @cgpa.setter
   def cgpa(self, new_value):
       print(">> Setter called")
       if 0 <= new_value <= 10:
           self.__cgpa = new_value
       else:
           print("❌ Invalid CGPA! Must be 0-10.")


# --- USAGE ---
s = Student("Aditi", 8.5)


# Looks like a variable access, but runs the method!
print(s.cgpa)      # Calls the Getter
s.cgpa = 9.2       # Calls the Setter
s.cgpa = 12        # Calls the Setter (Validation fails)
```


**Output:**


```text
>> Getter called
8.5
>> Setter called
>> Setter called
❌ Invalid CGPA! Must be 0-10.
```


-----


##  4️⃣ LINE-BY-LINE EXPLANATION


*(Addressing the Core Concept)*


### Line 1-4: The Setup


```python
def __init__(self, name, cgpa):
   self.name = name
   self.__cgpa = cgpa
```


 * `name` is public.
 * `__cgpa` is **private**. This is where the actual data lives.


### Line 6: The Magic Keyword (`@property`)


```python
@property
def cgpa(self):
```


**Student Question:**


> *"Sir, if I write `@property` over a method, how does it work for that variable?"*


**The Answer:**
When you write `@property` above a method `def cgpa(self):`:


1. Python **converts** this method into a **Virtual Attribute**.
2. It tells the interpreter: *"Whenever someone types `s.cgpa` (without brackets), do **not** look for a variable. Instead, run this method automatically."*


This is the "Magic Trick":


 * You **write** a method.
 * You **access** it like a variable.
 * **Result:** You get the cleanness of a variable with the power (validation) of a method.


### Line 11: The Setter Connection


```python
@cgpa.setter
def cgpa(self, new_value):
```


 * `@cgpa.setter` tells Python: *"If someone tries to assign a value (`s.cgpa = 9`), run THIS method."*
 * Notice the function name `def cgpa(...)` is **exactly the same** as the getter. This links them together.


-----


##  5️⃣ ADVANCED: COMPUTED PROPERTIES


Properties are also great for values that don't exist in memory but are calculated on the fly.


```python
class Rectangle:
   def __init__(self, length, width):
       self.length = length
       self.width = width


   @property
   def area(self):
       return self.length * self.width


r = Rectangle(5, 10)
print(r.area)  # 50 (Calculated instantly, looks like a variable)
# r.area = 60  # Error! Read-only property.
```


-----


##  6️⃣ WHY PROPERTIES ARE BETTER?


1. **Cleaner Syntax:** `s.cgpa` is easier to read than `s.get_cgpa()`.
2. **Backward Compatibility:** You can start with a public variable, and later change it to a Property (add validation) **without breaking** the code that uses your class.
3. **Read-Only Fields:** You can define a Getter without a Setter, making the variable impossible to change.


-----


##  7️⃣ COMMON MISTAKES STUDENTS MAKE




 **Mistake 1 — Naming Mismatch**


```python
@property
def cgpa(self): ...


@marks.setter       # WRONG! Must be @cgpa.setter
def marks(self): ...
```


**Fix:** Getter and Setter methods must have the **exact same name**.


 **Mistake 2 — Infinite Recursion**


```python
@property
def cgpa(self):
   return self.cgpa  # WRONG! Calls itself forever
```


**Fix:** The property must return the **private** variable (`self.__cgpa`), not the property name itself.


 **Mistake 3 — Accessing like a function**


```python
s.cgpa()  # TypeError: 'float' object is not callable
```


**Fix:** Properties are accessed without parentheses: `s.cgpa`.


 **Mistake 4 — Thinking Privacy is gone**


 * The property `cgpa` is public.
 * The storage `__cgpa` is **still private**. Encapsulation is intact.


-----


##  8️⃣  SUMMARY


1. **Properties** = The Pythonic way to implement Encapsulation.
2. **`@property`** = Turns a getter method into a virtual attribute.
3. **`@name.setter`** = Defines the setter logic for that attribute.
4. **Syntax:** Access like a variable (`obj.name`), but it behaves like a method (validation runs).
5. **Rule:** Getter and Setter methods must share the same name.

