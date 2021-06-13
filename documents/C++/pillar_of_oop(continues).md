

## Pillar of OOP (continues)

### Overload and Override

<img src=".\assets\3_1.png" alt="3_1" style="zoom: 50%;" />

Overload in function is make one function support many input type.

```c++
#include <iostream>
using namespace std;
 
class printData {
   public:
      void print(int i) {
        cout << "Printing int: " << i << endl;
      }
      void print(double  d) {
        cout << "Printing double: " << d << endl;
      }
      void print(string c) {
        cout << "Printing character: " << c << endl;
      }
};

int main(void) {
   printData pd1;
 
   pd1.print(9); // Integer input
   pd1.print(123.456); // Double input
   pd1.print("Hello PIF !!!"); // Character input
 
   return 0;
}
```

We use override in derived class, to change all logic code of parent-class's method. It use when we want renew that method or actualize them (virtual method).

```c++
#include <iostream>
using namespace std;
  
class Parent {
public:
	virtual void func() // "virtual" method
	{
		cout << "func in base class" << endl;
	}
};
// child class inheriting from Parent class
class child : public Parent {
public:
	void func() override // must add "override" keyword
	{	// change logic code of func
		cout << "func in derived class" << endl;
	}
};
  
int main()
{
	Parent prnt;
    prnt.func();
	child chld;
    chld.func();
    return 0;
}
```

---

### Single and Multiple inherit

`Inheritance` is a method which can derive or construct new classes from the existing class. `Inheritance` allows us to reuse classes by having other classes inherit their members. The class being inherited from is called the `parent class`, `base class`, or `superclass`, and the class doing the inheriting is called the `child class`, `derived class`, or `subclass`.

When we inherited from only one `base class` to only one `derived class`. It's call `single inherit`. 

<img src=".\assets\3_2.png" style="zoom:50%;" />

In` multiple inherit`, we have more than one `base class` which are inherited by only one `derived class`. 

<img src=".\assets\3_3.png" style="zoom: 50%;" />

**Syntax:**

```c++
#include<iostream>
using namespace std;
class base1 {
public:
	base1() 
	{
		cout<<"this is base1 class"<<endl;
	}
};
class base2 {
public:
	base2() 
	{
		cout<<"this is base2 class"<<endl;
	}
};
class single: public base1 { // Single inherit from class base1
public:
    single()
    {
		cout<<"this class use single inherit"<<endl;
    }
};
class multi: public base1, public base2 {
// Multiple in herit from base1 & base 2
public:
    multi()
    {
		cout<<"this clase use multi inherit"
			<<endl;
    }
};
int main() {
	single foo1;
    multi foo2;
	return 0;
}
```

We not recommend to use `multiple inherrit ` in your code. Because it can become `diamond inherit` (anti-pattern). In below diagram, `method a()` in `class D` doesn't know to run as `class B` or `class C`.

<img src="D:\workspace\git\pif_c_cpp_sharing\documents\C++\assets\3_4.png" style="zoom: 67%;" />

**Example 1:**

```c++
#include<iostream>
using namespace std;
class group { // Base class 
protected:
	float group_mark_f = 8.5;
};
class stdnt : public group { // Derive class: "stdnt", inherit from class "group"
    uint32_t stdnt_id;
    float bonus_mark_f = 0;
public:
	stdnt(uint32_t id, float bonus)
	{
		stdnt_id = id;
		if (bonus<=10 || bonus>=0) bonus_mark_f = bonus;
		else cout<<"invalid mark !!!"
				 <<endl;
	}
	void showMark()
 	{
		cout<<"Student "<<stdnt_id<<" have "<<group_mark_f+bonus_mark_f<<"-point"
			<<endl;
	}
};
int main()
{
	stdnt foo(1234, 1.5);
    foo.showMark();
    return 0;
}
```

**Example 2:**

```c++
#include<iostream>
using namespace std;
class animal { // Base1 class
public:
	animal() 
	{
		cout<<"This is an animal"<<endl;
    }
};
class fourLegs { // Base2 class
public:
    fourLegs()
    {
		cout<<"have 4 legs"<<endl;
    }
};
class dog : public animal, public fourLegs {
// Derive class
public:
	dog()
    {
		cout<<"this is a dog"<<endl;
    }
};
int main()
{
	dog foo;
    return 0;
}
```

---

### Inheritance vs Aggregation

Inheriting from a `base class` means we don’t have to redefine the information from the `base class` in our `derived classes`. We automatically receive the member `method` and `attribute` of the `base class` through `inheritance`, and then simply add the additional `method` and `attribute` we want. It means that if we ever update or modify the base class, all of our derived classes will automatically inherit the changes.

Aggregation is where one object just contains a reference to another. The container doesn’t control the life cycle of the component. The component can exist without the container and can be linked to several another containers at the same time.
