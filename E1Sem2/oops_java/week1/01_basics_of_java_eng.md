

-----


# Topic: Structure of a Java Program


We will break down how a Java program works, why it is different from C, and understand the specific meaning of every word in `public static void main`.


-----

## 1\. What is a Java Program?


A Java program is simply a set of instructions given to the computer to perform a task.


 * We write these instructions in English-like statements (Source Code).
 * We save the file with a **`.java`** extension (e.g., `HelloWorld.java`).


However, the computer does not understand Java directly. It needs a translator.


-----


## 2\. Why Do We Need Bytecode?


In C language, the compiler turns code directly into machine code (specific to your operating system).
In Java, there is a special intermediate step called **Bytecode**.


**The Process:**


1. **Source Code:** You write `HelloWorld.java`.
2. **Compiler (`javac`):** Checks for errors and converts it into **Bytecode** (`HelloWorld.class`).
3. **JVM (Java Virtual Machine):** The JVM takes this bytecode and converts it into machine code that the computer understands.


**Why this extra step?**


 * **Platform Independence:** You can run the `.class` file on Windows, Mac, or Linux without changing a single line of code. ("Write Once, Run Anywhere").
 * **Security:** The code runs inside the JVM (a safe sandbox), not directly on the hardware.


-----


## 3\. Classes, Methods, and Objects (The Big Shift from C)


### A) Functions in C vs. Methods in Java


In C, you can write functions like `printf()` or `add()` anywhere. They exist independently.


**In Java, the rule is strict:**


> "Every function (Method) and every variable MUST live inside a **Class**."


You cannot have a method floating around freely. It must be inside a Class.


### B) Class = The Blueprint


Think of a Class as a container or a blueprint.


 * It holds **Data** (Variables).
 * It holds **Behavior** (Methods).


**Example:**
When you write `System.out.println("Hello");`


 * **`System`** is the Class.
 * **`out`** is an Object (inside that class).
 * **`println`** is the Method (belonging to that object).
 * *Proof that methods must belong to a class or object\!*


### C) The "Int vs. Variable" Analogy (The Key Concept)


This is the easiest way to understand objects.


In C or Java, you cannot use the data type `int` directly to store number 10. You must create a **variable**.


```c
int a;    // 'int' is the Type, 'a' is the Variable
a = 10;
```


**Similarly in Java:**
A **Class** is just a user-defined **Type**. You cannot use the Class directly to do work. You must create a **variable** of that Class type. We call this variable an **Object**.


```java
Car myCar;   // 'Car' is the Class (Type), 'myCar' is the Object (Variable)
```


**Comparison Table:**


| Concept | Primitive Example | Class/Object Example |
| :--- | :--- | :--- |
| **The Type** | `int` | `Car` (The Class) |
| **The Variable** | `a` | `myCar` (The Object) |


> **Conclusion:** Just as `a` is a variable of type `int`, `obj` is a variable of type `Class`.


-----


## 4\. Decoding `public static void main`


When the Java program starts, no Objects exist yet. But we need a way to start the program. This is why `main` is written this way:


```java
public static void main(String[] args)
```


1. **`public`**: Access Modifier. It means this method can be accessed by anyone (specifically, the JVM needs to access it to start the program).
2. **`static`**: This is the most important part.
     * Normally, you need an Object to call a method.
     * **Static** means: "You can call this method **without** creating an Object."
     * Since the program is just starting, we have no objects, so `main` *must* be static.
3. **`void`**: The main method performs a task (running the program) but does not return any value.
4. **`main`**: The specific name the JVM looks for to start execution.
5. **`String[] args`**: Command-line arguments. It allows us to pass information to the program when we start it (we will learn this later).


-----


## 5\. The Hello World Program


Here is the complete code combining these concepts.


```java
public class HelloWorld {
   public static void main(String[] args) {
       System.out.println("Hello World");
   }
}
```


**Line-by-Line Explanation:**


 * **`public class HelloWorld {`**
     * We define a class named `HelloWorld`. Everything must live inside this block.
 * **`public static void main(String[] args) {`**
     * This is the entry point. The JVM calls this method to start the app. It is static so it can run without an object.
 * **`System.out.println("Hello World");`**
     * **System**: The class provided by Java.
     * **out**: The object inside System that handles output.
     * **println**: The method that prints the text to the console.
 * **`}`**
     * Closing braces to end the method and the class.


-----

