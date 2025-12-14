# 📘 CHAPTER: METHOD OVERRIDING


**(Why Child Classes Need Their OWN Version of a Method)**


**Goal:** Understand how to modify inherited behavior to suit specific Child classes.


-----


##  1️⃣ FIRST: UNDERSTAND THE POWER OF `extends`


When we write:


```java
class CR extends Student
// or
class HOD extends Teacher
```


A **HUGE** thing happens:
The child object can use **ALL** methods of the parent, even without writing them again.


**Example:**


```java
HOD h = new HOD();
h.displayInfo();   // Already exists in Person → used by HOD automatically
```


Just one word — `extends` — connects both classes so smoothly.


-----


##  2️⃣ BUT A PROBLEM APPEARS…


What if the Child class wants:


1. The **Same Method Name**,
2. But **Different Behavior**?


This is a very practical scenario.


### 🚗 Analogy: Car vs. Sports Car


Both perform the action: **`drive()`**.


 * **Normal Car** → Drives normally.
 * **Sports Car** → Drives super-fast, aerodynamic.


We never want to write different names like:


 * `normalCar.drive()`
 * `sportsCar.superDrive()`


**Ideally:** Same action name → Same method name → **Different result**.
This is **EXACTLY** where Method Overriding becomes necessary.


[Image of polymorphism concept diagram]


-----


##  3️⃣ OUR COLLEGE SYSTEM EXAMPLE


Let's look at our hierarchy:


```text
Person
├── Student
│     └── CR
└── Teacher
       ├── Faculty
       └── HOD
```


[Image of Java class structure diagram]


All of them have a common behavior: **`displayInfo()`**.
But each one’s “info” is different.


 * **Person** → Basic identity info.
 * **CR** → Student details + Class Representative role.
 * **Faculty** → Teaching details + Faculty role.
 * **HOD** → Administrative details + Head of Dept role.


We want the **Same Name**, but **Custom Behavior**.
This is Method Overriding.


-----


##  4️⃣ WHY OVERRIDING IS USEFUL


1. **Real-world modeling:** Same action → different output based on the object.
2. **Clean naming:** Developers only need to remember ONE method name: `displayInfo()`.
3. **Reduces code duplication:** All classes share a common method structure.
4. **Supports Runtime Polymorphism:** Java decides **at runtime** which version to execute (we will learn this logic next).


-----


##  5️⃣ FULL CODE EXAMPLE (Person, CR, Faculty, HOD)


```java
// 1. PARENT CLASS
class Person {
   String name = "Ravi";
   String email = "ravi@college.com";


   // The Standard Version
   void displayInfo() {
       System.out.println("PERSON INFO");
       System.out.println("Name: " + name);
       System.out.println("Email: " + email);
   }
}


// 2. CHILD CLASS (CR)
class CR extends Person {
   String className = "CSE-A";


   // OVERRIDING: Providing CR-specific implementation
   void displayInfo() { 
       System.out.println("CR INFO");
       System.out.println("Name: " + name);
       System.out.println("Email: " + email);
       System.out.println("Class: " + className);
       System.out.println("Role: Class Representative");
   }
}


// 3. CHILD CLASS (Faculty)
class Faculty extends Person {
   String subject = "Data Structures";


   // OVERRIDING: Providing Faculty-specific implementation
   void displayInfo() { 
       System.out.println("FACULTY INFO");
       System.out.println("Name : " + name);
       System.out.println("Email: " + email);
       System.out.println("Subject: " + subject);
       System.out.println("Role: Faculty Member");
   }
}


// 4. CHILD CLASS (HOD)
class HOD extends Person {
   String department = "CSE";


   // OVERRIDING: Providing HOD-specific implementation
   void displayInfo() { 
       System.out.println("HOD INFO");
       System.out.println("Name : " + name);
       System.out.println("Email: " + email);
       System.out.println("Department: " + department);
       System.out.println("Role: Head of Department");
   }
}


// 5. MAIN CLASS
public class OverrideDemo {
   public static void main(String[] args) {
       Person p = new Person();
       CR cr = new CR();
       Faculty f = new Faculty();
       HOD hod = new HOD();


       // Each call looks the same, but behaves differently
       p.displayInfo();
       System.out.println("-----");


       cr.displayInfo();
       System.out.println("-----");


       f.displayInfo();
       System.out.println("-----");


       hod.displayInfo();
   }
}
```


-----


##  6️⃣ OUTPUT


```text
PERSON INFO
Name: Ravi
Email: ravi@college.com
-----
CR INFO
Name: Ravi
Email: ravi@college.com
Class: CSE-A
Role: Class Representative
-----
FACULTY INFO
Name : Ravi
Email: ravi@college.com
Subject: Data Structures
Role: Faculty Member
-----
HOD INFO
Name : Ravi
Email: ravi@college.com
Department: CSE
Role: Head of Department
```


-----


##  7️⃣ LINE-BY-LINE EXPLANATION


 * **Person class:** Provides the default/general `displayInfo()` method.
 * **CR class overrides it:** Adds `className` and the specific CR role.
 * **Faculty class overrides it:** Adds `subject` and the specific Faculty role.
 * **HOD class overrides it:** Adds `department` and the HOD authority role.
 * **Main Program:** Each object (`cr`, `f`, `hod`) calls its **own** version of `displayInfo()`. Java knows which one to pick.


-----


## 8️⃣ COMMON MISTAKES STUDENTS MAKE


*(Write this on the board — The Lab Survival Guide)*


❌ **Mistake 1: Changing method signature**


 * *Code:* `void displayInfo(String msg)` inside Child.
 * *Correction:* This is **NOT** overriding; it becomes **Overloading**. To override, the Name, Parameters, and Return Type must be **exactly** the same.


❌ **Mistake 2: Reducing access level**


 * *Parent:* `public void displayInfo()`
 * *Child:* `void displayInfo()` (Default access)
 * *Correction:* Child cannot be "less accessible" than the Parent. It must be `public`.


❌ **Mistake 3: Forgetting that overriding needs inheritance**


 * *Correction:* You cannot override a method if you do not use `extends`.


❌ **Mistake 4: Thinking child automatically uses parent behavior**


 * *Correction:* If overriding happens, the Child version **completely replaces** the Parent version—unless you explicitly call `super.displayInfo()`.


❌ **Mistake 5: Believing overriding happens at compile time**


 * *Correction:* No — it is **Runtime Polymorphism**. Java decides at runtime which method to call based on the actual object.


❌ **Mistake 6: Putting constructor instead of method**


 * *Correction:* Constructors **cannot** be overridden.


❌ **Mistake 7: Adding different return type**


 * *Code:* `int displayInfo()` (when parent has `void`).
 * *Correction:* Return type must match exactly (or be a covariant type, but for beginners: keep it exact).


-----


##  9️⃣ WRONG ASSUMPTIONS BEGINNERS HAVE


❌ **Assumption 1: “If child has same method name, parent’s method disappears.”**


 * *Truth:* Not true. The Parent’s method still exists — it is just hidden. You can access it inside the child using `super.displayInfo()`.


❌ **Assumption 2: “Why not use different names for every method?”**


 * *Truth:* This is terrible for readability. Real systems need standard names like `displayInfo()`, `drive()`, or `save()`. NOT `displayStudentInfo()`, `displayFacultyInfo()`, etc. Overriding keeps naming consistent.


❌ **Assumption 3: “Overriding is same as Overloading.”**


 * *Truth:* No.
     * **Overloading:** Same name, different parameters (Same class).
     * **Overriding:** Same name, same parameters (Parent-Child classes).


❌ **Assumption 4: “Child must call parent method first.”**


 * *Truth:* No. The Child can completely **REPLACE** the parent's behavior without ever calling the parent.


❌ **Assumption 5: “Overriding is optional.”**


 * *Truth:* In real-world OOP, overriding is essential. Without it:
     * CR displays incomplete info.
     * HOD displays generic info.
     * Faculty displays no subject info.
       Overriding gives each class its **Identity**.


-----


##  1️⃣0️⃣ FINAL SUMMARY


**Method Overriding** lets child classes redefine a method from the parent class.


**The Rules:**


1. Same Name
2. Same Parameters
3. Same Return Type
4. **Different Behavior**


**The Usage:**
Used when different roles (CR, Faculty, HOD) perform the same action (`displayInfo`), but need to do it differently.


 * `extends` gives them access.
 * **Overriding** gives them individuality.


**Next Step:**
"Now that we know how to replace a parent's method, what if we want to refer to a Child object using a Parent variable? This leads to the most powerful concept in Java: **Runtime Polymorphism**."

