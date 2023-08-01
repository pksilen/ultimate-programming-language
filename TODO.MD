- Ultimate programming language is called Dragon
- Logo image is in shutterstock silensoft favorites folder

- Dragon should enforce single statements like Python lambdas. This will require users to create new functions
  for code that is more than one statement
- Everything in Dragon is immutable by default, mutable must be declared by a specific mutable keyword. It is ugly
  in Java when you need to use a lot of final keyword in the code
- Class member variables/methods are private by default, this reduces the verbosity of having `private` keyword like in Java
  or TypeScript
- Ugly: In Java you need to create a class for everything like a collection of methods without state. You should only write
  function without a class and put functions in a module named like the class. In dependency injection, you can inject different
  modules e.g. for unit testing. You don't need a User Service class, but you can have a 'user service' module (user service.drgn) that contains
  functions like create user, fetch user, delete user.

- Code block syntax is derived from Python:
  - Ugly: Curly braces just make code more verbose and are visual clutter compared to single colon
  - Ugly: Semicolon ending a statement is unnecessary clutter that comes from times when more than one statement
    very put on a single line. This practise is nowadays discouraged in most of the languages and a line should
    contain a single statement only making the ending semicolon unnecessary

- Variable and function/method name syntax is 
  - Python: controller.update_view_with_todos(self.__todos)
  - Dragon: update view with todos using controller. Ugly: Usage of underscores and parenthesis are not needed
  - Dragon: add number to numbers

```
numbers = []
number = ...
index = ...

shape = create Circle Shape with radius of 30
draw shape

new numbers = add number at index to numbers

if ambiguity, e.g there is a method "add number", we must use
add 'number' at index

remove number from numbers
find index of number in numbers
has number in numbers
sort numbers
clear numbers
number count = count numbers
```

Dragon does not have
- if/else statements
- for/while loops
- switch statement

Dragon supports match/switch statement only for factories:

**interface** Logger<br/>
    **function** log(...) returns nothing

**class** Abc Logger implements Logger
**class** Xyz Logger implements Logger

# Language will automatically generate Null Logger class
**class** Null Logger implements Logger

*enum* Logger Type is one of
    Abc
    Xyz

**function** create logger(of logger type: Logger Type) returns Logger
    create Abc Logger if logger type is Abc or
    create Xyz Logger if logger type is Xyz
    otherwise create Null Logger


# Function names returning possibly an Error must begin with _try_ prefix

**function** try fetch user(by id: int) eventually returns User or Error
**function** try to create(user: User) eventually returns nothing or Error


Code in a Todo Model class method:
try to create a _todo_
but on failure show error in view using the _controller_


try fetch user by _id_
and then show user in view using the _controller_
otherwise show error in view using the _controller_

If the 'otherwise' is missing, the compiler gives an error when value returned by the 'and then' method
is not used. (Also the function return value is incorrect, it should be 'nothing') The 'and then' function returns 
a special Promise-kind instance containing the 'otherwise' method

Ugly:
  - Python convention of underscores for private and protected class/instance members
    - Hinders readability and make the code look ugly
    - This is also bad because it is only a convention and member visibility is not really constrained
  - Python "self" and Javascript "this" is ugly and adds verbosity to code in methods. Java convention should be
    used where "this" is at least optional
  - CamelCase is not as readable as snake_case. But using no casing is the best alternative. 
  - Type annotation should be after the function parameter name, not before like in Java


Bad:
- Zero-based indexing => results in off-by-one errors. Dragon uses one-based indexing