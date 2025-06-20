Introduced by Bertrand Meyer.

Software entities should be open for extension but closed to modification: devs should be able to add new functionality without changing its existing implementation.

Can be done through abstraction, inheritance, polymorphism.

Benefits:
- improved maintainability
- reduced risk of bugs
- enhanced flexibility

Things to consider to implement open/closed principle:
- use abstraction: define abstract classes/interfaces to represent common behaviors
- polymorphism: allows different implementations to be used interchangeably
- avoid tight coupling: design classes to be loosely coupled
- follow [[Design Patterns|design patterns]]: use [[Strategy Design Pattern|Strategy]], [[Template Method|Templates]], [[Visitor]] design patterns