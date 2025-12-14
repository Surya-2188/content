


# Chapter: Recursion in C Functions


## 1\. Why Do We Need Recursion If We Already Have Loops?


Earlier, we saw:


 * **Functions** help us reuse a set of instructions.
 * **Loops** help us repeat a step many times.


Then we studied two classic mathematical examples: **Factorial** and **Fibonacci**. In both cases, we noticed a common pattern:


> **The current output depends on previous outputs.**


### 1.1 Factorial – Previous Result Multiplied


Mathematically:
$$n! = 1 \times 2 \times 3 \times \cdots \times n$$


We can compute $n!$ step by step:


1. Start with `result = 1`.
2. Multiply by 1 $\rightarrow$ `result = 1`.
3. Multiply by 2 $\rightarrow$ `result = 2`.
4. Multiply by 3 $\rightarrow$ `result = 6`.
   ...
   Each step uses the **previous value** of `result`.


**C Loop Version:**


```c
#include <stdio.h>


int factorial_iter(int n) {
   int i;
   int result = 1;


   for (i = 1; i <= n; i++) {
       result = result * i;   // uses previous result
   }


   return result;
}


int main(void) {
   int n = 5;
   int fact = factorial_iter(n);
   printf("Iterative: %d! = %d\n", n, fact);
   return 0;
}
```


Here:


 * The function is called **once**: `factorial_iter(n)`.
 * The `for` loop repeatedly updates `result` inside a single call.


So loops already solve "use previous result" type problems. So why do we need recursion?


-----


## 2\. Two Stages of a Function: Definition vs. Call


Before defining recursion, recall that every function in C has two separate stages.


### 2.1 Stage 1 – Function Definition (The "Rule")


This tells the compiler **how** the function works. It is the blueprint.


```c
// Definition
void greet(void) {
   printf("Hello\n");
}
```


This is like a mathematical rule: *“Whenever you call greet, print Hello.”*


### 2.2 Stage 2 – Function Call (The "Trigger")


This is where we actually **use** the function.


```c
// Call
int main(void) {
   greet();     // function call
   return 0;
}
```


On this line, we are not defining, we are asking: *“Run whatever rule is written inside greet.”*


### 2.3 Where does recursion happen?


Recursion is a special situation where:
**The function call appears inside the function definition itself.**


 * **Normal:** `main` calls `funcA`. `funcA` does work, returns.
 * **Nested:** `main` calls `funcA`, and inside, `funcA` calls `funcB`.
 * **Recursive:** `main` calls `funcA`, and inside, `funcA` calls `funcA` again (with a smaller input).


This “self-call” is what we call recursion.


-----


## 3\. A New Data Structure: Stack (How Calls Are Stored)


To understand recursion, we must understand how C stores function calls.


### 3.1 What is a stack?


A stack is a way of storing a collection of items with a special access rule:
**LIFO – Last In, First Out**


The last item you put in is the first one you take out.


**Real-life examples:**


1. **Stack of plates/books:** You keep plates one on top of another. You always take the topmost plate. You don’t pull from the middle.
2. **Calling students for viva from the last entered:** Suppose students enter a small room and stand one behind another. If you decide to examine the last student who entered first, then: Last in $\rightarrow$ examined first.


**Important points:**


 * You do not access arbitrary positions.
 * You always add (`push`) and remove (`pop`) at one end, called the **top** of the stack.


-----


## 4\. The Call Stack: How C Manages Function Calls


C uses a stack internally to manage function calls. We call it the **Call Stack**.


We imagine each active function as a **box** on the stack.


1. When `main` starts, its box is pushed on the stack.
2. When `main` calls `func1`, a box for `func1` is pushed on top.
3. When `func1` calls `func2`, a box for `func2` is pushed on top.


**Important rule:**


> **The last function that was called is the first one to finish.**
> This is exactly the LIFO behavior of a stack.


### 4.1 Understanding Figure 1 – How the Call Stack Grows and Shrinks


Let’s look at the diagram above (Figure 1). Each small box is a **Stack Frame** – a record of which function is currently active. The arrow labelled “stack top” always points to the function that is currently executing. We read the figure from left to right like a story.
![alt text](recursion_image.jpg)


**Panel 1 – `main` calls `func1()`**


 * **Stack:** `main` (bottom) $\rightarrow$ `func1()` (top).
 * **What happened?** The program started in `main`. Inside `main`, we called `func1()`. A new frame for `func1` is pushed on top. `main` is paused waiting for it to finish.


**Panel 2 – `func1()` calls `func2()`**


 * **Stack:** `main` $\rightarrow$ `func1` $\rightarrow$ `func2`.
 * Inside the code of `func1` there is a line like `func2();`.
 * The CPU pauses `func1` and pushes a new frame for `func2` on top. Now `func2` is running.


**Panel 3 – `func2()` calls `func3()`**


 * **Stack:** `main` $\rightarrow$ `func1` $\rightarrow$ `func2` $\rightarrow$ `func3`.
 * We push a frame for `func3` on top. `func2`, `func1`, and `main` are all paused, each waiting for the one above it.


**Panels 4, 5 – More nested function calls**


 * `func3` calls `func4`... calls `func5`... calls `func6`.
 * Each new call = one more frame pushed on top.
 * Older functions (`main`, `func1`, …) are still there, but frozen, waiting for control to come back.


**Final Panel – Returning back to `main`**


 * The dashed green arrow shows the return path.
 * When `func6` finishes, its frame is **popped**. Control returns to `func5`.
 * When `func5` finishes, its frame is popped.
 * This continues back to `func1`, and finally to `main`.


**What Figure 1 Teaches:**
Every function call creates a new stack frame on top. When a function finishes, its frame is removed (popped). Later, when we study recursion, the same picture will appear again – only difference is that the stack will have many frames of the **same function name** (e.g., `fact(5)`, `fact(4)`...), but the mechanism is identical.


-----


## 5\. What Is Recursion (Formally)?


Now we can define:
A **recursive function** is a function that calls itself inside its own definition.


Every recursive function has two parts:


1. **Base case:** A simple situation where the function **does not** call itself. It returns a direct answer.
2. **Recursive case:** A situation where the function calls itself with a **smaller/simpler** input. It moves towards the base case.


If the base case is missing, or if the recursive case does not move towards it, the function calls itself forever, and the call stack overflows $\rightarrow$ **Stack Overflow**.


-----


## 6\. Factorial: From Math to C


### 6.1 Mathematical recursive definition


For a non-negative integer $n$:


$$
n! =
\begin{cases}
1 & \text{if } n = 0 \\
n \times (n-1)! & \text{if } n > 0
\end{cases}
$$


This is already recursive: To define $n!$, we use $(n-1)!$. We stop at $0! = 1$ (base case).


### 6.2 Iterative (loop) factorial in C


We already saw `factorial_iter`.


 * `main` calls `factorial_iter(5)`.
 * The loop happens inside one frame.
 * **Stack:** Only one stack frame for `factorial_iter`, no matter how big $n$ is.


### 6.3 Recursive factorial in C


Now we copy the mathematical definition directly into C:


```c
#include <stdio.h>


// Recursive factorial
int fact(int n) {
   if (n == 0) {              // base case
       return 1;
   } else {                   // recursive case
       return n * fact(n - 1);
   }
}


int main(void) {
   int n = 5;
   int result = fact(n);
   printf("Recursive: %d! = %d\n", n, result);
   return 0;
}
```


**Line-by-line for `fact`:**


 * `if (n == 0) { return 1; }`: If $n$ is 0, we do not call `fact` again. We directly return 1.
 * `return n * fact(n - 1);`: If $n > 0$, we compute `fact(n - 1)`, multiply by $n$, and return the product. The function definition contains a call to itself.


-----


## 7\. Call Stack: Iterative vs. Recursive (What Actually Changes)


We now see how the stack behaves differently.


### 7.1 Iterative stack for `factorial_iter(5)`


 * **Call chain:** `main` $\rightarrow$ `factorial_iter(5)`.
 * **Stack:** `main` frame (bottom), `factorial_iter` frame (top).
 * Only **one** `factorial_iter` frame. Variables `i` and `result` are updated many times within that one frame.


### 7.2 Recursive stack for `fact(5)`


 * **Call chain:** `main` $\rightarrow$ `fact(5)` $\rightarrow$ `fact(4)` $\rightarrow$ `fact(3)` $\rightarrow$ `fact(2)` $\rightarrow$ `fact(1)` $\rightarrow$ `fact(0)`.


At the deepest point ($n=0$), the stack looks like this:


**Top of stack**
$\downarrow$
`fact frame (n=0)`
`fact frame (n=1)`
`fact frame (n=2)`
`fact frame (n=3)`
`fact frame (n=4)`
`fact frame (n=5)`
`main frame`


There are **6 frames** of the same function `fact`, each with a different $n$.


**Unwinding:**


1. `fact(0)` returns 1. Frame popped.
2. `fact(1)` computes $1 \times 1 = 1$, returns 1. Frame popped.
3. `fact(2)` computes $2 \times 1 = 2$, returns 2.
4. `fact(3)` computes $3 \times 2 = 6$, returns 6.
5. `fact(4)` computes $4 \times 6 = 24$, returns 24.
6. `fact(5)` computes $5 \times 24 = 120$, returns 120 to `main`.


This logic ensures that `fact(5)` has access to `fact(4)` before it completes its execution. This is why activities that need previous outputs fit so well into recursion.


-----


## 8\. Fibonacci: Similar Idea, More Branches


**Mathematical definition:**
$$F_0 = 0, \quad F_1 = 1, \quad F_n = F_{n-1} + F_{n-2}$$


**Recursive C version:**


```c
int fib(int n) {
   if (n == 0) return 0;        // base case 1
   if (n == 1) return 1;        // base case 2
   return fib(n - 1) + fib(n - 2);  // recursive case
}
```


Here, the call structure for `fib(5)` looks like a **tree**:


 * `fib(5)` calls `fib(4)` and `fib(3)`
 * `fib(4)` calls `fib(3)` and `fib(2)`
 * ...


The stack still behaves in the same LIFO way, but now many overlapping subproblems appear.


-----


## 9\. Base Case and Stack Overflow


While writing a recursive function, the base case is the most important concept.


**Why base case?**
If a recursive function never reaches a base case, it keeps calling itself forever. Each call uses some stack memory. Since stack memory is limited, the stack fills up, and the program crashes with a **Stack Overflow**.


**Example with no base case:**


```c
int bad_fact(int n) {
   return n * bad_fact(n - 1);   // ❌ no base case
}
```


**Example with wrong direction:**


```c
int bad_fact2(int n) {
   if (n == 0) return 1;
   return n * bad_fact2(n + 1);  // ❌ going away from 0
}
```


**Rule for students:**
Every recursive function must:


1. Have a correct base case.
2. Make recursive calls that move **towards** that base case.


-----


## 10\. Template for Designing Recursive Functions


When you write a new recursive function, follow this 4-step template:


1. **Identify the base case(s):** Example: $n == 0$ or $(n == 0 || n == 1)$.
2. **Write the base case in C:**
   ```c
   if (n == 0) return 1;
   ```
3. **Write the recursive case using smaller input:**
   Factorial: `return n * fact(n - 1);`
4. **Check the direction:**
   Starting from $n$, will repeated calls eventually hit the base case?
     * $n \rightarrow n-1 \rightarrow n-2 \rightarrow \dots \rightarrow 0$ (Correct ✔)
     * $n \rightarrow n+1 \rightarrow n+2 \dots$ (Wrong ✘)


-----


## 11\. Common Mistakes in Recursion (Extended List)


Here are 25 common mistakes students make with recursion.


**Mistake 1 – No base case**


```c
int fact(int n) {
   return n * fact(n - 1);   // ❌ infinite recursion
}
```


*Issue:* No condition to stop. Stack grows until overflow.


**Mistake 2 – Wrong base case value**


```c
int fact(int n) {
   if (n == 0) return 0;     // ❌ should return 1
   return n * fact(n - 1);
}
```


*Issue:* `fact(0)` becomes 0. All higher factorials become 0 (multiplied by 0).


**Mistake 3 – Base case never reached (wrong direction)**


```c
int fact(int n) {
   if (n == 0) return 1;
   return n * fact(n + 1);   // ❌ 5 -> 6 -> 7 ... (never 0)
}
```


**Mistake 4 – Negative inputs not handled**


```c
int fact(int n) {
   if (n == 0) return 1;
   return n * fact(n - 1);   // ❌ fact(-3) -> fact(-4) ...
}
```


*Fix:* Add a guard check `if (n < 0)`.


**Mistake 5 – Missing return for recursive step**


```c
int fact(int n) {
   if (n == 0) return 1;
   fact(n - 1);              // ❌ value ignored
}
```


*Issue:* Function does not return the computed value. May return garbage.


**Mistake 6 – Using `n--` in the recursive call**


```c
int fact(int n) {
   if (n == 0) return 1;
   return n * fact(n--);     // ❌ passes original n, decrements later
}
```


*Issue:* `fact(5)` calls `fact(5)` again. Infinite recursion. Use `n - 1`.


**Mistake 7 – Forgetting to multiply in factorial**


```c
int fact(int n) {
   if (n == 0) return 1;
   return fact(n - 1);       // ❌ missing "* n"
}
```


*Issue:* Every factorial becomes 1.


**Mistake 8 – Mixing printing with returning**


```c
void fact_print(int n) {
   if (n == 0) {
       printf("1\n");
   } else {
       // ❌ uses void function in expression
       printf("%d\n", n * fact_print(n - 1));
   }
}
```


*Issue:* `fact_print` is `void`, it cannot be used as a value.


**Mistake 9 – Using global accumulator incorrectly**


```c
int acc = 1; // Global


int fact(int n) {
   if (n == 0) {
       return acc;
   }
   acc = acc * n;
   return fact(n - 1);
}
```


*Issue:* Works once, but fails if called a second time because `acc` is already modified.


**Mistake 10 – Wrong base cases in Fibonacci**


```c
int fib(int n) {
   if (n == 0) return 1;     // ❌ should be 0
   if (n == 1) return 1;
   return fib(n - 1) + fib(n - 2);
}
```


**Mistake 11 – Missing one base case in Fibonacci**


```c
int fib(int n) {
   if (n == 0) return 0;
   // ❌ missing n == 1
   return fib(n - 1) + fib(n - 2);
}
```


*Issue:* `fib(1)` calls `fib(-1)`, leading to infinite recursion.


**Mistake 12 – Same argument twice in Fibonacci**


```c
int fib(int n) {
   if (n == 0) return 0;
   if (n == 1) return 1;
   return fib(n - 1) + fib(n - 1);  // ❌ second should be fib(n-2)
}
```


**Mistake 13 – Off-by-one when printing Fibonacci**


```c
void print_fib(int n) {
   int i;
   for (i = 0; i < n; i++) {
       printf("%d ", fib(i + 1));   // ❌ skips F0
   }
}
```


**Mistake 14 – Unnecessary loop inside recursive function**


```c
int fib(int n) {
   if (n == 0) return 0;
   if (n == 1) return 1;


   int i, sum = 0;
   for (i = 0; i < n; i++) {       // ❌ extra loop
       sum = fib(n - 1) + fib(n - 2);
   }
   return sum;
}
```


*Issue:* Recursion already repeats work. The loop multiplies that work unnecessarily.


**Mistake 15 – No guard for negative n in Fibonacci**


```c
int fib(int n) {
   if (n == 0) return 0;
   if (n == 1) return 1;
   return fib(n - 1) + fib(n - 2);  // ❌ undefined for n < 0
}
```


**Mistake 16 – Mixing iteration and recursion wrongly**


```c
int fact(int n) {
   int i, result = 1;
   for (i = 1; i <= n; i++) {
       result *= i;
       return fact(n - 1);   // ❌ leaves the loop in first iteration
   }
   return result;
}
```


*Fix:* Use pure loop OR pure recursion.


**Mistake 17 – Unreachable code after return**


```c
int fib(int n) {
   if (n == 0) return 0;
   return fib(n - 1);
   return fib(n - 2);        // ❌ never executed
}
```


**Mistake 18 – Assuming recursion “remembers” values**


```c
int fib(int n) {
   if (n == 0) return 0;
   if (n == 1) return 1;
   int a = fib(n - 1);
   int b = fib(n - 2);
   int c = fib(n - 1);       // ❌ recomputes fib(n-1) again
   return a + b;
}
```


*Issue:* Each call is independent. Results are recomputed unless you store them explicitly.


**Mistake 19 – Ignoring return value**


```c
int fact(int n) {
   if (n == 0) return 1;
   fact(n - 1);              // ❌ result ignored
   return n;                 // only returns n
}
```


**Mistake 20 – Very deep recursion (Stack Overflow)**


```c
int fact(int n) {
   if (n == 0) return 1;
   return n * fact(n - 1);   // correct logic
}
```


*Issue:* If we call `fact(50000)`, the recursion depth is too large for the memory. Stack is finite.


**Mistake 21 – Returning before doing required work**


```c
int fact(int n) {
   if (n == 0) return 1;
   return n * fact(n - 1);  
   printf("Done\n");         // ❌ never executed
}
```


*Issue:* Once `return` executes, the function ends immediately.


**Mistake 22 – Using recursion where stopping condition is not obvious**


```c
int weird(int n) {
   if (n % 2 == 0) return n;
   return weird(n * 3 + 1);   // ❌ may or may not terminate
}
```


*Advice:* Beginners should start with clear base-case problems like factorial.


**Mistake 23 – Returning wrong type conceptually**


```c
char grade(int n) {
   if (n == 0) return fact(0);  // ❌ returns int as char
   return 'A';
}
```


*Issue:* Common when mixing functions; results in loss of data or weird characters.


**Mistake 24 – Recursion inside `main` without clear design**


```c
int main(void) {
   int x;
   scanf("%d", &x);
   if (x > 0) {
       main();   // ❌ main calling main recursively
   }
   return 0;
}
```


*Issue:* `main` is not typically used for recursion. Very confusing flow.


**Mistake 25 – Blindly translating a loop to recursion**
*Bad Translation:*


```c
void print_down_rec(int n) {
   while (n > 0) {
       printf("%d ", n);
       print_down_rec(n - 1);    // ❌ recursion + loop explosion
       n--;
   }
}
```


*Issue:* Recursion must **replace** the loop, not live inside it.


-----


## 12\. Final Conclusion


Many activities in mathematics and programming have this pattern:


> **Current value = some function of previous value(s)**


Recursion is a natural way to express such problems because:


1. Each recursive call represents a smaller subproblem.
2. The call stack automatically remembers who is waiting for whose result.
3. Results come back in **Last In, First Out** order: the smallest subproblem (base case) finishes first, then each waiting call above it finishes using the previous output.


In C, recursion works smoothly because function calls are stored on a stack. This stack’s LIFO behaviour matches exactly what recursive definitions need.


**Recursion is not magic.** It is just functions calling themselves, and the call stack (LIFO) doing the book-keeping for us.

