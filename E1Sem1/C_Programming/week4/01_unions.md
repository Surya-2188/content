


#  LECTURE: `union` IN C


**(Definition $\rightarrow$ Memory $\rightarrow$ Usage $\rightarrow$ Pitfalls $\rightarrow$ Arrays $\rightarrow$ Pointers $\rightarrow$ Call by Reference $\rightarrow$ Closure)**


## 1\. Start from the Known: A Normal Variable


Before learning `union`, we must anchor the idea in what students already know.


### 1.1 Defining a Normal Variable


```c
int x;
```


Let us explain this fully.


 * `int` $\rightarrow$ **built-in datatype keyword**
     * tells the compiler: *“Reserve memory suitable to store an integer.”*
 * `x` $\rightarrow$ **variable name**
     * a label for that memory location.


So this line means:


> **“Create ONE memory box capable of holding an integer and name it x.”**


Now:


```c
x = 10;
```


means:


> **“Put the value 10 into the memory box named x.”**


### 1.2 Important Observation


`x` has:


1. a datatype
2. a size
3. a memory address


**Crucially:** `x` stores exactly **one** interpretation of memory. This model works only when one datatype is enough.


-----


## 2\. Where This Model Fails (Motivation for `union`)


Now consider this requirement:


> **“I want a variable that can store an integer OR a float OR a character — but never all at the same time.”**


A naive attempt might look like this:


```c
int i;
float f;
char c;
```


**The Problem:**


1. Three separate memory boxes are allocated.
2. Only one value is actually meaningful at any given moment.
3. **Memory is wasted.**


So we need a way to say:


> **“These variables should reuse the same memory.”**


That is exactly what a **union** does.


-----


## 3\. What is `union` (Core Definition)


> **A union is a user-defined datatype in which all members share the same memory location.**


This single sentence is the heart of unions.


-----


## 4\. Defining a Union — Line-by-Line Explanation


### 4.1 Union Definition (Blueprint Stage)


```c
union Data {
   int i;
   float f;
   char c;
};
```


Now let us explain every word and symbol.


**Line 1:** `union Data {`


 * `union` $\rightarrow$ **keyword**. Tells the compiler: *“I am defining a union datatype.”*
 * `Data` $\rightarrow$ **name of the union type**. Just like `int` or `float`, but user-defined.
 * `{` $\rightarrow$ **start of member list**.


⚠️ **Important:** At this point, **no memory is allocated.** We are only defining a type (a rule), not creating a variable.


**Line 2:** `int i;`


 * `int` $\rightarrow$ datatype
 * `i` $\rightarrow$ member name
 * **Meaning:** *“This union can treat its memory as an integer called `i`.”*


**Line 3:** `float f;`


 * **Meaning:** *“The SAME memory can also be treated as a float called `f`.”*


**Line 4:** `char c;`


 * **Meaning:** *“The SAME memory can also be treated as a character called `c`.”*


**Line 5:** `};`


 * Ends the union definition. **Semicolon is mandatory.**


### 4.2 Critical Understanding (Repeat This to Students)


A union has **ONE** memory box, but **MULTIPLE** interpretations.


> **Only ONE interpretation is valid at a time.**


-----


## 5\. Declaring a Union Variable (Memory Allocation)


Now compare these two declarations.


**Normal variable:**


```c
int x;
```


 * Allocates memory immediately.
 * Size = size of `int`.


**Union variable:**


```c
union Data d;
```


 * **Meaning:** *“Allocate ONE memory box large enough to hold the largest member of union Data and name it `d`.”*


-----


## 6\. Size of a Union (Why Memory Is Saved)


```c
union Data {
   int i;    // 4 bytes
   float f;  // 4 bytes
   char c;   // 1 byte
};
```


**Allocated size:**


 * **NOT** $4 + 4 + 1$
 * **ONLY** $\max(4, 4, 1) = 4$ bytes.


**Because:**


1. All members share the same memory space.
2. Only one exists at a time.


-----


## 7\. Using a Union Variable


### 7.1 Assigning Values


```c
d.i = 10;
```


 * **Meaning:** *“Treat the union memory as an integer and store 10.”*


Next:


```c
d.f = 3.14f;
```


 * **Meaning:** *“Reuse the SAME memory and treat it as a float.”*


**This overwrites the integer value.** This is expected behavior, not an error.


-----


## 8\. The Dot (`.`) Operator


The dot operator means:


> **“Access a member inside a composite datatype.”**


**Examples:**


 * `d.i`
 * `d.f`
 * `d.c`


**Read as:** *“member `i` inside union variable `d`”*.
Normal variables like `x` have no members, so the dot operator is never used with them.


-----


## 9\. First Complete Observation Program


```c
#include <stdio.h>


union Data {
   int i;
   float f;
   char c;
};


int main(void) {
   union Data d;


   // Store integer
   d.i = 10;
   printf("i = %d\n", d.i);


   // Store float (overwrites integer)
   d.f = 3.14f;
   printf("f = %f\n", d.f);


   // Store char (overwrites float)
   d.c = 'A';
   printf("c = %c\n", d.c);


   return 0;
}
```


**Conceptual Reading:**


1. One memory box is created.
2. Integer stored.
3. Same memory reused as float.
4. Same memory reused as char.


-----


## 10\. Disadvantages of `union`


1. **Only one member** can exist at a time.
2. **No automatic tracking** of which member is active.
3. Compiler gives **no warnings** if you access the wrong member (logic error).
4. High risk of **bugs**.
5. **Poor readability** for beginners.
6. **Not suitable** for real-world grouped data (like a Student record).


-----


## 11\. Common Mistakes (Simple, Complete Programs)


`❌` marks the wrong line.


###  Mistake 1: Expecting Independent Storage


```c
#include <stdio.h>


union Data {
   int i;
   float f;
};


int main(void) {
   union Data d;
   d.i = 10;
   d.f = 2.5f;            // Overwrites d.i
   printf("%d\n", d.i);   // ❌ Garbage value or interpreted float bits
   return 0;
}
```


**Explanation:** `d.f` overwrites the same memory used by `d.i`. You cannot have both.


###  Mistake 2: Using Union Where Structure Is Needed


```c
#include <stdio.h>


union Student {
   int roll;
   float marks;
};


int main(void) {
   union Student s;
   s.roll = 1;
   s.marks = 90.0f;                        // Overwrites roll
   printf("%d %.2f\n", s.roll, s.marks);   // ❌ roll is lost
   return 0;
}
```


**Explanation:** A student needs a Roll Number **AND** Marks at the same time. Union forces a choice between them. This is logically wrong.


###  Mistake 3: Forgetting `union` Keyword


```c
union Data {
   int i;
};


int main(void) {
   Data d;   // ❌ Compiler doesn't know 'Data'
   return 0;
}
```


**Explanation:** You must write `union Data d;`.


-----


## 12\. Union Is a User-Defined Datatype (Very Important Insight)


Just like:


```c
int x;
```


we can write:


```c
union Data d;
```


So:


1. `union Data` is a **datatype**.
2. `d` is a **variable**.
3. `d` has: **size, memory, and an address.**


That means:


> **A pointer can point to a union just like it points to an `int` or `float`.**


-----


## 13\. Pointers and Union (Interaction)


```c
union Data d;
union Data *p = &d;
```


 * `p` stores the **address** of `d`.
 * `p->i` accesses member `i` using the pointer.


-----


## 14\. Call by Reference with Union (Detailed Explanation)


**Key Idea:**
Since a union is a datatype with memory and address, we can pass it by reference using a pointer.


### Complete Program: Passing Union by Reference


```c
#include <stdio.h>


union Data {
   int i;
   float f;
};


void update(union Data *u) {
   u->i = 100;
}


int main(void) {
   union Data d;


   d.i = 20;
   update(&d);
   printf("After update: %d\n", d.i);


   return 0;
}
```


**Line-by-Line Explanation:**


1. `union Data *u` $\rightarrow$ pointer that can store the address of a union variable.
2. `update(&d)` $\rightarrow$ address of union variable `d` is passed.
3. `u->i = 100;` $\rightarrow$ go to the memory of `d` $\rightarrow$ treat it as integer $\rightarrow$ update original value.


This is **call by reference**, exactly like with `int`, but now applied to a user-defined datatype.


-----


## 15\. Arrays of Union (Very Important Concept)


### 15.1 Defining an Array of Union


```c
union Data arr[3];
```


**Meaning:**


> **“Create an array of 3 union variables, each with its own shared memory box.”**


### Complete Example: Array of Union


```c
#include <stdio.h>


union Data {
   int i;
   float f;
};


int main(void) {
   union Data arr[3];


   arr[0].i = 10;     // Element 0 holding int
   arr[1].f = 2.5f;   // Element 1 holding float
   arr[2].i = 30;     // Element 2 holding int


   printf("%d\n", arr[0].i);
   printf("%f\n", arr[1].f);
   printf("%d\n", arr[2].i);


   return 0;
}
```


**Explanation:**


1. Each array element (`arr[0]`, `arr[1]`, etc.) is a **separate** union.
2. Each element has its **own** memory.
3. But **inside** each element, the members share memory.


-----


## 16\. Final Mental Model (Union)


> **A union is a single memory box with multiple labels. Only one label is meaningful at any moment.**


-----


## 17\. Final Conclusion (Union Fully Closed)


**Students must remember:**


1. Union is a datatype.
2. Union variable has memory and address.
3. Only one member is valid at a time.
4. Size = largest member.
5. Union works with pointers.
6. Union can be passed by reference.
7. Arrays of unions are possible.
8. **Programmer must track correctness.**


**One-line takeaway:**


> **Use union for mutually exclusive data only. Never use it when data must exist together.**



