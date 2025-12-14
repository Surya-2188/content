
-----


# Topic: Introduction to Arrays in C


## 1\. Why Do We Need Arrays? (The Problem)


Imagine you are a teacher and you need to store the marks of **60 students**.


If you use normal variables, you would have to write:


```c
int m1, m2, m3, m4, ..., m60;
```


**This is a nightmare.**


 * It is exhausting to type.
 * It is impossible to remember which variable belongs to which student.
 * You cannot use a loop to check all marks at once.


**The Solution:**
Instead of creating 60 separate variables, we create **one single container** that holds 60 partitions.


 * Roll no 1 $\rightarrow$ `marks[0]`
 * Roll no 2 $\rightarrow$ `marks[1]`
 * Roll no 3 $\rightarrow$ `marks[2]`


This is why arrays were invented: **To store multiple related values in an organized way.**


-----


## 2\. What Is an Array?


An **array** is a collection of **homogeneous data types** stored in **continuous memory blocks**.


Let's break that definition down:


1. **Homogeneous:** All elements must be of the **same type** (e.g., all must be integers, or all must be floats). You cannot mix them.
2. **Continuous Memory:** Each element sits exactly next to the previous one in the computer's RAM (Memory). No gaps.


**Example:**


```c
int marks[5];
```


-----


## 3\. Syntax of Arrays


To create an array, you need three things: the type, the name, and the size.


```c
datatype arrayName[size];
```


**Examples:**


```c
int a[10];       // Can hold 10 integers
float cgpa[60];  // Can hold 60 decimal numbers
char grade[5];   // Can hold 5 characters
```


The number inside the square brackets `[]` is the **Size**. It tells the computer how many boxes to reserve in memory.


-----


## 4\. Accessing Array Elements


In C programming, counting always starts from **0**, not 1. This is called **Zero-based Indexing**.


 * `a[0]` $\rightarrow$ The **1st** element.
 * `a[1]` $\rightarrow$ The **2nd** element.
 * `a[4]` $\rightarrow$ The **5th** element.


-----


## 5\. How Arrays Are Stored in Memory


Let's visualize this. Suppose we declare:


```c
int a[5] = {10, 20, 30, 40, 50};
```


In C, an integer typically takes **4 bytes** of memory. Since arrays are **continuous**, the addresses will increase by 4 every time.


**Visual Diagram:**


```text
Index:     a[0]       a[1]       a[2]       a[3]       a[4]
         +------+   +------+   +------+   +------+   +------+
Value:   |  10  |   |  20  |   |  30  |   |  40  |   |  50  |
         +------+   +------+   +------+   +------+   +------+
Address:   1000       1004       1008       1012       1016
```


Notice how the address jumps from 1000 to 1004? That is because the data is stored side-by-side.


-----


## 6\. The "Secret" of the Array Name


This is a concept many students miss.
In C, the array name `a` is not just a label. It represents the **Base Address** (the address of the first element).


```c
a == &a[0]
```


So, if the array starts at memory location `1000`, then `a` literally means `1000`.


-----


## 7\. The Indexing Formula


How does the computer know where `a[3]` is located? It uses a simple mathematical formula behind the scenes:


$$Address = Base + (Index \times SizeOfDatatype)$$


Or, in pointer notation:


```c
a[i] = *(a + i)
```


**Translation:**


1. **`a`**: Start at the base address (1000).
2. **`+ i`**: Take `i` steps forward.
3. **`*`**: Look inside that location to get the value.


**Example for `a[3]`:**


 * Start at 1000.
 * Jump 3 steps (each step is 4 bytes).
 * $1000 + (3 \times 4) = 1012$.
 * The computer goes to address 1012 and fetches value **40**.


-----


## 🛑 8. Common Mistakes Students Make


### ❌ Mistake 1: Thinking indexing starts from 1


 * **Wrong:** `a[1]` is the first element.
 * **Correct:** `a[0]` is the first element.


### ❌ Mistake 2: The "Off-by-One" Error


If an array has a size of **5** (`int a[5]`), the valid indexes are **0, 1, 2, 3, 4**.


 * **Wrong:** `a[5] = 10;`
 * **Why?** Index 5 refers to the **6th** element, which doesn't exist. This is out of bounds.


### ❌ Mistake 3: Mixing Data Types


 * **Wrong:**
   ```c
   int a[5];
   a[0] = 10;
   a[1] = 5.5;  // Error! You cannot put a float in an int array.
   ```
 * **Correct:** Arrays must be **homogeneous** (all elements must be the same type).


### ❌ Mistake 4: Forgetting Initialization


 * **Code:** `int a[5]; printf("%d", a[2]);`
 * **Result:** It might print `32512` or `-8934`.
 * **Why?** C does not clean the memory for you. If you don't give a value, the array contains "garbage values" (leftover data from previous programs).


### ❌ Mistake 5: No Boundary Checking


In many modern languages (like Java or Python), if you try to access `a[100]` in a size-5 array, the program stops and gives an error.


 * **The Danger in C:** C will **not** stop you. It will try to access that memory address anyway. This can cause your program to crash silently or behave unpredictably.


-----


## 9\. Summary


 * **Arrays** solve the problem of handling too many variables.
 * They store data of the **same type** in **continuous memory**.
 * Counting starts at **0**.
 * The array name is actually a **pointer** to the first element.
 * Be very careful with **index limits** (0 to size-1).
