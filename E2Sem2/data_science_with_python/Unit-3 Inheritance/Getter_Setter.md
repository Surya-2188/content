
#  CHAPTER: GETTERS & SETTERS IN PYTHON


**(Encapsulation + Real Deployment Examples)**



**Prerequisite:** Basic knowledge of Classes and Objects.


-----


##  1️⃣ WHY DO WE NEED GETTERS & SETTERS?


**Encapsulation** in Python is based on a simple rule: **Keep your data private and only give access through methods.**


When you declare a variable as **Private** (using `__`):


```python
self.__balance = 5000
```


Then:


1.  You **CANNOT** access it directly from outside (`obj.__balance` fails).
2. You **MUST** use **Getter and Setter methods** to access it.


This is the **Industry Standard**. It is used in:


 * **Banking Systems** (Protecting balance)
 * **University Portals** (Validating marks)
 * **Login Systems** (Hiding passwords)


**The Rule:** Whenever a variable is private, you **must** write Getters and Setters, otherwise, that variable becomes useless (unreachable).


-----


##  2️⃣ GENERAL SYNTAX


```python
# 1. GETTER (Read Access)
def get_value(self):
   return self.__value


# 2. SETTER (Write Access with Validation)
def set_value(self, new_value):
   self.__value = new_value
```


**Important:** In real software, Setters **ALWAYS** include validation logic (checking if data is correct) before saving it.


-----


##  3️⃣ INDUSTRY EXAMPLE 1 — BANK ACCOUNT SYSTEM


**(Protecting Balance & Validating Phone Numbers)**


###  The Program


```python
class BankAccount:


   def __init__(self, name, phone):
       self.__name = name        # Private
       self.__balance = 0        # Private
       self.__phone = phone      # Private


   # --- GETTER METHODS (Safe Read) ---
   def get_balance(self):
       return self.__balance


   def get_phone(self):
       return self.__phone


   # --- SETTER METHOD (Safe Update) ---
   def set_phone(self, new_phone):
       # Validation: Phone must be 10 digits and numeric
       if len(new_phone) == 10 and new_phone.isdigit():
           self.__phone = new_phone
           print("Phone updated successfully.")
       else:
           print("Invalid phone number!")


   # --- BUSINESS METHODS ---
   def deposit(self, amt):
       if amt > 0:
           self.__balance += amt
       else:
           print("Invalid deposit amount")


   def withdraw(self, amt):
       if amt <= 0:
           print("Invalid withdrawal amount")
       elif amt > self.__balance:
           print("Insufficient balance")
       else:
           self.__balance -= amt
```


###  Line-by-Line Explanation


1. **`class BankAccount:`** → Creates the blueprint.
2. **`self.__name`, `self.__balance`** → These are **Private Variables**. They are hidden inside the object.
3. **`get_balance(self)`** → This is a **Getter**. Since `__balance` is private, this public method allows the user to *see* the balance without touching it directly.
4. **`set_phone(self, new_phone)`** → This is a **Setter**. It doesn't just save the data; it checks if the number is valid (10 digits). This prevents "Bad Data" from entering the system.
5. **`deposit()` & `withdraw()`** → These act as setters for the balance, ensuring no negative money is added or removed.


###  Why is this Encapsulation?


 * **Private Data** → Hidden from the world.
 * **Getter** → Controlled Reading.
 * **Setter** → Controlled Writing (Validation).


-----


##  4️⃣ INDUSTRY EXAMPLE 2 — STUDENT CGPA SYSTEM


**(Data Integrity & Logic Validation)**


###  The Program


```python
class Student:


   def __init__(self, name, roll):
       self.name = name               # Public
       self.roll = roll               # Public
       self.__cgpa = 0.0              # Private (Sensitive)


   # --- GETTER ---
   def get_cgpa(self):
       return self.__cgpa


   # --- SETTER ---
   def set_cgpa(self, new_cgpa):
       # Validation: CGPA must be between 0 and 10
       if 0 <= new_cgpa <= 10:
           self.__cgpa = new_cgpa
       else:
           print("Error: CGPA must be between 0 and 10")
```


###  Line-by-Line Explanation


1. **`self.__cgpa = 0.0`** → CGPA is private. We don't want anyone setting it to 100 or -5 by mistake.
2. **`get_cgpa()`** → Returns the current CGPA.
3. **`set_cgpa(new_cgpa)`** → This Setter enforces the university rule: **CGPA must be between 0 and 10**. If someone tries `s1.set_cgpa(12)`, the system rejects it.


###  Why is this Encapsulation?


It prevents **Data Corruption**. A student object can never exist with an invalid CGPA because the Setter acts as a guard.


-----


##  5️⃣ INDUSTRY EXAMPLE 3 — USER LOGIN SYSTEM


**(Security & Authentication)**


This is the most critical example. Passwords must never be readable, only verify-able.


### The Program


```python
class User:


   def __init__(self, username, password):
       self.username = username
       self.__password = password          # Private


   # --- PASSWORD CHECKER (Not a standard Getter) ---
   def check_password(self, pwd):
       return pwd == self.__password      # Returns True/False, NOT the password


   # --- PASSWORD SETTER ---
   def set_password(self, old_pwd, new_pwd):
       # 1. Security Check: Must know old password
       if old_pwd != self.__password:
           print("Incorrect old password")
           return


       # 2. Validation: New password strength
       if len(new_pwd) < 6:
           print("Password too short (Min 6 chars)")
           return


       # 3. Update
       self.__password = new_pwd
       print("Password updated successfully")
```


###  Line-by-Line Explanation


1. **`self.__password`** → Stored privately.
2. **`check_password(self, pwd)`** → Notice there is **NO** `get_password()`. We never return a password. We only compare input against the stored private data.
3. **`set_password(...)`** → This Setter implements strict logic:
     * User must prove identity (Old Password).
     * New password must meet rules (Length \> 6).
     * Only then is the private variable updated.


###  Why is this Encapsulation?


It represents **Security**. The internal state (`__password`) is completely hidden and can only be modified if specific security conditions are met inside the method.


-----


##  6️⃣ HOW GETTERS & SETTERS CONNECT TO ENCAPSULATION


**Encapsulation** = Binding Data + Methods together to ensure safety.


| Component | Role in Encapsulation |
| :--- | :--- |
| **Private Variable** | Hides the data (`__var`). Safety. |
| **Getter** | Allows **Safe Reading**. |
| **Setter** | Allows **Safe Updating** (with Validation). |


**The Q\&A:**


 * **Q:** Why use Getters and Setters?
 * **A:** Because private variables are inaccessible from the outside. To read or update them safely without breaking the rules, we **MUST** provide Getters and Setters.


-----


##  7️⃣ COMMON STUDENT MISTAKES




 **1. Accessing private variable directly**


 * `print(emp.__salary)` → **ERROR**. Use the getter.


 **2. Writing Setters without Validation**


 * `def set_age(self, age): self.__age = age`
 * **Bad Practice:** You lost the benefit of encapsulation. You should check if `age > 0`.


 **3. Returning Passwords**


 * `def get_password(self): return self.__password`
 * **Security Risk:** Never create getters for passwords or PINs.


 **4. Thinking `_name` is Private**


 * In Python, `_name` is just Protected (convention). Only `__name` is Private.


 **5. Forgetting to create Getters/Setters**


 * If you make a variable `__private` but don't write methods, that data becomes locked forever and you can't use it.


-----


##  8️⃣  SUMMARY

1. **Encapsulation** is the technique of hiding internal object details and controlling access.
2. **Private Variables (`__var`)** cannot be accessed directly from outside the class.
3. **Getters** are used to **Retrieve** values safely.
4. **Setters** are used to **Update** values, usually with validation rules.
5. **Industry Rule:** Whenever you declare a private variable, you must decide how to expose it using Getters and Setters.

