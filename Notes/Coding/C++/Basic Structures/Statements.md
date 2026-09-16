A statement is a complete, executable unit of code. Standard statements include:
- definition of variables, functions, and types.
- expr; (any expression ending with a ;)
- {list of instructions} - composite instructions that are executed together as a block of code and are grouped together by **{ }**.
	- This includes if and if/else statements, as well as loops.
### Loops
The **for** loop:
- executes for each iteration in a range of numbers/values.
- syntax: for (expr1; expr2; expr3) instr
- expr1 assigns a local variable, expr2 defines the stop condition, expr3 is the end statement to execute at the end of each loop:
```cpp
for(i=0; i<235;i=i+1) cout << T[i];

//we can nest loops in the same statement
for(i=0, j=1; i<235; i=i+1, j=j+3) cout << T[i][j];
```
The **while** loop:
- executes while an expression evaluates to true.
- syntax: while (expr) instr
The **do while** loop:
- executes once, then loops while an expression evalutes to true.
- syntax: do instr while (expr)

- **break** exits the loop.
- **continue** exist the current iteration and switches to the beginning of the next one.
### Other Statements
A **switch case** statement evaluates an expression, then executes the code corresponding to the matching result of that expression (a case). A default case is defined in case the expression matches none of the ones explicitly defined.
- Syntax:
```cpp
switch (expr) {
	case expr1: list of statements;
	...
	case exprn: list of statements;
	default: list of statements;
}
```

The **ternary operator** is of the form:
```cpp
(exp) ? exp1 : exp2
```
- exp1 is returned if exp evaluates to true, else exp2 is returned.

The form ==var op= expr== is shorthand for ==var = var op expr== (op being either +,-,\*,/,%)
Examples:
- i +=2 is equivalent to i = i + 2;
- j \*=10 is equivalent to j = j \*10
- j \*= (i++) is equivalent to j = j\* i, then i = i + 1.