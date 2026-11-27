How the = operator is handled depends on whether the derived class over-defines it or not.

If the derived class does not over-define the = operator, a call is automatically made to A's = operator (whether over-defined or default) to handle the inherited A parts, while the members unique to B are handled based on their type (e.g. int uses built-in assignment, strings use String's operator=).

If the derived class does over-define the = operator, A's will not be called. Thus, B's = operator must handle all members of B, including members inherited from A.