

# Chapter: Introduction to Functions in C


## 1\. Why Do We Need Functions in C?


We start with a simple but realistic problem from the programming lab.


### 1.1 Repeating logic in `main`


Imagine we want to:


1. Read marks of 3 students.
2. Each has marks in 3 subjects.
3. For each student, compute total, average, and decide PASS/FAIL.


A straightforward C program (without user-defined functions) might look like this:


```c
#include <stdio.h>


int main(void) {
   int m1, m2, m3;   // marks in 3 subjects
   int total;        // total marks
   float avg;        // average marks


   // ----- Student 1 -----
   printf("Enter marks of student 1 (3 subjects): ");
   scanf("%d%d%d", &m1, &m2, &m3);
   total = m1 + m2 + m3;
   avg = total / 3.0f;
   printf("Student 1: Average = %.2f -> %s\n",
          avg,
          (avg >= 40.0f) ? "PASS" : "FAIL");


   // ----- Student 2 -----
   printf("Enter marks of student 2 (3 subjects): ");
   scanf("%d%d%d", &m1, &m2, &m3);
   total = m1 + m2 + m3;
   avg = total / 3.0f;
   printf("Student 2: Average = %.2f -> %s\n",
          avg,
          (avg >= 40.0f) ? "PASS" : "FAIL");


   // ----- Student 3 -----
   printf("Enter marks of student 3 (3 subjects): ");
   scanf("%d%d%d", &m1, &m2, &m3);
   total = m1 + m2 + m3;
   avg = total / 3.0f;
   printf("Student 3: Average = %.2f -> %s\n",
          avg,
          (avg >= 40.0f) ? "PASS" : "FAIL");


   return 0;
}
```


Inside `main`, for each student:


1. We read 3 marks (`scanf`).
2. Compute `total = m1 + m2 + m3;`.
3. Compute `avg = total / 3.0f;`.
4. Print PASS/FAIL based on `avg`.


The logic is exactly the same for student 1, 2, and 3. Only the input values (marks) and the student label (1, 2, 3) change.


**The Problem:**
If we later want this for **50 students**, or want to reuse this logic in another part of the program, this repetition becomes painful:


 * The code becomes long and messy.
 * If we change the rule (say PASS is now 35), we must fix it in many places.
 * Debugging and reading the program becomes harder.


So we have a real problem: **One fixed rule (same set of instructions) needs to be applied to many different inputs.** This is exactly where we need functions.


-----


## 2\. Mathematical Analogy: Functions as Rules


Before talking about functions in C, recall what we do in mathematics.


### 2.1 Functions in Maths


In mathematics, when we see the same type of relation between a variable $x$ and a value $y$, we define a function.


**Example:**
$$y = 2x + 3$$


We don’t keep writing "$(2x + 3)$" everywhere. Instead we define:
$$f(x) = 2x + 3$$


Now:


 * $f(3)$ means: apply the same rule at input $x = 3$.
 * $f(10)$ means: apply the same rule at input $x = 10$.


**One rule, many inputs $\rightarrow$ many outputs.**
Programming functions are built on this exact idea.


### 2.2 Example 1: Verifying $\sin^2 x + \cos^2 x = 1$


In mathematics, we know the trigonometric identity:
$$\sin^2 x + \cos^2 x = 1$$


Define:
$$f(x) = \sin^2 x + \cos^2 x$$


We can “check” this identity numerically for various values of $x$.


#### 2.2.1 C function for $f(x) = \sin^2 x + \cos^2 x$


```c
#include <stdio.h>
#include <math.h>


// Function definition: f(x) = sin^2(x) + cos^2(x)
double trig_identity(double x) {
   double s = sin(x);           // local variable: sine of x
   double c = cos(x);           // local variable: cosine of x
   double value = s * s + c * c;
   return value;                // return the computed value
}


int main(void) {
   double x;


   x = 0.0;
   printf("x = 0.0,   f(x) = %.6f\n", trig_identity(x));


   x = M_PI / 6; // π/6
   printf("x = pi/6,  f(x) = %.6f\n", trig_identity(x));


   x = M_PI / 4; // π/4
   printf("x = pi/4,  f(x) = %.6f\n", trig_identity(x));


   x = M_PI / 3; // π/3
   printf("x = pi/3,  f(x) = %.6f\n", trig_identity(x));


   return 0;
}
```


#### 2.2.2 Line-by-line: function part


 * `double trig_identity(double x)`
     * `double` $\rightarrow$ **Return Type**: This function will return a double.
     * `trig_identity` $\rightarrow$ **Function Name** (like the name $f$ in maths).
     * `(double x)` $\rightarrow$ **Parameter List**:
         * `x` is a parameter, representing the input angle.
         * Its datatype is `double`.
 * `double s = sin(x);`
     * `s` and `c` are **local variables** inside the function.
     * We call library functions `sin` and `cos` (from `<math.h>`).
 * `double value = s * s + c * c;`
     * We implement $\sin^2 x + \cos^2 x$ as `s * s + c * c`.
 * `return value;`
     * We return the value to the caller (`main`).


In `main`, each time we write `trig_identity(x)`, it is exactly like computing $f(x)$ in mathematics.


### 2.3 Example 2: Mean of a Discrete Probability Distribution


In probability, the mean (expected value) of a discrete distribution is:
$$\mu = \sum_i x_i p_i$$


For a simple case with 3 values, where $X$ takes $x_1, x_2, x_3$ with probabilities $p_1, p_2, p_3$:
$$\mu = x_1 p_1 + x_2 p_2 + x_3 p_3$$


Define a mathematical function:
$$E(x_1, p_1, x_2, p_2, x_3, p_3) = x_1 p_1 + x_2 p_2 + x_3 p_3$$


#### 2.3.1 C function for mean of 3-point distribution


```c
#include <stdio.h>


// Function: mean of a discrete distribution with 3 points
double mean3(double x1, double p1,
            double x2, double p2,
            double x3, double p3) {
   double mu = x1 * p1 + x2 * p2 + x3 * p3;
   return mu;
}


int main(void) {
   double x1 = 1.0,  p1 = 0.2;
   double x2 = 2.0,  p2 = 0.5;
   double x3 = 4.0,  p3 = 0.3;


   double mu = mean3(x1, p1, x2, p2, x3, p3);


   printf("Mean of X = %.4f\n", mu);


   return 0;
}
```


In `main`, `mu = mean3(x1, p1, x2, p2, x3, p3);` directly corresponds to $\mu = E(x_1, p_1, x_2, p_2, x_3, p_3)$.


-----


## 3\. Function Definition and Function Call


Now we formalize the C terminology.


### 3.1 Function Definition


The general pattern is:


```c
return_type function_name(parameter_type1 param1,
                         parameter_type2 param2, ...) {
   // local variables
   // statements (body)
   return expression;      // if return_type is not void
}
```


The C compiler waits for and checks:


1. **Return type (Output Datatype):** e.g., `int`, `double`, `char`, or `void`. Tells what type of value the function will give back.
2. **Function Name:** e.g., `trig_identity`, `mean3`, `square`.
3. **Parameter List:** inside `(...)`. For each parameter, it needs a type and a name. The compiler records the number of parameters, their datatypes, and their order.
4. **Function Body:** enclosed in `{ ... }`. Contains local variable declarations and instructions.
5. **Return Statement:** If the return type is not `void`, there must be a `return` of that type.


### 3.2 Function Call


The general pattern is:


```c
result = function_name(arg1, arg2, ...);
```


At the function call, the compiler checks:


1. Does a function with that name exist?
2. What is its return type?
3. How many parameters does it expect?
4. Are the arguments compatible in **number, type, and order**?


If the variables given during the function call (arguments) do not match the expected parameters, the compiler gives you an error or a warning.


-----


## 4\. Parameters vs Arguments


 * **In the Function Definition:**
   `double trig_identity(double x)`
   `x` is a **Parameter**. It is like the mathematical variable in $f(x)$. It is a placeholder.


 * **In the Function Call:**
   `value = trig_identity(M_PI / 4);`
   `M_PI / 4` is an **Argument**. It is the actual value passed in this call.


> **Student Note:**
>
>   * **Parameters:** The variable names given in the function definition.
>   * **Arguments:** The real values/variables we pass in the function call.


-----


## 5\. What Datatypes Can We Use as Parameters?


In C, many types can be used as parameters. Right now, we will stick to basic datatypes:


 * `int`: integers, indices, counts.
 * `double` / `float`: for real numbers (trig, probabilities).
 * `char`: for characters (grades, symbols).


Examples:


 * `int square_int(int x);`
 * `double trig_identity(double x);`


-----


## 6\. Scope: Where You Can Use a Variable


Earlier, we said a C program consists of **variables + instructions**.
Now we must refine that: **Every variable has a scope.**


### 6.1 Intuition from Mathematics


In maths, you cannot use a variable $x$ unless you define it.


 * Example: “Let $x$ be a real number…”
 * From that point onwards, in that problem, you may use (access) or update $x$.
 * The place from where you are allowed to use that variable is called its **scope**.


### 6.2 Scope in C


**Scope of a variable = the part of the program where you can access or update that variable.**


Inside the scope of a variable, you can do only two basic things with it:


1. **Access** its value (read it, use it in expressions, print it).
2. **Update** its value (assign a new value).


**Important Rules:**


 * Both access and update operations must happen within the scope.
 * If two variables have the **same name in the same scope**, the compiler will give an **error** (ambiguity).
 * If the same name is used in **different scopes**, it is allowed (they are treated as different variables).


### 6.3 Local Variables


A local variable is declared inside a function or inside a block `{ ... }`.


 * **Scope:** Starts from the line where it is declared, continues until the closing `}` of that block.
 * **Behavior:** Created when the block is entered, destroyed when the block is exited.


<!-- end list -->


```c
#include <stdio.h>


double square(double x) {
   double result = x * x;      // local to square
   return result;
}


int main(void) {
   double y = 3.0;             // local to main
   double z = square(y);       // y used as argument


   printf("y = %.1f, y^2 = %.1f\n", y, z);
   // printf("%f", result);    // ❌ Error! 'result' is not in scope here
   return 0;
}
```


 * `x` and `result`: Local to `square`. Cannot be used in `main`.
 * `y` and `z`: Local to `main`. Cannot be used inside `square`.


### 6.4 Global Variables


A global variable is declared outside all functions.


 * **Scope:** Throughout the entire file, after the declaration.


<!-- end list -->


```c
#include <stdio.h>


int counter = 0;   // global variable


void increment(void) {
   counter = counter + 1;  // access/update global
}


int main(void) {
   printf("counter = %d\n", counter);
   increment();
   printf("counter = %d\n", counter);
   return 0;
}
```


### 6.5 Parameters are Local Variables Too


In `double trig_identity(double x)`, `x` is both a parameter and a local variable. You can access and update `x` only inside `trig_identity`. You cannot use `x` in `main` unless you declare a separate variable `x` there.


-----


## 7\. Library Functions vs Our Own Functions


So far, we have created our own functions like `trig_identity` and `mean3`.
But from the very first C program, we have been using functions like `printf`, `scanf`, `sin`, `cos`.


These are **Built-in Library Functions**.


 * They are already defined and implemented in the C standard library.
 * We are only **calling** them.


**Why do we need `#include <stdio.h>`?**
The compiler needs to know the **definition** (return type, parameters) of `printf` before you call it. This definition is written in the header file `stdio.h`.


If we do not include the correct header:


 * The compiler doesn’t know what `printf` is.
 * It will complain: “implicit declaration of function” or “undefined reference.”


-----


## 8\. Common Mistakes with Functions


Here are typical mistakes students make. Use them as “spot and correct” exercises.


**Mistake 1: Too few arguments**


```c
int s = sum(10);     // ❌ only one argument, function needs two
```


**Mistake 2: Too many arguments**


```c
int s = sum(10, 20, 30);  // ❌ three arguments, function needs two
```


**Mistake 3: Wrong type of argument**


```c
int s = square_int(3.5);   // ❌ 3.5 is double, parameter is int
```


**Mistake 4: Using return value of a void function**


```c
void show_double(int x) { ... }
int main(void) {
   int y = show_double(5);   // ❌ show_double returns nothing!
}
```


**Mistake 5: Non-void function without a proper return**


```c
int max2(int a, int b) {
   if (a > b) return a;
   // ❌ missing return when a <= b (undefined behavior)
}
```


**Mistake 6: Conceptual mismatch of return type**


```c
int get_grade(int marks) {  // ❌ should be char, logically
   return 'A'; // Returns ASCII 65, not the character 'A' for printing as %c
}
```


**Mistake 7: Conflicting declarations**


```c
int sum(int a, int b);      // prototype says (int, int)
float sum(float a, float b) { ... } // ❌ definition says (float, float)
```


**Mistake 8: Using function before declaration**


```c
int main(void) {
   int s = sum(2, 3);    // ❌ sum not declared yet
}
int sum(int a, int b) { ... }
```


**Mistake 9: Defining a function inside another function**


```c
int main(void) {
   int square(int x) { ... } // ❌ nested function definitions are not allowed in standard C
}
```


**Mistake 10: Typo inside function body**


```c
int square(int x) {
   return y * y;   // ❌ 'y' undeclared, meant 'x'
}
```


**Mistake 11: Passing uninitialized variable**


```c
int a;              // uninitialized
int s = square(a);  // ❌ garbage value passed
```


**Mistake 12: Old-style function definition (Avoid)**


```c
// ❌ Old K&R style
int sum(a, b)
int a, b;
{ ... }
```


**Mistake 13: Assuming pass-by-reference**


```c
void set_to_100(int x) { x = 100; } // only local copy changes
int main(void) {
   int a = 5;
   set_to_100(a);  // ❌ a is still 5
}
```


**Mistake 14: Shadowing global and expecting it to change**


```c
int g = 10;
void change(void) {
   int g = 20;   // ❌ local variable creates a NEW g, doesn't change global
}
```


**Mistake 15: Wrong order of arguments**


```c
// Definition: void print_ratio(int total, int count)
print_ratio(count, total);   // ❌ passed in reverse order
```


**Mistake 16: Missing header for a library function**


```c
// Missing #include <math.h>
double x = sqrt(4.0);   // ❌ compiler doesn't know sqrt
```


**Mistake 17: Different parameter types in prototype and definition**


```c
int sum(float a, float b);   // ❌ prototype: (float, float)
int sum(int a, int b) { ... } // definition: (int, int)
```


**Mistake 18: Using local variable before declaration**


```c
printf("%d\n", x);  // ❌ x not declared yet
int x = 10;
```


**Mistake 19: Missing return type**


```c
get_value() { ... } // ❌ Implicit int (in old C) or error (in modern C)
```


**Mistake 20: Using assignment instead of comparison in if**


```c
if (x = 0) { ... }   // ❌ assigns 0 (false), does not compare
```


**Mistake 21: Semicolon after function header in definition**


```c
int square(int x); // ❌ The semicolon here ends the statement!
{
   return x * x;
}
// This separates the header from the body, causing a syntax error.
```


-----


## 9\. Big Picture Summary


 * A C program is made of **variables** and **instructions**.
 * Each variable has a **scope**: the region where you can access or update it.
 * If two variables with the same name are declared in the **same scope**, it is an error.
 * When we find ourselves writing the same block of instructions multiple times, we should create a **function**.
 * A function in C is just like a function in maths: **define once (rule), apply to many inputs.**
 * The **function definition** tells the compiler the output datatype, name, and parameters.
 * The **function call** passes arguments. C checks that arguments match parameters in number and type.
 * **Built-in functions** (like `printf`) are defined in libraries. We must `#include` their headers so the compiler knows their definitions.

