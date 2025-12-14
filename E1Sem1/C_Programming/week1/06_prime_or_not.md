
# Check if a Number is Prime


We will use this program to practice **loops**, **if-conditions**, and the **modulus operator (`%`)**.


```c
1  #include <stdio.h>
2  int main() {
3      int n, i, count = 0;
4     
5      printf("Enter a number: ");
6      scanf("%d", &n);
7     
8      // Loop from 1 to n to find factors
9      for(i = 1; i <= n; i++) {
10         if(n % i == 0) {
11             count++;
12         }
13     }
14    
15     // A Prime number has exactly 2 factors (1 and itself)
16     if(count == 2) {
17         printf("%d is a Prime Number.\n", n);
18     } else {
19         printf("%d is NOT a Prime Number.\n", n);
20     }
21    
22     return 0;
23 }
```


-----


# Small Introduction: What is a Prime Number?


A number is **Prime** if it has **exactly two factors**:


1. The number **1**.
2. The **number itself**.


**Examples:**


 * **5** is Prime (Divisible only by 1 and 5).
 * **7** is Prime (Divisible only by 1 and 7).
 * **4** is **NOT** Prime (Divisible by 1, 2, and 4 — that's 3 factors).


**The Logic:**
To check if a number `n` is prime, we divide it by every number from 1 to `n`. We count how many times the division is perfect (remainder is 0). If the final count is exactly 2, it is Prime.


-----


#  Line-by-Line Explanation


### Line 3: Variable Declaration


```c
int n, i, count = 0;
```


We declare three integer variables:


 * **`n`**: The number we want to check (e.g., 5).
 * **`i`**: The loop counter (1, 2, 3...).
 * **`count`**: To store how many factors we found.
     * *Important:* We must initialize `count = 0`. If we don't, it might start with a random garbage value.


### Lines 5-6: Input


```c
printf("Enter a number: ");
scanf("%d", &n);
```


We ask the user for a number. Let's assume the user enters **5**.


### Lines 9-13: The Loop (Finding Factors)


```c
for(i = 1; i <= n; i++) {
   if(n % i == 0) {
       count++;
   }
}
```


This is the main logic. The loop runs from `i = 1` up to `n`.


**Inside the loop:**


1. **Check:** `if(n % i == 0)`
     * The `%` symbol is the **Modulus Operator**. It gives the remainder.
     * If `n` divided by `i` leaves a remainder of `0`, it means `i` is a factor.
2. **Count:** `count++`
     * If we find a factor, we increase the count.


**Let's Trace for n = 5:**


 * `5 % 1 == 0`? **Yes**. (Count becomes 1).
 * `5 % 2 == 0`? No.
 * `5 % 3 == 0`? No.
 * `5 % 4 == 0`? No.
 * `5 % 5 == 0`? **Yes**. (Count becomes 2).


**End result:** `count` is 2.


### Lines 16-20: The Final Verdict


```c
if(count == 2) {
   printf("%d is a Prime Number.\n", n);
} else {
   printf("%d is NOT a Prime Number.\n", n);
}
```


We check the final value of `count`.


 * If `count` is exactly **2**, it means the number was divisible ONLY by 1 and itself. **It is Prime.**
 * If `count` is anything else (1, 3, 4, etc.), it is **Not Prime**.


-----


# 🛑 Common Mistakes Students Make


 * ❌ **Initializing `count` to 1 or forgetting to initialize**


     * **Wrong:** `int count;` (It might contain garbage values like 3251).
     * **Correct:** `int count = 0;` (Start counting from zero).


 * ❌ **Confusing `%` (Modulus) with `/` (Division)**


     * `/` gives the **quotient** (5 / 2 = 2).
     * `%` gives the **remainder** (5 % 2 = 1). We need the remainder to check divisibility.


 * ❌ **Looping starting from 0**


     * **Wrong:** `for(i = 0; ...)`
     * **Reason:** You cannot divide a number by 0. The program will crash (Run-time error). Always start from 1.


-----

