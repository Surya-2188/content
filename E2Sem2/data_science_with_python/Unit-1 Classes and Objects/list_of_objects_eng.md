


#   Lists of Objects in Python (The "60 Students" Scenario)


**Goal:** Learn how to manage multiple objects using Python Lists, moving from "One Student" to "An Entire Class".


-----


## 1️⃣ THE REAL SCENARIO — 60 Students, No Details Yet





**Narrative:**
"We know how to create one student using `__init__`.


```python
s1 = Student("Raju", 1001)
```


But imagine you are building software for a college. You know there are **60 students** in a section.


Do you really want to write this?


```python
s1 = Student(...)
s2 = Student(...)
...
s60 = Student(...)
```


**No.** That is ugly, boring, and hard to manage. What if next year there are 70 students? You'd have to rewrite the code.


**The Solution:**
We need a **Container** to hold all these students. In Python, we use a **List**.
We will create a `List` where every item inside it is a `Student` object."


-----


## 2️⃣ Quick Recall: The Student Class





Let's bring back our standard Student class with a constructor.


```python
class Student:
   def __init__(self, name="Unknown", student_id=-1):
       self.name = name
       self.student_id = student_id


   def show_identity(self):
       print(f"Name: {self.name} | ID: {self.student_id}")
```


-----


## 3️⃣ SCENARIO A: Creating a List from User Input





Let's say we want to ask the user to enter details for `n` students one by one.


### The Complete Program


```python
# (Class definition is above)


if __name__ == "__main__":
   # 1. Ask how many students to enter
   n = int(input("How many students are there in the class? "))


   # 2. Create an empty list to store our objects
   students = []


   # 3. Use a loop to create objects one by one
   for i in range(n):
       print(f"\n--- Enter details for Student {i + 1} ---")
       name = input("  Name: ")
       # Convert ID to int immediately
       student_id = int(input("  ID: "))


       # STEP A: Create the object
       s = Student(name, student_id)


       # STEP B: Add it to the list
       students.append(s)


   # 4. Display all students
   print("\n=== CLASS LIST ===")
   for s in students:
       s.show_identity()
```


###  Line-by-Line Explanation


**1. The Setup**
`students = []`


 * Creates an empty list. This will be our container.


**2. The Loop**
`for i in range(n):`


 * If `n` is 60, this loop runs 60 times.


**3. Creating the Object**
`s = Student(name, student_id)`


 * **Crucial:** Inside the loop, we create a **new** object in memory. `s` points to this new object.


**4. Storing the Object**
`students.append(s)`


 * We take that new object and put it safely into the list.
 * Now `students[0]` is the first student, `students[1]` is the second, and so on.


**5. Processing the List**
`for s in students:`


 * This loop goes through the list, picking out one student object at a time (calling it `s`) and asking it to show its identity.


-----


## 4️⃣ SCENARIO B: We Know the Count, But Not the Details





This is the specific question: What if I know there are 60 students, but I don't have their names yet?


We can pre-fill the list with **Default Objects** (Placeholders).


###  The Complete Program (Pre-allocation)


```python
if __name__ == "__main__":
   # Suppose we know the class capacity is 60
   total_students = 60


   # 1. Create a list of 60 "Default" Student objects
   # This loop runs 60 times, creating a new Student() each time
   students = [Student() for _ in range(total_students)]


   # 2. Later, we can update specific students
   # Updating the first student
   students[0].name = "Raju"
   students[0].student_id = 1001


   # Updating the second student
   students[1].name = "Rani"
   students[1].student_id = 1002


   # 3. Check the status
   print("=== First 5 Students ===")
   for i in range(5):
       print(f"Index {i}: ", end="")
       students[i].show_identity()
```


###  Analysis


 * `students = [Student() for _ in range(total_students)]`
     * This uses a **List Comprehension**. It’s a shortcut for "Run `Student()` 60 times and put results in a list".
     * Now you have 60 students, all named "Unknown" with ID -1.
 * `students[0].name = "Raju"`
     * You access the first object in the list and update its data.
 * **The Result:** Indices 0 and 1 have real data. Indices 2 to 59 still have default data.


-----


## 5️⃣ MEMORY VISUALIZATION (List of Objects)





How does this look in memory? Remember **Memory Isolation**.


**Visual:**


 * The variable `students` is just a list of references (addresses).
 * `students[0]` points to **Object A**.
 * `students[1]` points to **Object B**.
 * `students[2]` points to **Object C**.


Changing `students[0]` **only** touches Object A. It does not touch Object B or C.


-----


## 6️⃣ COMMON MISTAKES





## 1. The "One Object" Mistake (Very Common\!)


```python
s = Student()          # ❌ Created ONE object outside loop
students = []


for i in range(5):
   students.append(s) # ❌ Adding the SAME object 5 times
```


 * **Result:** You have a list of 5 items, but they all point to the **same** memory box. If you change one name, **all 5 names change**.
 * **Fix:** `s = Student()` must be **inside** the loop.


## 2. Treating the List as an Object


```python
students.show_identity()   # ❌ Error!
```


 * **Reason:** `students` is a **List**. A list doesn't have a name or ID.
 * **Fix:** You must pick an object *from* the list: `students[0].show_identity()`.


## 3. Confusing Index and Object


```python
for s in students:
   # s is the OBJECT now
   print(students[s]) # ❌ TypeError: indices must be integers
```


 * **Fix:** Just use `s`: `print(s.name)`.


## 4. Off-by-One Errors


```python
for i in range(len(students) + 1): # ❌ Goes too far
   print(students[i])
```


 * **Result:** `IndexError: list index out of range`.


-----


## 7️⃣ FINAL SUMMARY




 * **Lists of Objects** allow us to manage large groups of data (60 students, 1000 bank accounts).
 * You can build them dynamically using a loop (`students.append(s)`).
 * You can pre-fill them with default objects if you know the count (`[Student() for _ in range(60)]`).
 * **Critical Rule:** Every item in the list is an independent object.


