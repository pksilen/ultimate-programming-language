- Ultimate programming language is called Dragon
- Logo image is in shutterstock silensoft favorites folder
  - Logo font Bebas Neue (Google fonts)

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
- In Dragon you can add articles like 'a' and 'the' freely
- Dragon uses 'is' instead of ==, so that it is not possible to write accidentally = instead of ==

- Code block syntax is derived from Python:
  - Ugly: Curly braces just make code more verbose and are visual clutter compared to single colon
  - Ugly: Semicolon ending a statement is unnecessary clutter that comes from times when more than one statement
    very put on a single line. This practise is nowadays discouraged in most of the languages and a line should
    contain a single statement only making the ending semicolon unnecessary

- Variable and function/method name syntax is 
  - Python: controller.update_view_with_todos(self.__todos)
  - Dragon: update view with todos using controller. Ugly: Usage of underscores and parenthesis are not needed
  - Dragon: add number to numbers

- Exception classes in Dragon are created automatically when you use a class name that ends in Error it is automatically 
  generated and extended from base "Error" class. 

```
numbers = []
number = ...
index = ...

number is between [1, 255]
number is between [1, 255[

shape = Circle Shape(radius: 30)
draw shape

data struct Shape Options
    @positive
    @optional
    stroke width: int
    
    @hexcolor
    @optional
    fill color: string

default shape options = valid {
    stroke width: 1
    fill color: '#555555'
}

If valid keyword is not given, we get Either<ShapeOptions, Error>

shape options|error = {
    stroke width: stroke width from environment variables 
    fill color: fill color from environment variables
}

# Properties are by default private and immutable
class Circle Shape implements Shape
    constructor(
        property radius: int,
        property shape options: Shape Options? = default shape options
    )
    
public mutable property radius: int


Shape Options? == Maybe<Shape Options> == Either<Shape Options, nothing>

circle = Circle Shape(radius: 30, left of shape options|error otherwise nothing)
mutable circle = Circle Shape(radius: 30, left of shape options|error otherwise nothing)

new numbers = add _number_ at _index_ to _numbers_

if ambiguity, e.g there is a method "add number", we must use
add 'number' at index

new numbers = remove number from numbers
index = find first index of number in numbers
has number = has number in numbers
sorted numbers = sort numbers
clear numbers (only for a mutable list)
number count = count numbers
```

Dragon does not have
- if/else statements
- for/while loops
- switch statement

Dragon supports match/switch statement only for factories:

**interface** Logger<br/>
    **function** _log_(...) returns nothing

**class** Abc Logger implements Logger
**class** Xyz Logger implements Logger

# Language will automatically generate Null Logger class
**class** Null Logger implements Logger

*enumerated type* Logger Type is one of
    Abc
    Xyz

**function** create logger(of logger type: Logger Type) returns Logger
    create Abc Logger if logger type is Abc or
    create Xyz Logger if logger type is Xyz
    otherwise create Null Logger

# Function names returning possibly an Error must begin with _try_ prefix

user service module:
**function** _try to fetch user_(by id: int) eventually returns Either<User, Error>
**function** _try to create_(a user: User) eventually returns Either<nothing, Error>

todo service module:
**function** _try to fetch todos_ eventually returns Either<Todo[], Error>
**function** _try to create_(a todo: Todo) eventually returns Either<nothing, Error>

Code in Todo View class:

**data struct** Todo
    id: int

    @max length(256)
    description: string

    is done: boolean

InputTodo = Todo without id

# The below todo variable is immutable be default
# When todo variable is assigned we assure that data structure is valid, because
# we validate the description in other place
todo: InputTodo = valid {
    description
    is done: false
} 
create _todo_ using the _controller_

class Todo Controller
    function create(a todo: Todo) returns nothing
    function show(the todos: Todo List) in the view returns nothing
    function show(an error: Error) in the view returns nothing

Code in a Todo Model class method:
try to create a _todo_
but on failure show an error in the view using the _controller_

try to fetch todos
and then show the todos in the view using the _controller_
otherwise show an error in the view using the _controller_

class User Controller
    function show(the user: User) in the view returns nothing
    function show(an error: Error) in the view returns nothing

try to fetch user by _id_
and then show the user in the view using the _controller_
otherwise show an error in the view using the _controller_

If the 'otherwise' is missing, the compiler gives an error when value returned by the 'and then' method
is not used. (Also the function return value is incorrect, it should be 'nothing') The 'and then' function returns 
a special Promise-kind instance containing the 'otherwise' method

Backend User service class:

user|error = wait: try to get user by _id_ from the _repository_
return left of user|error otherwise Not Found Error(entity: 'User', id)
(return user|error.left() ?? new NotFoundError(entity: 'User', id)))

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