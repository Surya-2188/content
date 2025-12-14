

# COMPLETE SECTION


## Array of Structures in C — Meaning, Power, and Correct Usage


This section answers a very natural student question:


> **“If structures already group data, why do we need an array of structures?”**


We will answer this slowly and clearly.


-----


## 1\. Start from the Problem: Where Simple Arrays Fail


### 1.1 Attempt Using Separate Arrays (Bad Design)


Suppose we want to store data of 5 students:


 * Roll number
 * Marks


A beginner might write:


```c
#include <stdio.h>


int main(void) {
   // Two separate arrays
   int roll[5]   = {1, 2, 3, 4, 5};
   float marks[5] = {78.5, 82.0, 91.0, 66.5, 74.0};


   // Accessing student 2 (index 2)
   printf("Roll: %d, Marks: %.2f\n", roll[2], marks[2]);
   return 0;
}
```


### Why This Is a Problem


1. **Data is split:** `roll` and `marks` are in completely different parts of memory.
2. **Implicit relationship:** You just *hope* that `roll[2]` matches `marks[2]`. There is no actual link in the code.
3. **Fragile:** If you sort the `marks` array, the `roll` array doesn't automatically move with it. The data becomes mismatched.
4. **Hard to manage:** To pass a "student" to a function, you have to pass two separate arrays and an index.


This design does not model reality well.


-----


## 2\. Structures Fix Grouping, But Not Quantity


A structure solves the grouping problem perfectly:


```c
struct Student {
   int roll;
   float marks;
};
```


Now `roll` and `marks` are locked together in one entity.


**But this creates only ONE student.**


To store many students, we need multiple structure variables. Writing this is tedious:


```c
struct Student s1, s2, s3, s4, s5;
```


 * **Repetitive code.**
 * **Non-scalable:** What if you need 100 students?
 * **Impossible to loop over.**


We need a better way.


-----


## 3\. Solution: Array of Structures (Key Idea)


> **An array of structures is a collection of structure variables stored sequentially in memory.**


This gives us the best of both worlds:


1. **Grouping:** The structure keeps related data (roll, marks) together.
2. **Repetition:** The array lets us manage many of them using a simple index (`i`).


-----


## 4\. Defining an Array of Structures


### 4.1 Complete Program


```c
#include <stdio.h>


struct Student {
   int roll;
   float marks;
};


int main(void) {
   // Array of 3 structures
   struct Student s[3];


   // Assigning values to first student
   s[0].roll = 1;
   s[0].marks = 85.5;


   // Assigning values to second student
   s[1].roll = 2;
   s[1].marks = 90.0;


   // Assigning values to third student
   s[2].roll = 3;
   s[2].marks = 78.0;


   // Printing all students using a loop
   for (int i = 0; i < 3; i++) {
       printf("Roll: %d, Marks: %.2f\n", s[i].roll, s[i].marks);
   }


   return 0;
}
```


-----


## 5\. Line-by-Line Explanation


### Structure Definition


```c
struct Student {
   int roll;
   float marks;
};
```


Defines a datatype (blueprint). No memory allocated yet.


### Array Declaration


```c
struct Student s[3];
```


**Meaning:** *“Create an array of 3 `Student` structures.”*


So we get: `s[0]`, `s[1]`, `s[2]`.


 * Each element is a **complete structure**.
 * Each element has its **own memory** for `roll` and `marks`.


### Access Pattern (Very Important)


```c
s[i].roll
s[i].marks
```


**Read as:** *“roll of the i-th student”*.


This is applying:


1. Array indexing (`[]`) to select the specific student.
2. Structure access (`.`) to select the member inside that student.


-----


## 6\. Why Array of Structures Is Powerful (Real-World Thinking)


### What This Design Gives You


1. **Data Integrity:** If you swap `s[0]` and `s[1]`, the roll number and marks move together. You never mix up data.
2. **Easy Looping:** A standard `for` loop handles all records.
3. **Easy Sorting/Searching:** You can sort students by marks just like sorting simple integers.
4. **Database Thinking:** This is exactly how rows in a database table work.


This is the foundation of real programs.


-----


## 7\. Taking Input Using Array of Structures


In real programs, we don't hardcode values. We ask the user.


### Complete Program


```c
#include <stdio.h>


struct Student {
   int roll;
   float marks;
};


int main(void) {
   struct Student s[3];


   // Input Loop
   for (int i = 0; i < 3; i++) {
       printf("Enter roll and marks for student %d: ", i + 1);
       scanf("%d %f", &s[i].roll, &s[i].marks);
   }


   // Output Loop
   printf("\nStudent Records:\n");
   for (int i = 0; i < 3; i++) {
       printf("Roll: %d, Marks: %.2f\n", s[i].roll, s[i].marks);
   }


   return 0;
}
```


### Key Observation


```c
scanf("%d", &s[i].roll);
```


We still use `&` because `scanf` needs the **address** of the integer to store the input.


 * `s[i]` gets the structure.
 * `.roll` gets the member.
 * `&` gets the address of that member.


-----


## 8\. Memory Layout Insight (Important Conceptual Point)


An array of structures is stored **sequentially** in memory.


Imagine memory like this:


```text
[  s[0]  ] [  s[1]  ] [  s[2]  ]
```


Zooming in on `s[0]`:


```text
| roll (4B) | marks (4B) | ... | roll (4B) | marks (4B) | ...
```


So memory is:


 * **Contiguous:** No gaps between students (except alignment padding).
 * **Predictable:** Easy for the CPU to read quickly.


-----


## 9\. Array of Structures vs Array of Pointers (Preview Insight)


It is important to distinguish these two early:


1. `struct Student s[10];`
     * **Array of Structures.** Contiguous memory block. The structures live *inside* the array.
2. `struct Student *p[10];`
     * **Array of Pointers.** The array holds addresses. The actual structures live somewhere else (usually allocated with `malloc`).


We are currently studying \#1.


-----


# COMMON MISTAKES: Array of Structures


This section covers the specific errors students make when transitioning from single structures to arrays.


### Mistake 1: Treating Structure Like a 2D Array


```c
s[i][j] = 10;    // ❌ WRONG
```


**Explanation:** Structures are not indexed. Members must be accessed by **name** (`.roll`). Only the array part uses `[]`.


###  Mistake 2: Forgetting the Dot Operator


```c
s[i]roll = 5;   // ❌ WRONG
```


**Explanation:** The dot (`.`) is mandatory to connect the structure variable to its member.


###  Mistake 3: Using Pointer Syntax Without Pointer


```c
s[i]->roll = 5;   // ❌ WRONG
```


**Explanation:** The arrow (`->`) is only for **structure pointers**. `s[i]` gives you an actual structure variable, so you must use the dot (`.`).


###  Mistake 4: Using `sizeof(s)` as Loop Limit


```c
for (int i = 0; i < sizeof(s); i++)  // ❌ WRONG
```


**Explanation:** `sizeof(s)` gives the total size in **bytes**, not the number of elements.
**Correct Way:**


```c
int n = sizeof(s) / sizeof(s[0]);
```


###  Mistake 5: Mixing Up Members Across Indices


```c
s[0].roll = 1;
s[1].marks = 90.0;   // ❌ Unrelated student
```


**Explanation:** While syntactically valid, logically you are setting the roll number of one student and the marks of a *different* student. Be careful with indices.


### Mistake 6: Thinking Array of Structures Is Same as Structure of Arrays


```c
struct Class {
   int roll[10];
   float marks[10];
};
```


**Explanation:** This is a "Structure of Arrays". It is valid, but different. It stores all rolls together, then all marks together. It does not group "one student" into one unit. "Array of Structures" (`struct Student s[10]`) is usually better for data modeling.


###  Mistake 7: Passing Whole Array Without Size


```c
printStudents(s);   // ❌ Ambiguous
```


**Explanation:** In C, arrays decay to pointers. The function receiving the array won't know how long it is unless you pass the size explicitly: `printStudents(s, 3);`.


-----


## 10\. Final Mental Model (Must Be Repeated)


1. **Array** gives **quantity** (many items).
2. **Structure** gives **meaning** (grouped data).
3. **Array of structures** gives **real-world modeling** (a list of records).


-----


## FINAL CLOSURE STATEMENT


If a structure represents one real object (like one book or one employee), an **array of structures** represents a real-world collection (a library or a company). This is the backbone of all database-like programs in C.

