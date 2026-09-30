A **structure** designates a set of values that can be of different types under one name. Each of these elements, called a **field**, can be accessed by its name.
```cpp
struct Regist{
	int number;
	int quantity;
	float price;
};
```
This is a definition of a class, which serves as the model. We can then create variables of this type and access its fields like so:
```cpp
Regist art1, arg2;
art1.number = 15;
cout << art1.price;
art1.number++;
```
The "." operator has very high priority, so these expressions do not require parentheses. 

It is also possible to assign the fields of one struct to another struct:
```cpp
art1 = art2
```
This copies all the values of art2 to their respective fields in art1.
### Initialization of Structures
**Automatic** structures without explicit initialization will have their values randomized. **Static** class structures have their scalar fields initialized to zero; if some fields are themselves structures, this rule applies to their fields, and so on.

We can initialize a structure like so:
```cpp
Regist art1 = {100, 285, 200};
```
Omitting certain fields will initialize them based on the allocation of the struct (either zero for static or random for automatic). We can also initialize the fields of a structure with the values of another structure of the same type. Constant structures can be initialized too; their fields cannot be modified thereafter.