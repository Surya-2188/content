

#  Classes and Objects in Python (The Bank Example)






## 1️⃣ The Starting Point — Understanding the Real Problem





**Narrative:**
Imagine you have just been hired to build software for a major bank. Your manager gives you the requirements. Your system must handle a customer and do the following:


1. Store their **Name**.
2. Store their **Account Balance**.
3. **Greet** the customer when they log in.
4. Allow them to **Deposit** money.
5. Allow them to **Withdraw** money.
6. Optionally, **Estimate Interest** on their balance.


We will now see how to design this in Python step-by-step, starting with why the "old ways" don't work.


-----


## 2️⃣ Why a Single Variable Fails





In Python, if you try to solve this with simple variables:


```python
balance = 5000.0
```


**The Issue:**
This stores only the balance. But a real customer has a Name, Account Number, Phone Number, Email, Aadhaar, etc. A single variable cannot hold distinct pieces of information for one entity.


So, the natural next thought is... **"Okay, let's group them in a List."**


-----


## 3️⃣ Why a Simple List Fails





Students often try this:


```python
customer1 = ["Rahul", 5000.0]
# Index 0 is Name, Index 1 is Balance
```


At first, this looks okay. But it fails quickly in a real project.


**Problem 1 – No Meaningful Labels**
What is `customer1[0]`? Is it the Name? The City? The Account Type? After 2–3 months of coding, nobody remembers what "Index 0" stands for.


**Problem 2 – Hard to Extend**
If you add more fields later:


```python
customer1 = ["Rahul", 5000.0, "HYD", "Savings"]
```


Now you must remember: `0`→Name, `1`→Balance, `2`→City, `3`→Type. It is very easy to make a mistake and access the wrong index.


**Problem 3 – No Behavior**
This list is just data. It cannot "deposit" or "withdraw." You are forced to write separate functions elsewhere:


```python
def deposit(customer_list, amount):
   customer_list[1] += amount  # Modifying the list manually
```


We are separating the **Data** (the list) from the **Behavior** (the function).


-----


## 4️⃣ Why Dictionaries Are Better, But Not Enough





A dictionary gives us labels, which solves the "Index" problem:


```python
customer1 = {
   "name": "Rahul",
   "balance": 5000.0
}


def deposit(customer, amount):
   customer["balance"] += amount
```


**The Limit:**
This is better, but it is still **Procedural Programming**.


 * The Data is in `customer1`.
 * The Behavior is in `deposit(...)`, `withdraw(...)`.
 * They are not tied together in one unit.


**Real World Analogy:**


 * A **Bank Account** object should hold money and know how to deposit/withdraw.
 * A **Car** object holds fuel and knows how to accelerate/brake.


We want that same modeling power inside Python.


-----


## 5️⃣ The Solution — Object Oriented Programming (OOP)





The solution is the same as in Java, just "Pythonic." We treat each real-world entity as an **Object** with its own data and behavior.


To do this, Python provides:


 * `class` → The blueprint.
 * `objects` → The actual instances created from that blueprint.
 * `__init__` → The constructor (initialization).
 * `self` → Refers to the current object.


###  The "Self" & "Init" Disclaimer


*Before writing the first class, address the elephant in the room.*


> **Instructor Note:** "In Python classes, you are about to see two strange-looking things: a word called **`self`** inside the brackets, and a function named **`__init__`**.
>
> **For today's class, do not overthink them.**
>
>   * Think of **`self`** as a required syntax pattern: 'We must write self as the first parameter in methods.'
>   * Think of **`__init__`** as the specific way we set up starting values.
>
> **It is not bad to memorize the pattern first and learn the mechanics later.** In the next class, we will open the hood and explain exactly how Python passes `self` internally. For today, just treat them as necessary parts of the grammar to make our class work."


-----


## 6️⃣ STEP 1 – The Smallest Possible Class




**Goal:** Create a Customer who has a name and can greet us.


```python
# CODE STEP 1


class Customer:
   # Behavior (method) – note the 'self' parameter
   # Don't worry about WHY 'self' is there yet, just include it.
   def greet_customer(self):
       print("Welcome", self.name, "to ABC Bank!")


# --- EXECUTION AREA ---
# (No main() method needed, just top-level code)


c1 = Customer()        # 1. Create an object
c1.name = "Rahul"      # 2. Attach data to the object manually (for now)
c1.greet_customer()    # 3. Call the behavior
```


**Explanation:**


1. `class Customer`: Defines the new type.
2. `def greet_customer(self)`: A method. `self` allows the code to know which customer we are talking about.
3. `c1.name = "Rahul"`: In Python, unlike Java, we can attach attributes dynamically (we will fix this later with `__init__`).


-----


## 7️⃣ STEP 2 – Add Balance and Deposit





Now we add a balance property and a way to change it.


```python
# CODE STEP 2


class Customer:
   def greet_customer(self):
       print("Welcome", self.name, "to ABC Bank!")
       print("Current balance:", self.balance)
  
   def deposit(self, amount):
       # Notice we use 'self.balance' to access THIS object's money
       self.balance = self.balance + amount
       print("Deposited:", amount)


# --- EXECUTION ---
c1 = Customer()
c1.name = "Rahul"
c1.balance = 5000.0       # Initial balance


c1.greet_customer()
c1.deposit(2000.0)        # Balance becomes 7000
c1.greet_customer()
```


**Key Takeaway:**
Notice how `deposit` directly accesses `self.balance`. The **data** and **behavior** now live inside the same object.


-----


## 8️⃣ STEP 3 – Add Withdraw with Logic





Real banking needs rules. We can't withdraw money we don't have.


```python
# CODE STEP 3


class Customer:
   # ... previous methods ...
  
   def withdraw(self, amount):
       if amount <= self.balance:
           self.balance -= amount
           print("✅ Withdrawn:", amount)
       else:
           print("❌ Error: Insufficient balance!")


# --- TESTING ---
c1 = Customer()
c1.name = "Rahul"
c1.balance = 5000.0


c1.withdraw(3000.0)    # Works. Balance = 2000
c1.withdraw(10000.0)   # Fails. Prints Error.
```


The logic is now **encapsulated** inside the object. The user of the object doesn't need to write the `if` statement; the object does it for them.


-----


## 9️⃣ STEP 4 – Returning Values (Interest Calculation)





So far, our methods just print things. Now we want a calculation that **returns** a value.
$$Interest = \frac{Balance \times Rate}{100}$$


```python
# CODE STEP 4


class Customer:
   # ... previous methods ...


   def get_estimated_interest(self, rate):
       """Return simple interest on current balance."""
       interest = (self.balance * rate) / 100
       return interest  # Returning, not printing!


# --- TESTING ---
c1 = Customer()
c1.name = "Rahul"
c1.balance = 4000.0


profit = c1.get_estimated_interest(4.0)
print("📈 Estimated interest profit:", profit)
```


**Concept Check:**


 * `deposit` / `withdraw` → Change internal state (void/Action).
 * `get_estimated_interest` → Calculates and gives back an answer (Value).


-----


## 🔟 STEP 5 – Use `__init__` for Clean Initialization





Up until now, we did this "ugly" setup:


```python
c1 = Customer()
c1.name = "Rahul"   # Manually adding data
c1.balance = 5000.0 # Manually adding data
```


Python gives us a special method called **`__init__`** to do this automatically when the object is created.


> **Reminder:** Again, don't worry about the internal mechanics of `__init__` today. Just treat it as the place where we set up our starting data.


###  The Final Full Code


```python
class Customer:
   # Constructor: Runs automatically when object is created
   # We pass data here to set up the object immediately
   def __init__(self, name, balance):
       self.name = name
       self.balance = balance


   # BEHAVIOR 1: GREET
   def greet_customer(self):
       print("---------------------------")
       print("Welcome", self.name, "to ABC Bank!")
       print("Current balance:", self.balance)
       print("---------------------------")
  
   # BEHAVIOR 2: DEPOSIT
   def deposit(self, amount):
       self.balance += amount
       print("✅ Deposited:", amount)
  
   # BEHAVIOR 3: WITHDRAW WITH LOGIC
   def withdraw(self, amount):
       if amount <= self.balance:
           self.balance -= amount
           print("✅ Withdrawn:", amount)
       else:
           print("❌ Error: Insufficient balance!")
  
   # BEHAVIOR 4: RETURN INTEREST
   def get_estimated_interest(self, rate):
       interest = (self.balance * rate) / 100
       return interest


# EXECUTION / TESTING AREA
if __name__ == "__main__":
   # Step A: Create object with initial data (Clean & Easy)
   c1 = Customer(name="Rahul", balance=5000.0)


   # Step B: Test greeting
   c1.greet_customer()


   # Step C: Test transactions
   c1.deposit(2000.0)      # Balance: 7000
   c1.withdraw(3000.0)     # Balance: 4000
   c1.withdraw(10000.0)    # Error


   # Step D: Test interest calculation
   profit = c1.get_estimated_interest(4.0)
   print("📈 Estimated interest profit:", profit)


   print("Final balance in system:", c1.balance)
```


-----


##  Common Mistakes in Python Classes





These are the most common errors beginners make when coming from Java/C++.


## 1. Forgetting `self` in Method Definition


```python
def greet_customer():  # ❌ Missing 'self'
   print("Hello")
# Result: TypeError when called. Python tries to pass the object, but the method won't take it.
```


## 2. Forgetting `self.` When Accessing Attributes


```python
def __init__(self, name, balance):
   name = name          # ❌ Creates a local variable, doesn't save to object
   balance = balance    # ❌
# Result: The object remains empty. You MUST use self.name = name
```


## 3. Mixing `print` and `return` Confusion


```python
def get_interest(self, rate):
   ans = (self.balance * rate) / 100
   print(ans)  # ❌ Prints but returns None
# Result: variable = c1.get_interest(5) -> variable will be None.
```


## 4. Calling Methods on the Class, not the Object


```python
Customer.deposit(2000)   # ❌ Wrong
# Result: You must create 'c1' first, then call c1.deposit(2000)
```


## 5. Misunderstanding Types*


```python
c1.balance = "five thousand" # ❌ Python allows this because it's dynamic!
c1.withdraw(500)             # Crash! Cannot subtract int from string.
# Advice: Be disciplined with your data types.
```


-----


##  Final Summary 





**Quick Comparison for Java Developers:**


 * **Java Class** $\leftrightarrow$ **Python `class`**
 * **Java Constructor** $\leftrightarrow$ **Python `__init__`**
 * **Java `this`** $\leftrightarrow$ **Python `self`**
 * **Java `main()`** $\leftrightarrow$ **Python `if __name__ == "__main__":`**


**Summary:**
We learned how to bundle data and behavior into a Class. We saw that `self` is required in methods and `__init__` helps us set up data. Even if you don't fully understand the *internals* of `self` and `__init__` yet, you can now write working OOP code.



