# Multiplication Table of a Given Number


We will use this program to practice **loops** and **formatting output**. The goal is to take a number from the user (e.g., 5) and print its table from $1$ to $10$.


```c
1  #include <stdio.h>
2 
3  int main() {
4      int n, i;
5 
6      printf("Enter a number: ");
7      scanf("%d", &n);
8 
9      printf("Multiplication Table of %d:\n", n);
10
11     for(i = 1; i <= 10; i++) {
12         printf("%d x %d = %d\n", n, i, n * i);
13     }
14
15     return 0;
16 }
```


-----


# Line-by-Line Explanation


### Line 1: The Setup


```c
#include <stdio.h>
```


We include the standard input-output header file. This is required because we use functions like `printf` and `scanf`, which are already written by the C language developers. Without this line, the computer will not understand those commands.


### Line 3: The Entry Point


```c
int main() {
```


The `main()` function is mandatory. It tells the computer exactly where to start executing the program logic.


### Line 4: Variable Declaration


```c
int n, i;
```


We declare two integer variables to store our data:


1. **`n`**: Stores the number the user enters (the number whose table we want).
2. **`i`**: Acts as the iterator or counter for the loop (it will count from 1 to 10).


### Lines 6-7: Input


```c
printf("Enter a number: ");
scanf("%d", &n);
```


 * **Line 6:** Shows a message on the screen asking the user to input a number.
 * **Line 7:** Reads the integer typed by the user and stores it in the address of `n` (`&n`).


### Line 9: Table Header


```c
printf("Multiplication Table of %d:\n", n);
```


This prints a header to make the output look neat. If `n` is 5, it prints: **"Multiplication Table of 5:"**.


### Line 11: The Loop


```c
for(i = 1; i <= 10; i++) {
```


This is the core of the program. We need to print 10 lines, so we use a `for` loop to repeat the activity 10 times.


 * **Start:** `i = 1`
 * **Stop:** `i <= 10`
 * **Step:** `i++` (Increase `i` by 1 after every step).


[Image of for loop flowchart C programming]


### Line 12: The Calculation & Printing


```c
printf("%d x %d = %d\n", n, i, n * i);
```


This line runs 10 times. It prints the mathematical format.
Notice there are three `%d` placeholders:


1. **First `%d`** is replaced by **`n`** (the fixed number, e.g., 5).
2. **Second `%d`** is replaced by **`i`** (the changing counter: 1, 2, 3...).
3. **Third `%d`** is replaced by **`n * i`** (the calculated result: 5, 10, 15...).


### Line 15: Completion


```c
return 0;
```


This indicates that the program has finished execution successfully.


-----


# 🔄 Loop Analogy: The Roll Call


To understand how `for(i = 1; i <= 10; i++)` works, think of a teacher taking attendance (Roll Call) in a classroom.


1. **Initialization (`i = 1`):**
   The teacher starts with **Student \#1**.


2. **Condition (`i <= 10`):**
   The teacher checks: "Is this roll number within the limit (10)?"


     * *If Yes:* The teacher performs the action (Calls the name / Prints the table line).


3. **Increment (`i++`):**
   After finishing with the current student, the teacher moves to the **next number**.


This cycle repeats—checking and calling—until the teacher crosses roll number 10. Once `i` becomes 11, the condition fails, and the loop stops.


So, the code translates to:
**"Start from 1 $\rightarrow$ Keep working $\rightarrow$ Stop when you cross 10."**


-----


# Final Output Example


If the user enters the number **5**, the output on the screen will look like this:


```text
Enter a number: 5
Multiplication Table of 5:
5 x 1 = 5
5 x 2 = 10
5 x 3 = 15
5 x 4 = 20
5 x 5 = 25
5 x 6 = 30
5 x 7 = 35
5 x 8 = 40
5 x 9 = 45
5 x 10 = 50
```



