# CHAPTER: Runtime Polymorphism — The MOST Important OOP Concept


Runtime polymorphism is often confusing not because of Java, but because students don't understand the **timeline** of a program. They confuse **checking** code with **running** code.


So, we begin from the absolute root.


-----


##  1️⃣ WHAT IS COMPILE TIME? (The Rule Checker Phase)


When you type:
`javac MyProgram.java`


The compiler behaves like a strict grammar teacher.


**What the compiler checks:**


 * **Syntax** (spelling, braces, semicolons)
 * **Existence** (whether variables exist)
 * **Signatures** (whether method names and parameters match)
 * **Types** (whether data types are valid)
 * **Inheritance** (whether rules are followed)


**What the compiler DOES NOT know:**


 * Which objects will actually be created.
 * Which overridden method will run.
 * Which path the program will take (user input).


** The Consequence:**
If something is wrong here → **Syntax Error**. The program does not start.


> ** IMPORTANT:** At compile time, **no objects exist**. Only the structure is checked.


-----


##  2️⃣ WHAT IS RUNTIME? (The Action Phase)


Runtime begins when you type:
`java MyProgram`


The JVM (Java Virtual Machine) now executes the code.


**At runtime:**


 * **Objects are born** (`new Student()`, `new HOD()`).
 * **Memory is allocated** in the RAM.
 * **Logic starts running**.
 * **User input is processed**.


** The Consequence:**
If something goes wrong here → **Runtime Exception** (e.g., `NullPointerException`, `ArithmeticException`).


> ** IMPORTANT:** At runtime, Java knows the **actual object** (Student, HOD, Faculty, etc.). This is the key to runtime polymorphism.


-----


## 3️⃣ FUNCTION CALL RESOLUTION (Compile Time vs Runtime)


Normally, method calls are linked at compile time.


**Example:**
`int x = sum(10, 20);`


The compiler sees:


1. A method called `sum`.
2. With correct parameters (`int`, `int`).
3. It links the call to the method immediately.


This is called **Compile-Time Binding** (or Static Binding).


-----


## 4️⃣ THE PROBLEM THAT FORCES RUNTIME DECISION


Consider this code:


```java
Person p;


if(role.equals("student")) {
   p = new Student();
} else {
   p = new HOD();
}
```


**At Compile Time:**


 * `p` is a `Person` reference.
 * The `Person` class HAS a `displayInfo()` method.
 * So the compiler does NOT complain.


**BUT the compiler cannot predict:**


 * Will `p` refer to a **Student**?
 * Will `p` refer to an **HOD**?
 * Will `p` refer to a **Guest**?


Objects are created at runtime, so the compiler **cannot** decide which method to bind.


**The Solution:** Java delays the decision to runtime. This is **Runtime Polymorphism**.


-----


## 5️⃣ REAL-WORLD SCENARIO — LOGIN SYSTEM


Imagine a college portal. People logging in may be: **Students, CRs, Faculty, HODs, or Guests.**


**The Challenge:**
Before login, the system does **NOT** know who the user will be. Yet, after login, the portal must call:


`user.displayInfo();`


And the correct dashboard must load.


**We CANNOT write:**


```java
if(user is student) { ... }
else if(user is hod) { ... }
```


**Why?**


 * It is hard to maintain.
 * It breaks when new roles (like "Dean") are added.
 * It violates OOP principles.


**Instead, one line must work:**
`user.displayInfo();`


The JVM must decide which version to call based on the **object**, not the reference.


-----


## 6️⃣ STEP-BY-STEP FLOW OF LOGIN SCENARIO


1. **User enters credentials.**
2. **System checks the database.**
3. **Depending on role, different objects are created:**
     * `new Student()`
     * `new HOD()`
     * `new Faculty()`
4. **All are stored in a Parent Reference:**
     * `Person loggedUser = login(role);`
5. **One single call is made:**
     * `loggedUser.displayInfo();`
6. **Resolution:**
     * JVM looks at the real object in memory → decides the version at runtime.


-----


## 7️⃣ COMPLETE RUNTIME POLYMORPHISM CODE


```java
class Person {
   void displayInfo() {
       System.out.println("General Person Info");
   }
}


class Student extends Person {
   void displayInfo() {
       System.out.println("Student Info Displayed");
   }
}


class HOD extends Person {
   void displayInfo() {
       System.out.println("HOD Info Displayed");
   }
}


public class LoginSystemDemo {
  
   // Returns different child objects based on input string
   public static Person login(String role) {
       if(role.equals("student"))
           return new Student();
       else if(role.equals("hod"))
           return new HOD();
       else
           return new Person();
   }


   public static void main(String[] args) {
      
       // The variable type is always 'Person'
       Person user1 = login("student");
       Person user2 = login("hod");
       Person user3 = login("guest");


       // The behavior changes based on the object
       user1.displayInfo();
       user2.displayInfo();
       user3.displayInfo();
   }
}
```


-----


## 8️⃣ OUTPUT


```text
Student Info Displayed
HOD Info Displayed
General Person Info
```


**This proves runtime polymorphism:**


 * Same method name.
 * Same reference type (`Person`).
 * Different objects → Different results.


-----


## 9️⃣ VISUAL PROOF: THE MEMORY MODEL


*(Draw this to explain HOW the JVM finds the method)*


**Scenario:** `Person p = new Student();`


```text
     STACK MEMORY                       HEAP MEMORY
  (References live here)             (Real Objects live here)


  +-----------------+                +---------------------------+
  |                 |   POINTS TO    |  Object: STUDENT          |
  |   Person p      | -------------> |---------------------------|
  |                 |                |  [Method Table]           |
  +-----------------+                |                           |
                                     |  displayInfo() {          |
                                     |    "Student Info"         |
                                     |  }                        |
                                     +---------------------------+
                                                 ^
                                                 |
  Code: p.displayInfo();                         |
                                                 |
  1. Compiler checks 'p':                        |
     "Does Person have displayInfo?"             |
      -> YES. (Compile Success)                  |
                                                 |
  2. Runtime (JVM) execution:                    |
     "I don't care that 'p' is Person."          |
     "I follow the arrow to the HEAP."           |
     "I run the STUDENT version."  --------------+
```


-----


## 1️⃣0️⃣ COMMON MISTAKES STUDENTS MAKE


**Mistake 1: Thinking method calls depend on reference type.**


 * *Correction:* In overriding, method calls depend on the **Object**, not the reference variable.


**Mistake 2: Trying to override static methods.**


 * *Correction:* Static methods are bound at compile time. They cannot be overridden (only hidden).


**Mistake 3: Changing method signature in child class.**


 * *Correction:* If you change parameters, it is **Overloading**, not Overriding.


**Mistake 4: Forgetting to override at all.**


 * *Correction:* If the method is not overridden, the Parent's method will run.


**Mistake 5: Expecting runtime polymorphism without inheritance.**


 * *Correction:* You need 3 things: Parent Class + Child Class + Overridden Method.


**Mistake 6: Using parent-only methods through child object.**


 * *Correction:* If `Person p = new Student()`, you cannot call methods that exist *only* inside Student. The compiler checks the Person reference first.


-----


## 1️⃣1️⃣ COMPARISON CHEAT SHEET


| Feature | Compile-Time Polymorphism | Runtime Polymorphism |
| :--- | :--- | :--- |
| **Also Known As** | Static / Early Binding | Dynamic / Late Binding |
| **Mechanism** | Method **Overloading** | Method **Overriding** |
| **When it happens** | Compilation (`javac`) | Execution (`java`) |
| **Who decides?** | The **Compiler** | The **JVM** |
| **Basis of decision** | Reference Type + Arguments | The **Actual Object** in Heap |
| **Flexibility** | Low | High (Scalable) |


-----


## 1️⃣2️⃣ FINAL NOTEBOOK SUMMARY


**Compile Time:**
Checks source code rules. No objects exist yet. Syntax errors occur here.


**Runtime:**
Objects are created. Logic executes. Runtime exceptions occur here.


**Runtime Polymorphism:**
Requires: **Inheritance + Overriding + Parent Reference + Child Object**.
Method call is decided at **runtime** by the JVM based on the actual object.


**Best One-Line Definition:**


> Runtime Polymorphism is when the method to be executed is chosen at runtime based on the **actual object**, even though the reference is of the **parent type**.

