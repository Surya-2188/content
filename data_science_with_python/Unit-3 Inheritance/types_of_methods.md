


#  CHAPTER: Types of Methods in Python


**(Instance, Class, Static, and Property Methods)**


**Goal:** Understand **where** behavior belongs—to the object, to the class, or to neither—and how to write clean, Pythonic code.


-----


##  1️⃣ WHY DO WE NEED DIFFERENT METHOD TYPES?


Before jumping into syntax, let's ask a fundamental question:


> *"Do all actions inside a class belong to a specific object?"*


Think of a **Student Management System**:


1. **Object-Level Actions:**


     * `submit_assignment()` or `view_profile()`.
     * These depend on **specific student data** (Raju's assignment vs. Rani's assignment).
     * **Method Type:** **Instance Method**.


2. **Class-Level Actions:**


     * `get_total_students()`.
     * This does not depend on Raju or Rani specifically. It depends on the **entire class**.
     * **Method Type:** **Class Method**.


3. **Utility Actions:**


     * `validate_email(email)` or `check_id_format(id)`.
     * This logic doesn't need student data *or* class data. It's just a helper function living inside the class for organization.
     * **Method Type:** **Static Method**.


4. **Controlled Access:**


     * Accessing `phone_number` or `cgpa`.
     * We need validation (getters/setters), but we want clean syntax.
     * **Method Type:** **Property Method**.


-----


##  2️⃣ INSTANCE METHODS (The Standard)


**Intuition:** An instance method is the behavior of an **Object**.


 * **Key Characteristic:** It receives `self` automatically.
 * **Usage:** When the action needs to access or modify `self.variable`.


###  Example 1: Greeting User


```python
class Person:
   def __init__(self, name):
       self.name = name


   def greet(self):             # Instance Method
       print("Hello", self.name)


# Usage
p = Person("Alice")
p.greet()  # Python converts this to: Person.greet(p)
```


### Example 2: Deposit Money (Modifying State)


```python
class BankAccount:
   def __init__(self, name, balance):
       self.name = name
       self.balance = balance


   def deposit(self, amount):      # Instance Method
       self.balance += amount
       print(f"{self.name} deposited {amount}. New balance = {self.balance}")


# Usage
acc = BankAccount("Ravi", 2000)
acc.deposit(500)
```


**Why Instance Method?** Because the deposit belongs to *that particular* account object.


**Common Mistakes:**


 * Forgetting `self` in the definition: `def deposit(amount):` (Error).
 * Trying to call it without an object: `BankAccount.deposit(500)` (Error).


-----


##  3️⃣ CLASS METHODS (Shared Behavior)


**Intuition:** A class method is the behavior of the **Class itself**.


 * **Key Characteristic:** It uses the `@classmethod` decorator and receives `cls` instead of `self`.
 * **Usage:** For factories, counters, or configuration that affects all objects.


###  Example 1: Tracking Total Students


```python
class Student:
   count = 0  # Class Variable


   def __init__(self, name):
       self.name = name
       Student.count += 1


   @classmethod
   def show_count(cls):
       # We use 'cls' to access the class variable
       print("Total students =", cls.count)


# Usage
s1 = Student("Ram")
s2 = Student("Shyam")
Student.show_count()
```


###  Example 2: Library Book Counter


```python
class Library:
   total_books = 0


   def __init__(self, title):
       self.title = title
       Library.total_books += 1


   @classmethod
   def show_total(cls):
       print("Books in library:", cls.total_books)


# Usage
b1 = Library("Python 101")
b2 = Library("AI Handbook")
Library.show_total()
```


**Why Class Method?** Because `total_books` is a shared counter, not specific to book 1 or book 2.


**Common Mistakes:**


 * Using `self` instead of `cls`.
 * Trying to access `self.name` inside a class method (The class doesn't know which object you mean\!).


-----


##  4️⃣ STATIC METHODS (Utility Helpers)


**Intuition:** A static method belongs to **neither** the object nor the class.


 * **Key Characteristic:** It uses `@staticmethod` and receives **NO** implicit first argument (no `self`, no `cls`).
 * **Usage:** For logic that relates to the class theme but doesn't touch class data.


###  Example 1: Simple Message


```python
class Robot:
   @staticmethod
   def commercial():
       print("This robot is built by Cyberdyne Systems!")


# Usage
Robot.commercial()
```


###  Example 2: Leap Year Checker (Utility)


```python
class DateUtils:
   @staticmethod
   def is_leap(year):
       return (year % 4 == 0 and year % 100 != 0) or (year % 400 == 0)


# Usage
print(DateUtils.is_leap(2024))
```


**Why Static?** Because leap year logic is pure math. It doesn't need to know *which* date object called it.


**Common Mistakes:**


 * Trying to access `self` or `cls` inside a static method.
 * Using static methods when you actually need to modify object state.


-----


##  5️⃣ PROPERTY METHODS (The Pythonic Encapsulation)


**Intuition:**
In languages like Java, we write `get_name()` and `set_name()`.
Python says: *"Don't do that. It looks ugly."*
We use **Properties** to make methods look and feel like variables.


###  Example 1: Phone Number Validation


```python
class User:
   def __init__(self, phone):
       self._phone = phone  # Protected variable


   # The Getter (Read)
   @property
   def phone(self):
       return self._phone


   # The Setter (Write with Validation)
   @phone.setter
   def phone(self, value):
       if len(value) != 10:
           print("Invalid phone! Must be 10 digits.")
       else:
           self._phone = value


# Usage
u = User("9876543210")
print(u.phone)      # Looks like a variable, runs the Getter
u.phone = "123"     # Looks like a variable, runs the Setter -> Error!
```


###  Example 2: Computed Property (Read-Only)


```python
class Circle:
   def __init__(self, radius):
       self.radius = radius


   @property
   def diameter(self):
       return 2 * self.radius


# Usage
c = Circle(5)
print(c.diameter)  # 10
c.radius = 7
print(c.diameter)  # 14 (Updates automatically!)
```


**Why Property?** Because `diameter` is calculated from `radius`. We don't want to store it separately, but we want to access it simply like `c.diameter`.


-----


##  6️⃣ FINAL SUMMARY TABLE (Write in Notebook)


| Method Type | Decorator | First Argument | Purpose |
| :--- | :--- | :--- | :--- |
| **Instance** | None | `self` | Actions on specific objects (Deposit, Greet). |
| **Class** | `@classmethod` | `cls` | Actions on the whole class (Counters, Factory). |
| **Static** | `@staticmethod` | None | Utility/Helper functions (Math, Validation). |
| **Property** | `@property` | `self` | Getters/Setters with clean syntax. |


-----


##  7️⃣ FINAL CONCLUSION


 * **Instance Methods:** "I work with **MY** data."
 * **Class Methods:** "I work with **OUR** shared data."
 * **Static Methods:** "I am just a helper function here."
 * **Properties:** "I look like a variable, but I act like a method."


Together, these four types allow you to model any behavior in a Python system efficiently.

