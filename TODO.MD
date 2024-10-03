- Ultimate programming language is called Dragon
- Logo image is in shutterstock silensoft favorites folder
  - Logo font Bebas Neue (Google fonts)

- Dragon should enforce single statements like Python lambdas. This will require users to create new functions
  for code that is more than one statement
- Everything in Dragon is immutable by default, mutable must be declared by a specific mutable keyword. It is ugly
  in Java when you need to use a lot of final keyword in the code
- Class member variables/methods are private by default, this reduces the verbosity of having `private` keyword like in Java
  or TypeScript
- May be wrong: (Ugly: In Java you need to create a class for everything like a collection of methods without state. You should only write
  function without a class and put functions in a module named like the class. In dependency injection, you can inject different
  modules e.g. for unit testing. You don't need a User Service class, but you can have a 'user service' module (user service.drgn) that contains
  functions like create user, fetch user, delete user.)
- Dragon should contain functionality needed for any non-trivial application, e.g. DI or unit testing should not come from separate 3rd party libs
  - Dependency injection is built-in in the language
    - Dependency injection has a DI container where dependencies are defined by name, 
        e.g. 'user service' = a Singleton(of User Service)
        e.g. 'other_dep' = an Instance(of Another Class)
        class User Controller 
            user service = user service from DI container
- Unit testing is built-in in the laguages
- Integration testing language (Gherkin style ) is built-in in the language
  - Sometimes the "When" keyword could be replaced with "After" keyword, e.g. "AFTER 15 seconds is waited, THEN ..."
- In Dragon you can add articles like 'a' and 'the' freely
- Dragon IDE tries to complete a function call after first word is written, e.g. if user writes 'add' and there
  is a list variable "number" and "numbers" in the scope, the IDE can suggest "add number to numbers" and user
  can easily complete the sentence with a tab key.
- Dragon uses 'is' instead of ==, so that it is not possible to write accidentally = instead of ==
- Data types: boolean, int, number, float, string, nothing
- Automatic data conversions when ever possible: e.g. boolean -> int/number/float/string, int/number/float -> string/boolean
- "not" in expression can be anywhere, e.g.
```
class Message Buffer
    function write(message: Message) to buf if not full returns Either<nothing, Error>
        buffer is full = messages' length >= max length
        add message to messages if buffer is not full else return a Buffer Full Error
```

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

shape = Circle Shape(radius 30)
draw shape

data struct Shape Options
    @positive
    stroke width: Maybe<int> = 1
    
    @hexcolor
    fill color: Maybe<string>

default shape options = valid {
    stroke width: 1
    fill color: '#555555'
}

If the 'valid' keyword is not given, we get Either<ShapeOptions, Error>

stroke width string = get 'STROKE_WIDTH' value from environment variables
shape options|error = {
    stroke width: an int(parsed from: stroke width string)
    fill color: get 'FILL_COLOR' value from environment variables
}

Dragon allows only one function call per line to avoid unreadable statements like:
an int(get 'STROKE_WIDTH' value from environment variables)

# Properties are by default private and immutable
# In other languages it is ugly to always define "private readonly" (TypeSCript) or "private final" in Java
class Circle Shape implements Shape
    Circle Shape(
        property radius: int,
        property shape options: Maybe<Shape Options> = default shape options
    )
    
public mutable property radius: int

circle = a Circle Shape(radius 30, left of shape options|error)
mutable circle = a Circle Shape(radius 30, left of shape options|error)

new numbers = add _number_ at _index_ to _numbers_

if ambiguity, e.g there is a method "add number", we must use
add 'number' at index

new numbers = remove the _number_ from numbers
index = find first index of number in numbers
has number = has number in numbers
sorted numbers = sort numbers
clear numbers (only for a mutable list)
number count = length of numbers
number count = numbers' length
```

interface List<T>
    function sort this returns List<T>
    function has (a value: T) in this returns boolean
    function find first index(of value: T) in this returns int
    function remove(the value: T) from this returns List<T>
    function length of this returns int
    function this' length returns int


class Either<TLeft, TRight where TLeft != TRight>
    Either(of value: TLeft)
    Either(of value: TRight)
    function left of this returns Maybe<TLeft>
    function right of this returns Maybe<TRight>
    function map left of this(to value: TLeft -> U) returns Maybe<U>
    function map right of this(to value: TRight -> U) returns Maybe<U>

user = ...
error = ...
user|error = an Either<User, Error>(of user)
user|error = an Either<User, Error>(of error)

username = user -> user.name
maybe username = map left of user|error to username

Dragon does not have
- if/else statements
- for/while loops
- switch statement

Dragon supports match/switch statement only for factories:

*enumerated type* Log Level is one of
    Error
    Warning
    Info
    Debug

**actor interface** Logger<br/>
    **function** _logs_(a message: string, at level: Log Level) and returns nothing

**actor class** Standard Output Logger implements Logger
**actor class** File Logger implements Logger

logger = a Standard Output Logger
logger logs 'Application started' at level Info

# Language will automatically generate Nothing Logger class
**actor class** Nothing Logger implements Logger

*enumerated type* Logger Type is one of
    Standard Output
    File

public **function** create a logger(of logger type: Logger Type) returns Logger
    return a Standard Output Logger if logger type is Standard Output
           or a File Logger if logger type is File
           otherwise a Nothing Logger

std out logger = create a logger of logger type Standard Output
file logger = create a logger of logger type File

# Function names returning possibly an Error must begin with _try_ prefix

user service actor class:
**function** _tries to fetch user_(by id: int) and eventually returns Either<User, Error>
**function** _tries to create_(a user: User) and eventually returns Either<nothing, Error>

todo service actor class:
**function** _tries to fetch todos_() and eventually returns Either<Todo[], Error>
**function** _tries to create_(a todo: Todo) and eventually returns Either<nothing, Error>

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
controller creates a _todo_

actor class Todo Controller
    public function creates(a todo: Todo) and returns nothing
    public function shows(the todos: Todo[]) in the view and returns nothing 
    public function shows(an error: Error) in the view and returns nothing

Code in a Todo Model class method:
todo service tries to create a _todo_
but on failure controller shows an error in the view

todo service tries to fetch todos
and then controller shows the todos in the view
otherwise controller shows an error in the view

actor class User Controller
    public function shows(the user: User) in the view and returns nothing
    public function shows(an error: Error) in the view and returns nothing

user service tries to fetch user by _id_
and then controller shows the user in the view
otherwise controller shows an error in the view

If the 'otherwise' is missing, the compiler gives an error when value returned by the 'and then' method
is not used. (Also the function return value is incorrect, it should be 'nothing') The 'and then' function returns 
a special Promise-kind instance containing the 'otherwise' method

Backend User service class:

# wait while is a keyword
user|error = wait while repository tries to get user by _id_
return left of user|error otherwise a Not Found Error(entity: 'User', id)
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


Examples:

```
_drivable_ must implement:
  drive to a _destination_: Location

_cargo carrying_ must implement:
  carry _amount_ kgs of cargo

_automobile_ is _drivable_ and _cargo carrying_

_car_ has:
   engine: Engine
   steering wheel: Steering Wheel
   wheels: Wheels

car is _drivable_:
  drive to a _destination_: Location

car is _cargo Carrying_:
   carry _amount_ kgs of cargo

car object behaves:
  ...
  ...


import console

_value_ is 100
get _signedness_ of _value_
print _signedness_ to _console_

get _signedness_:string of integer:_value_:
  when _value_ is
     zero, then return "zero"
     less than zero, then return "negative"
     greater than zero, then return "positive"
```
