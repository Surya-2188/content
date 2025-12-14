
#  LECTURE: Object-Oriented Programming (The Student Example)


**Prerequisite:** Students have understood the basic need for OOP from the Bank example.
**Goal:** Master the syntax, memory logic, and strict isolation of objects using a Student Management System.


-----


## 1️⃣ The Objective: Building a Student Management System


Now that we understand *why* we need OOP, let's apply it to a college scenario.
We need to create a software representation of a **Student**.


**The Requirements:**


1. We need a **Template** (Class) that defines what a student is.
     * Data: Name, Roll Number, Branch.
     * Behavior: Introduce themselves, Study.
2. We need to create **Multiple Real Students** (Objects) from this template.
     * Raju (CSE)
     * Rani (ECE)
3. We must prove that **Raju's data never mixes with Rani's data**.


-----


## 2️⃣ The Code: Building the Class Step-by-Step


### Step A: Defining the Blueprint


We start by writing the class. Remember, this is just the design. No memory is allocated yet.


```java
class Student {
   // 1. ATTRIBUTES (The Data)
   String name;
   int roll;
   String branch;


   // 2. BEHAVIOR (The Methods)
   void study() {
       System.out.println(name + " is studying in the " + branch + " lab.");
   }


   void showIdentity() {
       System.out.println("ID: " + roll + " | Name: " + name + " | Branch: " + branch);
   }
}
```


### Step B: The Execution (Creating Objects)


Now, inside the `main` method, we bring this blueprint to life.


```java
public class CollegeApp {
   public static void main(String[] args) {
      
       // 1. Create the First Student (Raju)
       Student s1 = new Student();
       s1.name = "Raju";
       s1.roll = 101;
       s1.branch = "CSE";


       // 2. Create the Second Student (Rani)
       Student s2 = new Student();
       s2.name = "Rani";
       s2.roll = 102;
       s2.branch = "ECE";


       // 3. Let them interact
       s1.showIdentity();
       s2.showIdentity();
   }
}
```


-----


## 3️⃣ THE CORE LOGIC: Memory Isolation


*(This is the most critical part of the lecture. Explain this slowly.)*


### The "Application Form" Analogy


Imagine the college office has a stack of **Blank Application Forms**.


 * The **Class (`Student`)** is the **Blank Form**.
 * The **Object (`s1`, `s2`)** is the **Filled Form**.


When Raju fills his form, he writes "CSE".
When Rani fills her form, she writes "ECE".


**Crucial Point:** If Raju takes an eraser and changes his branch to "Mech", **Rani's paper does NOT change.** Her paper is physically separate.


### How Java Handles This in Memory


[Image of Java class structure diagram]


1. **The `new` Keyword:**
   Every time you type `new Student()`, Java goes to the **Heap Memory** and creates a **Brand New Box**.
2. **Separate Variables:**
     * `s1` gets its own copy of `name`, `roll`, `branch`.
     * `s2` gets its own copy of `name`, `roll`, `branch`.
3. **Shared Logic:**
   They both use the *same* code for `showIdentity()`, but the method runs using *their specific box's data*.


-----


## 4️⃣ THE PROOF: Modifying One Object


Let's modify the code to prove that `s1` and `s2` are totally independent.


```java
public class CollegeApp {
   public static void main(String[] args) {
      
       // SETUP: Create two students
       Student s1 = new Student();
       s1.name = "Raju";
       s1.roll = 101;


       Student s2 = new Student();
       s2.name = "Rani";
       s2.roll = 102;


       // TEST 1: Print Initial State
       System.out.println("--- Before Change ---");
       s1.showIdentity(); // Prints Raju
       s2.showIdentity(); // Prints Rani


       // TEST 2: Modify ONLY Raju
       System.out.println("--- Modifying Raju's Roll Number ---");
       s1.roll = 999;


       // TEST 3: Check Both Again
       System.out.println("--- After Change ---");
       System.out.println("s1 Roll: " + s1.roll); // Changed to 999
       System.out.println("s2 Roll: " + s2.roll); // REMAINS 102
   }
}
```


**Conclusion:** Changing `s1` had **Zero Effect** on `s2`. This proves Memory Isolation.


-----


## 5️⃣ MEMORY VISUALIZATION


*(Draw this diagram on the board)*


When the code runs, Memory looks like this:


**HEAP MEMORY**


```text
-----------------------------       -----------------------------
|  Address: 500 (s1)        |       |  Address: 800 (s2)        |
|                           |       |                           |
|  name   : "Raju"          |       |  name   : "Rani"          |
|  roll   : 999             |       |  roll   : 102             |
|  branch : "CSE"           |       |  branch : "ECE"           |
-----------------------------       -----------------------------
```


 * `s1` points to Box 500.
 * `s2` points to Box 800.
 * They are physically in different locations.


-----


## 6️⃣ COMMON MISTAKES STUDENTS MAKE


*(A detailed guide to debugging lab errors)*


### ❌ 1. The "Ghost" Object (NullPointerException)


**Mistake:** Creating a variable but not the object.


```java
Student s1;
s1.name = "Raju"; // CRASH!
```


**Why:** You created a label (`s1`) but didn't create the memory box. `s1` is pointing to nothing (null).
**Correction:** Always use `new`: `Student s1 = new Student();`


### ❌ 2. The "Statue" Error (Static Context)


**Mistake:** Calling a method without an object.


```java
public static void main(String[] args) {
   showIdentity(); // ERROR
}
```


**Why:** The `showIdentity` method belongs to a Student. You are asking the *air* to show identity. Java doesn't know *which* student you mean.
**Correction:** `s1.showIdentity();`


### ❌ 3. The "Amnesia" Variable (Scope Shadowing)


**Mistake:** Re-declaring variables inside methods.


```java
void setRoll(int r) {
   int roll = r; // ERROR logic
}
```


**Why:** By writing `int roll` inside the method, you create a **temporary variable**. You are updating the temporary one, not the Student's actual roll number.
**Correction:** Use `roll = r;` or `this.roll = r;`.


### ❌ 4. File Naming Blunders


**Mistake:** Naming the file `Student.java` when the `main` method is in `CollegeApp`.
**Correction:** The file name must match the class that contains `public static void main`. Save as `CollegeApp.java`.


### ❌ 5. Integer Math Errors


**Mistake:** Using integers for things that need decimals (like CGPA).
**Correction:** Always use `double` or `float` for marks/percentages.


### ❌ 6. The "Main" inside Class


**Mistake:** Putting `main` inside the `Student` class.
**Why:** While technically allowed, it is bad design. A Blueprint (Student) shouldn't know how to run the app.
**Correction:** Separate them: `Student` class for data, `CollegeApp` class for `main`.


-----


## 7️⃣ FINAL SUMMARY


1. **Class:** The Template (The blank form).
2. **Object:** The Instance (The filled form).
3. **`new`:** The keyword that allocates separate memory.
4. **Isolation:** `s1` and `s2` share code logic, but **never** share data.
5. **Syntax:** Always `ClassName obj = new ClassName();`.


**Next Steps for Students:**
"Now that you can create objects manually, wouldn't it be nice if we could set the name and roll number *at the moment* we create the object, instead of writing 3 lines of code? In the next lecture, we will learn **Constructors** to do exactly that."

