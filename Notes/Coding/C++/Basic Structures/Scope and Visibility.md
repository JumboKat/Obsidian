The scope of a variable or function is the area of code in which its name has a particular meaning
- The scope of a **global** variable or function spans the whole program.
- The scope of a **local** variable is confined to the range from its definition to the end of the first composite instruction that contains its definition (or, where the curly braces end).
- Two variables can have the same name but have different scopes; the variable with the most confined scope takes precedence within the range of code its scope presides.
### Visibility
The visibility of a variable or function indicates when it is accessible. It is a component of the scope. The **lifespan** of a variable refers to the period from its inception to its deletion. Lifespan and scope typically coincide.
### Static
A local variable that is declared **static** will be assigned a permanent location in memory. Thus, it retains its value outside of its local instance; its lifespan is that of the entire program. The syntax for declaring a static variable is:
```cpp
static type name;

//example
#include <iostream>
using namespace std;
int main(){
	void fct();
	int n;
	for (n = 1; n <= 5; n++)
		fct();
}
void fct(){
	static int i;
	i++;
	cout << "number call : " << i << endl;
}
```
In the example above, even though i is a local variable declared within the fct() function, its value persists after each subsequent call, so its value will actually increase from 1 to 5. 

Static variables are by default initialized to zero, but we can also explicitly initialize static variables; static variables are only initialized once even if the block of code they preside in is executed multiple times.