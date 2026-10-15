The **auto** keyword is used to ask the compiler to decide an instance's type.
```cpp
int i = 3;
const int ci = 2;
auto a = i;
auto b = ci;
const auto c = ci;
decltype(i) d;
decltype(ci) dd;
```

**decltype()** can be used to do the same thing, but without initializing the variable.