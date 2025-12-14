
# Factorial of a Number


We will use this program to understand how to handle **loops** and **logic** in C.


```c
1  #include <stdio.h>
2  int main() {
3      int n, i;
4      int fact = 1;
5      printf("Enter a number: ");
6      scanf("%d", &n);
7      for(i = 1; i <= n; i++) {
8          fact = fact * i;
9      }
10     printf("Factorial = %d\n", fact);
11     return 0;
12 }
```


-----


# Detailed Explanation


## 1\. Setup and Variables


**Header Files & `main()` Function**
As we established in the previous program:


 * We include `#include <stdio.h>` to access the pre-written `printf` and `scanf` commands.
 * Execution always begins inside the `main()` function block.
 * Every instruction must end with a semicolon (`;`).


**Variables and Data (Line 3)**
A program contains data and instructions. In this specific program, we declare three integer variables (`int`):


1. **`n`**: Stores the number entered by the user (the number we want to calculate the factorial for).
2. **`i`**: Acts as the **loop counter** (it counts 1, 2, 3... up to `n`).
3. **`fact`**: Stores the running total of the multiplication (the final result).


**Initialization (Line 4)**


```c
int fact = 1;
```


You **must** initialize `fact` to **1**. This is crucial because factorial involves multiplication.


 * If you initialize `fact` to **0**, the result will always be **0** (because anything multiplied by 0 is 0).
 * Since we are multiplying, we start with the neutral number **1**.


-----


## 2\. Line-by-Line Logic


**Input (Lines 5-6)**


```c
printf("Enter a number: ");
scanf("%d", &n);
```


 * **Line 5:** Displays the message to the user.
 * **Line 6:** Reads the input. As explained previously, `%d` expects an integer, and `&n` provides the memory address of variable `n` so `scanf` knows where to store the value.


**The Loop (Line 7)**


```c
for(i = 1; i <= n; i++)
```


We use a loop here because a factorial requires **repeated multiplication** ($n! = 1 \times 2 \times 3 \times \dots \times n$).
Instead of writing a separate multiplication line for every number, the `for` loop handles this repetition automatically:


1. **Start:** It starts `i` at **1**.
2. **Condition:** It continues running as long as `i` is less than or equal to `n`.
3. **Update:** It increases `i` by 1 (`i++`) after every step.


**The Calculation Logic (Line 8)**


```c
fact = fact * i;
```


This is the most important line. In every step of the loop, the program takes the current value of `fact`, multiplies it by the current counter `i`, and updates `fact`.


**Let's trace how this works if the user enters 5 (n = 5):**


| Iteration | Value of `i` | Calculation (`fact * i`) | New Value of `fact` |
| :--- | :--- | :--- | :--- |
| **1st Pass** | 1 | $1 \times 1$ | **1** |
| **2nd Pass** | 2 | $1 \times 2$ | **2** |
| **3rd Pass** | 3 | $2 \times 3$ | **6** |
| **4th Pass** | 4 | $6 \times 4$ | **24** |
| **5th Pass** | 5 | $24 \times 5$ | **120** |


When `i` becomes 6, the loop stops because 6 is not less than or equal to 5. The final value in `fact` is **120**.


**Output (Line 10)**


```c
printf("Factorial = %d\n", fact);
```


The program prints the final result. We use `%d` because the result (`fact`) is an integer.


-----


## 🛑 Common Mistakes Students Make


 * ❌ **Initializing `fact` to 0**


     * **Wrong:** `int fact = 0;`
     * **Why:** This logic error causes the entire result to become **0** immediately upon the first multiplication.


 * ❌ **Starting the Loop from 0**


     * **Wrong:** `for(i = 0; i <= n; i++)`
     * **Why:** Multiplying by 0 turns the result into 0. Factorials must start multiplication from 1.


 * ❌ **Forgetting `&` in `scanf`**


     * **Wrong:** `scanf("%d", n);`
     * **Why:** As discussed in the first program, `scanf` requires the address (`&`) to store the input.


 * ❌ **Using `float` instead of `int`**


     * **Why:** Factorials define values for whole numbers only. Integers are the appropriate data type here. Consequently, using `%f` instead of `%d` would also be incorrect.


 * ❌ **Entering Negative Numbers**


     * **Note:** Factorials are mathematically undefined for negative numbers. If a user enters -5, this basic program will simply return 1 (because the loop condition `1 <= -5` fails immediately), which might confuse the user.

