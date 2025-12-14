# Addition of Two Matrices





## 1\. Quick Recall: What is a Matrix?


A **Matrix** is simply a **2D Array** that looks like a table with Rows and Columns.


**Example (2 Rows, 3 Columns):**


```text
 Col 0  Col 1  Col 2
[  10     20     30  ]  Row 0
[  40     50     60  ]  Row 1
```


In C, we declare this as:


```c
int a[2][3];
```


To access the value `50`, we go to **Row 1, Column 1**:


```c
a[1][1] = 50;
```


-----


## 2\. The "Two Loops" Rule (Critical Concept)


Before we add matrices, you must understand one rule:


> **🛑 Rule:** To work with a Multidimensional Array, you ALWAYS need **Two Loops** (Nested Loops).


**Why? Think of a Building.**
Imagine a school building with **Floors (Rows)** and **Classrooms (Columns)**.
If you want to clean every classroom, you cannot just run in a straight line.


1. **Outer Loop (The Elevator):** Go to **Floor 0**.
2. **Inner Loop (The Corridor):** Visit **Room 0**, then **Room 1**, then **Room 2**.
3. **Outer Loop:** Move to **Floor 1**.
4. **Inner Loop:** Visit **Room 0**, then **Room 1**, then **Room 2**.


**In Programming terms:**


 * **Outer Loop (`i`):** Controls the **Rows** (Moves slowly).
 * **Inner Loop (`j`):** Controls the **Columns** (Moves fast).


Without two loops, you cannot access every cell in the grid.


-----


## 3\. The Logic: Matrix Addition


Matrix addition is very simple. You just add the number in the **first matrix** to the number in the **exact same position** in the **second matrix**.


**Formula:**
$$C[i][j] = A[i][j] + B[i][j]$$


**Visual Diagram:**


```text
   Matrix A         Matrix B          Result (C)
 [ 1   2 ]        [ 4   5 ]        [ 1+4   2+5 ]
 [ 3   4 ]   +    [ 6   7 ]   =    [ 3+6   4+7 ]
```


**Constraint:**
You can only add matrices if they have the **same dimensions** (same number of rows and columns).


-----


## 4\. The C Program
We will use this program to understand how to manipulate **2D Arrays** using **Nested Loops**.

```c
#include <stdio.h>


int main() {
   int rows, cols, i, j;


   // 1. Decide the size of the matrix
   printf("Enter number of rows: ");
   scanf("%d", &rows);


   printf("Enter number of columns: ");
   scanf("%d", &cols);


   // Declare three 2D arrays (Matrices)
   int A[rows][cols], B[rows][cols], C[rows][cols];


   // 2. Input for Matrix A (Using Two Loops)
   printf("\nEnter elements of Matrix A:\n");
   for(i = 0; i < rows; i++) {           // Outer Loop: Changes Row
       for(j = 0; j < cols; j++) {       // Inner Loop: Changes Column
           scanf("%d", &A[i][j]);
       }
   }


   // 3. Input for Matrix B (Using Two Loops)
   printf("\nEnter elements of Matrix B:\n");
   for(i = 0; i < rows; i++) {
       for(j = 0; j < cols; j++) {
           scanf("%d", &B[i][j]);
       }
   }


   // 4. PERFORM ADDITION (The Logic)
   // We visit every cell using Nested Loops and add them up
   for(i = 0; i < rows; i++) {
       for(j = 0; j < cols; j++) {
           C[i][j] = A[i][j] + B[i][j];
       }
   }


   // 5. Print the Result Matrix (Using Two Loops)
   printf("\nResultant Matrix C (A + B):\n");
   for(i = 0; i < rows; i++) {
       for(j = 0; j < cols; j++) {
           printf("%d ", C[i][j]);
       }
       // After printing all columns in a row, move to the next line
       printf("\n");
   }


   return 0;
}
```


-----


## 5\. Line-by-Line Explanation


### Step 1: Declaration


```c
int A[rows][cols], B[rows][cols], C[rows][cols];
```


We create three tables in memory.


 * **A:** For the first set of numbers.
 * **B:** For the second set.
 * **C:** To store the answer.


### Step 2: Input (Why Nested Loops?)


```c
for(i = 0; i < rows; i++) {       // Row control
   for(j = 0; j < cols; j++) {   // Column control
       scanf("%d", &A[i][j]);
   }
}
```


 * When `i = 0` (Row 0), the inner loop runs for `j = 0, 1, 2...`
 * This fills the **first row completely**.
 * Then `i` becomes `1`, and we fill the **second row**.
 * **Key Takeaway:** You cannot take input for a matrix without this `i` and `j` combination.


### Step 3: The Addition Logic


```c
C[i][j] = A[i][j] + B[i][j];
```


The computer goes to a specific box (say, Row 1, Col 2) in Matrix A, adds it to the same box in Matrix B, and puts the result in Matrix C.


### Step 4: Printing the Output


```c
printf("%d ", C[i][j]);
```


We print the number followed by a space.


```c
printf("\n");
```


**This is very important.** This `printf("\n")` is inside the **Outer Loop** but outside the **Inner Loop**.


 * **Meaning:** "After finishing printing all columns of one row, press 'Enter' to go to the next line."
 * Without this, your matrix would look like a single long line of numbers\!


-----


## 6\. Summary for Students


1. **Matrices are Tables:** They allow us to store data in rows and columns.
2. **The Golden Rule:** If you are using a 2D Array, you **must** use **Nested Loops** (Loop inside a Loop).
3. **Roles:**
     * Outer Loop (`i`) $\rightarrow$ Handles Rows.
     * Inner Loop (`j`) $\rightarrow$ Handles Columns.
4. **Formatting:** Don't forget the `\n` (newline) after the inner loop to make the output look like a real grid.


-----


**Would you like me to translate this explanation into the Telugu version?**
Here are the **Common Mistakes** students make when working with **Matrix Addition (2D Arrays)**, provided in both **English** and **Telugu**.


-----


# Common Mistakes


### ❌ Mistake 1: Forgetting the Newline (`\n`) in Output


**The Mistake:**


```c
printf("%d ", C[i][j]);
// Missing printf("\n") after the inner loop
```


**Why it is wrong:**
Without the `\n` inside the outer loop, your matrix will print as one long, confusing line of numbers (e.g., `1 2 3 4 5 6`). It won't look like a table.
**Correct Way:** Always add `printf("\n");` after the inner loop finishes printing a row.


### ❌ Mistake 2: Confusing Row (`i`) and Column (`j`) Indexes


**The Mistake:**


```c
scanf("%d", &A[j][i]); // Swapped i and j
```


**Why it is wrong:**
If you swap them, you might accidentally fill the matrix **column-by-column** instead of row-by-row, or worse, access memory that doesn't exist (if rows and columns are different sizes).
**Correct Way:** Stick to the standard: `A[i][j]` (Row first, then Column).


### ❌ Mistake 3: Using One Loop Instead of Two


**The Mistake:**
Trying to access elements using a single `for` loop.
**Why it is wrong:**
A matrix has two dimensions. One loop only moves in a straight line. You cannot navigate a grid without an outer loop (for rows) and an inner loop (for columns).


### ❌ Mistake 4: Forgetting `&` in `scanf`


**The Mistake:**


```c
scanf("%d", A[i][j]);
```


**Why it is wrong:**
Just like 1D arrays, `scanf` needs the **address** of the specific box in the matrix. Without `&`, the program will crash.
**Correct Way:** `scanf("%d", &A[i][j]);`


-----

