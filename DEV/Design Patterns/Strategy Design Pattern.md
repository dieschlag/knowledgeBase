[[Behavioral Design Pattern]]

https://www.geeksforgeeks.org/behavioral-design-patterns/
https://www.geeksforgeeks.org/strategy-pattern-set-1/

Defines family of algorithms, encapsulates each one of them, and makes them interchangeable, allowing to switch algorithms dynamically
## Components

![[Pasted image 20250517104843.png]]
### Context

Serves as intermediary between client and strategy, integrated approach for task execution without exposing details of the process.

Maintains reference to a strategy object and calls it when needed, the use of references makes possible the use of interchangeable strategies.

### Strategy Interface

Defines a set of methods that all concretes strategies must implement, Thanks to that, we make sure that all strategies follow the same rules and can be used interchangeably. It also enables decoupling between context and the actual strategies

### Concrete Strategies

Defines concrete algorithms to perform tasks defined in Strategy Interface. They encapsulate the details of the algo and provide a method to execute it.

### Client

