# 														**C++ Classes**

### 																		OOP

- OOP stands for Object-Oriented Programming.

- Classes and objects are the two main aspects of object-oriented programming.

| Class | Object |
| ----- | ------ |
| Fruit | Passion  fruit |
|  | Rambutan       |

---

### 																Classes/Objects

- Everything in C++ is associated with classes and objects, along with its attributes and methods. Attributes and methods are basically **variables** and **functions** that belongs to the class.

- Create a class
  ```c++
  class MyClass {       		 // The class
  	public:            		 // Access specifier
  		int myNum;        	 // Attribute (int variable)
  		string myString;  	 // Attribute (string variable)
  };
  ```

- Create an Object 

  In C++, an object is created from a class. We have already created the class named `MyClass`, so now we can use this to create objects.

  To create an object of `MyClass`, specify the class name, followed by the object name.

  To access the class attributes (`myNum` and `myString`), use the dot syntax (`.`) on the object.

  ```c++
  int main() {
    MyClass myObj;  // Create an object of MyClass
  
    // Access attributes and set values
    myObj.myNum = 2021; 
    myObj.myString = "Payitforward";
  
    // Print attribute values
    cout << myObj.myNum << "\n";
    cout << myObj.myString;
    return 0;
  }
  ```

  ---

  ### 																Class Methods

  Methods are **functions** that belongs to the class.

  There are two ways to define functions that belongs to a class:

  * Inside class definition
  * Outside class definition

  > Inside Example
  >
  > ``` c++
  > class MyClass {        
  >   public:             
  >     void myMethod() {  // Method/function defined inside the class
  >       cout << "PayItForWard!";
  >     }
  > };
  > 
  > int main() {
  >   MyClass myObj;     // Create an object of MyClass
  >   myObj.myMethod();  
  >   return 0;
  > }
  > ```
  >
  > 


* To define a function outside the class definition, you have to declare it inside the class and then define it outside of the class. This is done by specificity the name of the class, followed the scope resolution `::` operator, followed by the name of the function.

  > Outside Example 
  >
  > ```c++
  > class MyClass {      
  >   public:              
  >     void myMethod();   // Method/function declaration
  > };
  > 
  > // Method/function definition outside the class
  > void MyClass::myMethod() {
  >   cout << "PayItForWard!";
  > }
  > 
  > int main() {
  >   MyClass myObj;     // Create an object of MyClass
  >   myObj.myMethod();  
  >   return 0;
  > }
  > ```
  >
  > 

---

### 														Constructors

- A constructor in C++ is a **special method** that is automatically called when an object of a class is created.

  To create a constructor, use the same name as the class, followed by parentheses `()`.

> ```c++
> class MyClass {     // The class
>   public:           // Access specifier
>     MyClass() {     // Constructor
>       cout << "PayItForWard!";
>     }
> };
> 
> int main() {
>   MyClass myObj;    // Create an object of MyClass (this will call the constructor)
>   return 0;
> }
> ```

---

### 																Access Specifiers 

> ```c++
> class MyClass {     // The class
> public:           // Access specifier
>  MyClass() {     // Constructor
>    cout << "PayItForWard!";
>  }
> };
> ```

* The `public` keyword is an **access specifier.** Access specifiers define how the members (attributes and methods) of a class can be accessed. 
* In C++, there are three access specifiers:
  - `public` - members are accessible from outside the class.
  - `private` - members cannot be accessed (or viewed) from outside the class.
  - `protected` - members cannot be accessed from outside the class, however, they can be accessed in inherited classes.

---

### 															

### 																 Encapsulation 

* The meaning of **Encapsulation**, is to make sure that "sensitive" data is hidden from users.

* We need to declare class variables as `private`.

  > ```c++
  > #include <iostream>
  > using namespace std;
  > 
  > class Employee {
  >   private:
  >     // Private attribute
  >     int salary;
  > 
  >   public:
  >     void setSalary(int s) {
  >       salary = s;
  >     }
  >     int getSalary() {
  >       return salary;
  >     }
  > };
  > int main() {
  >   Employee myObj;
  >   myObj.setSalary(50000);
  >   cout << myObj.getSalary();
  >   return 0;
  > }
  > ```
  >
  > - The `salary` attribute is `private`, which have restricted access.
  > - The public `setSalary()` method takes a parameter (`s`) and assigns it to the `salary` attribute (salary = s).
  > - The public `getSalary()` method returns the value of the private `salary` attribute.





---

### 																Inheritance

* We group the "inheritance concept" into two categories:

  * **base class** (parent) - the class being inherited from
  * **derived class** (child) - the class that inherits from another class

* To inherit from a class, use the `:` symbol.

  > ``` c++
  > // Base class 
  > class Vehicle {
  >   public:
  >     string brand = "Ford";
  >     void honk() {
  >       cout << "Grusss, gruss! \n" ;
  >     }
  > };
  > 
  > // Derived class
  > class Car: public Vehicle {
  >   public:
  >     string model = "Bently";
  > };
  > 
  > int main() {
  >   Car myCar;
  >   myCar.honk();
  >   cout << myCar.brand + " " + myCar.model;
  >   return 0;
  > }
  > ```

---

### 																Polymorphism

* Polymorphism means "many forms", and it occurs when we have many classes that are related to each other by inheritance.

  > ``` c++
  > // Base class
  > class Animal {
  >   public:
  >     void animalSound() {
  >     cout << "The animal makes a sound \n" ;
  >   }
  > };
  > 
  > // Derived class
  > class Pig : public Animal {
  >   public:
  >     void animalSound() {
  >     cout << "The pig says: wee wee \n" ;
  >   }
  > };
  > 
  > // Derived class
  > class Dog : public Animal {
  >   public:
  >     void animalSound() {
  >     cout << "The dog says: bow wow \n" ;
  >   }
  > };
  > ```

