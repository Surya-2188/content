# Transpose of a Matrix





## 1\. What Is the Transpose of a Matrix?


The transpose of a matrix is obtained by **flipping** it over its diagonal. In simple terms, we turn **Rows into Columns** and **Columns into Rows**.


**Visual Example:**
If Matrix **A** ($2 \times 3$) is:


```text
 1  2  3  (Row 0)
 4  5  6  (Row 1)
```


Then its Transpose **T** ($3 \times 2$) becomes:


```text
 1  4
 2  5
 3  6
```


Notice how the horizontal line `1 2 3` became a vertical line.


-----


## 2\. The Logic: Swapping Indices


This is the core concept.


 * In the original matrix, a number is at position `A[i][j]` (Row `i`, Column `j`).
 * In the transpose matrix, that same number moves to `T[j][i]` (Row `j`, Column `i`).


**The Formula:**
$$T[j][i] = A[i][j]$$


-----


## 3\. The C Program
We will use this program to understand how to **manipulate indices** (`i` and `j`) to rearrange data in a 2D Array.

```c
#include <stdio.h>


int main() {
   int rows, cols, i, j;


   // 1. Input size of matrix
   printf("Enter number of rows: ");
   scanf("%d", &rows);


   printf("Enter number of columns: ");
   scanf("%d", &cols);


   // 2. Declare Matrix A (Original) and Matrix T (Transpose)
   // IMPORTANT: Dimensions of T must be swapped!
   int A[rows][cols];
   int T[cols][rows];


   // 3. Input Matrix A
   printf("\nEnter elements of Matrix A:\n");
   for(i = 0; i < rows; i++) {
       for(j = 0; j < cols; j++) {
           scanf("%d", &A[i][j]);
       }
   }


   // 4. Compute Transpose (The Logic)
   for(i = 0; i < rows; i++) {
       for(j = 0; j < cols; j++) {
           // Assign Element at [i][j] to position [j][i]
           T[j][i] = A[i][j];
       }
   }


   // 5. Print Transpose Matrix
   // Note: We loop through 'cols' first because T has 'cols' number of rows
   printf("\nTranspose of Matrix A:\n");
   for(i = 0; i < cols; i++) {
       for(j = 0; j < rows; j++) {
           printf("%d ", T[i][j]);
       }
       printf("\n");
   }


   return 0;
}
```


-----


## 4\. Line-by-Line Explanation


### Step 1: Declaration (The Flip)


```c
int A[rows][cols];
int T[cols][rows];
```


This is the most critical step.
If Matrix A is **2 Rows** and **3 Columns** ($2 \times 3$).
Matrix T must be declared as **3 Rows** and **2 Columns** ($3 \times 2$).
If you declare them with the same size, your program will crash for rectangular matrices.


### Step 2: The Logic Loop


```c
for(i = 0; i < rows; i++) {
   for(j = 0; j < cols; j++) {
       T[j][i] = A[i][j];
   }
}
```


We walk through the original matrix A. For every number we find at `[i][j]`, we throw it into the transpose matrix at the swapped position `[j][i]`.


### Step 3: Printing


```c
for(i = 0; i < cols; i++) { ... }
```


Why start with `cols`?
Because the Transpose Matrix `T` has `cols` number of rows. We are printing `T`, so we must follow `T`'s dimensions.


-----


## 🔍 5. Dry Run (Let's Trace It)


Imagine `rows = 2`, `cols = 3`.
Matrix A:
`[10, 20, 30]`
`[40, 50, 60]`


**Iteration 1 (`i=0, j=1`):**


 * We pick number **20** from A at `[0][1]`.
 * We place it into T at `[1][0]`.


**Iteration 2 (`i=1, j=2`):**


 * We pick number **60** from A at `[1][2]`.
 * We place it into T at `[2][1]`.


The logic automatically rearranges every number to its new home\!


-----


## 🛑 6. Common Mistakes Students Make


### ❌ Mistake 1: Wrong Dimensions for Transpose


 * **Wrong:** `int T[rows][cols];`
 * **Why:** If A is $2 \times 5$, T must be $5 \times 2$. If you don't swap them, T won't fit the data.
 * **Correct:** `int T[cols][rows];`


### ❌ Mistake 2: Copying instead of Transposing


 * **Wrong:** `T[i][j] = A[i][j];`
 * **Why:** This just creates a duplicate copy of A. You must swap the indices.
 * **Correct:** `T[j][i] = A[i][j];`


### ❌ Mistake 3: Confusing Printing Loops


 * **Wrong:** Printing T using `i < rows`.
 * **Why:** T has a different number of rows than A. You must use the dimensions of T (which are `cols` first).


### ❌ Mistake 4: Thinking Only Square Matrices Can Be Transposed


 * **Clarification:** Students often think transpose only works for $3 \times 3$ or $2 \times 2$. No\! It works for any size ($1 \times 10$, $5 \times 2$, etc.), as long as you swap the dimensions correctly.


-----


## 7\. Summary


1. **Transpose** = Swap Rows and Columns.
2. **Formula:** $T[j][i] = A[i][j]$.
3. **Dimensions:** If A is $R \times C$, then T is $C \times R$.
4. **Use Case:** Essential for graphics, physics simulations, and solving equations.

