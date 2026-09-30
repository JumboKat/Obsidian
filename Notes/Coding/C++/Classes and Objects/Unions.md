A **union** is similar to a [[Structures||structure]], except that it has a single memory location that is shared by all of its fields (which can be of different types). We can define one as such:
```cpp
union Test{
	long n;
	float x;
} u; //shorthand for declaring a variable of type Test immediately after
```
This reserves a space in memory equal to the size of the union's largest field type. A union can have any number of fields of any type (including structures). Because there is only one memory address, only one field can hold a value at any time. Reassigning *any* field's value will replace the one held in memory:
```cpp
int main(){
	test z;
	z.x = 10.5;
	z.n = 20;
	cout << z.x << endl; //this outputs a random number; this field's value was overwritten.
	cout << z.x << endl; //this outputs 20
}
```

