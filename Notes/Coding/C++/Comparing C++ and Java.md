Both C++ and Java are derived from C, and so share many similarities in their syntax. They also share the same concept of objects, though they differ in syntax.
## Differences Between C++ and Java
#### Compilation
C++ compilation produces a binary file written in machine code native to the machine on which it was compiled. The binary contains instructions for a specific **instruction set architecture** (*ISA*), along with calls specific to the OS and is linked to libraries in a specific format for that OS. This means that to use on another machine, it is often necessary to recompile. The advantage of this approach is that it is much faster to start up as it has little overhead compared to Java. C++ also has improved performance and gives direct control over memory.

Java compilation produces intermediate bytecode, that requires a **Java Virtual Machine** (*JVM*) to be interpreted. The code can be used on any device with a JVM. The advantage of this approach is that the code is portable and is simpler to distribute across platforms. However, JVM is itself a runtime dependency with its own memory/CPU overhead.
#### Design
C++ allows both Object-oriented Programming (*OOP*) and procedural programming. 

Java is structured around OOP; classes are obligatory, however primitive types exist. We can also create static class methods that can be used without an object.
#### Constant Variables
A constant variable in C++ is a variable that must be initialized at declaration, and its value cannot be changed afterward. We declare them with keyword **const**. Ex:
```cpp
const int var = 5;
```
In Java, we use the keyword **final**, which works the same way:
```java
final int x = 5;
```
#### Constant Methods
A **constant** method in C++ means that it cannot modify the state of the object that calls it. The keyword is placed after the declaration of the method:
```c++
double getBalance() const;
```

The same concept does not exist in Java. While the **final** keyword can be used with methods, it instead ensures that the method cannot be overridden by subclasses (in fact, C++ also uses final for this purpose).
#### Type Conversion
In C++, **all** type conversions by assignment are valid even if it leads to information loss. C++ prioritizes flexibility, performance, and low-level control. So while this allows more precision, it may also lead to bugs.

In Java, implicit conversions are only performed on 'widening' primitive conversions. 
```java
int i = 100;
long l = i;
double d = l;
```
Any conversions that result in loss of information must be made explicitly (known as **casting**):
```java
double d = 3.99;
int i = (int)d;
```

For objects, only implicit **upcasting** (subtype → supertype) occurs. This is always safe since a child object is always an instance of its parent:
```java
Dog d = new Dog();
Animal a = d;
```
#### Definition/Class Statement
In C++, a defined class is in a ".h" file separate from the main ".cpp" file.

In Java, classes can be in the same or different file as the main, which all end in ".java."
#### Pointers
A pointer is a variable that stores the address in memory of another variable, rather than a direct value.
```cpp
int age = 25;
int* ptr = &age;
```
A pointer variable is denoted by the **dereference operator (\*)*** . This operator access/modifies the actual value stored at the address held by the pointer. The **address-of operator (&)** retrieves the memory address of a variable.

There is no pointer in Java.
#### Instantiation of Objects
In C++, you can choose whether an object lives on the stack or the heap. Essentially, we can declare it by its statement (like a primitive), which produces an automatic object on the stack:
```cpp
void foo() {
	Point p(1, 2);
}
```
Or use the new keyword, which creates a dynamic object on the heap:
```cpp
Point* p = new Point(1, 2);
```

In Java, all objects exist on the heap and are always created with the new keyword.
#### Instantiation - Default Constructor
In C++, parentheses are not compulsory when declaring an object. Each declaration below is valid in C++:
```cpp
A a;
A* a = new A;
A* a = new A();
```

In java, there is only one possible statement for creating an object:
```java
A a = new A();
```
#### Memory Management
In C++, all variables created with **new** must be explicitly erased using **delete**. Declaring an object reserves a spot in memory for it.

Java has a built-in 'garbage collector'/'crumb pick.' It detects objects that are no longer referenced and deletes them. There is no explicit way to delete them. Declaring an object reserves a spot in memory for a reference to the object rather than the object itself.
#### Passing Arguments
In C++, there are three possibilities:
- by value (changes remain local to function)
- by address or pointer
- by reference
(changes are effective out of the function for the last two).

In Java, there only passing by value is allowed. Changes are local to the function for primitive values but are effective out of function for objects.
#### Default Initialization of object fields