
# Roll Call Program & The Power of Loops


## 1️⃣ Start with the Simple Code


Let us begin with a realistic example: **Taking Attendance**.
Imagine a classroom. The logic is simple:


1. Go roll number by roll number.
2. Ask if the student is present (1) or absent (0).
3. Count them.
4. Finally, print the total.


Here is the C code for this:


```c
#include <stdio.h>


int main() {
   int n, i;
   int present = 0, absent = 0;
   int status;


   // Ask total strength of class
   printf("Enter number of students: ");
   scanf("%d", &n);


   // The Loop: Taking attendance for every student
   for(i = 1; i <= n; i++) {
       printf("Roll number %d (1 = present, 0 = absent): ", i);
       scanf("%d", &status);


       if(status == 1)
           present++;
       else
           absent++;
   }


   printf("\nPresent: %d\n", present);
   printf("Absent : %d\n", absent);


   return 0;
}
```


-----


## 2️⃣ Convert to Pseudocode 


Before we analyze the loop, let's write the **Pseudocode**.
Pseudocode means explaining the logic in simple English steps.


**START**


1. Ask "How many students are there?" (`n`)
2. Set `present` counter to 0.
3. Set `absent` counter to 0.
4. **Start roll call from Student 1.**


**REPEAT** for each roll number up to `n`:


 * Ask: "Is this student present or absent?"
 * If **Present** → increase `present` count.
 * Else → increase `absent` count.


**AFTER** finishing all roll numbers:


 * Print Total Present.
 * Print Total Absent.
   **END**


-----


## 3️⃣ Observe the THREE IMPORTANT PARTS


If you look closely at the "REPEAT" step in our pseudocode, there are **three specific things** happening to control the flow:


1. **Initialization (Start):**
     * Where do we begin?
     * *Answer:* Start with Roll Number 1.
2. **Condition (Stop):**
     * When do we stop?
     * *Answer:* Stop when we reach the last student (`n`).
3. **Increment (Move Forward):**
     * How do we go to the next person?
     * *Answer:* Move to the next roll number (`i++`).


**Key Takeaway:** These three steps (Start, Check, Move) are required for **any** counting task.


-----


## 4️⃣ Why the `for` Loop is Powerful


This is where the `for` loop shines. Instead of writing these three steps in different places (which can be messy and confusing), the `for` loop combines them into **one single line**.


```c
for (i = 1;  i <= n;  i++)
```


Look at how neatly it maps:


 * `i = 1` $\rightarrow$ **Start** (Initialization)
 * `i <= n` $\rightarrow$ **Check** (Condition)
 * `i++` $\rightarrow$ **Move** (Increment)


**Why do we need it?**
Without this loop, if you had 50 students, you would have to write the `printf` and `scanf` lines **50 times**\! The `for` loop allows you to write the logic **once** and repeat it as many times as you need. It makes your code clean, readable, and mistake-free.


-----
