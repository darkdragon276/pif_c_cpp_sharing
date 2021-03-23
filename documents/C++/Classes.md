# 																													**Classes**

### 																		OOP

- OOP stands for Object-Oriented Programming.

- Classes and objects are the two main aspects of object-oriented programming.

  <img src="C:\Users\ad\AppData\Roaming\Typora\typora-user-images\image-20210323104214268.png" alt="image-20210323104214268" style="zoom: 50%;" />
  
  
  
  


---

### 																									Classes Syntax

- Everything in C++ is associated with classes and objects, along with its attributes and methods. Attributes and methods are basically **variables** and **functions** that belongs to the class.

![Lightbox](https://media.geeksforgeeks.org/wp-content/cdn-uploads/Classes-and-Objects-in-C.png)

**Declaring Objects:** When a class is defined, only the specification for the object is defined; no memory or storage is allocated. To use the data and access functions defined in the class, you need to create objects.



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

										### 										Abstraction(Tính trừu tượng)



<img src="https://cdn.journaldev.com/wp-content/uploads/2019/09/data-abstraction.png" alt="What is Abstraction in OOPS? - JournalDev" style="zoom: 67%;" /><

* Members declared as **public** in a class, can be accessed from anywhere in the program.
* Members declared as **private** in a class, can be accessed only from within the class. They are not allowed to be accessed from any part of code outside the class.

* **Advantages of Data Abstraction**:
  * Helps the user to avoid writing the low level code
  * Avoids code duplication and increases reusability.
  * Can change internal implementation of class independently without affecting the user.
  * Helps to increase security of an application or program as only important details are provided to the user.



---

​	

### 																 										Encapsulation(Tính đóng gói) 

* The meaning of **Encapsulation**, is to make sure that "sensitive" data is hidden from users.

* We need to declare class variables as `private`.

  
  
  ![Why should Encapsulation to be used? | LaptrinhX](https://cdn-images-1.medium.com/max/559/1*CLBzWEo22SXvh-0dT3eV_w.png)
  
  
  
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

### 																											Inheritance

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

  <img src="C:\Users\ad\Desktop\Untitled Diagram.png" alt="Untitled Diagram"  />

  

  * Compile Time Polymorphism

  > ``` c++
  > #include <iostream>
  > using namespace std;
  >  
  > class PrintData {
  > public:
  >     void print(int i) {
  >         cout << "Printing int: " << i << endl;
  >     }
  >  
  >     void print(double  f) {
  >         cout << "Printing float: " << f << endl;
  >     }
  >     
  >     void print(char* c) {
  >         cout << "Printing character: " << c << endl;
  >     }
  > };
  >  
  > int main(void) {
  >     PrintData pd;
  >  
  >     pd.print(5);// Call print to print integer
  >     pd.print(500.263);// Call print to print float
  >     pd.print("Hello C++");// Call print to print character
  >  
  >     return 0;
  > }
  > ```

  * Runtime Polymorphism

    >  ``` c++
    > #include <iostream>
    > using namespace std;
    >  
    > class Pet {
    > protected:
    >     string Name;
    > public:
    >     Pet(string n) { Name = n; }
    >     virtual string getSound() { return "";};
    >     void makeSound(void) { cout << Name << "says: " << getSound() << endl; }
    > };
    >  
    > class Cat : public Pet {
    > public:
    >     Cat(string n) : Pet(n) { }
    >     string getSound() { return "Meow! Meow!";};
    > };
    >  
    > class Dog : public Pet {
    > public:
    >     Dog(string n) : Pet(n) { }
    >     string getSound() { return "Woof! Woof!";};
    > };
    >  
    > int main(void) {
    >     Pet *a_pet = new Cat("Kitty");;
    >     a_pet->makeSound();
    >     delete a_pet;
    >  
    >     a_pet = new Dog("Doggie");
    >     a_pet->makeSound();
    >     delete a_pet;
    >  
    >     return 0;
    > }
    >  ```
    >
    > ​	

---

### 											Identifiers

- Names can contain letters, digits and underscores
- Names must begin with a letter or an underscore (_)
- Names are case sensitive (`myVar` and `myvar` are different variables)
- Names cannot contain whitespaces or special characters like !, #, %, etc.
- Reserved words (like C++ keywords, such as `int`) cannot be used as names





