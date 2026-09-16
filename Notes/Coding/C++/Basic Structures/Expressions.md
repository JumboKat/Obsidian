Expressions are statements that manipulate data. In C++, any assignment is an expression and is of the form v = expr, where the value of expr is assigned to v and returns the value that was assigned to v. 

For example, i = (j = 0) assigns 0 to j, then assigns 0 to i, then returns i (which is 0).
### Operators
- Arithmetic operators: \*, +, -, / (integer and real division), % (modulo)
- Comparison operators: <, <=, >, >=, \==, !=.
- Boolean operators: && = AND, || = OR, ! = NOT
#### Pre and Post Increment and Decrement
A pre-increment/decrement (++var/--var) updates the variable first, then returns its new value in the same instruction.

A post-increment/decrement (var++/var--) returns its current value, then updates the variable to its new value in the *next* instruction.
Example:
```cpp
n = i++ - 5 //i becomes 6, but n becomes 0
```

Outside of expressions, however, pre and post increments are functionally equivalent.
#### Priorities
Operators of the same-level priority are evaluated from left to right. The priority of operators, from highest to lowest, is as follows:
- (), x\[y], x++ x--
- ++x, --x, !x, -x
- x\*y, x/y x%y
- x+y, x-y
- x >> y, x << y
- x < y, x > y, x >=y, x <= y
- x == y, x != y
- x && y
- x || y
- x = y, x op=y
- x ? y : z (ternary operator, if x is true, then return y; else return z)
### Evaluating Expressions
#### Boolean Expressions
- For the statement e1 && e2, e2 is only assessed if e1 is 'true.'
- For the statement e1 || e2, e2 is only assessed if e1 is 'false.'
#### Arithmetic Expressions
If an expression mixes several types, the result uses the widest type that was used. However, the result can be implicitly converted to match the assigned variable's type.
Example:
```cpp
int i=3, j=2, m;
double r=3.4;
m = (i/j)*r 
// i/j uses integer division, so this becomes 1
// 1 is implicitly converted to 1.0 to perform 1.0*3.4
//since m is an integer, the result becomes m = 3.
```

To avoid errors, we can explicitly convert certain types to another:
```cpp
int i=3, j=2, m;
double r=3.4;
m = (double(i)/j)*r 
//because i's type is wider than j's (double > int), the result of i/j is 1.5.
// 1.5*3.4 = 5.1, so m becomes 5.
```