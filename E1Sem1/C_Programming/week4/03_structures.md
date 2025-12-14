


# COMPLETE LECTURE: `struct` IN C


**(Motivation $\rightarrow$ Definition $\rightarrow$ Memory $\rightarrow$ Usage $\rightarrow$ Comparison $\rightarrow$ Pitfalls $\rightarrow$ Closure)**


## 1\. Why `union` Is Not Enough (Transition from Union to Structure)


Before introducing `struct`, we must explicitly expose the limitation of `union`, because structure exists to fix exactly this problem.


Recall what we learned about `union`:


> **A union allows multiple interpretations of the same memory, but only one member can exist at a time.**


Now consider this real-world requirement:


> **“I want to store information about a student.”**


A student has:


 * Roll number (Integer)
 * Marks (Float)
 * Grade (Character)


All of these:


1. **Exist together**.
2. **Are meaningful at the same time**.


### Attempt Using `union` (Wrong Approach)


```c
union Student {
   int roll;
   float marks;
   char grade;
};
```


If we write:


```c
s.roll = 10;
s.marks = 85.5;
```


**What happens?**


 * `marks` overwrites `roll`.
 * Roll number is lost forever.


### Key Insight


**Union fails when data must coexist.**
So we need a new mechanism that says:


> **“Each member gets its own independent memory.”**


That mechanism is called a **structure**.


-----


## 2\. What is a `struct` (Core Definition)


> **A structure is a user-defined datatype that groups related variables, where each member has its own independent memory.**


This one sentence is the entire philosophy of structures.


-----


## 3\. Defining a Structure — Line-by-Line Explanation


### 3.1 Structure Definition (Blueprint Stage)


```c
struct Student {
   int roll;
   float marks;
   char grade;
};
```


Now we explain every word and symbol, slowly and clearly.


**Line 1:** `struct Student {`


 * `struct` $\rightarrow$ **keyword**. Tells the compiler: *“I am defining a structure datatype.”*
 * `Student` $\rightarrow$ **name of the structure type**. Just like `int`, `float`, or a union name.
 * `{` $\rightarrow$ **beginning of member list**.


 **Important (Repeat Clearly):**
At this stage:


1. **NO memory is allocated.**
2. This is only a **type definition** (a blueprint).
3. It is exactly like defining a rule.


**Line 2:** `int roll;`


 * **Meaning:** *“This structure has an integer member called `roll`.”*


**Line 3:** `float marks;`


 * **Meaning:** *“This structure has a float member called `marks`.”*


**Line 4:** `char grade;`


 * **Meaning:** *“This structure has a character member called `grade`.”*


**Line 5:** `};`


 * Ends the structure definition. **Semicolon is mandatory.**


-----


## 4\. Critical Difference: `struct` vs `union` (Conceptual Pivot)


This comparison must be explicitly stated to clear any confusion.


| Aspect | `union` | `struct` |
| :--- | :--- | :--- |
| **Memory** | Shared | Separate |
| **Members Valid** | One at a time | All at once |
| **Size** | Largest member | Sum of all members |
| **Real-world Data** | ❌ Poor fit | ✅ Perfect fit |
| **Safety** | Risky | Safe |


-----


## 5\. Declaring a Structure Variable (Memory Allocation)


Now compare this with a normal variable.


**Normal Variable:**


```c
int x;
```


 * Allocates memory for one integer.


**Structure Variable:**


```c
struct Student s;
```


 * **Meaning:** *“Allocate memory for ALL members of `struct Student` and name it `s`.”*
 * This is the moment memory is actually allocated.


-----


## 6\. Memory Layout of a Structure (Very Important)


Given:


```c
struct Student {
   int roll;     // 4 bytes
   float marks;  // 4 bytes
   char grade;   // 1 byte
};
```


Memory allocated for `s` is approximately:


| roll (4 bytes) | marks (4 bytes) | grade (1 byte) | ...padding... |


**Total size:**


 * **NOT** maximum (like union).
 * **SUM** of all members (+ some padding for alignment).


This is the exact opposite of union behavior.


-----


## 7\. Using a Structure Variable (Dot Operator)


### Assigning Values


```c
s.roll = 10;
s.marks = 85.5;
s.grade = 'A';
```


**Meaning:**


1. Store roll number.
2. Store marks.
3. Store grade.
4. **All values coexist safely.**


### 8\. The Dot (`.`) Operator — Deep Understanding


The dot operator means:


> **“Access a member inside a structure variable.”**


**Examples:**


 * `s.roll`
 * `s.marks`
 * `s.grade`


**Read as:** *“member `roll` inside structure variable `s`”*.


-----


## 9\. First Complete Structure Program (Observation-Based)


```c
#include <stdio.h>


struct Student {
   int roll;
   float marks;
   char grade;
};


int main(void) {
   // Declare a structure variable
   struct Student s;


   // Assign values to each member
   s.roll = 10;
   s.marks = 85.5;
   s.grade = 'A';


   // Access and print each member
   printf("Roll: %d\n", s.roll);
   printf("Marks: %.2f\n", s.marks);
   printf("Grade: %c\n", s.grade);


   return 0;
}
```


**How Students Should Read This:**


1. Define a structure type.
2. Declare a structure variable.
3. Assign values to each member.
4. Access and print each member.
5. **Observe that NO data is lost.**


-----


## 10\. Why Structures Match Real-World Thinking


Structures allow us to model **entities**, not just numbers.


**Examples:**


 * Student
 * Employee
 * Date
 * Book
 * Sensor data
 * Vehicle state


Each entity has:


1. **Multiple properties.**
2. **All properties exist together.**


This is exactly what `struct` provides.


-----


## 11\. Structures vs Arrays vs Unions (Very Important Comparison)


Students often confuse these three because all of them group data. But they solve very different problems.


### 11.1 Structures vs Arrays


**What an Array Is (Recall):**


```c
int marks[5];
```


 * **Meaning:** *“Create 5 memory boxes of the **same** datatype arranged sequentially.”*
 * **Key properties:** All elements same type, accessed by index (`[]`).
 * **Mental Model:** Collection of items.


**What a Structure Is:**


```c
struct Student { ... };
```


 * **Meaning:** *“Create ONE entity that has **different** kinds of data.”*
 * **Key properties:** Members can be different types, accessed by dot operator (`.`).
 * **Mental Model:** A single object/record.


**Why Array Fails Where Structure Works:**
If we try to store student data using arrays (`int roll[50]`, `float marks[50]`), data gets scattered. Structure fixes this by keeping everything related to one student together.


| Feature | Array | Structure |
| :--- | :--- | :--- |
| **Data type** | Same | Different |
| **Purpose** | Collection | Entity |
| **Access** | Index `[]` | Dot `.` |
| **Meaning** | Many values | One object |


### 11.2 Structures vs Unions (Real-World Analogy)


 * **Union** $\rightarrow$ One room, multiple labels (Bedroom / Office / Kitchen — only one use at a time).
 * **Structure** $\rightarrow$ House with multiple rooms (Bedroom AND Office AND Kitchen — all usable together).


-----


## 12\. Decision Rule: When to Use What?


Students should remember this decision logic:


1. **Ask: Do all values need to exist together?**
     * **Yes** $\rightarrow$ **Structure**
     * **No** $\rightarrow$ go to next question.
2. **Ask: Are values mutually exclusive?**
     * **Yes** $\rightarrow$ **Union**
3. **Ask: Are all values same type and many in number?**
     * **Yes** $\rightarrow$ **Array**


-----


## 13\. Common Mistakes in Structures (Initialization & Basic Usage)


**Instruction:** `❌` marks the wrong line. Read the explanation after the code.


###  Mistake 1: Using Structure Without Initialization


```c
#include <stdio.h>


struct Student {
   int roll;
   float marks;
};


int main(void) {
   struct Student s;
   printf("%d %.2f\n", s.roll, s.marks);  // ❌ uninitialized values
   return 0;
}
```


**Explanation:** Declaring `struct Student s;` allocates memory, but **no values are assigned**. Members contain garbage values. Structures are not auto-initialized.
 **Correct thinking:** *“Declaration creates space, not values.”*


###  Mistake 2: Assuming Structure Members Get Default Values


```c
#include <stdio.h>


struct Student {
   int roll;
   float marks;
};


int main(void) {
   struct Student s;   // ❌ student assumes roll = 0, marks = 0.0
   if (s.roll == 0) {
       printf("Roll is zero\n");
   }
   return 0;
}
```


**Explanation:** C does not assign default values (like 0) to structure members. Any comparison before initialization is meaningless.
 **Rule:** Always initialize before reading.


### Mistake 3: Trying to Initialize Structure Like an Array


```c
#include <stdio.h>


struct Student {
   int roll;
   float marks;
};


int main(void) {
   struct Student s = [10, 85.5];  // ❌ array-style initialization
   return 0;
}
```


**Explanation:** Square brackets `[]` are only for arrays. Structures use **curly braces `{}`**. Structure initialization follows member order, not indexing.


###  Mistake 4: Treating Structure Like an Indexed Collection


```c
#include <stdio.h>


struct Student {
   int roll;
   float marks;
};


int main(void) {
   struct Student s;
   s[0] = 10;   // ❌ invalid thinking
   return 0;
}
```


**Explanation:** A structure is not a collection. It does not support indexing. Members are accessed by **name**, not position.
**Correct mental model:** *“Structure is one object, not many elements.”*


### Mistake 5: Confusing Structure with Array of Same-Type Data


```c
#include <stdio.h>


struct Data {
   int a;
   int b;
};


int main(void) {
   struct Data d;
   d[1] = 20;   // ❌ thinking struct behaves like int array
   return 0;
}
```


**Explanation:** Even if members have the same type (`int a`, `int b`), a structure is not an array. Member access is semantic (`d.a`, `d.b`). Indexing has no meaning.


###  Mistake 6: Expecting Structure Memory to Behave Like Union


```c
#include <stdio.h>


struct Data {
   int x;
   float y;
};


int main(void) {
   struct Data d;
   d.x = 10;
   d.y = 5.5;   // ❌ student fears x is overwritten
   printf("%d\n", d.x);
   return 0;
}
```


**Explanation:** This fear comes from union thinking. In a structure, `x` and `y` have **separate memory**. Assigning `y` does not affect `x`.
 **Key contrast:** Union shares memory. Structure does not.


### Mistake 7: Forgetting That Structure Is a Single Variable


```c
#include <stdio.h>


struct Student {
   int roll;
   float marks;
};


int main(void) {
   struct Student s1, s2;
   s1.roll = 10;
   s2 = s1;          // ❌ student unsure what happens
   printf("%d\n", s2.roll);
   return 0;
}
```


**Explanation:** `s2 = s1;` copies **all** members from `s1` to `s2`. Structures support direct assignment. This is valid and useful.
 **Important early clarification:** Structure assignment copies the entire object.


-----


## 14\. Final Mental Model (Structure)


> **A structure is a container where each member has its own independent memory, allowing related data to exist together safely.**


-----


## 15\. Final Conclusion (Structure – Part 1 Closure)


**Students must remember:**


1. `struct` is a user-defined datatype.
2. Structure members have **independent memory**.
3. Size = **sum** of members.
4. Dot operator is used to access members.
5. Structures model real-world entities.
6. Structures fix exactly what unions cannot.


**One-Line Takeaway:**


> **Use struct when data must exist together. Use union when data is mutually exclusive.**

