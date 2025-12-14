


#  LECTURE: The `static` Keyword — Shared Memory & Class Variables


**Goal:** Understand how to share data between objects and why `main` is static.


-----


##  1️⃣ THE REAL-WORLD PROBLEM — COUNTING CUSTOMERS


Imagine you are developing a banking system.
Every time a new **Customer** account is created, the bank manager wants to know:
**"How many total customers do we have in the bank right now?"**


For example:


 * Customer 1 created → Total = 1
 * Customer 2 created → Total = 2
 * Customer 3 created → Total = 3


This count is **Global**. It is common to the entire bank, not just one person.


-----


##  2️⃣ BEGINNER'S FIRST IDEA (WHY IT FAILS)


Students usually try to store the count inside the object.


```java
class Customer {
   String name;
   int count = 0;   // ❌ WRONG: Each object gets its OWN count
  
   Customer() {
       count++; // Increases the count inside THIS object only
   }
}
```


**Why does this fail?**
Recall **Memory Isolation**: "Every object gets its own separate memory."


 * **Raju (c1)** has his own `count` variable. It starts at 0, becomes 1.
 * **Rani (c2)** has her own `count` variable. It starts at 0, becomes 1.
 * **Result:** The program prints "Total Customers: 1" every single time. They are not sharing the number\!


**Conclusion:**
Object variables (Instance Variables) belong to the specific object. We need a variable that belongs to the **Entire Class**.


-----


## 3️⃣ THE SOLUTION: `static` VARIABLE


We need a "Notice Board" on the wall that everyone can see and update, rather than a "Private Notebook" in everyone's pocket.


```java
static int count = 0;
```


**What does `static` mean?**


1. **Shared Memory:** Only **ONE** copy of the variable is created in memory.
2. **Class Level:** It belongs to the **Class**, not the individual objects.
3. **Universal Update:** If Object 1 changes it, Object 2 sees the change immediately.


Because it belongs to the class, we call it a **Class Variable**.


-----


##  4️⃣ FULL WORKING PROGRAM (WITH STATIC)


Here is the correct code to count customers.


```java
class Customer {


   // 1. Instance Variables (Unique to each object)
   String name;
   int id;


   // 2. Static Variable (Shared by ALL objects)
   static int count = 0;


   // Constructor
   Customer(String n, int i) {
       name = n;
       id = i;
      
       // Increment the SHARED counter
       count++;
      
       System.out.println("✅ Customer Created: " + name +
                          " | Total Customers So Far: " + count);
   }
}


public class StaticDemo {
   public static void main(String[] args) {
       // Watch the count increase globally
       Customer c1 = new Customer("Raju", 101);  // Count becomes 1
       Customer c2 = new Customer("Rani", 102);  // Count becomes 2
       Customer c3 = new Customer("Kiran", 103); // Count becomes 3
   }
}
```


-----


##  5️⃣ LINE-BY-LINE EXPLANATION


**`static int count = 0;`**


 * This variable is created in a special memory area (Static/Method Area), not inside the object's heap memory.
 * It initializes only once.


**`count++;` (Inside Constructor)**


 * Every time `new Customer()` is called, this line runs.
 * Since `count` is shared, **Raju** updates it to 1. **Rani** sees 1 and updates it to 2. **Kiran** sees 2 and updates it to 3.


**`Customer c1 = new Customer(...);`**


 * This triggers the constructor, which triggers the increment.


-----


## 6️⃣ STATIC METHODS (CLASS METHODS)


Sometimes, we want to ask: *"What is the total count?"* without creating a new customer.


If we write a normal method, we need an object (`c1.showTotal()`). But what if no customers exist yet? We should be able to check the count even if it is 0.


**The Solution:**


```java
static void showTotalCustomers() {
   System.out.println("Total customers: " + count);
}
```


**How to call it:**
We use the **Class Name**, not the object name.


```java
Customer.showTotalCustomers();
```


-----


## 7️⃣ WHEN TO USE STATIC? (The Rules)


### A. When to make a Variable Static?


Use static when the value is **Common/Shared** for everyone.


 * ✔ Total Number of Students (Common)
 * ✔ College Name (Common)
 * ✔ Bank Interest Rate (Common)
 * ❌ Student Name (Unique - NOT Static)


### B. When to make a Method Static?


Use static when the method **does not need object data** (like `name` or `id`) to work.


 * ✔ `Math.sqrt()` (Calculates square root, doesn't need an object)
 * ✔ `showTotalCount()` (Only needs the static count)


-----


##  8️⃣ THE BIG INTERVIEW QUESTION: `public static void main`


Why is the `main` method always `static`?


**The Logic (Chicken and Egg Problem):**


1. To run a normal method, you need an Object (`obj.method()`).
2. To create an Object, your program must be running (inside `main`).
3. **Problem:** If `main` was not static, Java would need an object to run `main`. But `main` is where we create objects\!


**The Solution:**
Java makes `main` **static**.


 * This allows the JVM to call `Customer.main()` **without** creating a Customer object first.
 * It is the entry point that runs before any objects are born.


-----


## 9️⃣ COMMON MISTAKES STUDENTS MAKE



1. ❌ **Making Unique Data Static:**
     * `static String name;` -\> If you do this, changing Raju's name to "Kiran" will change **everyone's** name to Kiran. Never make unique attributes static.
2. ❌ **Accessing Static via Object:**
     * `c1.count` works, but it is confusing.
     * **Correct:** Always use Class Name: `Customer.count`.
3. ❌ **Static accessing Non-Static:**
     * Inside a static method (`main`), you cannot say `System.out.println(name);`.
     * **Why:** `main` belongs to the class. `name` belongs to an object. The class doesn't know *which* object's name you want.
4. ❌ **Confusing `static` with `final`:**
     * `static` means Shared.
     * `final` means Constant (Cannot change).
     * `static final` means Shared Constant (e.g., `Math.PI`).


-----


##  🔟 FINAL SUMMARY


 * **Instance Variable:** One copy per object (e.g., `name`).
 * **Static Variable:** One copy per Class (e.g., `count`).
 * **Static Method:** Belong to the class, called using `ClassName.method()`.
 * **Main Method:** Must be static so JVM can run it without objects.
 * **Key Use Case:** Counters, Shared Settings (College Name), Utility Functions (Math).

