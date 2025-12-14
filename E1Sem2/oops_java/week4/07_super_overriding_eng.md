# 🎓 PROJECT: Building Real Systems with OOP


**(Why Overriding & Super Are Essential)**


**Goal:** Apply Method Overriding and `super` to build realistic Student and Banking systems.


-----


## 🏫 PROJECT 1 — STUDENT MANAGEMENT SYSTEM


###  1️⃣ REAL-WORLD PROBLEM


A college needs a system to display information for different types of people:


1. **Person** (General)
2. **Student** (Person + Academic info)
3. **CR** (Student + Leadership info)
4. **Faculty** (Person + Teaching info)
5. **HOD** (Faculty + Administrative info)


All of them perform the **SAME** action: **`displayInfo()`**.
But what they display is **DIFFERENT**.


 * **Person** → name, email
 * **Student** → name, email, roll number
 * **CR** → name, email, roll number, class, responsibilities
 * **Faculty** → name, email, subject
 * **HOD** → name, email, department, authority


**❌ The Bad Way:**
Using different method names like:


 * `displayPerson()`
 * `displayStudent()`
 * `displayCRInfo()`
 * `displayHODInfo()`


**Why it fails:** It destroys readability and breaks the logical connection between these classes.


**✔ The OOP Way:**


 * Same method name: `displayInfo()`
 * Different behavior per class (Overriding)
 * Reuse Parent info (using `super`)


-----


### 2️⃣ HOW OVERRIDING SOLVES IT


1. **Person** writes the general version of `displayInfo()`.
2. **Student, CR, Faculty, HOD** override `displayInfo()` to add their own details.
3. They call **`super.displayInfo()`** to reuse Person’s common info instead of rewriting it.


**Result:** No duplication. Clean structure. Realistic modeling.


-----


###  3️⃣ STUDENT MANAGEMENT SYSTEM — COMPLETE CODE


```java
// 1. BASE CLASS
class Person {
   String name;
   String email;


   Person(String name, String email) {
       this.name = name;
       this.email = email;
   }


   void displayInfo() {
       System.out.println("PERSON INFO");
       System.out.println("Name : " + name);
       System.out.println("Email: " + email);
   }
}


// 2. CHILD CLASS (Student)
class Student extends Person {
   String roll;


   Student(String name, String email, String roll) {
       super(name, email); // Calling parent constructor
       this.roll = roll;
   }


   @Override
   void displayInfo() {
       super.displayInfo(); // Reuse parent info
       System.out.println("Roll : " + roll);
   }
}


// 3. GRANDCHILD CLASS (CR)
class CR extends Student {
   String className = "CSE-A";


   CR(String name, String email, String roll) {
       super(name, email, roll);
   }


   @Override
   void displayInfo() {
       super.displayInfo();  // Reuse student info
       System.out.println("Class: " + className);
       System.out.println("Role : Class Representative");
   }


   void sendNotification(String msg) {
       System.out.println("📢 CR " + name + " Announcement: " + msg);
   }
}


// 4. CHILD CLASS (Faculty)
class Faculty extends Person {
   String subject = "Data Structures";


   Faculty(String name, String email) {
       super(name, email);
   }


   @Override
   void displayInfo() {
       super.displayInfo(); // Reuse Person info
       System.out.println("Subject: " + subject);
       System.out.println("Role   : Faculty Member");
   }
}


// 5. CHILD CLASS (HOD)
class HOD extends Person {
   String department;


   HOD(String name, String email, String department) {
       super(name, email);
       this.department = department;
   }


   @Override
   void displayInfo() {
       super.displayInfo(); // Reuse Person info
       System.out.println("Department: " + department);
       System.out.println("Role      : Head of Department");
   }


   void sendNotification(String msg) {
       System.out.println("📢 HOD " + name + " Notification: " + msg);
   }
}


// 6. MAIN CLASS
public class StudentManagementDemo {
   public static void main(String[] args) {


       Student s = new Student("Arun", "arun@college.com", "CSE101");
       CR cr = new CR("Ravi", "ravi@college.com", "CSE102");
       Faculty f = new Faculty("Meena", "meena@college.com");
       HOD hod = new HOD("Dr. Meera", "meera@college.com", "CSE");


       s.displayInfo();
       System.out.println("-----");


       cr.displayInfo();
       cr.sendNotification("Tomorrow assignment submission!");
       System.out.println("-----");


       f.displayInfo();
       System.out.println("-----");


       hod.displayInfo();
       hod.sendNotification("Department meeting at 3 PM");
   }
}
```


-----


###  4️⃣ FINAL OUTPUT


```text
PERSON INFO
Name : Arun
Email: arun@college.com
Roll : CSE101
-----
PERSON INFO
Name : Ravi
Email: ravi@college.com
Roll : CSE102
Class: CSE-A
Role : Class Representative
📢 CR Ravi Announcement: Tomorrow assignment submission!
-----
PERSON INFO
Name : Meena
Email: meena@college.com
Subject: Data Structures
Role   : Faculty Member
-----
PERSON INFO
Name : Dr. Meera
Email: meera@college.com
Department: CSE
Role      : Head of Department
📢 HOD Dr. Meera Notification: Department meeting at 3 PM
```


-----


## 🏦 PROJECT 2 — BANK MANAGEMENT SYSTEM


###  1️⃣ REAL-WORLD PROBLEM


A bank needs to support:


1. **Regular Account**
2. **Savings Account**
3. **Current Account**


All accounts perform: **`withdraw()`**.
But the rules are different:


 * **Savings Account:** Has a withdrawal limit (cannot withdraw more than 5000 at once).
 * **Current Account:** Allows **Overdraft** (can withdraw more than balance).
 * **Regular Account:** Standard rules.


**The OOP Solution:**
We keep the method name **`withdraw()`** but change the logic using **Overriding**.


-----


###  2️⃣ HOW `super` IS NEEDED


SavingsAccount and CurrentAccount should NOT rewrite the entire withdrawal logic (updating balance, logging, etc.).
They only **check conditions**, and if valid, call **`super.withdraw(amt)`** to do the actual work.


-----


###  3️⃣ BANK MANAGEMENT SYSTEM — COMPLETE CODE


```java
// 1. BASE CLASS
class Account {
   String accNo;
   double balance;


   Account(String accNo, double balance) {
       this.accNo = accNo;
       this.balance = balance;
   }


   void deposit(double amt) {
       balance += amt;
       System.out.println("Deposited: " + amt);
   }


   // The Standard Logic
   void withdraw(double amt) {
       balance -= amt;
       System.out.println("✅ Withdrawn: " + amt);
   }


   void displayBalance() {
       System.out.println("Account : " + accNo);
       System.out.println("Balance : " + balance);
   }
}


// 2. CHILD CLASS (Savings)
class SavingsAccount extends Account {


   SavingsAccount(String accNo, double balance) {
       super(accNo, balance);
   }


   @Override
   void withdraw(double amt) {
       if (amt > 5000) {
           System.out.println("❌ Savings Account Limit Exceeded! Max 5000.");
       } else {
           super.withdraw(amt); // Reuse parent logic
       }
   }
}


// 3. CHILD CLASS (Current)
class CurrentAccount extends Account {
   double overdraftLimit = 2000;


   CurrentAccount(String accNo, double balance) {
       super(accNo, balance);
   }


   @Override
   void withdraw(double amt) {
       // Allows withdrawing more than balance up to a limit
       if (amt > balance + overdraftLimit) {
           System.out.println("❌ Overdraft Limit Exceeded!");
       } else {
           super.withdraw(amt); // Parent logic handles the math
       }
   }
}


// 4. MAIN CLASS
public class BankDemo {
   public static void main(String[] args) {


       SavingsAccount sa = new SavingsAccount("S1001", 8000);
       CurrentAccount ca = new CurrentAccount("C2001", 3000);


       // Scenario 1: Savings
       sa.withdraw(6000);  // Overridden logic blocks this
       sa.withdraw(2000);  // Parent logic allows this via super


       System.out.println("-----");


       // Scenario 2: Current
       ca.withdraw(4500);  // Overdraft allowed (3000 - 4500 = -1500)
       ca.withdraw(6000);  // Overdraft limit exceeded


       System.out.println("-----");


       sa.displayBalance();
       ca.displayBalance();
   }
}
```


-----


### 4️⃣ FINAL OUTPUT


```text
❌ Savings Account Limit Exceeded! Max 5000.
✅ Withdrawn: 2000.0
-----
✅ Withdrawn: 4500.0
❌ Overdraft Limit Exceeded!
-----
Account : S1001
Balance : 6000.0
Account : C2001
Balance : -1500.0
```


-----


## FINAL CONCLUSION


**(Why Overriding + Super Matter in Real Projects)**


1. **Real Systems have Shared Roles:** (Student, CR, HOD) or (Savings, Current).
2. **Consistent Naming:** We use the SAME method names (`displayInfo`, `withdraw`) for everyone.
3. **Role-Based Behavior:** Overriding allows each class to behave differently based on its role.
4. **Code Reusability:** `super` allows us to write the common logic ONCE in the parent and reuse it everywhere.
5. **Identity:** Overriding gives each class its own unique identity while remaining part of the family.


**This is TRUE OOP.** This is how professional software is built.


**Next Step:**
"Now that we have built complete systems, how do we make them flexible at runtime? How can we write a single method that can handle Students, Faculty, and HODs all at once? This leads to **Runtime Polymorphism**."

