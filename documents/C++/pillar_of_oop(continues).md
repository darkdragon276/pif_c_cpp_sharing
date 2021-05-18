## Pillar of OOP (continues)

### Overload and Override

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



### Single and multi inherit

### Inherit vs Aggregation

