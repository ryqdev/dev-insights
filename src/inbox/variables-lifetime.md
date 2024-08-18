
In many scenarios, one variable could be used in several functions. There will be three different kinds of implementations:
- Using Global Variabels
- Passing Local Variables
- Manage the lifetime with OOP

## Global Variables
In summary: It is a bad idea.

In a multi-thread system, it will be a nightmare in parallel computing.

## Local Variables
Passing local variables to functions is the most straightforward way. We can also pass by pointers or references to reduce the overhead.

However, if the system is large and the business logic is complex, we might pass too many variables, which is also not a good idea. 

## Object Oriented Programming (OOP)
In the modern paradigm, object-oriented programming (OOP) is a superior approach for accessing variables across different functions and effectively managing the lifetime of objects.
