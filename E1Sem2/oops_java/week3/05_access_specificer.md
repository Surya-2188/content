#  LECTURE: Access Modifiers & Encapsulation


**(Making Your Code Safe and Secure)**


**The Core Concept:**
Up until now, we have made everything `public`. This is like leaving your house door wide open—anyone can walk in and mess up your things. Today, we learn how to **lock the doors** (`private`) and give keys only to specific people (`methods`).


-----


##  1️⃣ THE PROBLEM — Why "Public" is Dangerous


Let's look at a bank account class where the balance is `public`.


```java
class Account {
   public int balance;   // ❌ public balance - OPEN TO EVERYONE
}


public class BadAccountDemo {
   public static void main(String[] args) {
       Account a = new Account();
      
       // 1. Normal usage
       a.balance = 1000;        
       System.out.println("Balance: " + a.balance);


       // 2. THE DANGER
       // Since it is public, I can set it to a nonsense number!
       a.balance = -999999;     
       System.out.println("Corrupted Balance: " + a.balance);
   }
}
```


### 🔍 Line-by-Line Explanation


1. `class Account`: We define a simple class.
2. `public int balance;`: **This is the mistake.** `public` means any other part of the code can change this variable directly. There is no security.
3. `a.balance = 1000;`: We set the balance. This looks fine.
4. `a.balance = -999999;`: **Disaster.** A bank account cannot be negative like this. But because the variable is open, Java allows the programmer to make this mistake.


**Conclusion:** We need to Stop direct access. We need **Encapsulation**.


-----


##  2️⃣ SOLUTION A — Encapsulation for Attributes


**The Rule:** Data (`variables`) should be `private`. Access (`methods`) should be `public`.


```java
class Account {
   private int balance;          // 🔒 LOCKED inside the class


   // The Gatekeeper (Setter)
   public void setBalance(int amount) {
       if (amount >= 0) {        // ✅ VALIDATION LOGIC
           balance = amount;
       } else {
           System.out.println("❌ Error: Negative amount not allowed.");
       }
   }


   // The Viewer (Getter)
   public int getBalance() {
       return balance;
   }
}


public class AccountDemo {
   public static void main(String[] args) {
       Account a = new Account();
      
       a.setBalance(1000);      // We ask permission to set value
       System.out.println(a.getBalance());


       // a.balance = -999;     // ❌ ERROR! This line won't even compile now.
   }
}
```


### 🔍 Key Takeaway


 * **`private int balance`**: Only the `Account` class can touch this. `AccountDemo` cannot see it.
 * **`setBalance`**: This is a public door. It checks the data (`if amount >= 0`) before letting it in.
 * **Result:** You can never have a negative balance by accident.


-----


##  3️⃣ SOLUTION B — Encapsulation for Methods


Sometimes, we have complex internal logic that the outside world doesn't need to see. We make those methods `private` too.


**Example: A Student Result System**


```java
class Student {
   private int internalMarks;
   private int externalMarks;


   public void setMarks(int internal, int external) {
       internalMarks = internal;
       externalMarks = external;
   }


   //  INTERNAL HELPER METHOD
   // The outside world doesn't need to know HOW we calculate total.
   private int calculateTotal() {        
       return internalMarks + externalMarks;
   }


   //  PUBLIC INTERFACE
   // This is what the user actually calls.
   public void showResult() {            
       int total = calculateTotal();      // The class uses its own private method
       System.out.println("Total Marks: " + total);
   }
}


public class StudentDemo {
   public static void main(String[] args) {
       Student s = new Student();
       s.setMarks(20, 60);
      
       s.showResult(); // Works perfectly


       // s.calculateTotal();   // ❌ ERROR! You cannot call private methods from outside.
   }
}
```


### 🔍 Key Takeaway


 * **Encapsulation isn't just for variables.**
 * We hide the *complexity* (`calculateTotal`) and show only the *result* (`showResult`).


-----


##  4️⃣ THE TOOLKIT — The 4 Access Modifiers


Here are the four levels of security in Java.


### 🔹 4.1 public (Open to World)


Can be accessed from **anywhere** (any class, any package).


```java
class StudentPublic {
   public String name;        
   public void showName() { System.out.println(name); }
}
// Usage: s.name = "Raju"; // Allowed
```


### 🔹 4.2 private (Strictly Confidential)


Can be accessed **ONLY** inside the **same class**.


```java
class StudentPrivate {
   private int roll;             
   public void setRoll(int r) { roll = r; } // Must use method
}
// Usage: s.roll = 10; // ERROR
// Usage: s.setRoll(10); // Allowed
```


### 🔹 4.3 default (Package Private)


If you don't write any keyword, it is `default`.
Can be accessed by any class in the **same folder (package)**.


```java
class CollegeDefault {
   String dept = "CSE";      // No keyword = default
}
// Usage: c.dept works if Demo class is in the same folder.
```


### 🔹 4.4 protected (Inheritance Special)


Used for **Child Classes**. We will learn this deeply when we study **Inheritance**.
For now: It works like `default` but is also visible to children in other packages.


-----


##  5️⃣ THE FINAL SAFE PROGRAM — SafeStudent


This brings everything together. This is how professional Java code is written.


```java
class SafeStudent {
   // ----------------------------------------------------
   // 1. DATA HIDING (Private Attributes)
   // ----------------------------------------------------
   private int roll;
   private String name;
   private int marks;


   // ----------------------------------------------------
   // 2. PUBLIC METHODS (Controlled Access)
   // ----------------------------------------------------
  
   // Setter for Roll (Validates input)
   public void setRoll(int r) {
       if (r > 0) {
           roll = r;
       } else {
           System.out.println("Invalid Roll Number");
       }
   }


   // Setter for Name (Ensures not empty)
   public void setName(String n) {
       if (n != null && n.length() > 0) {
           name = n;
       }
   }


   // Setter for Marks (Ensures range 0-100)
   public void setMarks(int m) {
       if (m >= 0 && m <= 100) {
           marks = m;
       }
   }


   // ----------------------------------------------------
   // 3. PUBLIC DISPLAY METHOD
   // ----------------------------------------------------
   public void showDetails() {
       System.out.println("--- Student Details ---");
       System.out.println("Roll  : " + roll);
       System.out.println("Name  : " + name);
       System.out.println("Marks : " + marks);
   }
}


public class SafeProgramDemo {
   public static void main(String[] args) {
       SafeStudent s = new SafeStudent();


       // Using safe setters
       s.setRoll(10);
       s.setName("Raju");
       s.setMarks(85);


       s.showDetails();


       // ❌ The following lines are IMPOSSIBLE now:
       // s.roll = -5;     (Private variable)
       // s.marks = 5000;  (Private variable)
   }
}
```


-----


###  Detailed Line-by-Line Explanation


**Inside `class SafeStudent`:**


 * **`private int roll;`**: We define the roll number. The `private` keyword ensures that no other class can touch this variable directly. It is locked.
 * **`public void setRoll(int r)`**: This is the "Gatekeeper." It is `public`, so `main()` can call it.
 * **`if (r > 0)`**: This is the **Validation Logic**. Before we modify the private data, we check if the request is valid.
 * **`roll = r;`**: If the check passes, we assign the value. If the check fails, the private variable remains safe.
 * **`public void showDetails()`**: Since variables are private, we need a public method to print them out so the user can see the result.


**Inside `public class SafeProgramDemo`:**


 * **`SafeStudent s = new SafeStudent();`**: We create the object.
 * **`s.setRoll(10);`**: We do NOT say `s.roll = 10`. We call the method. The method checks "Is 10 \> 0?" -\> Yes. -\> Updates the internal data.
 * **`s.showDetails();`**: We ask the object to display its data.


-----


##  6️⃣ FINAL SUMMARY FOR STUDENTS


 * **Public:** "The Door is Open." Anyone can enter and change things. (Unsafe).
 * **Private:** " The Door is Locked." Only the owner (the Class) can touch it. (Safe).
 * **Encapsulation:** The technique of wrapping data (private variables) and code (public methods) together as a single unit.
 * **Why use it?** To prevent bad data (like negative marks or balances) from crashing your system.


**Next Step:** "Students, notice how we are calling `s.setRoll`, `s.setName`, `s.setMarks` manually every time? Wouldn't it be nice if we could set all this data **in a single line** the moment we create the object? That is what we will learn in the next topic: **Constructors**."

