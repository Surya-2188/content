


# COMPLETE SECTION


## Structures, Pointers, and Member Access (\*, ., -\>) — Final Consolidation


This section brings together everything the student has learned so far:


 * Basic variables
 * Pointers
 * User-defined datatypes
 * Structures
 * Pointers to structures
 * Why `->` exists
 * How it differs from `*` and `.`


We will build slowly, always starting from what the student already knows.


-----


## 1\. Start from the Simplest Known Case: Normal Variable


### 1.1 A Normal Integer Variable


```c
#include <stdio.h>


int main(void) {
   int x = 10;
   printf("%d\n", x);
   return 0;
}
```


**Explanation:**


 * `int x = 10;`
     * `int` $\rightarrow$ datatype
     * `x` $\rightarrow$ variable name
 * A memory box is created to store an integer.
 * `x` **directly stores** the value 10.
 * We access the value directly using the variable name.


**Key idea:** A normal variable directly represents a value.


-----


## 2\. Pointer to a Basic Datatype (Revision)


### 2.1 Pointer to an Integer


```c
#include <stdio.h>


int main(void) {
   int x = 10;
   int *p = &x;


   printf("Value of x: %d\n", x);
   printf("Value using pointer: %d\n", *p);


   return 0;
}
```


**Explanation:**


 * `int *p`
     * `*` here means *“p is a pointer variable”*.
 * `p = &x`
     * `&x` means **address of** `x`.
     * `p` stores the address of `x`.
 * `*p`
     * means *“value at the address stored in p”*.


| Expression | Meaning |
| :--- | :--- |
| `p` | address |
| `*p` | value at that address |


**Key rule (Repeat to students):** `*` gives you the value at the address.


-----


## 3\. Moving to User-Defined Datatypes: Structure Variable


### 3.1 Defining and Using a Structure


```c
#include <stdio.h>


struct Student {
   int roll;
   float marks;
};


int main(void) {
   struct Student s;


   s.roll = 1;
   s.marks = 85.5;


   printf("Roll: %d\n", s.roll);
   printf("Marks: %.2f\n", s.marks);


   return 0;
}
```


**Explanation:**


 * `struct Student` $\rightarrow$ user-defined datatype.
 * `struct Student s` $\rightarrow$ creates **one** structure variable. Memory is allocated for all members together.
 * `s.roll`, `s.marks` $\rightarrow$ accessed using **dot (`.`) operator**.


**Key rule:** Use `.` when you have a structure **variable**.


-----


## 4\. Pointer to a Structure (Critical Transition)


Now we combine structures + pointers.


### 4.1 Pointer to Structure


```c
#include <stdio.h>


struct Student {
   int roll;
   float marks;
};


int main(void) {
   struct Student s;
   struct Student *p = &s;


   p->roll = 10;
   p->marks = 90.0;


   printf("Roll: %d\n", p->roll);
   printf("Marks: %.2f\n", p->marks);


   return 0;
}
```


-----


## 5\. Line-by-Line Explanation (Very Important)


**Step 1: Structure Variable**
`struct Student s;`


 * Creates a structure variable.
 * Has memory, size, and an address.


**Step 2: Pointer to Structure**
`struct Student *p = &s;`


 * `struct Student *p`: `p` is a pointer that can store the address of a `Student` structure.
 * `&s`: Address of the structure variable.


Now:


 * `p` $\rightarrow$ address
 * `*p` $\rightarrow$ entire structure


-----


## 6\. Why `*p.roll` Is WRONG


Students often write:


```c
*p.roll = 10;   // ❌ WRONG
```


**Why?**
Because C reads this as: `*(p.roll)`
But `p` is a pointer, so `p.roll` is invalid. The dot operator binds tighter than the star.


-----


## 7\. Correct but Ugly Form


```c
(*p).roll = 10;   // ✅ correct
```


**Meaning:**


1. `*p` $\rightarrow$ fetch the structure (Go to address).
2. `.roll` $\rightarrow$ access member.


-----


## 8\. Arrow Operator (`->`) — Why It Exists


To avoid confusion and ugly syntax, C provides the **Arrow Operator**:


```c
p->roll = 10;
```


**Meaning:** Go to the structure pointed to by `p`, then access `roll`.


So:
$$p \rightarrow roll \equiv (*p).roll$$
They are exactly the same.


-----


## 9\. Comparison Table (Must Be Memorized)


| Situation | Syntax |
| :--- | :--- |
| Normal variable | `x` |
| Pointer to int | `*p` |
| Structure variable | `s.roll` |
| Pointer to structure | `p->roll` |


-----


## 10\. Why Dot (`.`) Cannot Be Used with Structure Pointer


```c
p.roll = 10;   // ❌ WRONG
```


**Because:** `p` is not a structure; `p` is an **address**.
**Rule:** Dot (`.`) works only on actual structure variables, not pointers.


-----


## 11\. Structure Pointer Without Memory (Danger Zone)


```c
struct Student *p;
p->roll = 10;   // ❌ UNDEFINED BEHAVIOR
```


**Why?** `p` points nowhere (uninitialized). No memory is allocated.


-----


## 12\. Correct Ways to Use Structure Pointers


**Case 1: Point to Existing Structure**


```c
struct Student s;
struct Student *p = &s;
p->roll = 5;    // ✅ safe
```


**Case 2: Dynamic Memory Allocation**


```c
#include <stdio.h>
#include <stdlib.h>


struct Student {
   int roll;
   float marks;
};


int main(void) {
   struct Student *p = malloc(sizeof(struct Student));


   p->roll = 20;
   p->marks = 75.0;


   printf("Roll: %d\n", p->roll);
   printf("Marks: %.2f\n", p->marks);


   free(p);
   return 0;
}
```


**Explanation:**


 * `malloc` allocates memory.
 * `p` now points to valid structure memory.
 * `->` accesses members.
 * `free` releases memory.


-----


## 13\. Final Unified Mental Model


1. A pointer never stores data. It stores an **address**.
2. `*` $\rightarrow$ value at address.
3. `.` $\rightarrow$ member of structure.
4. `->` $\rightarrow$ member of structure via pointer.


### Final One-Line Conclusion


**Structures group data, pointers reference memory, and the arrow operator (`->`) exists only to safely connect the two.**


-----


#  COMMON MISTAKES: Structures, Structure Pointers, and Member Access


This section exists to prevent silent logical bugs. Most of these programs compile successfully but behave incorrectly, which is why they are dangerous.


###  Mistake 1: Using `.` Instead of `->` with a Structure Pointer


```c
#include <stdio.h>


struct Student {
   int roll;
};


int main(void) {
   struct Student s;
   struct Student *p = &s;


   p.roll = 10;        // ❌ WRONG
   printf("%d\n", p.roll);


   return 0;
}
```


**Explanation:** `p` is a pointer, not a structure. Dot (`.`) works only with structure variables.
*Compiler error:* “request for member in something not a structure”
 **Correct Line:** `p->roll = 10;`


###  Mistake 2: Writing `*p.roll` Instead of `(*p).roll`


```c
#include <stdio.h>


struct Student {
   int roll;
};


int main(void) {
   struct Student s;
   struct Student *p = &s;


   *p.roll = 5;        // ❌ WRONG
   printf("%d\n", s.roll);


   return 0;
}
```


**Explanation:** C reads `*p.roll` as `*(p.roll)`. `p.roll` is invalid because `p` is a pointer. This is an operator precedence mistake.
**Correct Line:** `(*p).roll = 5;` (or better: `p->roll = 5;`)


###  Mistake 3: Using a Structure Pointer Without Memory


```c
#include <stdio.h>


struct Student {
   int roll;
};


int main(void) {
   struct Student *p;


   p->roll = 10;       // ❌ WRONG (no memory)
   printf("%d\n", p->roll);


   return 0;
}
```


**Explanation:** `p` only declares a pointer. It does not point to valid memory. Writing through it causes undefined behavior (crash).
**Fix:** Use an existing variable (`struct Student s; p=&s;`) or `malloc`.


### Mistake 4: Forgetting `sizeof(struct Student)` in `malloc`


```c
#include <stdio.h>
#include <stdlib.h>


struct Student {
   int roll;
   float marks;
};


int main(void) {
   struct Student *p = malloc(sizeof(p));   // ❌ WRONG


   p->roll = 1;
   p->marks = 80.0;


   printf("%d %.2f\n", p->roll, p->marks);


   free(p);
   return 0;
}
```


**Explanation:** `sizeof(p)` gives the size of the pointer itself (usually 4 or 8 bytes), not the structure. This allocates insufficient memory, leading to corruption.
 **Correct Line:** `struct Student *p = malloc(sizeof(struct Student));`


###  Mistake 5: Thinking `*p` Gives a Member


```c
#include <stdio.h>


struct Student {
   int roll;
};


int main(void) {
   struct Student s = {10};
   struct Student *p = &s;


   printf("%d\n", *p);     // ❌ WRONG


   return 0;
}
```


**Explanation:** `*p` gives the **entire structure**. `printf` expects an `int`. This is a type mismatch.
 **Correct Line:** `printf("%d\n", p->roll);`


###  Mistake 6: Treating Structure Like an Array


```c
#include <stdio.h>


struct Student {
   int roll;
   float marks;
};


int main(void) {
   struct Student s;


   s[0] = 10;          // ❌ WRONG
   printf("%d\n", s.roll);


   return 0;
}
```


**Explanation:** Structures are not indexed. Only arrays use `[]`. Each member must be accessed by name.


###  Mistake 7: Forgetting That `->` Is Only for Pointers


```c
#include <stdio.h>


struct Student {
   int roll;
};


int main(void) {
   struct Student s;


   s->roll = 5;        // ❌ WRONG
   printf("%d\n", s.roll);


   return 0;
}
```


**Explanation:** `s` is a structure, not a pointer. Arrow (`->`) works only with structure pointers.
**Correct Line:** `s.roll = 5;`


###  Mistake 8: Confusing `*` in Declaration vs Usage


```c
struct Student *p;
```


**Wrong Thinking:** “`*p` already has a value.”
**Reality:**


 * `*` in declaration $\rightarrow$ declares a pointer type.
 * `*` in usage $\rightarrow$ fetches value at address.
   These are two different meanings of `*`.


### Mistake 9: Forgetting to Free Dynamically Allocated Structure


```c
#include <stdlib.h>


struct Student {
   int roll;
};


int main(void) {
   struct Student *p = malloc(sizeof(struct Student));
   p->roll = 10;
   return 0;          // ❌ memory leak
}
```


**Explanation:** Memory remains allocated after the program ends (conceptually), which leads to memory leaks in long-running programs.
**Correct Line:** `free(p);`


###  Mistake 10: Assuming Structure Assignment Copies Pointer


```c
struct Student a = {1, 90};
struct Student b = a;
```


**Wrong Thinking:** “b points to a.”
**Reality:** This is a **value copy**. `a` and `b` are independent variables with separate memory.


-----


###  FINAL WARNING TO STUDENTS


Most structure-pointer mistakes compile successfully but produce wrong logic or crashes. That makes them more dangerous than syntax errors.


### FINAL CLOSURE STATEMENT


**If you know when to use `*`, when to use `.`, and when to use `->`, you have mastered structures and pointers in C.**

