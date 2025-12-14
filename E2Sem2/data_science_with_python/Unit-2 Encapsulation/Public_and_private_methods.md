
#  PUBLIC, PROTECTED & PRIVATE METHODS IN PYTHON






##  0️⃣ THE CONCEPT


Encapsulation in Python applies to two things:


1. **Data** (Private variables like PIN, balance).
2. **Behavior** (Private and Protected methods).


Just as we hide sensitive data (like a PIN), we must also hide **sensitive actions** (like updating internal records) so they cannot be triggered accidentally or maliciously from the outside.


-----


##  1️⃣ REAL-WORLD SCENARIO (The Credit Card)


A credit card system allows three types of operations. This maps perfectly to Python's access levels:


### ✔ 1. Public Operation (User-facing)


 * **Action:** **Pay Bill**
 * **Access:** Anyone can call it.
 * **Logic:** This is what customers are allowed to do.


### ✔ 2. Protected Operation (Internal Calculation)


 * **Action:** **Calculate Interest**
 * **Access:** The system **MUST** do it, but the user **SHOULD NOT** trigger it manually.
 * **Logic:** It is an internal helper task.


### ✔ 3. Private Operation (Highly Sensitive)


 * **Action:** **Update Registered Mobile Number**
 * **Access:** Must be **LOCKED**. Cannot be called from outside.
 * **Logic:** This happens automatically only after secure verification (like paying a bill or KYC), never directly.


-----


##  2️⃣ FULL PYTHON PROGRAM


Here is the complete code implementing the scenario above.


```python
class CreditCard:


   def __init__(self, card_number, pin, mobile):
       self.card_number = card_number
       self.__pin = pin            # Private data
       self.__mobile = mobile      # Private data


   # -------------------------------------------------
   # 1. PUBLIC METHOD (Accessible by everyone)
   # -------------------------------------------------
   def pay_bill(self, amount):
       print(f"Payment of ₹{amount} received for card {self.card_number}.")
      
       # Public method internally calling protected & private helpers
       self._calculate_interest()
       self.__update_mobile("98765XXXXX")    # Simulate secure update


   # -------------------------------------------------
   # 2. PROTECTED METHOD (Internal use only - Convention)
   # -------------------------------------------------
   def _calculate_interest(self):
       print(">> (Internal) Calculating interest...")


   # -------------------------------------------------
   # 3. PRIVATE METHOD (Strictly Hidden)
   # -------------------------------------------------
   def __update_mobile(self, new_mobile):
       self.__mobile = new_mobile
       print(">> (Secure) Mobile number updated internally!")


# --- TESTING THE CODE ---
my_card = CreditCard("1234-5678", 1234, "91234XXXXX")


print("--- 1. Calling Public Method ---")
my_card.pay_bill(5000)        # WORKS


print("\n--- 2. Calling Protected Method ---")
my_card._calculate_interest() # WORKS (but bad practice)


print("\n--- 3. Calling Private Method ---")
# my_card.__update_mobile("98765XXXXX")  # FAILS (Uncommenting this crashes program)
```


-----


##  3️⃣ LINE-BY-LINE EXPLANATION


###  `__init__()` — The Constructor


```python
def __init__(self, card_number, pin, mobile):
```


 * Stores `card_number` (Public).
 * Stores `__pin` and `__mobile` (Private).
 * **Note:** These private attributes cannot be accessed from outside the class.


###  PUBLIC METHOD — `pay_bill()`


```python
def pay_bill(self, amount):
```


 * This method is meant for customers.
 * **Key Rule:** Public methods **CAN** access protected and private methods internally.
 * Inside this method, we call `self.__update_mobile()`. This is allowed because we are **inside** the class.


###  PROTECTED METHOD — `_calculate_interest()`


```python
def _calculate_interest(self):
```


 * **Single Underscore (`_`)**: Signals "Protected".
 * **Behavior:** Python does **NOT** enforce security here. You *can* call it from outside.
 * **Meaning:** It acts as a warning sign to other programmers: *"This is internal logic. Don't touch unless you know what you are doing."*


### PRIVATE METHOD — `__update_mobile()`


```python
def __update_mobile(self, new_mobile):
```


 * **Double Underscore (`__`)**: Signals "Private".
 * **Behavior:** This method is **Hidden** and **Secure**.
 * **Mechanism:** Python performs **Name Mangling**. It secretly renames this method to `_CreditCard__update_mobile`.
 * **Result:** If you try `my_card.__update_mobile(...)`, Python says "I don't know what that is."


-----


## 4️⃣ OUTPUT ANALYSIS


**1. When calling Public Method:**


```text
Payment of ₹5000 received for card 1234-5678.
>> (Internal) Calculating interest...
>> (Secure) Mobile number updated internally!
```


 * *Observation:* The public method successfully triggered the private logic.


**2. When calling Protected Method:**


```text
>> (Internal) Calculating interest...
```


 * *Observation:* It works, but in a real project, you should avoid calling this manually.


**3. When calling Private Method:**


```text
AttributeError: 'CreditCard' object has no attribute '__update_mobile'
```


 * *Observation:* **Encapsulation Success.** The outside world cannot touch the mobile update logic directly.


-----


##  5️⃣ WHY THIS EXAMPLE IS PERFECT


Students relate to this hierarchy instantly:


1. **Paying bills** → Public (Anyone can do it).
2. **Bank calculations** → Protected (Happens in background).
3. **Updating sensitive info** → Private (Locked process).


This creates a mental model of **Accessibility** that is hard to forget.


-----


##  6️⃣ COMMON MISTAKES STUDENTS MAKE




❌ **Mistake 1 — Calling private method directly**


 * `my_card.__update_mobile(...)`
 * **Result:** Fails.


❌ **Mistake 2 — Thinking `_method` is private**


 * `my_card._calculate_interest()`
 * **Reality:** This works in Python. It is only a convention, not a lock.


❌ **Mistake 3 — Forgetting `self.` inside methods**


 * **Wrong:** `__mobile = new_mobile`
 * **Correct:** `self.__mobile = new_mobile` (Variables belong to the object, so `self` is mandatory).


❌ **Mistake 4 — Using the mangled name**


 * `my_card._CreditCard__update_mobile(...)`
 * **Reality:** Technically works, but is considered "hacking" your own code. Never do this in production.


❌ **Mistake 5 — Writing private method incorrectly**


 * **Wrong:** `def _update_mobile():` (Only Protected)
 * **Wrong:** `def ___update_mobile():` (Too many underscores)
 * **Correct:** `def __update_mobile():` (Exactly two leading underscores).


-----


## 7️⃣ FINAL SUMMARY TABLE (Exam-Ready)


| Type | Syntax | Access | When to Use |
| :--- | :--- | :--- | :--- |
| **Public** | `def pay_bill()` | Fully accessible | User-facing actions |
| **Protected** | `def _calc()` | Accessible but discouraged | Internal helper logic |
| **Private** | `def __update()` | **Hidden** (Name Mangled) | Sensitive operations |


-----


##  8️⃣ ONE-LINE EXAM ANSWER


**Encapsulation of Behavior** in Python allows us to restrict access to methods. **Public** methods are open to all, **Protected** (`_`) methods imply internal use, and **Private** (`__`) methods are strictly hidden using name mangling to prevent external access.

