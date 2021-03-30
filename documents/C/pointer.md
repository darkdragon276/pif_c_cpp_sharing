## Pointer

### 1. Introduction

Pointer is the special data type in C. It store address of variables or a memory location.  Address of variables is the instruction for CPU know about this variable. 

```c
datatype * pointer_name;
uint8_t* ptr1;
```

Pointers have 2 operator: * and &

```c
uint8_t* ptr1;
uint8_t foo = 99;
ptr1 = &foo; // &variable_name is get the address of variable
printf("%d", *ptr1); // *pointer_name is get the value of variable whose address store in this pointer
```



### 2. Dangling Pointer

### 3. Void Pointer

### 4. Null Pointer and Wild Pointer



