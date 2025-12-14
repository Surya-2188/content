

# 📘 CHAPTER: INHERITANCE IN JAVA


**(The College Management System Example)**


## 1️⃣ WHY WE NEED INHERITANCE


Imagine designing a **College Management System**.
We have different categories of people in a college:


1. **Person** (The common base for everyone)
2. **Student**
3. **Teacher**


And more specific versions:
4\.  **CR (Class Representative)** -\> A special type of Student.
5\.  **HOD (Head of Department)** -\> A special type of Teacher.
6\.  **Faculty** -\> A standard Teacher.


### 🌍 In the Real World:


 * **All people** have: `name`, `email`, `phoneNumber`.
 * **Students** have: `studentId`, `branch`, and can `registerForCourse`.
 * **Teachers** have: `employeeId`, `subject`, and can `conductClasses`.
 * **CR** has: extra responsibilities (announcements).
 * **HOD** has: extra authority (approving leaves).


### ❌ The Problem Without Inheritance


If we write separate classes for each one:


 * We will repeat `name`, `email`, `phoneNumber`, `viewProfile()`, `sendMessage()` inside **every single class**.
 * Updating a common method (like `viewProfile`) means updating it in 5 different files.
 * Code becomes lengthy, repetitive, and error-prone.


**Solution:**
We need a mechanism where **common attributes & methods are defined ONCE** and automatically available to all related classes.


This mechanism is called **INHERITANCE**.


-----


## 2️⃣ BASE IDEA OF INHERITANCE


Inheritance lets a **New Class** reuse fields and methods of an **Existing Class**.


**Key Terminology:**


 * **Parent / Super / Base Class:** The class giving the features.
 * **Child / Sub / Derived Class:** The class receiving the features.


**Syntax:**
Java uses the keyword: **`extends`**


```java
class Student extends Person
```


**Meaning:**


1. Student **IS-A** Person.
2. Student **inherits** all Person’s properties & behaviors.
3. Student **adds** its own extra features.


-----


## 3️⃣ OUR REALISTIC COLLEGE SYSTEM HIERARCHY


This hierarchy demonstrates almost all types of inheritance.


```text
               Person
             /       \
       Student        Teacher
         |             /    \
         CR        Faculty  HOD
```


 * **Person** is the Parent.
 * **Student** and **Teacher** are Children of Person.
 * **CR** is a Child of Student.
 * **Faculty** & **HOD** are Children of Teacher.


**Types of Inheritance Shown:**


1. **Single Inheritance:** `Student` → `Person`
2. **Multi-level Inheritance:** `Person` → `Teacher` → `HOD`
3. **Hierarchical Inheritance:** `Person` → `Student` & `Person` → `Teacher`
4. **Hybrid Inheritance:** The overall combination of the above.


*(Note: Java does NOT allow Multiple Inheritance using classes.)*


-----


## 4️⃣ FULL PROGRAM WITH ALL CLASSES


Below is the complete, runnable code for the entire hierarchy.


```java
// ----------------------------------------------------
// 1. THE ROOT PARENT CLASS
// ----------------------------------------------------
class Person {
   String name;
   String email;
   String phoneNumber;


   void viewProfile() {
       System.out.println("Name : " + name);
       System.out.println("Email: " + email);
       System.out.println("Phone: " + phoneNumber);
   }


   void sendMessage(String message) {
       System.out.println("Sending message to " + name + ": " + message);
   }
}


// ----------------------------------------------------
// 2. CHILD CLASS 1 (Student)
// ----------------------------------------------------
class Student extends Person {
   String studentId;
   String branch;


   void registerForCourse(String courseCode) {
       System.out.println(name + " registered for " + courseCode);
   }


   void submitAssignment(String courseCode) {
       System.out.println(name + " submitted assignment for " + courseCode);
   }
}


// ----------------------------------------------------
// 3. CHILD CLASS 2 (Teacher)
// ----------------------------------------------------
class Teacher extends Person {
   String employeeId;
   String subject;


   void takeClass(String className) {
       System.out.println(name + " is taking " + subject + " class for " + className);
   }


   void giveMarks(String studentId, int marks) {
       System.out.println(name + " gave " + marks + " marks to Student " + studentId);
   }
}


// ----------------------------------------------------
// 4. GRANDCHILD CLASS (CR extends Student)
// ----------------------------------------------------
class CR extends Student {
   void sendAnnouncementToClass(String msg) {
       System.out.println("CR " + name + " announced to class: " + msg);
   }


   void requestMeetingWithHOD() {
       System.out.println("CR " + name + " requested a meeting with HOD.");
   }
}


// ----------------------------------------------------
// 5. GRANDCHILD CLASS (Faculty extends Teacher)
// ----------------------------------------------------
class Faculty extends Teacher {
   // Future faculty-specific features can be added here
}


// ----------------------------------------------------
// 6. GRANDCHILD CLASS (HOD extends Teacher)
// ----------------------------------------------------
class HOD extends Teacher {
   void approveLeave(String employeeId) {
       System.out.println(name + " (HOD) approved leave for Employee " + employeeId);
   }


   void scheduleDeptMeeting() {
       System.out.println(name + " scheduled a department meeting.");
   }
}


// ----------------------------------------------------
// MAIN EXECUTION CLASS
// ----------------------------------------------------
public class CollegeDemo {
   public static void main(String[] args) {


       // --- Demo 1: Creating a CR ---
       System.out.println("=== CR DEMO ===");
       CR cr = new CR();
       // Inherited from Person
       cr.name = "Raju";
       cr.email = "raju@student.college.com";
       cr.phoneNumber = "9876543210";
       // Inherited from Student
       cr.studentId = "S1001";
       cr.branch = "CSE";


       cr.viewProfile();                    // From Person
       cr.registerForCourse("CSE101");      // From Student
       cr.sendAnnouncementToClass("Exam on Monday!"); // From CR


       System.out.println("\n=== HOD DEMO ===");


       // --- Demo 2: Creating an HOD ---
       HOD hod = new HOD();
       // Inherited from Person
       hod.name = "Dr. Ravi";
       hod.email = "ravi@college.com";
       hod.phoneNumber = "9123456780";
       // Inherited from Teacher
       hod.employeeId = "T5001";
       hod.subject = "Data Structures";


       hod.viewProfile();                  // From Person
       hod.takeClass("CSE-A");             // From Teacher
       hod.approveLeave("T4003");          // From HOD
       hod.scheduleDeptMeeting();          // From HOD
   }
}
```


-----


## 5️⃣ LINE-BY-LINE EXPLANATION OF ENTIRE HIERARCHY


### 🟢 PERSON CLASS


```java
class Person {
   String name;
   String email;
   String phoneNumber;
```


 * **Defines the Base:** All people have these identity details.


<!-- end list -->


```java
   void viewProfile() { ... }
   void sendMessage(String message) { ... }
```


 * **Common Behaviors:** Methods that apply to everyone.


### 🟢 STUDENT CLASS


```java
class Student extends Person {
```


 * **Inheritance:** Student gets everything from Person.


<!-- end list -->


```java
   String studentId;
   String branch;
```


 * **Specialization:** Adds student-specific details.


<!-- end list -->


```java
   void registerForCourse(String courseCode) { ... }
```


 * **Behavior:** Adds student-specific actions.


### 🟢 TEACHER CLASS


```java
class Teacher extends Person {
```


 * **Inheritance:** Teacher gets everything from Person.


<!-- end list -->


```java
   String employeeId;
   String subject;
```


 * **Specialization:** Adds teacher-specific details.


<!-- end list -->


```java
   void takeClass(String className) { ... }
```


 * **Behavior:** Adds teaching actions.


### 🟢 CR CLASS


```java
class CR extends Student {
```


 * **Multi-level:** CR inherits Student (which inherited Person).


<!-- end list -->


```java
   void sendAnnouncementToClass(String msg) { ... }
```


 * **Extra Power:** Adds functionality specific to a Class Representative.


### 🟢 HOD CLASS


```java
class HOD extends Teacher {
```


 * **Multi-level:** HOD inherits Teacher (which inherited Person).


<!-- end list -->


```java
   void approveLeave(String employeeId) { ... }
```


 * **Extra Power:** Adds administrative capabilities.


-----


## 6️⃣ SAMPLE OUTPUT OF THE FULL PROGRAM


```text
=== CR DEMO ===
Name : Raju
Email: raju@student.college.com
Phone: 9876543210
Raju registered for CSE101
CR Raju announced to class: Exam on Monday!


=== HOD DEMO ===
Name : Dr. Ravi
Email: ravi@college.com
Phone: 9123456780
Dr. Ravi is taking Data Structures class for CSE-A
Dr. Ravi (HOD) approved leave for Employee T4003
Dr. Ravi scheduled a department meeting.
```


-----


## 7️⃣ INDIVIDUAL COMPLETE DEMO PROGRAMS


*(Run these separately to understand specific inheritance levels)*


### A. Student Demo (Person + Student)


```java
public class StudentDemo {
   public static void main(String[] args) {
       Student s = new Student();
       s.name = "Kiran";           // Parent (Person)
       s.studentId = "S202";       // Child (Student)
       s.viewProfile();            // Parent Method
       s.registerForCourse("Java");// Child Method
   }
}
```


### B. Teacher Demo (Person + Teacher)


```java
public class TeacherDemo {
   public static void main(String[] args) {
       Teacher t = new Teacher();
       t.name = "Prof. Sita";      // Parent (Person)
       t.subject = "Maths";        // Child (Teacher)
       t.viewProfile();            // Parent Method
       t.takeClass("ECE-B");       // Child Method
   }
}
```


-----


## 8️⃣ HOW EACH CLASS USES ITS PARENT MEMBERS (FINAL SUMMARY)


| Class | Inherits From | What it gets (Automatically) | What it Adds (New) |
| :--- | :--- | :--- | :--- |
| **Person** | None | N/A | name, email, phone, viewProfile |
| **Student** | Person | name, email, phone, viewProfile | studentId, branch, registerForCourse |
| **Teacher** | Person | name, email, phone, viewProfile | employeeId, subject, takeClass |
| **CR** | Student | All Person + All Student features | sendAnnouncement, requestMeeting |
| **HOD** | Teacher | All Person + All Teacher features | approveLeave, scheduleMeeting |


**Without Inheritance:**
We would have to re-type `name`, `email`, `phoneNumber`, and `viewProfile()` **four different times**.
**With Inheritance:**
We wrote it **once** in `Person`, and everyone else reused it.


-----


## 9️⃣ TYPES OF INHERITANCE IN THIS EXAMPLE


1. **Single Inheritance:** `Student` extends `Person`.
2. **Multi-level Inheritance:** `Person` -\> `Student` -\> `CR`.
3. **Hierarchical Inheritance:** `Person` is parent to both `Student` and `Teacher`.
4. **Hybrid Inheritance:** The complete tree structure.


**Note:** Java does **not** support Multiple Inheritance (One child extending two parents like `class CR extends Student, Person`) to avoid ambiguity.


-----


## 1️⃣0️⃣ COMMON MISTAKES STUDENTS MAKE


1. ❌ **Redefining Parent Fields:**
     * Writing `String name` again inside `Student` class. This shadows the parent variable and causes confusion.
2. ❌ **Rewriting Parent Methods Unnecessarily:**
     * Writing `viewProfile` again in `Student` without using `@Override` correctly.
3. ❌ **Forgetting `extends`:**
     * If you forget `extends Person`, the `Student` class won't have a name or email\!
4. ❌ **Thinking Private Members are Inherited:**
     * If `name` was `private` in `Person`, `Student` could not access it directly. (We used `default` access here for simplicity).
5. ❌ **Attempting Multiple Inheritance:**
     * Writing `class A extends B, C` causes a compiler error.


-----


## 1️⃣1️⃣ COMPLETE FINAL CONCLUSION


Inheritance is the backbone of Object-Oriented Programming.


 * It allows us to **Model the Real World** (Person -\> Student -\> CR).
 * It allows **Code Reusability** (Write once in Person, use everywhere).
 * It makes code **Clean and Maintainable**.


**The Keyword:** `extends`
**The Logic:** "Is-A" Relationship (A Student **is a** Person).


**Next Step:**
"Now that the structure is set, what happens if the Child class wants to **change** the behavior of a Parent method? (e.g., A Teacher's `viewProfile` should look different from a Student's). This is called **Method Overriding**, which we will cover next."

