# ENCAPSULATION IN PYTHON 







## 0️⃣ THE FOUNDATION


Students already understand that **Objects** hold their own **separate data** in memory. **Encapsulation** is the rule that protects this data, ensuring that one object’s internal state cannot be accidentally changed by another.


## 1️⃣ WHAT IS ENCAPSULATION? (Simple Definition)


Encapsulation is the OOP technique of **hiding an object’s internal data** and providing **controlled access** only through public **methods** defined inside the object.


Think of an object as a **secure box**:


 * The **Data** inside is hidden/private.
 * You use **Public Methods** (like keys or secure ports) to access or modify the data.


Encapsulation prevents: **Accidental modification, tampering, wrong updates,** and **mixing of data** between objects.


-----


###  STUDENT ANALOGY


A college stores each student’s personal details in a closed folder:


 * **Students** cannot open their own folder.
 * **Other students** definitely cannot.
 * Only **Office Staff (Methods)** can update the folder.


Encapsulation forces this logical separation:
| OOP Concept | Analogy |
| :--- | :--- |
| **Private Data** | Inside the secure folder |
| **Public Methods** | Office staff updating the folder |
| **Objects** | Each student’s separate folder |


-----


##  3️⃣ HOW PYTHON IMPLEMENTS ENCAPSULATION


Unlike Java or C++, Python does **not** have strict access modifiers (`public`, `private`). It uses **naming conventions** to achieve the same result:


| Type | Syntax | Meaning |
| :--- | :--- | :--- |
| **Public** | `self.name` | Anyone can access |
| **Protected (Convention)** | `self._name` | Internal use (Programmers should avoid external access) |
| **Private (Encapsulation)**| `self.__name` | **Hidden** from outside access |


We use **double underscores `__`** for **TRUE encapsulation** in Python.


-----


##  4️⃣ STUDENT EXAMPLE — SIMPLE ENCAPSULATION


This program separates the data storage from the access mechanism.


### ✅ Program


```python
class Student:


   def __init__(self):
       # Private Data (Hidden)
       self.__name = None
       self.__roll = None
       self.__branch = None


   def set_name(self, name):
       self.__name = name


   def set_roll(self, roll):
       self.__roll = roll


   def set_branch(self, branch):
       self.__branch = branch


   def show_identity(self):
       # Accessing private data safely from INSIDE the object
       print("ID:", self.__roll, "| Name:", self.__name, "| Branch:", self.__branch)


# Usage
s1 = Student()
s2 = Student()


s1.set_name("Raju")
s1.set_roll(101)
s1.set_branch("CSE")


s2.set_name("Rani")
s2.set_roll(102)
s2.set_branch("ECE")


s1.show_identity()
s2.show_identity()
```
---

###  5️⃣ PROOF OF ENCAPSULATION


If you try to access the private data directly:


```python
print(s1.__name)
```


**Error:** `AttributeError: 'Student' object has no attribute '__name'`


This proves the data is **hidden** and can only be accessed by the public methods (`set_name`, `show_identity`) defined within the `Student` class itself.


-----


##  6️⃣ BANKING APPLICATION — PERFECT ENCAPSULATION


Banking is the ideal example: sensitive data like **PIN, balance, and account number** must be protected. The only way to modify the balance should be through verified transactions (methods).


 * **Goal:** Protect data and enforce mandatory rules (like checking the PIN before a withdrawal).


-----


##  7️⃣ FULL ENCAPSULATED BANK ACCOUNT PROGRAM


```python
import random


class BankAccount:


   def __init__(self, name, phone, pin):
       # 🔒 PRIVATE DATA
       self.__name = name
       self.__phone = phone
       self.__pin = pin
       self.__balance = 0
       self.__accno = random.randint(10000, 99999)


   def get_accno(self):
       return self.__accno


   def verify_pin(self, pin):
       # Internal check, PIN is never exposed outside
       return self.__pin == pin


   def show_details(self):
       # Reveals details safely, keeping PIN hidden
       print("Acc No :", self.__accno)
       print("Name   :", self.__name)
       print("Phone  :", self.__phone)
       print("Balance:", self.__balance)


   # 🔑 CONTROLLED ACCESS METHODS
   def deposit(self, amount):
       self.__balance += amount


   def withdraw(self, amount, pin):
       if self.verify_pin(pin):
           if amount <= self.__balance:
               self.__balance -= amount
               print(f"Withdrawal successful. Remaining balance: {self.__balance}")
           else:
               print("Insufficient Balance!")
       else:
           print("Incorrect PIN!")


   def update_phone(self, new_phone, pin):
       if self.verify_pin(pin):
           self.__phone = new_phone
           print("Phone number updated.")
       else:
           print("Incorrect PIN!")
```


###  8️⃣ PROVING ENCAPSULATION IS WORKING (Banking)


By enforcing private access and controlled methods, the system is secure:


| Action | Code | Result |
| :--- | :--- | :--- |
| **❌ Direct access fails** | `print(acc1.__balance)` | `AttributeError` |
| **❌ Wrong PIN fails** | `acc1.withdraw(1000, 2222)` | `Incorrect PIN!` |
| **✔ Correct PIN works** | `acc1.withdraw(1000, 1111)` | Withdrawal successful |
| **❌ External tampering fails** | `acc1.__balance = 50000` | Creates a NEW public variable, the real balance remains protected. |


-----


## 9️⃣ COMMON MISTAKES STUDENTS MAKE


(Combined from Student + Banking Programs)


❌ **1. Accessing private variables directly:**
\* **Mistake:** `print(acc1.__pin)`
\* **Fix:** Use a public method like `acc1.get_pin()` (if you were to implement one).


❌ **2. Creating a new public variable by mistake (Shallow Update):**
\* **Mistake:** `acc1.__balance = 5000`
\* **Explanation:** This does **NOT** update the internal private `__balance`. Python creates a new, accessible attribute named `_BankAccount__balance` on the outside, leaving the internal data unchanged.


❌ **3. Forgetting `self.` inside methods:**
\* **Mistake:** `__balance = amt`
\* **Fix:** `self.__balance = amt` (Always use `self.` to refer to object attributes).


❌ **4. Mixing objects:**
\* **Mistake:** Checking `acc2.__pin` to update `acc1` (Even if somehow possible, this breaks encapsulation).
\* **Rule:** An object's methods operate only on **that object's** data.


❌ **5. Indentation errors:**
\* **Python is strict:** Methods must be correctly indented inside the class block.


-----


##  1️⃣0️⃣ EXAM ANSWER (Perfect 2-Mark Definition)


**Encapsulation** in Python is the technique of **data hiding**, achieved by marking an object’s internal data as private (using **double underscores `__var`**), and providing **controlled, validated access** only through public methods (Getters/Setters/Business Logic) defined within the class.


## 1️⃣1️⃣5-MINUTE REVISION


 * **Encapsulation** = Locking data + Giving methods as keys.
 * **Private variables** → hidden inside the object (`__name`).
 * **Methods** → provide safe, validated access.
 * **Real systems** like banking rely entirely on encapsulation to protect sensitive data.




