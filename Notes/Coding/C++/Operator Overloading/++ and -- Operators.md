The **++ operator** (and --) can be over-defined. If it is, both the prefix and postfix operator must be explicitly and individually defined. If T designates a class type and ++ is defined as a member function:
- The header `T operator++()` is used to define the prefix operator.
- The header `T operator++(int)` is used to define the postfix operator. The int-type 