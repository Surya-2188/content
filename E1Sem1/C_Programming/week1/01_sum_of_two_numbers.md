

#  Sum of Two Numbers (C Program)


We will use this specific program as our reference to explain how C works.


```c
1  #include <stdio.h>
2  int main() {
3      int a, b, sum;
4      printf("Enter first number: ");
5      scanf("%d", &a);
6      printf("Enter second number: ");
7      scanf("%d", &b);
8      sum = a + b;
9      printf("Sum = %d\n", sum);
10     return 0;
11 }
```


-----


# Detailed Explanation


## 1\. Core Concepts


### The Role of Header Files


You require a header file because the C language developers have already written complex code for certain commands—such as `printf` and `scanf`. Instead of writing this code from scratch, you simply use it. To access these commands, you must include the appropriate header file using `#include <stdio.h>`.


 * **stdio.h** stands for **St**andar**d** **I**nput **O**utput.


### Where Execution Begins: The `main()` Function


A program acts as a sequence of instructions for the computer. However, execution does not necessarily start at the very first line of the file, as programmers often include comments or extra configurations at the top.


Therefore, you must tell the computer exactly where to begin. You do this by writing a block called `main()`. The computer **always** starts running from the first line inside the `main()` block. We will discuss why we write `int main` later when we learn about functions; for now, simply memorize this structure.


### Storing Data in Variables


A program consists of two primary components: **data** and **instructions**. You must store this data somewhere. In programming, we store data in **variables**. This mirrors mathematics, where you use symbols like $x$ or $y$ to hold values that may change.


### The Assignment Operator (`=`)


In mathematics, the symbol `=` often represents a condition (e.g., checking if two sides are equal). However, in programming languages like C, the `=` symbol performs an action: it **assigns** the value on the right side to the variable on the left side.


### Why You Must Declare Data Types


In C, you cannot simply write a variable name and use it immediately. You must first specify its **type**. This is necessary because different types of data require different amounts of memory space.


When you write numbers on paper, a dot (`.`) naturally indicates a decimal. However, in C, you must explicitly tell the computer whether a number is an integer (whole number) or a float (decimal), because the computer performs **integer arithmetic** and **floating-point arithmetic** differently. Declaring the type helps the computer allocate the correct amount of space and improves memory efficiency.


**Summary so far:**


 * You know what a header file is.
 * You know what a variable is.
 * You understand why you must declare its type.


> **Note:** In Python, you do not use a semicolon at the end of each line. In C programming, however, a **semicolon (;)** is mandatory to mark the end of a statement. Think of it as a full stop in a sentence.


-----


## 2\. Line-by-Line Concept Introduction


### Line 4: `printf` (Output)


The C library provides this pre-written piece of code. It prints text onto the screen. You must write anything you want to display inside double quotes `" "`. This output appears on what we call the **standard output** (your display or terminal).


### Line 5: `scanf` (Input)


What if you want the user to type something?


1. The user types data at the keyboard (the **standard input**).
2. The `scanf` command collects this input and stores it into a variable.


To use `scanf`, you must provide two specific pieces of information:


1. The **format type** (e.g., `%d` for integers).
2. The **address** of the variable where the computer should store the data (written using `&`).


**Key Distinction:** `printf` does not use `&`, but `scanf` **must** use `&` because it needs to locate the variable's storage box in memory to deposit the data.


### Line 8: The Logic (`sum = a + b`)


This line performs the actual calculation. The computer takes the value stored in `a`, adds it to the value stored in `b`, and assigns the result to the variable `sum` using the assignment operator (`=`).


### Line 10: `return 0`


This statement tells the operating system that the program has finished executing successfully.


-----


## 3\. Decoding the Symbols


### What is `%d`?


The `%d` symbol tells the computer that the number involved is an **integer** (decimal integer). It appears in both `printf` and `scanf`, but it serves different purposes:


 * In `printf`, `%d` acts as a placeholder that tells the computer **where and how to display** the integer.
 * In `scanf`, `%d` acts as a specific instruction telling the computer **what type of input to expect**.


### What is `&a`?


The `&a` symbol represents the **memory address** of variable `a`. `scanf` requires this address to know exactly where in the memory grid it should store the number entered by the user.


-----


## 4\. Handling Multiple Variables


### Printing Multiple Variables in `printf`


You can print more than one value in a single statement:


```c
printf("a = %d, b = %d", a, b);
```


 * Each `%d` corresponds to one variable in the list.
 * The number of format specifiers (`%d`) must match the number of variables exactly.


### Taking Input into Multiple Variables Using `scanf`


You can ask for multiple inputs at once:


```c
scanf("%d %d", &a, &b);
```


 * The user must enter values separated by a space or a newline (Enter key).
 * The first `%d` fills variable `a`.
 * The second `%d` fills variable `b`.


-----


## 🛑 Common Mistakes Students Make


 * ❌ **Forgetting `&` in `scanf`**


     * **Wrong:** `scanf("%d", a);`
     * **Correct:** `scanf("%d", &a);` (Without the address, the program cannot store the value).


 * ❌ **Mismatch of Format Specifiers**


     * **Example:** Using `%f` for an integer variable, or `%d` for a float variable. This confuses the computer.


 * ❌ **Missing Semicolons**


     * Students often forget the `;` at the end of statements. The compiler will produce an error without it.


 * ❌ **Extra Spaces inside Format Strings**


     * **Example:** `scanf(" %d", &a);` — The space inside the quotes is usually unnecessary and can cause input glitches.


 * ❌ **Printing Variables without Format Specifiers**


     * **Wrong:** `printf(a);`
     * **Correct:** `printf("%d", a);` (You must tell `printf` the format of the variable).


 * ❌ **Typing Incorrect Input**


     * Entering letters when the program expects numbers (`%d`) causes errors or unexpected behavior.

