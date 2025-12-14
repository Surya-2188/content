


#  CHAPTER: Polymorphism in Python


**(Same Action, Different Behavior... with a Twist\!)**


**Goal:** Understand how Python handles polymorphism differently from Java/C++, making code incredibly flexible.


-----


##  1️⃣ THE CONCEPT: START WITH A REAL-LIFE PROBLEM


Imagine you are building a **School Communication System**.
Different people in a school speak, but they say things differently:


 * **Student:** Raises a doubt or asks a question.
 * **Teacher:** Gives a lecture or statement.
 * **Principal:** Makes an announcement.


The system needs to call the **SAME** method: `speak()`.
But each person should respond in their **OWN** style.


This is **Polymorphism**:


> **"One common interface (action) → Many different forms (behaviors)."**


In languages like Java or C++, this only works if all classes share a common Parent.
**But Python does something shocking...**


-----


##  2️⃣ POLYMORPHISM WITH INHERITANCE (The Traditional Way)


This is what you learned in Java.


1. All classes inherit from a common Parent (`Person`).
2. Each Child class overrides the `speak()` method.


<!-- end list -->


```python
class Person:
   def speak(self):
       return "I am a person."


class Student(Person):
   def speak(self):
       return "I am a student, I have a doubt?"


class Teacher(Person):
   def speak(self):
       return "I am a teacher, listen carefully."


# A generic function that takes ANY person object
def announce(obj):
   print(obj.speak())


# Testing
s = Student()
t = Teacher()


announce(s)
announce(t)
```


**Output:**


```text
I am a student, I have a doubt?
I am a teacher, listen carefully.
```


**Conclusion:** The function `announce()` works for both because they are children of `Person`.


-----


##  3️⃣ THE PYTHON TWIST (Duck Typing)


Now, let's create a class that has **NO connection** to `Person`.


```python
class Robot:
   def speak(self):
       return "Beep bop, I am a robot."
```


In Java, passing this `Robot` to `announce()` would crash the program because `Robot` is not a `Person`.
**But try this in Python:**


```python
r = Robot()
announce(r)
```


**Output:**


```text
Beep bop, I am a robot.
```


**The Shocking Truth:**


1. Python **DOES NOT CARE** about inheritance or parent-child relationships here.
2. It only checks one thing: **"Does this object have a `speak()` method?"**
3. If yes, Python runs it.


**This is called Duck Typing:**


> *"If it walks like a duck and quacks like a duck, Python treats it like a duck."*


-----


##  4️⃣ REAL-WORLD PROJECT: NOTIFICATION SYSTEM


Let's build a notification system that sends messages via Email, SMS, and WhatsApp.


 * Note: These technologies are totally different. They don't share a "Parent".


### The Classes (Unrelated)


```python
class EmailSender:
   def send(self, msg):
       print("📧 Email Sent:", msg)


class SMSSender:
   def send(self, msg):
       print("📱 SMS Sent:", msg)


class WhatsAppSender:
   def send(self, msg):
       print("💬 WhatsApp Sent:", msg)
```


### The Polymorphic Function


```python
# This function works with ANYTHING that has a .send() method
def notify(sender, message):
   sender.send(message)
```


### The Execution


```python
email = EmailSender()
sms = SMSSender()
wa = WhatsAppSender()


notify(email, "Welcome to the App!")
notify(sms, "OTP: 4567")
notify(wa, "New Assignment Posted")
```


**Output:**


```text
📧 Email Sent: Welcome to the App!
📱 SMS Sent: OTP: 4567
💬 WhatsApp Sent: New Assignment Posted
```


-----


##  5️⃣ LINE-BY-LINE EXPLANATION


**`class EmailSender:`**
Defines a class that handles emails. It is independent.


**`def send(self, msg):`**
The specific implementation of sending a message.


**`def notify(sender, message):`**
This is the magic.


 * The parameter `sender` does not have a fixed type (like `EmailSender` or `Person`).
 * It can be **ANY** object in the universe.


**`sender.send(message)`**
Python performs a runtime check:


1. Look at the object inside `sender`.
2. Does it have a method named `send`?
3. **Yes?** Run it.
4. **No?** Raise an `AttributeError`.


This eliminates the need for complex Interface definitions (like in Java).


-----


##  6️⃣ WHY IS THIS IMPORTANT? (Real-World Usage)


Modern Python frameworks rely heavily on this flexibility.


| Framework | How it uses Duck Typing |
| :--- | :--- |
| **Django** | Views just need to implement `.get()` or `.post()`. They don't need to inherit from a strict base. |
| **Pandas** | Functions accept any object that has a `.to_numpy()` method. |
| **Logging** | The logger accepts any object that has a `.write()` method (File, Console, Database). |


**Benefits:**


 * **Flexible:** You can plug in new classes without changing existing code.
 * **Simple:** No need to write empty "Interface" classes.
 * **Extensible:** Third-party libraries can work with your code instantly.


-----


##  7️⃣ COMMON MISTAKES STUDENTS MAKE



**Mistake 1: Thinking classes MUST be related**


 * *Truth:* No. In Python, inheritance is optional for polymorphism.


 **Mistake 2: Forgetting to define the required method**


 * *Code:* `notify(5, "Hello")`
 * *Error:* `AttributeError: 'int' object has no attribute 'send'`.
 * *Fix:* Ensure the object actually has the method you are calling.


 **Mistake 3: Confusing Method Names**


 * If one class has `.send()` and another has `.sends()`, polymorphism breaks. The names must match **exactly**.


 **Mistake 4: Believing Python checks Types**


 * Python checks **Behavior**, not Types. It asks "Can you do this?", not "Who are you?".


-----


##  8️⃣ FINAL SUMMARY (Exam Points)


1. **Polymorphism:** Performing the same action (`.speak()`, `.send()`) in different ways based on the object.
2. **Inheritance Polymorphism:** (Traditional) Using Parent/Child relationships to override methods.
3. **Duck Typing:** (Pythonic) If an object has the required method, it works. No inheritance needed.
4. **Advantage:** Makes Python code shorter, cleaner, and more flexible than strictly typed languages.

