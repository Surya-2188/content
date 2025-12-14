

# FULL LECTURE: Introduction to Object-Oriented Programming (The Bank Example)



## 1️⃣ The Starting Point — Understanding the Real Problem


Imagine you are hired to build software for a bank.
Your system must do the following for a customer:


1. Store their **Name**.
2. Store their **Account Balance**.
3. **Greet** the customer when they log in.
4. Allow them to **Deposit** money.
5. Allow them to **Withdraw** money.


It seems simple, but let’s look at why our old programming methods fail.


-----


## 2️⃣ Why a Single Variable Fails


If we try to represent a customer using a standard variable:
`int customer1;`


**Why this fails:**
A customer is not just a number. A real customer has a Name (String), an Address (String), a Balance (double), and an ID (int). A single variable can only hold **one** piece of data. You cannot stuff a whole customer into an integer.


-----


## 3️⃣ Why Arrays Fail


Students often ask: *"Sir, why not use an array?"*
`int customers[] = new int[100];`


**Why this fails:**
Arrays have two strict rules:


1. **Same Type:** An array can store 100 integers OR 100 strings. It cannot store a Name (String) AND a Balance (double) together.
2. **No Behavior:** An array is just a storage box. You cannot teach an array how to "Deposit" or "Withdraw." It cannot hold functions.


-----


## 4️⃣ Why C (Procedural Programming) Fails


[Image of procedural programming vs object oriented programming]


In C (Procedural Programming), we are forced to separate data and behavior.


 * **Data** is stored in variables/structs.
 * **Behavior** is stored in functions.


Your code ends up looking like this:


```c
char name1[] = "Rahul";
double balance1 = 5000.0;
deposit(balance1, 1000); // We have to pass data into functions manually.
```


**The Major Disconnect:**
In the real world, entities (things) have their data and their behavior **bound together**.


 * **A Dog:** Has a breed (Data) + Can bark (Behavior).
 * **A Car:** Has a color (Data) + Can drive (Behavior).


If C cannot bind data and behavior together, it is not modeling the real world correctly.


-----


## 5️⃣ The Solution — OBJECT ORIENTED PROGRAMMING (OOP)


OOP introduces a new rule:


> **"Treat every real-world entity as a separate object in your code."**


To do this, we use two main concepts:


### A. The Class (The Blueprint)


Think of a **Class** as a **House Blueprint**.


 * It is just a drawing on paper.
 * It defines where the kitchen is and how big the bedroom is.
 * **Crucial Point:** You cannot live in a blueprint. It occupies no real space.


### B. The Object (The House)


Think of an **Object** as the **Actual House** built from that blueprint.


 * It takes up physical space (Memory).
 * You can actually live in it.
 * You can build 100 houses (Objects) from 1 blueprint (Class).


-----


## 6️⃣ Building the Program: STEP 1 (The Foundation)


We will not build the whole program at once. We will start with the absolute minimum to make it work.


**Goal:** Create a Customer that has a **Name** and can **Greet** us.


### The Code (Step 1)


```java
// THE BLUEPRINT
class Customer {
   // Data (What the customer HAS)
   String name;


   // Behavior (What the customer DOES)
   void greetCustomer() {
       System.out.println("Welcome " + name + " to ABC Bank!");
   }
}


// THE EXECUTION
public class BankDemo {
   public static void main(String[] args) {
       // 1. Create the Object (Build the house)
       Customer c1 = new Customer();


       // 2. Set the Data
       c1.name = "Rahul";


       // 3. Call the Behavior
       c1.greetCustomer();
   }
}
```


### Detailed Explanation of Step 1


1. `class Customer { ... }`: We are defining a new type of entity called a "Customer".
2. `String name;`: Every customer object created from this blueprint will have its own variable called `name`.
3. `void greetCustomer() { ... }`: This is a method. It is an action that the customer can perform. Notice it uses `name` directly—it knows its own name\!
4. `Customer c1 = new Customer();`:
     * `Customer c1`: Creates a variable label (Reference).
     * `new Customer()`: Goes to memory (Heap), allocates space for a customer, and links it to `c1`.
5. `c1.greetCustomer();`: We use the dot operator (`.`) to access the behavior inside the object.


-----


## 7️⃣ Building the Program: STEP 2 (Adding Money)


A customer is useless without money. Let's add a **balance** and a way to **deposit**.


**Changes to make:**


1. Add a `double balance` variable.
2. Add a `deposit` method that takes an amount and adds it to the balance.


### The Code (Step 2 Update)


```java
class Customer {
   String name;
   double balance; // <--- NEW DATA


   void greetCustomer() {
       System.out.println("Welcome " + name);
   }


   // <--- NEW BEHAVIOR
   void deposit(double amount) {
       balance = balance + amount;
       System.out.println("Deposited: " + amount);
   }
}


public class BankDemo {
   public static void main(String[] args) {
       Customer c1 = new Customer();
       c1.name = "Rahul";
       c1.balance = 5000; // Giving initial money


       c1.deposit(2000);  // Calling the new feature
       // Now, inside the object, balance is 7000
   }
}
```


-----


## 8️⃣ Building the Program: STEP 3 (Adding Logic)


Now we need to withdraw money. But we can't just subtract\! What if the customer has 5000 and tries to take 9000? We need **logic** inside the object.


**Changes to make:**


1. Add a `withdraw` method.
2. Include an `if-else` condition to check funds.


### The Code (Step 3 Update)


```java
   // <--- NEW BEHAVIOR
   void withdraw(double amount) {
       if (amount <= balance) {
           balance = balance - amount;
           System.out.println("Withdrawn: " + amount);
       } else {
           System.out.println("Failed: Insufficient Balance!");
       }
   }
```


*Note: We add this method inside the `Customer` class.*


-----


## 9️⃣ Building the Program: STEP 4 (Returning Values - The New Feature)


So far, our methods (`deposit`, `withdraw`) are `void`. They do the work and stay silent.
But sometimes, we need the object to **calculate an answer and give it back to us.**


**The Scenario:**
The customer asks: *"If I leave this money for 1 year at 4% interest, how much profit do I get?"*


**The Solution:**
We need a method that:


1. Accepts the Interest Rate.
2. Calculates the value.
3. **Returns** the value (does not just print it).


### The Code (Step 4 Update)


```java
   // <--- NEW BEHAVIOR (Returns a double)
   double getEstimatedInterest(double rate) {
       double interest = (balance * rate) / 100;
       return interest; // Gives this value back to main()
   }
```


-----


## 🔟 The Final Complete Code (Copy for Lab)


Here is the final version with all features (Greeting, Deposit, Withdraw, Interest) combined.


[Image of Java class structure diagram]


```java
// 1. THE CLASS (The Blueprint)
class Customer {
   // DATA
   String name;
   double balance;


   // BEHAVIOR 1: GREET
   void greetCustomer() {
       System.out.println("---------------------------");
       System.out.println("Welcome " + name + " to ABC Bank!");
       System.out.println("Current Balance: " + balance);
       System.out.println("---------------------------");
   }


   // BEHAVIOR 2: DEPOSIT (Modifies Data)
   void deposit(double amount) {
       balance = balance + amount;
       System.out.println("✅ Deposited: " + amount);
   }


   // BEHAVIOR 3: WITHDRAW (Contains Logic)
   void withdraw(double amount) {
       if (amount <= balance) {
           balance = balance - amount;
           System.out.println("✅ Withdrawn: " + amount);
       } else {
           System.out.println("❌ Error: Insufficient Balance!");
       }
   }


   // BEHAVIOR 4: ESTIMATE INTEREST (Returns Data)
   double getEstimatedInterest(double rate) {
       double interest = (balance * rate) / 100;
       return interest;
   }
}


// 2. THE MAIN CLASS (The Execution)
public class BankDemo {
   public static void main(String[] args) {
      
       // Step A: Create the Object
       Customer c1 = new Customer();
      
       // Step B: Initialize Data
       c1.name = "Rahul";
       c1.balance = 5000;


       // Step C: Test Greeting
       c1.greetCustomer();
      
       // Step D: Test Transactions
       c1.deposit(2000);   // Balance becomes 7000
       c1.withdraw(3000);  // Balance becomes 4000
       c1.withdraw(10000); // Should fail


       // Step E: Test Interest (Receiving the returned value)
       double profit = c1.getEstimatedInterest(4.0);
       System.out.println("📈 Estimated Interest Profit: " + profit);
      
       // Final Balance Check
       System.out.println("Final Balance in System: " + c1.balance);
   }
}
```


-----


## 1️⃣1️⃣ EXPANDED: Common Mistakes Students Make


*(It is highly recommended to write these on the board before the lab starts)*


### ❌ 1. The "Null Pointer" Crash


**Mistake:** Creating the variable but not the object.


```java
Customer c1;
c1.deposit(500); // ERROR!
```


**Why:** `c1` is just an empty label. It doesn't point to any memory yet.
**Fix:** Always use `new`: `Customer c1 = new Customer();`


### ❌ 2. The Filename Mismatch (Compilation Error)


**Mistake:** Saving the file as `Customer.java`.
**Why:** The file must be named after the class that contains the `public static void main`. In our code, that is `BankDemo`.
**Fix:** Save file as `BankDemo.java`.


### ❌ 3. "The Disappearing Data" (Scope Confusion)


**Mistake:** Redeclaring variables inside methods.


```java
void deposit(double amount) {
   double balance = 0; // Mistake! This creates a NEW temporary variable.
   balance = balance + amount;
   // The real object's balance is never touched!
}
```


**Fix:** Do not redeclare `double balance` inside the method. Use the class variable directly.


### ❌ 4. Integer Division Trap (Logic Error)


**Mistake:** Calculating interest using integers.


```java
// If variables were int
int interest = (balance * rate) / 100;
// If result is 0.9, Java makes it 0.
```


**Fix:** Always use `double` for currency and math calculations to keep the decimal precision.


### ❌ 5. Ignoring the Return Value


**Mistake:** Calling a return-type method like a void method.


```java
c1.getEstimatedInterest(4.0);
// The program calculates it, brings the answer back, and... drops it on the floor.
```


**Fix:** You must catch the value in a variable:
`double result = c1.getEstimatedInterest(4.0);`


### ❌ 6. The "Main Inside Customer" Mistake


**Mistake:** Writing `public static void main` inside the `Customer` class.
**Why:** While this technically works in Java, it is bad design. A Blueprint (Customer) should not know how to run the Program (BankDemo).
**Fix:** Keep them in separate classes as shown in the final code.


-----


## 1️⃣2️⃣ Final Summary


 * **Class:** The definition/blueprint (Data + Methods).
 * **Object:** The actual entity created in memory.
 * **Void Method:** Does an action, returns nothing (e.g., `deposit`).
 * **Return Method:** Does a calculation, gives back an answer (e.g., `getEstimatedInterest`).


**Next Step:**
"This concludes the lecture material. Would you like me to prepare a **10-question multiple-choice quiz** based on this lecture to test the students at the end of the class?"

