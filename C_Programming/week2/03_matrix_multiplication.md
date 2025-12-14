#  Topic: Multidimensional Arrays & Matrix Multiplication


## 1\. Why Do We Need Multidimensional Arrays? (The "Attendance" Problem)


We already know that a **1D array** is like a single straight line. It is perfect for storing a list, like the marks of 60 students.


But imagine you are the class teacher. You need to maintain an **Attendance Register** for **60 students** over **30 days**.


If you use a normal array:


```c
int attendance[60];
```


This can only store attendance for **one day**. To store 30 days, you would need 30 different arrays. That is inefficient and messy.


**What we need is a Table (Grid):**


 * **Rows** representing Students.
 * **Columns** representing Days.


This structure—data stored in rows and columns—is exactly what a **Multidimensional Array (Matrix)** is designed for.


-----


## 2\. Introduction to 2D Arrays (Matrices)


A 2D array is essentially a **collection of 1D arrays** stacked on top of each other.


**Syntax:**


```c
datatype arrayName[rows][columns];
```


**Example:**


```c
int attendance[60][30];
```


 * `60`: Number of rows (Students).
 * `30`: Number of columns (Days).
 * `attendance[i][j]`: Accesses the attendance of **Student `i`** on **Day `j`**.


-----


## 3\. How 2D Indexing Works Internally


Even though we visualize it as a grid, computer memory is a single long tape. C stores 2D arrays row-by-row (**Row-Major Order**).


When you write `matrix[i][j]`:


 * **`i`**: Tells the computer which **Row** to go to.
 * **`j`**: Tells the computer which **Column** (step) to move to inside that row.


**The Pointer Formula:**
The computer calculates the address using:
$$Address = *(*(matrix + i) + j)$$


-----


## 4\. The Challenge: Matrix Multiplication


Adding matrices is easy (element + element). But **Multiplication** is tricky because it is **NOT** element-by-element.


### The Golden Rules


1. **Condition:** You can only multiply Matrix A and Matrix B if:
   $$Columns \ of \ A == Rows \ of \ B$$
2. **Result Size:** If A is $(m \times n)$ and B is $(n \times p)$, the result C will be $(m \times p)$.


### The Formula


To find one single value in the result matrix ($C[i][j]$), you must calculate the **Dot Product** of **Row `i` of A** and **Column `j` of B**.


$$C[i][j] = \sum ( A[i][k] \times B[k][j] )$$


[Image of matrix multiplication row by column visual]


-----


## 5\. The "Why Do We Need 3 Loops?" Explanation


This is the most common interview question regarding matrices.


**The Logic:**


1. **Loop 1 (`i`):** Selects a **Row** from Matrix A.
2. **Loop 2 (`j`):** Selects a **Column** from Matrix B.
3. **Loop 3 (`k`):** This is the **Calculator Loop**.


**Why the `k` loop?**
To fill just **ONE cell** in the result matrix, you have to multiply pairs of numbers and add them up.


 * Multiply $1^{st}$ element of Row A with $1^{st}$ element of Col B.
 * Multiply $2^{nd}$ element of Row A with $2^{nd}$ element of Col B.
 * Multiply $3^{rd}$ element of Row A with $3^{rd}$ element of Col B.
 * **Sum them all up.**


Since this involves repeated multiplication and addition, we need a third loop (`k`) to travel across the row of A and down the column of B simultaneously.


-----


## 6\. The Complete C Program


```c
#include <stdio.h>


int main() {
   int r1, c1, r2, c2;
   int i, j, k;


   // 1. Input Size of Matrix A
   printf("Enter rows and columns of Matrix A: ");
   scanf("%d %d", &r1, &c1);


   // 2. Input Size of Matrix B
   printf("Enter rows and columns of Matrix B: ");
   scanf("%d %d", &r2, &c2);


   // 3. The Condition Check
   if (c1 != r2) {
       printf("Error! Multiplication not possible.\n");
       printf("Columns of A (%d) must match Rows of B (%d).\n", c1, r2);
       return 0;
   }


   // 4. Declare Matrices
   int A[r1][c1], B[r2][c2], C[r1][c2];


   // 5. Input Matrix A
   printf("\nEnter elements of Matrix A:\n");
   for (i = 0; i < r1; i++) {
       for (j = 0; j < c1; j++) {
           scanf("%d", &A[i][j]);
       }
   }


   // 6. Input Matrix B
   printf("\nEnter elements of Matrix B:\n");
   for (i = 0; i < r2; i++) {
       for (j = 0; j < c2; j++) {
           scanf("%d", &B[i][j]);
       }
   }


   // 7. Initialize Result Matrix C to 0
   // This is crucial to avoid garbage values during addition
   for (i = 0; i < r1; i++) {
       for (j = 0; j < c2; j++) {
           C[i][j] = 0;
       }
   }


   // 8. The MAIN LOGIC (3 Nested Loops)
   for (i = 0; i < r1; i++) {           // Iterate over Rows of A
       for (j = 0; j < c2; j++) {       // Iterate over Columns of B
           for (k = 0; k < c1; k++) {   // Calculate Dot Product
               C[i][j] += A[i][k] * B[k][j];
           }
       }
   }


   // 9. Print the Result
   printf("\nResultant Matrix C (A x B):\n");
   for (i = 0; i < r1; i++) {
       for (j = 0; j < c2; j++) {
           printf("%d ", C[i][j]);
       }
       printf("\n");
   }


   return 0;
}
```


-----


## 7\. Line-by-Line Explanation


1. **Size Input & Check:** We immediately check `if(c1 != r2)`. If this is false, mathematics forbids multiplication, so we stop the program.
2. **Declarations:** Notice `C[r1][c2]`. The result matrix takes the **Rows of A** and the **Columns of B**.
3. **Initialization:** `C[i][j] = 0`. This is necessary because we use `+=` (addition) later. If `C` contains garbage values (like 32512), our sum will be wrong.
4. **The `k` Loop Logic:**
     * `A[i][k]`: Row `i` stays constant, but `k` moves across the columns of A.
     * `B[k][j]`: Column `j` stays constant, but `k` moves down the rows of B.
     * This perfectly matches the "Row by Column" multiplication rule.


-----


## 🛑 8. Common Mistakes Students Make


### ❌ Mistake 1: Multiplying Element-by-Element


 * **Wrong:** `C[i][j] = A[i][j] * B[i][j];`
 * **Why:** This is how you **add** matrices, not multiply them. Multiplication involves the whole row and whole column.


### ❌ Mistake 2: Forgetting to Initialize C to 0


 * **Wrong:** Skipping the loop that sets `C[i][j] = 0`.
 * **Result:** The logic `C[i][j] += ...` adds the new calculation to whatever garbage value was already in memory, giving incorrect answers.


### ❌ Mistake 3: Wrong Order of Dimensions


 * **Wrong:** Declaring `C[r2][c1]` or checking `if(r1 != c2)`.
 * **Correct:** Always remember the chain: $(m \times n) \cdot (n \times p) = (m \times p)$. The "inner" dimensions ($n$) must match.


### ❌ Mistake 4: Swapping Indexes in the `k` Loop


 * **Wrong:** `A[k][i] * B[j][k]`
 * **Why:** This reads the matrix in the wrong direction (Column-Major), which breaks the mathematical rule of matrix multiplication. Stick to `A[i][k] * B[k][j]`.

