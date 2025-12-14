


# Chapter: Pointers, Arrays, Strings, NULL, sizeof, and malloc in C


## 1\. What We Will Cover in This Lecture


In the recursion chapter, we learned that a program’s execution is controlled by function calls and the call stack. Now we move to another fundamental idea of C: **Memory and addresses.**


This lecture answers one big question:


> **“How does C allow us to directly work with memory?”**


In this lecture we will cover, in one continuous flow:


 * Why we need pointers even though we already have variables.
 * What a pointer stores (address) and how we get:
     * address of (using `&`)
     * value at (using `*`)
 * The two meanings of `*`:
     * in definition (pointer indicator)
     * in usage (value at)
 * Why pointers need a data type (`int*`, `char*`, etc.).
 * Pointer arithmetic (`p + 1`) and why it depends on type.
 * Pointer vs Array: why array name behaves like a pointer but is not a pointer variable.
 * Strings: why strings are char arrays ending with `\0`.
 * NULL pointer: “points to nothing” and how it avoids crashes.
 * `sizeof` operator: array size vs pointer size.
 * Introducing `malloc` to create arrays at runtime.
 * A complete final demo program + line-by-line explanation.


We will keep the language beginner-friendly. We avoid heavy jargon: instead of saying “dereferencing,” we will consistently say: **“value at”**.


-----


## 2\. Why Do We Need Pointers If We Already Have Variables?


We already know:


```c
int x = 10;
```


We feel like **“x contains 10.”**


But inside the computer, the truth is:


1. The value is stored in **memory**.
2. That location has an **address**.
3. The variable name (`x`) is just a label for that address.


So every variable has:


 * **Value**
 * **Address**


Sometimes, the value is not enough — we need the address to manipulate memory directly.


-----


## 3\. Variables Have Two Parts: Value and Location


Think like this:


```text
+-----------+
|    10     |   ← value
+-----------+
  2000         ← address
```


So:


 * `x` gives us the **value**.
 * `&x` gives us the **address**.


-----


## 4\. What Is a Pointer?


A pointer is a variable that stores the **address** of another variable.


 * **Normal variable** $\rightarrow$ stores value.
 * **Pointer variable** $\rightarrow$ stores address.


-----


## 5\. The `&` Operator Means “Address Of”


The symbol `&` is called ampersand. It has one meaning: **address of**.


**Example:**


```c
int x = 10;
printf("%p\n", (void*)&x);
```


This prints the memory address where `x` is stored.


-----


## 6\. Declaring a Pointer (First Meaning of `*`)


Pointer declaration syntax:
`data_type *pointer_name;`


**Examples:**


```c
int *p;
char *c;
float *f;
```


Here, `*` means: **“This variable is a pointer.”**
This is the **first meaning** of `*`.


-----


## 7\. Using a Pointer to Get “Value At” (Second Meaning of `*`)


Once a pointer stores an address, we can get the value stored there using `*`.


**Example:**


```c
int x = 10;
int *p = &x;


printf("%d\n", *p);
```


Here:


 * `p` stores address of `x`.
 * `*p` means **value at** that address.


So in expressions, `*p` = **value at**.
This is the **second meaning** of `*`.


-----


## 8\. The Two Meanings of `*` (Must-Remember)


1. **In declaration:**
   `int *p;`
   `*` means: **pointer indicator**.


2. **In usage:**
   `*p`
   `*` means: **value at** the address stored in `p`.


-----


## 9\. Important Combinations of `&` and `*`


Assume:


```c
int x = 10;
int *p = &x;
```


Now:


| Expression | Meaning |
| :--- | :--- |
| `x` | value of x |
| `&x` | address of x |
| `p` | address stored in p |
| `*p` | value at address stored in p |
| `&p` | address of pointer p |
| `*&x` | value at address of x $\rightarrow$ **x** |
| `&*p` | address of value at p $\rightarrow$ **address of x** |


**Important cancellation idea:** `*&something` becomes `something` again.


-----


## 10\. Why Pointers Need a Data Type


A common question: **“Addresses are just numbers. Why `int*`, `char*`?”**


Because the type is **not** for the address. The type tells the compiler:


1. How many bytes to read.
2. How to interpret those bytes.


When you write `*p` (value at), the compiler must know the type to read the correct number of bytes.


-----


## 11\. Pointer Arithmetic (What Does `p + 1` Mean?)


Pointer arithmetic is **not** “add 1 to address”.
It means: **Move to the next location of the pointed type.**


 * `char*` $\rightarrow$ moves **1 byte**.
 * `int*` $\rightarrow$ moves **sizeof(int)** bytes (usually 4).


So pointer arithmetic depends on type.


-----


## 12\. Pointer vs Array (Most Important Bridge)


Students hear: *“Array name is a pointer.”*
**Better statement:**


> **In most expressions, array name behaves like the address of its first element.**


**Example:**


```c
int arr[4] = {10, 20, 30, 40};
```


Then: `arr == &arr[0]`


-----


## 13\. Indexing Is Pointer Arithmetic


A core identity:
$$arr[i] \equiv *(arr + i)$$


So indexing means:


1. Move `i` steps.
2. Take **value at** that location.


-----


## 14\. Why Array Name Is Not a Pointer Variable


 * **Pointer variable can be reassigned:**
   `p = &x;`
   `p = &y;`
 * **Array name cannot:**
   `arr = &x;`   // **illegal**


So:


 * Array name behaves like a pointer in expressions.
 * But it is **not** a pointer variable (it is a constant address).


-----


## 15\. Strings: Best Place to See Pointer Concepts Clearly


A string in C is: **A char array ending with `\0`.**


**Example:**


```c
char s[] = "HELLO";
```


Actually stored as:
`'H' 'E' 'L' 'L' 'O' '\0'`


The `\0` (Null Character) tells where the string ends.


-----


## 16\. `char[]` vs `char*` (Important Difference)


 * **Modifiable string:**
   ```c
   char s[] = "HELLO";
   s[0] = 'Y';   // allowed
   ```
 * **String literal pointer (read-only in many systems):**
   ```c
   char *p = "HELLO";
   p[0] = 'Y';   // undefined / crash
   ```


-----


## 17\. NULL Pointer


A pointer can safely point to nothing using `NULL`.


```c
char *p = NULL;
```


Always check:


```c
if (p != NULL) {
   printf("%s\n", p);
}
```


-----


## 18\. `sizeof` Operator (Array vs Pointer)


If:


```c
int arr[5];
int *p = arr;
```


Then:


 * `sizeof(arr)` $\rightarrow$ total bytes of the full array $\rightarrow$ $5 \times \text{sizeof(int)}$
 * `sizeof(p)` $\rightarrow$ size of pointer (address storage) $\rightarrow$ typically 8 bytes on 64-bit systems


This proves again: **array is not pointer variable.**


-----


## 19\. Why We Need `malloc` for Arrays


 * **Static array:** `int arr[5];` (Size fixed at compile time).
 * But if the user gives the size, we cannot write `int arr[n];` safely for all compilers and all cases.
 * We want **runtime memory allocation**.


So we use `malloc`: **memory allocation at runtime.**


-----


## 20\. Final Program: Dynamic Array Using `malloc` (Complete Code)


```c
#include <stdio.h>
#include <stdlib.h>


int main(void) {
   int n, i;
   int *arr;


   printf("Enter number of elements: ");
   scanf("%d", &n);


   // Request memory from Heap
   arr = (int *)malloc(n * sizeof(int));


   // Always check if memory allocation succeeded
   if (arr == NULL) {
       printf("Memory allocation failed\n");
       return 1;
   }


   // Fill array using standard indexing
   for (i = 0; i < n; i++) {
       arr[i] = (i + 1) * 10;
   }


   printf("Array elements using pointer arithmetic:\n");


   // Access array using pointer arithmetic
   for (i = 0; i < n; i++) {
       printf("%d ", *(arr + i));
   }


   printf("\n");


   // Free the memory
   free(arr);
   return 0;
}
```


-----


## 21\. Line-by-Line Explanation of the `malloc` Program


We will explain every line with meaning and purpose.


**21.1 Header Files**


 * `#include <stdio.h>`: Needed for `printf` and `scanf`.
 * `#include <stdlib.h>`: Needed for `malloc` and `free`. Without `<stdlib.h>`, `malloc` may not be properly declared.


**21.2 Start of Program**


 * `int main(void) {`: Execution begins here.


**21.3 Variable Declarations**


 * `int n, i;`: `n` will store the count, `i` is for loops.
 * `int *arr;`: `arr` is a pointer to `int`.
     * **Important:** `arr` will later store the **starting address** of the dynamic array. So `arr` will behave like an array name after allocation.


**21.4 Taking Input**


 * `scanf("%d", &n);`: Ask user for number of elements. `&n` is used because `scanf` needs the address of `n` to store the input.


**21.5 Allocating Memory for n Integers**


 * `arr = (int *)malloc(n * sizeof(int));`
     * `sizeof(int)` $\rightarrow$ number of bytes needed for one integer.
     * `n * sizeof(int)` $\rightarrow$ total bytes needed for `n` integers.
     * `malloc(...)` $\rightarrow$ requests that many bytes from **heap memory**.
     * `malloc` returns the **starting address** of the allocated block.
     * That address is stored in `arr`.


**21.6 Why We Must Check NULL**


 * `if (arr == NULL)`: If memory is not available, `malloc` returns `NULL`. We must not use `arr[i]` in that case. The program exits safely.


**21.7 Filling the Array (Using Indexing)**


 * `arr[i] = (i + 1) * 10;`
     * This works because `arr` is a pointer to the first element, and in C, indexing is allowed on pointers.
     * Internally: `arr[i] ≡ *(arr + i)`.


**21.8 Printing the Array Using Pointer Arithmetic**


 * `printf("%d ", *(arr + i));`
     * `*(arr + i)` means: Start at address in `arr`, Move `i` steps forward, Take **value at** that location.
     * This proves: Every array element can be accessed using pointers because memory is sequential.


**21.11 Freeing Memory**


 * `free(arr);`: This returns the allocated memory back to the system.
     * **Important rule:** Every `malloc` must eventually have a `free`. Otherwise, memory remains allocated (**memory leak**).


-----


## 22\. Final Conclusion (Whole Chapter Unified)


In this lecture we learned:


1. Variables store values in memory locations.
2. `&` gives **address of**.
3. `*` has two meanings:
     * in definition $\rightarrow$ **pointer indicator**.
     * in use $\rightarrow$ **value at**.
4. Pointer data type tells how many bytes to read.
5. Pointer arithmetic moves step-by-step through sequential memory.
6. Array name behaves like the address of the first element.
7. Indexing is pointer arithmetic: `arr[i] ≡ *(arr + i)`.
8. Strings are char arrays ending with `\0`.
9. `NULL` means pointer points to nothing.
10. `sizeof` helps compute memory sizes correctly.
11. `malloc` allows creating arrays at runtime.


Just like recursion becomes clear when we understand the call stack, pointers become clear when we understand memory, addresses, and sequential storage. **Pointers are not magic. They simply make memory visible.**


-----


##  Common Mistakes in Pointers, Arrays, Strings, sizeof, and malloc


**Instruction to students:**
In every program below, the ❌ mark shows the exact wrong line. Read the explanation after the code, not inside it.


### Mistake 1 – Using a Pointer Without Giving It an Address


```c
#include <stdio.h>


int main(void) {
   int *p;
   *p = 10;   // ❌
   return 0;
}
```


**Explanation:** The pointer `p` is declared but never assigned an address (it contains garbage). The marked line tries to store a value at an unknown address. This causes undefined behaviour or a crash.


### Mistake 2 – Forgetting `&` While Assigning to Pointer


```c
#include <stdio.h>


int main(void) {
   int x = 10;
   int *p = x;   // ❌
   return 0;
}
```


**Explanation:** The marked line assigns the **value** of `x` (10), not its **address**. A pointer must store an address, obtained using `&x`.


### Mistake 3 – Confusing Pointer with Value at Pointer


```c
#include <stdio.h>


int main(void) {
   int x = 20;
   int *p = &x;
   printf("%d\n", p);   // ❌
   return 0;
}
```


**Explanation:** The marked line prints the address stored in `p`, not the value of `x`. To print the value, `*p` must be used. Also, pointers should be printed with `%p`.


### Mistake 4 – Using `*` on a Non-Pointer Variable


```c
#include <stdio.h>


int main(void) {
   int x = 10;
   printf("%d\n", *x);  // ❌
   return 0;
}
```


**Explanation:** `x` is an integer, not a pointer. The `*` operator (value at) can only be applied to pointer variables.


### Mistake 5 – Mixing the Two Meanings of `*`


```c
#include <stdio.h>


int main(void) {
   int x = 10;
   int *p;
   *p = &x;   // ❌
   return 0;
}
```


**Explanation:** `*p` means "value at location p". `&x` is an address. The marked line tries to store an address inside a value location. Correct usage is `p = &x;`.


### Mistake 6 – Using Pointer of Wrong Type


```c
#include <stdio.h>


int main(void) {
   int x = 10;
   float *p = &x;   // ❌
   return 0;
}
```


**Explanation:** The pointer type determines how memory is read. Reading an `int` as a `float` leads to incorrect interpretation of the bits.


### Mistake 7 – Assuming Array Name Can Be Reassigned


```c
#include <stdio.h>


int main(void) {
   int arr[3] = {1, 2, 3};
   int x = 10;
   arr = &x;   // ❌
   return 0;
}
```


**Explanation:** `arr` is not a pointer variable; it represents a fixed block of memory. You cannot make `arr` point somewhere else.


### Mistake 8 – Out-of-Bounds Access Using Pointer Arithmetic


```c
#include <stdio.h>


int main(void) {
   int arr[3] = {10, 20, 30};
   printf("%d\n", *(arr + 3));   // ❌
   return 0;
}
```


**Explanation:** Valid indices are 0, 1, 2. `arr + 3` moves to index 3, which is outside the array.


### Mistake 9 – Pointer Arithmetic on Non-Array Variable


```c
#include <stdio.h>


int main(void) {
   int x = 5;
   int *p = &x;
   printf("%d\n", *(p + 1));   // ❌
   return 0;
}
```


**Explanation:** Pointer arithmetic should only be used within valid contiguous memory (like arrays). `x` is a single variable; moving `p + 1` accesses unknown memory.


### Mistake 10 – Missing `\0` in Character Array


```c
#include <stdio.h>


int main(void) {
   char s[3] = {'A', 'B', 'C'};
   printf("%s\n", s);   // ❌
   return 0;
}
```


**Explanation:** Strings in C must end with `\0`. Without it, `printf` continues printing into random memory until it hits a zero byte.


### Mistake 11 – Writing into String Literal


```c
#include <stdio.h>


int main(void) {
   char *p = "HELLO";
   p[0] = 'Y';   // ❌
   return 0;
}
```


**Explanation:** String literals like `"HELLO"` are often stored in read-only memory. Modifying them causes undefined behaviour or a crash.


### Mistake 12 – Assuming `sizeof(pointer)` Gives Array Size


```c
#include <stdio.h>


int main(void) {
   int arr[5];
   int *p = arr;
   printf("%zu\n", sizeof(p));   // ❌
   return 0;
}
```


**Explanation:** `sizeof(p)` gives the size of the pointer itself (usually 8 bytes), not the size of the array it points to.


### Mistake 13 – Using `sizeof` Inside Function on Array Parameter


```c
#include <stdio.h>


void f(int arr[]) {
   printf("%zu\n", sizeof(arr));   // ❌
}


int main(void) {
   int a[5];
   f(a);
   return 0;
}
```


**Explanation:** When passed to a function, an array decays into a pointer. Inside `f`, `arr` is just a pointer, so `sizeof(arr)` returns pointer size, not array size.


### Mistake 14 – Using Pointer After `free`


```c
#include <stdio.h>
#include <stdlib.h>


int main(void) {
   int *p = malloc(sizeof(int));
   free(p);
   *p = 10;   // ❌
   return 0;
}
```


**Explanation:** After `free(p)`, the memory is invalid. Using `*p` after that is unsafe (Use-After-Free error).


### Mistake 15 – Forgetting to `free` Allocated Memory


```c
#include <stdlib.h>


int main(void) {
   int *p = malloc(100 * sizeof(int));
   return 0;   // ❌
}
```


**Explanation:** The memory was allocated but never released. This causes a **memory leak**.


### Mistake 16 – Not Checking `malloc` Return Value


```c
#include <stdlib.h>


int main(void) {
   int *p = malloc(1000000000 * sizeof(int));
   p[0] = 5;   // ❌
   return 0;
}
```


**Explanation:** `malloc` returns `NULL` if memory is full. Using `p` without checking `if (p == NULL)` can crash the program.


### Mistake 17 – Using `%d` to Print a Pointer


```c
#include <stdio.h>


int main(void) {
   int x = 10;
   int *p = &x;
   printf("%d\n", p);   // ❌
   return 0;
}
```


**Explanation:** Pointers are addresses, not simple integers. They must be printed using `%p`.


### Mistake 18 – Returning Address of Local Variable


```c
#include <stdio.h>


int* f(void) {
   int x = 10;
   return &x;   // ❌
}


int main(void) {
   int *p = f();
   return 0;
}
```


**Explanation:** `x` is a local variable; it is destroyed when the function `f` ends. Returning its address is dangerous because that memory will be reused.


### Mistake 19 – Incorrect Double Pointer Initialization


```c
#include <stdio.h>


int main(void) {
   int x = 10;
   int **p = &x;   // ❌
   return 0;
}
```


**Explanation:** `&x` is of type `int*`. `p` is of type `int**`. You cannot store `int*` in `int**`. It should be `int *ptr = &x; int **p = &ptr;`.


### Mistake 20 – Using `**` When Only `*` Exists


```c
#include <stdio.h>


int main(void) {
   int x = 10;
   int *p = &x;
   printf("%d\n", **p);   // ❌
   return 0;
}
```


**Explanation:** `p` is a pointer to int, not a pointer to a pointer. Applying `*` twice accesses invalid memory.


### Mistake 21 – Forgetting to Initialize Pointer to `NULL`


```c
#include <stdio.h>


int main(void) {
   int *p;
   if (p != NULL) {   // ❌
       printf("Valid\n");
   }
   return 0;
}
```


**Explanation:** `p` is uninitialized and contains a random garbage value. Comparing it with `NULL` is meaningless unless you explicitly initialized it (`int *p = NULL;`).


### Mistake 22 – Using `NULL` Pointer Without Check


```c
#include <stdio.h>


int main(void) {
   char *p = NULL;
   printf("%s\n", p);   // ❌
   return 0;
}
```


**Explanation:** `p` points to nothing. Trying to print or access it causes a crash (Segmentation Fault).


### Mistake 23 – Incorrect `malloc` Size Calculation


```c
#include <stdlib.h>


int main(void) {
   int *p = malloc(10);   // ❌
   return 0;
}
```


**Explanation:** Memory size should be `10 * sizeof(int)`. Hardcoding "10" allocates 10 bytes, which is likely not enough for 10 integers (which need 40 bytes).


### Mistake 24 – Off-By-One Error in Loop


```c
#include <stdio.h>


int main(void) {
   int arr[3] = {1,2,3};
   for (int i = 0; i <= 3; i++) {   // ❌
       printf("%d ", arr[i]);
   }
   return 0;
}
```


**Explanation:** Valid indices are 0, 1, 2. The loop condition `i <= 3` tries to access index 3, which is out of bounds.


### Mistake 25 – Assuming Pointer Movement Changes Array


```c
#include <stdio.h>


int main(void) {
   int arr[3] = {10,20,30};
   int *p = arr;
   p++;   // ❌
   printf("%d\n", arr[0]);
   return 0;
}
```


**Explanation:** Moving the pointer `p` changes where `p` points. It does **not** shift the actual array elements. `arr[0]` remains 10.


-----


### Final Note (Very Important)


Just like recursion breaks when the base case is wrong, pointer programs break when address logic is wrong.


If you always ask:


1. **What stores the value?**
2. **What stores the address?**
3. **Is this memory valid right now?**


...then pointers become simple, predictable, and powerful.

