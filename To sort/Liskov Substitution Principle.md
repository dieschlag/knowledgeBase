Extends the Open/Closed Principle, focuses on behavior of a superclass and its subtypes.

introduced by Barbara Liskov:

Let _Φ(x)_ be a property provable about objects _x_ of type _T_. Then _Φ(y)_ should be true for objects _y_ of type _S_ where _S_ is a subtype of _T_.

=> objects of a subclass shall be replaced with objects of its subclasses without breaking the app

Enforcing: not really possible with the IDE, since it only deals with structural checking, but it can be tested with testing, where you implement a test working with subclasses instead of expected class in a part of your app.

