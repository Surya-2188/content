

-----


# Topic: Multidimensional Arrays (2D Arrays)


## 1\. Recap: What We Know


Earlier, we learned that **1D Arrays** are perfect for storing a list of items.


 * **Example:** Storing marks of 60 students.
 * **Structure:** One continuous line of memory.
 * **Access:** `arr[i]`


This works great for a single list. But life isn't always a single list\!


-----


## 2\. The Problem: The "Attendance Register" Scenario


Imagine you are a class teacher. You need to handle the attendance of **60 students** for **30 days**.


If you use a 1D array:


```c
int presentStatus[60];
```


This can only store attendance for **one day**.


If you want to store data for 30 days, you would need 30 different arrays (`day1[]`, `day2[]`... `day30[]`). **This is messy and impossible to manage.**


**What we actually need is a Table:**


| Roll No | Day 1 | Day 2 | Day 3 | ... | Day 30 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Student 1** | P | P | A | ... | P |
| **Student 2** | P | A | P | ... | P |
| **...** | ... | ... | ... | ... | ... |
| **Student 60** | A | P | P | ... | A |


This is not a line anymore. It has **Rows** (Students) and **Columns** (Days). A normal 1D array cannot handle this.


-----


## 3\. The Solution: Multidimensional Arrays


To store table-like data, C provides **Multidimensional Arrays**.


 * **1D Array:** A list (Line).
 * **2D Array:** A Table (Matrix) with Rows and Columns.
 * **3D Array:** Multiple tables stacked together (like a book with many pages).


For most programming tasks, we use **2D Arrays**.


-----


## 4\. Syntax of a 2D Array


To declare a 2D array, we need two sizes: one for rows, one for columns.


```c
datatype arrayName[rows][columns];
```


**Example for our Attendance Register:**


```c
int attendance[60][30];
```


 * **60 Rows:** Representing 60 Students.
 * **30 Columns:** Representing 30 Days.


**How to access specific data?**


 * `attendance[0][0]` $\rightarrow$ Roll No 1, Day 1
 * `attendance[5][2]` $\rightarrow$ Roll No 6, Day 3
 * `attendance[59][29]` $\rightarrow$ Last Student, Last Day


-----


## 5\. How to Visualize a 2D Array


Think of it as a grid.


```c
int a[3][4]; // 3 Rows, 4 Columns
```


**Visual Diagram:**


```text
        Col 0   Col 1   Col 2   Col 3
Row 0:  [ a00 ] [ a01 ] [ a02 ] [ a03 ]
Row 1:  [ a10 ] [ a11 ] [ a12 ] [ a13 ]
Row 2:  [ a20 ] [ a21 ] [ a22 ] [ a23 ]
```


Each element is identified by its coordinate: `a[row][col]`.


-----


## 6\. How It Is Stored in Memory (Row-Major Order)


This is a very important concept. Even though we imagine it as a square grid, **computer memory is always a single long line.**


C stores 2D arrays row by row. This is called **Row-Major Order**.


**It stores:**


1. All of Row 0 (`a[0][0]` to `a[0][3]`)
2. Then immediately, all of Row 1 (`a[1][0]` to `a[1][3]`)
3. Then Row 2...


So in RAM, it looks like this:
`[Row 0] [Row 1] [Row 2]`


-----


## 7\. The Internal Math: Pointer Logic


Just like 1D arrays, C uses pointers and math to find the address.


**1D Formula:** `a[i] = *(a + i)`


**2D Formula:**


```c
a[i][j] = *(*(a + i) + j)
```


**Let's decode this:**


1. **`a + i`**: Go to the starting address of the **i-th Row**.
2. **`*(a + i)`**: Enter that row.
3. **`+ j`**: Move **j** steps forward inside that row (to find the column).
4. **`*`**: Open that box and get the value.


So, `attendance[10][5]` means: Go to Row 10, then move to Column 5.


-----


## 8\. Working with 2D Arrays (Nested Loops)


Since we have Rows and Columns, we need **two loops** to handle the data.


 * **Outer Loop:** Controls the Rows (Students).
 * **Inner Loop:** Controls the Columns (Days).


**Example: Taking Input for a 2x2 Matrix**


```c
int matrix[2][2];
int i, j;


// Outer Loop: Rows
for(i = 0; i < 2; i++) {
   // Inner Loop: Columns
   for(j = 0; j < 2; j++) {
       printf("Enter value for row %d, col %d: ", i, j);
       scanf("%d", &matrix[i][j]);
   }
}
```


**How it runs:**


1. `i=0` (Row 0) starts.
     * `j=0`: Fill `matrix[0][0]`
     * `j=1`: Fill `matrix[0][1]`
2. Inner loop finishes. `i` becomes 1 (Row 1).
     * `j=0`: Fill `matrix[1][0]`
     * `j=1`: Fill `matrix[1][1]`


-----
