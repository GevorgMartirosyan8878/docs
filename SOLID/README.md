# S.O.L.I.D.

S.O.L.I.D. stands for:

- S - single responsibility principle
- O - open-close principle
- L - liskov substitution principle
- I - interface segregation principle
- D - dependency inversion principle


## Single responsibility principle

Class should have one and one only responsibility.

In example We have class, which haave following functionality

- Open database connection
- Fetch data from database
- Write the data in an external file

The issue with this class is that class doing a lot of things, if in the
feature will happen something from this list:

- new database 
- adopt ORM
- change in the output structure

In this cases class would be changed, 
which might affect into two other implementations too. So ideally we should have 3
classes for each operation, and keep single responsibility principle

Goal of single responsibility principle separate responsibilities between classes
that every class should do only one work, so if we need to make changes into one
of them, it doesn't affect to others functionality



## Open-close principle

Objects or entities should be open for extension and close for modification

What is means is that if the class A is written by the developer AA, 
and if the developer BB wants some modification on that
then developer BB should be easily do that by extending class A, 
but not by modifying class A.

Open for extensions mean that we should be able to add new features or components
to the application, without breaking existing code.

Close for modification means that we should not introduce the breaking changes
to existing functionality, because it will force you to refactor a lot of existing code


Problem

Changing the current behaviour of class will affect all system that uses the class

Goal

We should extend the classes instead od changing them, this will help us
to avoid the bugs in that places that class is used


## Liskov Substitution

If class or entity A is a subtype of B, it's mean that places, in which class A
have a usage we can replace with class B without changing some implementation.

If we have a Class and create another class from it, it becomes a parent and 
new class becomes to child. The child class should be able to do everything
that parent class can do.


- If you override a method of the superclass in the subclass,
these method’s parameters should match with the superclass’ method
or should be more abstract.


- If you override a method of the superclass in the subclass,
this method should return the same type with the superclass’s
method or should be subclass of the superclass method’s return type.


- If you override a method of the superclass in the subclass,
the method in a subclass shouldn’t throw types of exceptions
which the base method isn’t expected to throw


- If you override a method of the superclass in the subclass,
this method shouldn’t strengthen pre-conditions of superclass’ method.


- If you override a method of the superclass in the subclass,
this method shouldn’t weaken post-conditions of the superclass’ method


- A subclass shouldn’t change the values of the private fields of the superclass.


## Interface segregation principle

A client should never be forced to implement the interface that it doesn't use
or shouldn't be forced to depend on methods they don't use.

Just for example if we are specifying some objects with the same interface,
and some of the fields or methods in some entity doesn't need to implement, 
it's mean we need to create new interface for that entity, 
instead of making that field or method optional.
They need to implement that interface that really they use in at all.

## Dependency inversion principle

Entities must depend abstractions. It states that high level module must not
depend on low level module, but they both should depend on abstractions. 
Also abstractions should not depend on the details, details should depend on abstractions.

High-level classes are more strategic classes and contain business logic.

Low-level classes are more specific and operational classes like connecting databases
