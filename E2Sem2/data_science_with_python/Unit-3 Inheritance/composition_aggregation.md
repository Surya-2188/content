

#  CHAPTER: Inheritance vs Composition vs Aggregation


**(The Three Ways Objects Form Relationships in Python)**


**Goal:** Understand how to model relationships like "Is-A", "Has-A", and "Uses-A" correctly in code.


-----


##  1️⃣ INTRODUCTION — “Why do objects need relationships?”


Imagine you are designing a Ride-Sharing App like Ola/Uber.
Every ride involves:


1. A **Car**
2. A **Driver**
3. An **Engine**
4. A **Customer**


**The Big Question:**


> *"How do these objects relate to each other?"*


 * Is a Car "using" a Driver?
 * Does a Car "contain" an Engine?
 * Does a Car "inherit" something from a generic Vehicle?


This is where OOP gives us three distinct tools:


1. ✔ **Inheritance** → “IS-A” Relationship
2. ✔ **Composition** → “HAS-A” Relationship (Part created inside)
3. ✔ **Aggregation** → “USES-A” Relationship (Part passed from outside)


Let’s build this ride-sharing scenario to understand each one perfectly.


-----


##  2️⃣ INHERITANCE (IS-A Relationship)


###  Real World Analogy


 * A **Car** IS-A **Vehicle**.
 * A **Bike** IS-A **Vehicle**.


They inherit common traits like `number_of_wheels`, `max_speed`, and `fuel_type`.
So, the `Car` class doesn't create a Vehicle; it **extends** the concept of a Vehicle.


### ✔ Syntax (The only one with parentheses\!)


```python
class Car(Vehicle):     # INHERITANCE
   pass
```


This tells Python: *"Car is a specialized type of Vehicle."*


### ✔ Example in Code


```python
class Vehicle:
   def move(self):
       print("Vehicle is moving")


class Car(Vehicle):   # Child IS-A Parent
   pass


c = Car()
c.move()    # Inherited method works automatically
```


### ✔ Line-by-Line Explanation


1. **`class Car(Vehicle):`**
     * This is the only place where the relationship is declared.
2. **`c.move()`**
     * Python searches the `Car` class.
     * Not found? It looks up to the `Vehicle` class.
     * Finds it → Executes.


###  Common Mistakes


 * **Mistake 1:** Creating the parent inside the child.
     * `self.v = Vehicle()` inside `Car` is **Composition**, NOT Inheritance.
 * **Mistake 2:** Using Inheritance where "IS-A" doesn't make sense.
     * `class Driver(Car):` -\> WRONG. A Driver is **NOT** a Car.


-----


##  3️⃣ COMPOSITION (HAS-A Relationship — Strong Ownership)


###  Real World Hook


 * A **Car** HAS-A **Engine**.


But the Car does **NOT** receive the engine from outside.


 * It creates it internally.
 * **"If the Car is born, the Engine is automatically born with it."**
 * If the Car is destroyed (scrapped), the Engine is destroyed too. This is **Strong Ownership**.


### ✔ Syntax (Part created inside `__init__`)


```python
class Car:
   def __init__(self):
       # The Car creates its own engine
       self.engine = Engine()   # COMPOSITION
```


### ✔ Example in Code


```python
class Engine:
   def start(self):
       print("Engine started")


class Car:
   def __init__(self):
       self.engine = Engine()   # Car OWNS the engine


car = Car()
car.engine.start()
```


### ✔ Line-by-Line Explanation


1. **`self.engine = Engine()`**
     * The `Car` creates the `Engine`.
     * This means the `Car` is responsible for the `Engine`.
     * No external code touched the engine. It is strictly internal.


###  Common Mistakes


 * **Mistake 1:** Thinking composition is inheritance.
     * `class Car(Engine):` -\> WRONG. A Car is not an Engine.
 * **Mistake 2:** Forgetting responsibility.
     * If you create it, YOU must manage it.


-----


##  4️⃣ AGGREGATION (USES-A Relationship — Weak Connection)


###  Real World Hook


 * A **Driver** USES-A **Car**.


But the Driver does **NOT** create the car.


 * The car already exists.
 * The company assigns a car to the driver.
 * **If the Driver quits, the Car still exists.**
 * This is **Loose Coupling**.


### ✔ Syntax (Object passed from outside)


```python
class Driver:
   def __init__(self, car):
       # Driver receives an existing car
       self.car = car   # AGGREGATION
```


### ✔ Example in Code


```python
class Car:
   def drive(self):
       print("Car is moving")


class Driver:
   def __init__(self, car):
       self.car = car  # Driver USES this car


# 1. Create the Car independently
c = Car()


# 2. Assign it to a Driver
d = Driver(c)


d.car.drive()
```


### ✔ Line-by-Line Explanation


1. **`c = Car()`**
     * The Car is created independently in the main program.
2. **`d = Driver(c)`**
     * The Driver receives the *reference* to the existing car.
3. **`self.car = car`**
     * The Driver does **NOT** own the car. Just holds a link to it.


###  Common Mistakes


 * **Mistake 1:** Creating the car inside the Driver.
     * `self.car = Car()` inside `Driver` makes it **Composition**.
 * **Mistake 2:** Passing the wrong object type.
     * If `Driver` expects a `Car` object, passing a string `"Toyota"` will cause errors when calling `.drive()`.


-----


##  5️⃣ FINAL COMPARISON TABLE (Memory Trick)


| Concept | Meaning | Syntax Pattern | Lifecycle |
| :--- | :--- | :--- | :--- |
| **Inheritance** | **IS-A** | `class Child(Parent):` | Parent logic is extended. |
| **Composition** | **HAS-A (Strong)** | `self.part = Part()` (Inside `__init__`) | Part dies if Whole dies. |
| **Aggregation** | **USES-A (Weak)** | `self.part = external_obj` | Part survives if Whole dies. |


-----


##  6️⃣ SUPER SIMPLE REAL-WORLD MEMORY TRICK


 **Inheritance** → “I **AM** something”


 * `class Car(Vehicle)`


 **Composition** → “I **OWN** something” (I made it)


 * `self.engine = Engine()`


 **Aggregation** → “I **USE** something” (Someone gave it to me)


 * `self.car = car`


-----


##  7️⃣ FINAL CONCLUSION


Python gives three ways to build relationships between classes:


1. **Inheritance:** For shared behavior (code reuse).
2. **Composition:** For strong "part-of" relationships (Engine in Car).
3. **Aggregation:** For loose "using" relationships (Driver uses Car).


These three patterns form the structural backbone of almost every software system you will encounter.

