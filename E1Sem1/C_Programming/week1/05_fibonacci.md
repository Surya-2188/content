
#  Fibonacci Sequence (Up to a Given Number)


We will use this program to understand **while loops** and how to **swap/shift variables**.


```c
1  #include <stdio.h>
2  int main() {
3      int n, a = 0, b = 1, next;
4      printf("Enter the limit: ");
5      scanf("%d", &n);
6      printf("Fibonacci Series: %d %d ", a, b);
7      next = a + b;
8      while (next <= n) {
9          printf("%d ", next);
10         a = b;
11         b = next;
12         next = a + b;
13     }
14     return 0;
15 }
```


-----



## 1\. Small Introduction: What is the Fibonacci Series?


The Fibonacci series is a sequence of numbers where every new number is obtained by **adding the previous two numbers**.


It starts with: **0, 1**


 * $0 + 1 = 1$
 * $1 + 1 = 2$
 * $1 + 2 = 3$
 * $2 + 3 = 5$
 * ... and so on.


**Sequence:** 0, 1, 1, 2, 3, 5, 8, 13, 21...


This sequence appears in nature, spirals, plants, art, and even computer algorithms. We use loops (`while` or `for`) to generate it because each term depends on the previous two.


## 2\. Explanation Material


Just like in the previous program, we start by using a header file because commands like `printf` and `scanf` are already written in the C library. We include these commands using `#include <stdio.h>`.


The program execution begins from the first line inside the `main()` function.


To solve this, we need variables to store the numbers in the sequence:


 * `a = 0`: The first number.
 * `b = 1`: The second number.
 * `next`: To calculate and store the new number.


Since Fibonacci numbers are whole numbers, we declare them using the `int` data type.


## 3\. Line-by-Line Explanation


**Line 3: Variable Declaration**


```c
int n, a = 0, b = 1, next;
```


We declare four integer variables:


 * `n`: Stores the limit given by the user (stop when numbers go above this).
 * `a` and `b`: Store the starting two numbers (initialized to 0 and 1).
 * `next`: Stores the upcoming number.


**Lines 4-5: Taking Input**


```c
printf("Enter the limit: ");
scanf("%d", &n);
```


We ask the user for a maximum limit (e.g., "Print Fibonacci numbers up to 50"). `scanf` stores this value in `n`.
*Remember:* `scanf` needs `&` because it stores input into a variable’s memory address.


**Line 6: Printing the Start**


```c
printf("Fibonacci Series: %d %d ", a, b);
```


The Fibonacci series **always** begins with 0 and 1, so we print them manually before starting the loop.


**Line 7: Computing the First 'Next' Term**


```c
next = a + b;
```


We calculate the next number by adding the previous two (`0 + 1 = 1`).


**Lines 8-13: The Loop (The Logic)**


```c
while (next <= n) {
   printf("%d ", next);
   a = b;
   b = next;
   next = a + b;
}
```


This loop continues **as long as** the `next` number is less than or equal to the limit `n`.
Inside the loop:


1. **Print:** Display the calculated `next` number.
2. **Shift Values:** This is the most important step. To move forward in the series:
     * The old `b` becomes the new `a`.
     * The old `next` becomes the new `b`.
3. **Calculate Again:** Find the new `next` (`a + b`).


**Line 14: Completion**


```c
return 0;
```


Indicates successful program execution.


## 4\. Summary


By the end of this program, you have learned:


 * How loops generate a sequence.
 * How variables can store changing values.
 * The logic of swapping/shifting values (`a = b`, `b = next`) to move a series forward.


-----



