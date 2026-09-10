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
```c++
const int var = 5;
```
In Java, we use the keyword **final**, which works the same way:
```java
final int x = 5;
```
#### Constant Methods
A constant method in C++ means that it cannot modify the state of the object that calls it. The keyword is placed after the declaration of the method:
```c++
double getBalance() const;
```