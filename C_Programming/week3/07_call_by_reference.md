


# Chapter: Call by Value and Call by Reference


## 1\. Why Do We Need Something More Than Normal Function Calls?


So far, we have learned two important ideas about functions:


1. Every function has a definition.
2. When a function is called, arguments are passed to parameters, and these parameters become **local variables** inside the function.


This means: **Parameters are local copies of the original variables.**


Most of the time, this is perfectly fine. But sometimes, this behavior causes a program to fail logically, even though the syntax is correct. Let us see such a situation.


### Program 1: Attempt to Swap Two Numbers (Fails)


We want to write a program that swaps two numbers using a function.


```c
#include <stdio.h>


void swap(int x, int y) {
   int temp;
   temp = x;
   x = y;
   y = temp;
}


int main(void) {
   int a = 10;
   int b = 20;


   swap(a, b);


   printf("a = %d, b = %d\n", a, b);
   return 0;
}
```


### 2\. What We Expected vs What We Got


 * **Expected Output:** `a = 20, b = 10`
 * **Actual Output:** `a = 10, b = 20`


The program runs successfully, but the swap does not happen. So the question is: **Why did the logic fail even though the code is correct?**


### 3\. Understanding the Core Problem


Look carefully at this line in `main`:


```c
swap(a, b);
```


Here is what actually happens internally:


1. The value of `a` (10) is copied into `x`.
2. The value of `b` (20) is copied into `y`.
3. Inside `swap`, we only modify `x` and `y`.
4. `a` and `b` remain unchanged.


So: **The function receives copies, not the originals.** This method of passing arguments is called **Call by Value**.


-----


## 4\. Call by Value (Formal Definition)


**Call by Value** means the function receives a **copy** of the value, not the original variable.


**Key consequences:**


 * Parameters are local variables.
 * Any change made inside the function **does not** affect the original variables.
 * This is the default behavior in C.


### 5\. Memory Picture (Conceptual)


When we write `swap(a, b)`, memory looks like this:


```text
main():        swap():
a = 10   -->   x = 10
b = 20   -->   y = 20
```


Changes to `x` and `y` stay inside `swap` and are destroyed when the function ends.


### 6\. Why Call by Value Is Not Enough


Call by value is safe, but sometimes we need to modify the original data.
**Examples:**


 * Swapping two numbers.
 * Updating a variable inside a function.
 * Returning multiple results.


So we need a way to give the function access to the original memory. This is where **pointers** help us.


-----


##  Key Idea Before Moving Forward


Instead of sending the **value**, we will send the **address** of the variable.
That way, the function can reach back and change the value at that address. This leads to **Call by Reference**.


### Program 2: Swapping Using Call by Reference (Works)


```c
#include <stdio.h>


void swap(int *x, int *y) {
   int temp;
   temp = *x;
   *x = *y;
   *y = temp;
}


int main(void) {
   int a = 10;
   int b = 20;


   // Notice we pass addresses using &
   swap(&a, &b);


   printf("a = %d, b = %d\n", a, b);
   return 0;
}
```


### 7\. Step-by-Step Explanation


**Step 1: Function Call**
`swap(&a, &b);`


 * `&a` gives the address of `a`.
 * `&b` gives the address of `b`.
 * These addresses are passed to the function.


**Step 2: Function Parameters**
`void swap(int *x, int *y)`


 * `x` stores the address of `a`.
 * `y` stores the address of `b`.
 * `x` and `y` are pointer variables.


**Step 3: Working with “Value At”**


 * `temp = *x;`
     * `*x` means **value at** address stored in `x`. So this reads the value of `a`.
 * `*x = *y;`
     * Value of `b` is copied into `a`.
 * `*y = temp;`
     * Old value of `a` is copied into `b`.


Now the original variables are modified.


-----


## 8\. Call by Reference (Conceptual Deep Dive)


### 8.1 What Does “Call by Reference” Really Mean?


When we say “Call by Reference”, we do not mean that C magically passes variables differently. What we actually mean is:


> **Instead of passing the value, we pass the address of the variable.**
> Because we pass the address, the function can reach back into the caller’s memory and modify the original content.


That address is what we call the **reference**.


### 8.2 How a Function Must Be Written


If a function is expected to modify original data, then:


1. It must not expect a normal variable.
2. It must expect a **pointer variable**.


That is why the function definition looks like this:


```c
void swap(int *x, int *y)
```


 * `x` $\rightarrow$ address of an integer.
 * `*x` $\rightarrow$ value at that address.


### 8.3 What Must Be Sent During the Function Call?


If the function expects a pointer, then we must send an address.


```c
swap(&a, &b);
```


 * `a` $\rightarrow$ value stored in `a`.
 * `&a` $\rightarrow$ address of `a`.


### 8.4 Why We Call This “Call by Reference”


We say call by reference because:


 * The function is not working on copies.
 * It is working on the original memory location.
 * The address acts as a **reference** to the original variable.


-----


## 9\. Syntax Comparison (Very Important)


| Feature | Call by Value | Call by Reference (Using Pointers) |
| :--- | :--- | :--- |
| **Concept** | Value is copied | Address is passed |
| **Original Data** | Unchanged | Can be modified |
| **Definition Syntax** | `void func(int x)` | `void func(int *x)` |
| **Call Syntax** | `func(a);` | `func(&a);` |


-----


## 10\. Why Arrays Automatically Behave Like Call by Reference


Now comes an extremely important observation. When we pass an array to a function:


```c
func(arr);
```


We do **not** write `func(&arr)`. **Why?**


Because **the array name already behaves like the address of the first element.**


So when we write `func(arr)`, what actually gets passed is `&arr[0]`. That is an address.
This is why **arrays are always passed by reference in C** (effectively).


-----


## 11\. Common Mistakes in Call by Value and Call by Reference


**Instruction to students:** In every program below, the ❌ mark shows the exact wrong line. Read the explanation after the code.


### Mistake 1 – Expecting Call by Reference Without Using Pointers


```c
#include <stdio.h>


void update(int x) {
   x = 100;   // ❌
}


int main(void) {
   int a = 10;
   update(a);
   printf("%d\n", a);
   return 0;
}
```


**Explanation:** In `void update(int x)`, `x` is a parameter (a local variable). `x = 100` changes only the local copy. The original `a` remains 10. This is **Call by Value**.


### Mistake 2 – Forgetting `&` While Calling a Reference Function


```c
#include <stdio.h>


void increment(int *x) {
   *x = *x + 1;
}


int main(void) {
   int a = 5;
   increment(a);   // ❌
   printf("%d\n", a);
   return 0;
}
```


**Explanation:** The function expects a pointer (`int *x`), meaning it needs an address. The call `increment(a)` sends the **value**, not the address. Correct call: `increment(&a);`.


### Mistake 3 – Forgetting `*` in the Function Definition


```c
#include <stdio.h>


void increment(int x) {   // ❌
   *x = *x + 1;
}


int main(void) {
   int a = 5;
   increment(&a);
   return 0;
}
```


**Explanation:** `int x` declares a normal variable, not a pointer. To modify original data using an address, the parameter must be declared as `int *x`.


### Mistake 4 – Using Value Instead of “Value At” Inside Function


```c
#include <stdio.h>


void setZero(int *x) {
   x = 0;   // ❌
}


int main(void) {
   int a = 10;
   setZero(&a);
   printf("%d\n", a);
   return 0;
}
```


**Explanation:** `x` stores an address. Writing `x = 0` changes the pointer itself (making it NULL), not the original value. To change the original variable, we must write `*x = 0;`.


### Mistake 5 – Mixing Call by Value and Call by Reference


```c
#include <stdio.h>


void swap(int *x, int *y) {
   int t = *x;
   *x = *y;
   *y = t;
}


int main(void) {
   int a = 10, b = 20;
   swap(&a, b);   // ❌
   return 0;
}
```


**Explanation:** `swap` expects two addresses. `&a` is an address, but `b` is a value. Both arguments must be addresses: `swap(&a, &b);`.


### Mistake 6 – Expecting Array Changes Without Understanding Reference Behavior


```c
#include <stdio.h>


void change(int arr[]) {
   arr[0] = 99;
}


int main(void) {
   int a[3] = {1, 2, 3};
   change(a);
   printf("%d\n", a[0]);
   return 0;
}
```


**Explanation:** This code actually **works**, but students often mark it wrong or misunderstand it. `arr` receives the address of `a[0]`. Arrays are passed by reference automatically. No `&` is needed.


### Mistake 7 – Trying to Return Multiple Values Without References


```c
#include <stdio.h>


int compute(int a, int b) {
   int sum = a + b;
   int diff = a - b;   // ❌
   return sum;
}


int main(void) {
   int s = compute(10, 5);
   return 0;
}
```


**Explanation:** A function can return only **one** value. `diff` is lost after the function ends. To return multiple results (sum and diff), we must use pointer parameters.


-----


## 12\. Final Conceptual Clarification


Let us clearly state the core rules students must remember.


**1. Parameters**


 * Variables written in the function definition.
 * Always local variables.
 * Created when the function is called, destroyed when it ends.
 * Example: `void func(int x)` (Here, `x` is a parameter).


**2. Arguments**


 * Values or addresses sent during the function call.
 * Example: `func(a)` or `func(&a)`.


**3. Return Value**


 * A function can return only one value.
 * Return type must be declared in function definition.


### Where Program Execution Begins (`main`)


Even if you write many functions, the computer always starts execution from `main()`.


```c
#include <stdio.h>


int main(void) {
   printf("Hello\n");
   return 0;
}
```


 * `#include <stdio.h>`: Gives access to `printf`.
 * `int main(void)`: Entry point. `int` means it returns a status code to the OS. `void` means it takes no arguments.
 * `return 0;`: Tells the OS the program executed successfully.


-----


## 🎓 Final Conclusion


At this point, you should clearly understand:


1. **Call by Value:** Copies data. Safe, but cannot change originals.
2. **Call by Reference:** Passes addresses. Allows changing originals.
3. **Pointers:** The tool that makes call by reference possible in C.


**If you remember just one sentence, remember this:**


> **C passes values by default. To change originals, pass addresses.**


Once this idea is clear, functions, pointers, arrays, and dynamic memory all fit together naturally.

