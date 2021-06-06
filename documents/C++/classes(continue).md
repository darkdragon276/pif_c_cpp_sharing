## Classes in C++ (continue)

### Reference vs Pointer

A `reference` is value that acts as an alias (unique) to another object or variable.

In C++, we have Reference variable (have `reference`'s feature).

`Reference` seem like a `pointer`. But they have some difference:

* `Reference` can not be `NULL`.

* `Reference`  must be initialized when `declared`.

* `Reference` can not be `reassigned`.

* `Pointer` can have `multiple levels`, but reference can't.

* `Pointer` have `void*`, but reference doesn't have.

  |                              | Reference | Pointer |
  | ---------------------------- | :-------: | :-----: |
  | Value can be `NULL`          |     x     |    v    |
  | Declared but not initialize  |     x     |    v    |
  | Reassigned                   |     x     |    v    |
  | Multiple level               |     x     |    v    |
  | Using void*                  |     x     |    v    |
  | Avoid risk of memory         |     v     |    x    |
  | Using in function is simpler |     v     |    x    |

```C++
#include <iostream>
using namespace std;
class foo {
public:
	uint32_t u32 = 123; 
	uint32_t &r_u32 = u32; // References must be initialed when declared
	// The ampersand (&) doesn't mean “address of”, it means “reference to”.
    
	void func(uint32_t &out_ref, uint32_t in_a) {
		out_ref = in_a*2;
	}
};

int main() {
	uint32_t another_u32 = 999;
	foo test;
    // References can not be reassigned 
	test.r_u32 = another_u32;
    // this line actually assigns the value of another to u32
    
    test.r_u32++;
	cout<<"value of u32: "<<test.u32<< endl;
    cout<<"value of r_u32: "<<test.r_u32<< endl;
    cout<<"value of another_u32: "<<another_u32<< endl;
    
	test.func(test.r_u32, 100);
	cout<<"value of u32: "<<test.u32<< endl;
	return 0;
}
```

---

### New, delete vs malloc, free

`new` is an specific `operator` in C++.  `malloc()` is a C library `function`, can be use in C++.

Both `new` and `malloc()` are used  to allocate the dynamically memory in` heap`. But when `new` is used, it call the `constructor` of a class, whereas `malloc()` does not.

---

### 

`delete` is a keyword in C++. `free()` is a C library `function` that can also be used in C++.

`delete` frees memory create by `new` and calls `destructor` of a class. But `free()` only frees memory create by `malloc()`.

```C++
#include <iostream>
using namespace std;
 
class A {
    int a;
public:
    int* ptr;
	A() // Constructor of class A
    {
		cout << "Constructor was called!"
			 << endl;
	}
	~A() // Destructor of class A
	{
		cout << "Destructor was called!"
			 << endl;
	}
	void method() {
        A c;
        cout << "Object c was created using \"A c\""
             << endl;
    }
};
 
int main()
{
	A* a = new A;
	cout << "Object a was created using \"new\""
 		 << endl;
 	delete(a);
    cout << "Object a was deleted using \"delete\""
 		 << endl;
    cout << endl;   
	A* b = (A*)malloc(sizeof(A));
	cout << "Object b was created using malloc()"
		 << endl;
    // quiz line
    free(b);
    cout << "Object b was deleted using free()"
 		 << endl;
    
	return 0;
}
```

**Quiz:** When add "b->method();" in `quiz line`, what is result was print?

**Answer:** when run `method`, `object c` will created. And after `method` ending, memory of `object c` will be frees and call `detrucstor`.

---

### Call method by Object or Instance

![](./assets/2_1.png)

```c++
#include <iostream>
using namespace std;
 
class calculate {
    int a;
    int b;
public:
    calculate(int x, int y)
    {
		a = x;
		b = y;
    }
    int add()
    {
        return a+b;
    }
    int sub()
    {
        return a-b;
    }
};
int main()
{
    calculate cal_1(10, 9);
    calculate* ptr_cal_2 = new calculate(150, 50);

    // Call method using object
	cout <<"sum of 10 and 9 is: "<<cal_1.add()
		 <<endl;
    // Call method using instance
	cout <<"sub of 150 and 50 is: "<<ptr_cal_2->sub()
		 <<endl;
	return 0;
}

```

---

### Links references:

**References in C++: ** [www.geeksforgeeks.org/...](https://www.geeksforgeeks.org/references-in-c/)

**Reference variables: ** [www.learncpp.com/...](https://www.learncpp.com/cpp-tutorial/references/#:~:text=A%20reference%20is%20a%20type,ll%20discuss%20in%20this%20lesson.)

**new vs malloc() and free() vs delete in C++: ** [www.geeksforgeeks.org/...](https://www.geeksforgeeks.org/new-vs-malloc-and-free-vs-delete-in-c/#:~:text=malloc()%3A%20It%20is%20a,%E2%80%9Cmalloc()%E2%80%9D%20does%20not.)

