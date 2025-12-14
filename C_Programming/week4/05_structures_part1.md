


#  STRUCTURES IN PRACTICE


**(Modeling Entities $\rightarrow$ Functions on Structures $\rightarrow$ Time Operations $\rightarrow$ Common Mistakes)**


## 1\. Why We Need Structures for Student Data (Revisiting `union`)


Let us begin with a realistic requirement:


> **“We want to store information about a student and perform operations on it.”**


A student has:


 * Roll number (Integer)
 * Marks (Float)
 * Grade (Character)


All of these:


1. **Exist together**.
2. **Must be accessed together**.
3. **Must be processed together**.


### 1.1 Why `union` Fails for Student Data


Consider this incorrect attempt:


```c
union Student {
   int roll;
   float marks;
   char grade;
};
```


Now suppose we write:


```c
s.roll = 10;
s.marks = 82.5;
```


**What happens?**


 * `marks` overwrites `roll`.
 * The Roll number is lost.
 * The "Student" identity becomes meaningless.


 **Conclusion:** A student is **not** mutually exclusive data. Therefore, `union` is the wrong abstraction. This naturally leads us to **structures**.


-----


## 2\. Defining a Structure for Student


### 2.1 Structure Definition (Blueprint)


```c
struct Student {
   int roll;
   float marks;
   char grade;
};
```


**Meaning:** *“A student is an entity that has a roll number, marks, and a grade — all existing together.”*


**At this stage:** No memory is allocated. Only the definition (blueprint) exists.


-----


## 3\. Creating a Student Variable


```c
struct Student s1;
```


**Meaning:** *“Create one student record with independent memory for each member.”*


**Memory layout (conceptual):**
`| roll | marks | grade |`


-----


## 4\. Initializing Student Data


```c
s1.roll = 101;
s1.marks = 86.5;
s1.grade = 'A';
```


Each assignment targets a specific member and **does not** affect other members.


-----


## 5\. Writing Functions that Operate on Student Data


This is where structures become powerful. Instead of writing functions that take many unrelated parameters, we write functions that take **one meaningful entity**.


### 5.1 Function 1: Printing Student Details


**Complete Program:**


```c
#include <stdio.h>


struct Student {
   int roll;
   float marks;
   char grade;
};


// Function receives one complete student
void printStudent(struct Student s) {
   printf("Roll: %d\n", s.roll);
   printf("Marks: %.2f\n", s.marks);
   printf("Grade: %c\n", s.grade);
}


int main(void) {
   struct Student s1;


   s1.roll = 101;
   s1.marks = 86.5;
   s1.grade = 'A';


   printStudent(s1);


   return 0;
}
```


**Explanation:**


 * `struct Student s` $\rightarrow$ The function receives a complete copy of the student data.
 * Inside the function, we access members using the **dot (`.`) operator**.
 * This keeps the function readable, extensible, and logically grouped.


 **Important Conceptual Point:** A structure allows us to pass **meaning**, not just values.


### 5.2 Why Not Use Separate Parameters?


Without structures, the function would look like:


```c
void printStudent(int roll, float marks, char grade);
```


**Problems:**


1. Order matters (easy to mix up `int` and `float`).
2. Poor readability.
3. Hard to extend (adding "Name" means changing every function call).


Structures solve all of these.


-----


## 6\. Another Realistic Example: Time on a Clock


Now let us shift to a different real-world entity: **“We want to represent a time.”**


A time has:


 * Hours
 * Minutes


These values exist together and must be compared or subtracted together. This is a perfect use case for a structure.


-----


## 7\. Defining a Time Structure


```c
struct Time {
   int hour;
   int minute;
};
```


**Meaning:** *“A time consists of an hour and a minute.”*


-----


## 8\. Function: Print Time Nicely


```c
#include <stdio.h>


struct Time {
   int hour;
   int minute;
};


void printTime(struct Time t) {
   // %02d ensures two-digit formatting (e.g., 09:05)
   printf("%02d:%02d\n", t.hour, t.minute);
}


int main(void) {
   struct Time t1;


   t1.hour = 9;
   t1.minute = 5;


   printTime(t1);


   return 0;
}
```


**Explanation:** The structure keeps hour and minute together. The function operates on a single logical object.


-----


## 9\. Function: Convert Time to Total Minutes


This function is the key to time comparison and subtraction.
**Idea:** *“Convert a time into minutes since midnight.”*


```c
#include <stdio.h>


struct Time {
   int hour;
   int minute;
};


int toMinutes(struct Time t) {
   return t.hour * 60 + t.minute;
}


int main(void) {
   struct Time t;
   t.hour = 2;
   t.minute = 30;


   printf("Total minutes = %d\n", toMinutes(t));
   return 0;
}
```


**Explanation:** One structure $\rightarrow$ One integer result. No confusion about which hour belongs to which minute. This mirrors mathematical abstraction cleanly.


-----


## 10\. Function: Difference Between Two Times (Timeline Thinking)


Now let us solve a real problem: **“Find the difference between two times in minutes.”**


**Complete Program:**


```c
#include <stdio.h>


struct Time {
   int hour;
   int minute;
};


// Helper function
int toMinutes(struct Time t) {
   return t.hour * 60 + t.minute;
}


// Logic function
int timeDifference(struct Time t1, struct Time t2) {
   int m1 = toMinutes(t1);
   int m2 = toMinutes(t2);
   return m2 - m1;
}


int main(void) {
   struct Time start, end;


   start.hour = 10;
   start.minute = 15;


   end.hour = 12;
   end.minute = 45;


   printf("Difference = %d minutes\n", timeDifference(start, end));


   return 0;
}
```


**Explanation (Very Important):**


1. Each `Time` is a complete entity.
2. We reduce each to a common scale (minutes).
3. Subtraction becomes trivial.
4. Structures allow clear data flow.


**Comparison:**


 * With structures: `timeDifference(start, end);` (Self-documenting)
 * Without structures: `timeDifference(h1, m1, h2, m2);` (Messy and error-prone)


-----


## 11\. Why Structures Make Functions Cleaner


With structures:


1. Functions take **one** parameter instead of many.
2. **Meaning** is preserved.
3. **Errors** are reduced.
4. Code reads like **real-world logic**.


-----


##  Common Mistakes While Writing Functions Over Structures


**Instruction:** `❌` marks the wrong line. Read the explanation after the code.


### Mistake 1: Passing Individual Members Instead of the Structure


```c
#include <stdio.h>


struct Student {
   int roll;
   float marks;
};


void printStudent(int r, float m) {   // ❌ unnecessary split
   printf("Roll: %d, Marks: %.2f\n", r, m);
}


int main(void) {
   struct Student s;
   s.roll = 101;
   s.marks = 85.5;


   printStudent(s.roll, s.marks);    // ❌ loses structure meaning
   return 0;
}
```


**Explanation:** The function no longer works with a "Student"; it works with unrelated values. Semantic grouping is lost. Any future change to `Student` breaks all functions.
 **Correct thinking:** Functions should accept the entity (`struct Student`), not its scattered pieces.


###  Mistake 2: Assuming Function Can Modify Structure Automatically


```c
#include <stdio.h>


struct Student {
   int roll;
};


void changeRoll(struct Student s) {
   s.roll = 200;     // ❌ modifies only local copy
}


int main(void) {
   struct Student s;
   s.roll = 100;


   changeRoll(s);
   printf("%d\n", s.roll);   // still 100
   return 0;
}
```


**Explanation:** The structure is **passed by value**. The function receives a **copy**. The original structure remains unchanged.
 **Takeaway:** Passing a structure copies it, just like passing an `int`. (Call by reference solves this later).


###  Mistake 3: Forgetting to Use Structure Type in Function Prototype


```c
void printStudent(Student s) {  // ❌ invalid type in C
   // ...
}
```


**Explanation:** In C, `Student` alone is not a datatype. The correct type is `struct Student`. This mistake comes from confusing C with C++.


###  Mistake 4: Accessing Structure Members Incorrectly Inside Function


```c
void printTime(struct Time t) {
   printf("%d:%d\n", hour, minute);  // ❌ missing structure name
}
```


**Explanation:** `hour` and `minute` are not standalone variables. They belong to structure variable `t`. Must always use: `t.hour`, `t.minute`.


###  Mistake 5: Treating Structure Parameter Like an Array


```c
void printData(struct Data d) {
   printf("%d\n", d[0]);   // ❌ structure is not indexed
}
```


**Explanation:** A structure is not a collection. It has named members. Indexing has no meaning.


###  Mistake 6: Writing Functions That Depend on Member Order


```c
struct Student s = {85.5, 101};  // ❌ wrong order
```


**Explanation:** Structure initialization follows member order. The compiler does not check semantic meaning. Roll and marks are swapped silently. Know the structure layout before initializing.


###  Mistake 7: Writing Too Many Similar Functions Instead of Reusing Structure


```c
void printRoll(int r);
void printMarks(float m);
void printGrade(char g);   // ❌ unnecessary fragmentation
```


**Explanation:** This approach ignores the existence of a structure. Logic becomes fragmented.
 **Correct approach:** One structure $\rightarrow$ one meaningful function.


###  Mistake 8: Forgetting That Structure Parameter Is a Copy


```c
void normalize(struct Time t) {
   // Logic to fix time overflow...
   // Changes here are lost after function ends! ❌
}
```


**Explanation:** `t` inside the function is a local copy. Changes are lost when the function ends. This prepares students mentally for **Call by Reference** (using pointers).


-----


## 12\. Final Conceptual Closure for This Section


At this stage, students should clearly understand:


1. Why `union` fails for real entities like Student or Time.
2. Why `struct` is the correct abstraction.
3. How structures group related data.
4. How functions operate naturally on structures.
5. How real-world problems map cleanly to structured data.


**One-Line Takeaway:**


> **Structures allow us to think and program in terms of real-world objects, not disconnected variables.**


