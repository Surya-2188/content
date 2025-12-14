


# Topic: Getting User Input in Java (The Scanner Class)


## 1\. The Big Difference: C vs. Java


 * **In C:** You simply write `#include <stdio.h>` and use `scanf("%d", &a)`.
 * **In Java:** Input is not automatic. Java provides a special tool (a Class) called **`Scanner`**.
     * Think of `Scanner` as a sophisticated "listening robot."
     * You have to **import** the robot's instructions.
     * You have to **build** (create an object of) the robot.
     * Then, you tell the robot *what* to listen for (int, float, string).


## 2\. The 3 Steps to Take Input


To take input in Java, you must always follow these three steps:


### Step 1: Import the Package (The Toolbox)


The `Scanner` class lives inside a package (folder) called `java.util`. We must tell the compiler to look there.


```java
import java.util.Scanner;
```


### Step 2: Create the Scanner Object (Build the Robot)


This is the most important line in Java input.


```java
Scanner sc = new Scanner(System.in);
```


**Let's decode this using our previous analogy:**


 * **`Scanner`**: The Class (The **Type**).
 * **`sc`**: The Object (The **Variable** name). You can name it anything (like `input`, `scan`, `myScanner`).
 * **`new Scanner(...)`**: This command creates the object in memory.
 * **`System.in`**: This represents the **Standard Input** (Keyboard). We are telling the Scanner, "Please listen to the Keyboard."


### Step 3: Use Input Methods (Commands)


Unlike `scanf` where you use `%d` or `%f`, in Java, the `Scanner` object has specific **methods** for each data type:


| Data Type | C Code | Java Method |
| :--- | :--- | :--- |
| **Integer** | `scanf("%d", &a)` | `int a = sc.nextInt();` |
| **Float** | `scanf("%f", &f)` | `float f = sc.nextFloat();` |
| **String (Word)** | `scanf("%s", s)` | `String s = sc.next();` |
| **String (Line)** | `gets(s)` | `String s = sc.nextLine();` |


-----


## 3\. The Java Program: Adding Two Numbers


Here is a complete program that asks the user for two numbers and adds them.


```java
import java.util.Scanner;  // Step 1: Import


public class AddNumbers {
   public static void main(String[] args) {
      
       // Step 2: Create Scanner Object
       Scanner sc = new Scanner(System.in);
      
       // Step 3: Take Input
       System.out.println("Enter first number: ");
       int a = sc.nextInt();  // Reads an integer
      
       System.out.println("Enter second number: ");
       int b = sc.nextInt();  // Reads another integer
      
       // Calculate
       int sum = a + b;
      
       // Output
       System.out.println("Sum is: " + sum);
   }
}
```


-----


## 4\. Key Explanations (Connecting to C)


### Why no `&a`?


In C, `scanf("%d", &a)` needs the address (`&`) to modify the variable.
In Java, `sc.nextInt()` **returns** the value directly.
So we write:
`int a = sc.nextInt();`
(Take the integer from the scanner and put it into `a`).


### `System.in` vs `System.out`


 * **`System.out`**: Connected to your Screen (Display). used for `println`.
 * **`System.in`**: Connected to your Keyboard. Used for `Scanner`.


-----


## 🛑 5. Common Mistakes Students Make


### ❌ Mistake 1: Forgetting to Import


 * **Error:** "Cannot find symbol: class Scanner".
 * **Fix:** You must write `import java.util.Scanner;` at the very top.


### ❌ Mistake 2: Mixing `next()` and `nextLine()`


 * **Context:** `sc.next()` reads only **one word** (stops at space). `sc.nextLine()` reads the **whole sentence** (stops at Enter key).
 * **The Trap:** If you use `nextInt()` and then immediately try to use `nextLine()`, it often skips the input. (This happens because `nextInt` leaves the "Enter" key press in the buffer).


### ❌ Mistake 3: Trying to create Scanner without `System.in`


 * **Wrong:** `Scanner sc = new Scanner();`
 * **Why:** The Scanner is deaf\! It doesn't know *where* to listen. You must pass `System.in`.


-----


