
#  CHAPTER: INHERITANCE + ENCAPSULATION IN PYTHON


**(The College Management System Example)**


##  1️⃣ THE FOUNDATION: WHY WE NEED INHERITANCE


Imagine designing a software system for a college.
We have different categories of people:


1. **Person** (The common base)
2. **Student**
3. **Teacher**


And specific roles:

4.**CR (Class Representative)** → A special type of Student.

5.**HOD (Head of Department)** → A special type of Teacher.


**Without Inheritance:**
We would repeat code for `name`, `email`, and `phone` in every single class. If we wanted to change how an email is stored, we would have to edit 5 different files.


**✔ With Inheritance:**
We write the common logic **ONCE** in the `Person` class. Everyone else inherits it.


 * **Inheritance** = Code Reuse.
 * **Encapsulation** = Data Security.


-----


##  2️⃣ PYTHON INHERITANCE BASICS


**Syntax:**


```python
class Child(Parent):
   pass
```


**Meaning:**


 * Child **IS-A** Parent.
 * Child inherits attributes and methods.
 * Child can extend (add new) or override (change) behavior.


-----


##  3️⃣ ENCAPSULATION IN PYTHON (The Rules)


Inheritance allows classes to share data, but Encapsulation ensures that sensitive data is not misused. Python manages this via **Naming Conventions**:


| Type | Syntax | Meaning | Behavior in Inheritance |
| :--- | :--- | :--- | :--- |
| **Public** | `self.name` | Accessible everywhere | **Inherited** & Accessible directly. |
| **Protected** | `self._email` | Internal usage (Convention) | **Inherited**. Developers treat it as "Family Only". |
| **Private** | `self.__phone` | Strictly internal | **NOT directly inherited**. Name-mangled (`_Class__name`). |


-----


## 4️⃣ FULL PYTHON PROGRAM (Inheritance + Encapsulation)


Here is the complete system.


```python
# ----------------------------------------------------
# 1. BASE CLASS (Parent)
# ----------------------------------------------------
class Person:
   def __init__(self, name, email, phone):
       # 1. PUBLIC: Accessible everywhere
       self.name = name
      
       # 2. PROTECTED: Intended for internal/child use
       self._email = email
      
       # 3. PRIVATE: Accessible ONLY inside Person class
       self.__phone = phone


   # Public method to display details safely
   def view_profile(self):
       print(f"Name : {self.name}")
       print(f"Email: {self._email}")
       # Only Person class can access its own private __phone
       print(f"Phone: {self.__phone}")


   def send_message(self, msg):
       print(f"Sending message to {self.name}: {msg}")




# ----------------------------------------------------
# 2. CHILD CLASS (Student)
# ----------------------------------------------------
class Student(Person):
   def __init__(self, name, email, phone, student_id, branch):
       # Initialize Parent Attributes using super()
       super().__init__(name, email, phone)
      
       # Student specific Private Attribute
       self.__student_id = student_id
       self.branch = branch


   def register_for_course(self, course):
       print(f"{self.name} registered for {course}")


   def submit_assignment(self, course):
       print(f"{self.name} submitted assignment for {course}")




# ----------------------------------------------------
# 3. CHILD CLASS (Teacher)
# ----------------------------------------------------
class Teacher(Person):
   def __init__(self, name, email, phone, employee_id, subject):
       # Initialize Parent
       super().__init__(name, email, phone)
      
       self.__employee_id = employee_id  # Private
       self.subject = subject            # Public


   def take_class(self, class_name):
       print(f"{self.name} is teaching {self.subject} to {class_name}")




# ----------------------------------------------------
# 4. MULTILEVEL INHERITANCE (CR extends Student)
# ----------------------------------------------------
class CR(Student):
   def send_announcement(self, msg):
       print(f"📢 CR {self.name} announced: {msg}")


   def request_meeting(self):
       print(f"CR {self.name} requested a meeting with HOD")




# ----------------------------------------------------
# 5. MULTILEVEL INHERITANCE (HOD extends Teacher)
# ----------------------------------------------------
class HOD(Teacher):
   def approve_leave(self, emp_name):
       print(f"✅ HOD {self.name} approved leave for {emp_name}")


   def schedule_meeting(self):
       print(f"📅 HOD {self.name} scheduled a department meeting")




# ----------------------------------------------------
# MAIN EXECUTION
# ----------------------------------------------------
if __name__ == "__main__":


   print("=== CR DEMO ===")
   # Creating a CR Object
   # Arguments: Name, Email, Phone, ID, Branch
   cr = CR("Raju", "raju@college.com", "9876543210", "S1001", "CSE")


   cr.view_profile()                     # Inherited from Person
   cr.register_for_course("Python 101")  # Inherited from Student
   cr.send_announcement("Exam Tomorrow") # Own method


   print("\n=== HOD DEMO ===")
   # Creating an HOD Object
   hod = HOD("Dr. Ravi", "ravi@college.com", "9123456780", "T5001", "AI")


   hod.view_profile()                    # Inherited from Person
   hod.take_class("CSE-A")               # Inherited from Teacher
   hod.approve_leave("Prof. Meera")      # Own method
```


-----


##  5️⃣ OUTPUT ANALYSIS


```text
=== CR DEMO ===
Name : Raju
Email: raju@college.com
Phone: 9876543210
Raju registered for Python 101
📢 CR Raju announced: Exam Tomorrow


=== HOD DEMO ===
Name : Dr. Ravi
Email: ravi@college.com
Phone: 9123456780
Dr. Ravi is teaching AI to CSE-A
✅ HOD Dr. Ravi approved leave for Prof. Meera
```


-----


##  6️⃣ LINE-BY-LINE EXPLANATION


###  The Parent (`Person`)


 * **`self.name` (Public):** Inherited by everyone. `CR` and `HOD` can access it directly.
 * **`self._email` (Protected):** Inherited. By convention, children like `Student` use it, but outside code should avoid it.
 * **`self.__phone` (Private):** **NOT** directly inherited. `Student` cannot type `self.__phone`. It relies on the parent's public method `view_profile()` to display it.


###  The Child (`Student` & `Teacher`)


 * **`super().__init__(...)`**: This is the most critical line. It calls the Parent's constructor to set up `name`, `email`, and `phone`. Without this, the Parent part of the object is never created.
 * **`self.__student_id`**: A private variable specific to the Student. Even the `Person` class doesn't know about this.


###  The Grandchild (`CR` & `HOD`)


 * **Multilevel Inheritance:** `CR` gets features from `Student`, which got features from `Person`.
 * **Extensions:** `CR` adds `send_announcement`. This behavior didn't exist in `Person` or `Student`.


-----


##  7️⃣ INHERITANCE + ACCESS LEVELS TABLE


| Member in Parent | Visibility in Child (Student/Teacher) | Notes |
| :--- | :--- | :--- |
| `self.name` (Public) | ✅ **Yes** | Fully accessible. |
| `self._email` (Protected) | ✅ **Yes** | Accessible, but implies "Family Use Only". |
| `self.__phone` (Private) | ❌ **No** | Hidden via name mangling (`_Person__phone`). |
| `view_profile()` (Public Method) | ✅ **Yes** | Child can call this to access parent's private data. |


-----


##  8️⃣ COMMON MISTAKES STUDENTS MAKE


 **Mistake 1: Forgetting `super().__init__()`**


 * **Code:** `def __init__(self, name): self.name = name`
 * **Result:** You overwrite the initialization logic instead of reusing it. Always call `super()`.


 **Mistake 2: Trying to access Parent's Private Data directly**


 * **Code:** `print(self.__phone)` inside `Student` class.
 * **Result:** `AttributeError`. Private members stay with the owner class.


 **Mistake 3: Confusing Protected (`_`) with Private (`__`)**


 * **Reality:** Python does not enforce `_protected`. It is just a polite sign saying "Keep Out". `__private` is the one that actually changes the name to hide it.


 **Mistake 4: Passing wrong arguments to `super`**


 * **Code:** `super().__init__(name)` when Parent expects `(name, email, phone)`.
 * **Result:** `TypeError`.


-----


##  9️⃣ FINAL SUMMARY


1. **Inheritance (`class Child(Parent)`)** allows us to create a hierarchy (Person → Student → CR) to reuse code.
2. **Encapsulation** protects data within this hierarchy.
3. **Public Members** are shared freely.
4. **Protected Members (`_`)** are shared within the family (convention).
5. **Private Members (`__`)** stay strictly with the class that defined them.
6. **`super()`** is the bridge that connects the Child to the Parent.


**Next Step:**
"Now that we have built the structure, what if `Student` and `Teacher` both have a method called `get_role()`, but they need to behave differently? This leads to **Polymorphism (Method Overriding)**."

