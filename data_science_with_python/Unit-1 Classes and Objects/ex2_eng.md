

#  Classes & Objects in Python (The Library Book Example)




## 1️⃣ The Objective: Building a Library Book System





**Narrative:**
"Welcome back. In our previous session, we built a Bank System. We saw that a single variable wasn't enough to hold a customer's data, so we created a **Class**.


Today, we are going to solidify this knowledge by building a **Library Management System**.


**The Requirement:**
We need to model a **Book**.


1. **Data (Attributes):** Every book has a Title, an Author, and a Number of Copies available.
2. **Behavior (Methods):**
     * Show the book details.
     * **Issue** a copy (decrease stock).
     * **Return** a copy (increase stock).


**The Challenge:**
We will have multiple books—say, a Python book and a Data Structures book. We must prove that issuing a Python book does not accidentally reduce the copies of the Data Structures book. This is the concept of **Memory Isolation**."


### Reminder: The "Pattern" for Today


"Before we code, remember the rule from our last class:


1. You will see **`self`** as the first word in every method.
2. You will see **`__init__`** used to set up the data.


**Do not overthink them yet.**


 * Think of `self` as meaning 'This specific object'.
 * Think of `__init__` as the 'Setup Function'.
   Just follow the pattern for now. We will deep-dive into the internals in the next session."


-----


## 2️⃣ The Code: Building the Class Step-by-Step





We will write the blueprint (`class`) first.


### Step A: Defining the Blueprint


```python
class Book:
   # 1. ATTRIBUTES (The Data)
   # The '__init__' method runs automatically when we create a new book.
   def __init__(self, title, author, copies_available):
       self.title = title
       self.author = author
       self.copies_available = copies_available


   # 2. BEHAVIOR: Show details
   def show_details(self):
       print("Title  :", self.title)
       print("Author :", self.author)
       print("Copies :", self.copies_available)
       print("----------------------------")


   # 3. BEHAVIOR: Issue a copy
   def issue_book(self):
       # We check if we have copies before issuing
       if self.copies_available > 0:
           self.copies_available = self.copies_available - 1
           print("✅ Issued one copy of:", self.title)
           print("Copies left:", self.copies_available)
       else:
           print("❌ Cannot issue. No copies left for:", self.title)


   # 4. BEHAVIOR: Return a copy
   def return_book(self):
       self.copies_available = self.copies_available + 1
       print("📗 Returned one copy of:", self.title)
       print("Copies now:", self.copies_available)
```


###  Line-by-Line Explanation (The Blueprint)


**1. The Header**
`class Book:`


 * **Explanation:** We are defining a new custom type called "Book". This is the template.


**2. The Setup (`__init__`)**
`def __init__(self, title, author, copies_available):`


 * **Explanation:** This is a special function. It runs automatically the moment we create a Book.
 * `self`: Represents the specific object being created.
 * `title, author...`: The values we pass in from outside.


`self.title = title`


 * **Explanation:** Take the `title` passed in, and attach it to `self` (the object). This saves the data inside the object permanently.


**3. The Display Method (`show_details`)**
`def show_details(self):`


 * **Explanation:** A method to display info. `self` is required so the code knows *which* book's details to print.


`print("Title :", self.title)`


 * **Explanation:** Accesses the title stored in *this* specific object.


**4. The Logic Method (`issue_book`)**
`if self.copies_available > 0:`


 * **Explanation:** Checks the data inside *this* object. Do we have stock?


`self.copies_available = self.copies_available - 1`


 * **Explanation:** Modifies the data. It decreases the count *only for this specific book*.


-----


### Step B: The Execution (Creating Objects)





Now we move to the "Testing Area" (the main block) to bring these books to life.


```python
if __name__ == "__main__":
   # 1. Create the first Book (Python book)
   b1 = Book("Introduction to Python", "Guido van Rossum", 3)


   # 2. Create the second Book (Data Structures book)
   b2 = Book("Data Structures in C", "Reema Thareja", 2)


   # 3. Let them "interact"
   print("=== Initial Library Stock ===")
   b1.show_details()
   b2.show_details()
```


###  Line-by-Line Explanation (The Execution)


**1. The Entry Point**
`if __name__ == "__main__":`


 * **Explanation:** This is the standard Python way to say "Start the program here". It’s our testing zone.


**2. Creating Object 1 (`b1`)**
`b1 = Book("Introduction to Python", "Guido van Rossum", 3)`


 * **Explanation:**
     * Python calls `__init__`.
     * It sets `self.title` to "Introduction to Python".
     * It sets `self.copies_available` to 3.
     * It creates a box in memory and `b1` points to it.


**3. Creating Object 2 (`b2`)**
`b2 = Book("Data Structures in C", "Reema Thareja", 2)`


 * **Explanation:**
     * Python creates a **totally separate** box in memory.
     * It sets `self.title` to "Data Structures...".
     * `b2` points to this second box.


**4. Interacting**
`b1.show_details()`


 * **Explanation:** Calls the method. Inside the method, `self` becomes `b1`. So it prints the Python book's details.


`b2.show_details()`


 * **Explanation:** Calls the same method, but now `self` becomes `b2`. So it prints the DS book's details.


-----


## 3️⃣ THE CORE LOGIC: Memory Isolation





"This is the most important concept of OOP. Even though `b1` and `b2` were created from the same `Book` class code, they are strangers in memory."


**The Visual:**


 * Imagine **b1** is a physical card on the left side of a table. It has "Copies: 3" written on it.
 * Imagine **b2** is a physical card on the right side of the table. It has "Copies: 2" written on it.
 * If I erase "3" on the left card and write "2", does the card on the right change? **No.**


This is **Memory Isolation**.


-----


## 4️⃣ THE PROOF: Modifying One Book Object





Let's prove this with code. We will issue a copy of the Python book (`b1`) and see if the Data Structures book (`b2`) stays safe.


```python
if __name__ == "__main__":
   # SETUP: Create two books
   b1 = Book("Introduction to Python", "Guido van Rossum", 3)
   b2 = Book("Data Structures in C", "Reema Thareja", 2)


   # TEST 1: Print initial state
   print("=== BEFORE ISSUING ===")
   b1.show_details()  # Python book: 3 copies
   b2.show_details()  # DS book: 2 copies


   # TEST 2: Issue ONLY from b1 (Python book)
   print("=== Issuing Python Book (b1) ===")
   b1.issue_book()    # copies_available for b1 should reduce


   # TEST 3: Check both again
   print("=== AFTER ISSUING FROM b1 ONLY ===")
   print("b1 copies:", b1.copies_available)  # Changed (3 -> 2)
   print("b2 copies:", b2.copies_available)  # Still 2
```


### Explanation of the Result


1. **Before:** `b1` had 3, `b2` had 2.
2. **Action:** We called `b1.issue_book()`.
     * The code went into the object `b1`.
     * It calculated `3 - 1 = 2`.
     * It updated `b1`'s memory.
3. **After:**
     * `b1` is now 2.
     * `b2` is **still 2**.


**Conclusion:** Modifying one object **never** affects another object, unless we explicitly tell it to.


-----


## 5️⃣ COMMON MISTAKES (Python Specific)




Here are the errors you might face while writing this:


**1. Forgetting `self` in Methods**


```python
def show_details():   # ❌ Missing 'self'
   print("Title:", title)
```


 * **Why it fails:** Python tries to pass the object to the method automatically. If you don't define `self` to catch it, the program crashes.


**2. Forgetting `self.` for Attributes**


```python
def __init__(self, title, copies):
   title = title      # ❌ This is just a temporary variable!
   copies = copies    # ❌ It disappears when __init__ finishes.
```


 * **The Fix:** You *must* write `self.title = title`. The `self.` part acts like the glue that sticks the data to the object.


**3. Indentation Errors**


```python
class Book:
def show_details(self):   # ❌ Error: It's touching the left margin
   print(...)
```


 * **The Fix:** In Python, everything inside the class must be indented (pushed to the right).


**4. Calling the Class instead of the Object**


```python
Book.issue_book()   # ❌ Wrong
```


 * **Why:** Which book are you issuing? The concept of a book? No, you need a specific book.
 * **Correct:** `b1.issue_book()`


-----


## 6️⃣ FINAL SUMMARY




"Today, we reinforced three major pillars of Python OOP:


1. **The Class (Template):** `class Book`. It defines what data and behavior a book *should* have.
2. **The Object (Instance):** `b1` and `b2`. These are the actual books created in memory.
3. **Memory Isolation:** Each object has its own separate storage. Changing `b1` does not touch `b2`.


**Homework:**
Try to create a class called `MobilePhone`.


 * **Data:** Brand, BatteryLevel.
 * **Behavior:** `Phone()` (reduces battery), `charge_phone()` (increases battery).
   Create two phones and prove that using one doesn't drain the battery of the other\!"

