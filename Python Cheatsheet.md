# Python Cheatsheet for Boot.dev Course/General Learning

## Sections
1. [Data Types](#data-types)
    - [Lists](#lists)
    -[Tuples](#tuples)
2. [Object Oriented Programming (OOP)](#object-oriented-programming)
    - [What is Object Oriented Programming?](#what-is-object-oriented-programming)
    - [Concepts](#oop-concepts)
        - [Encapsulation](#encapsulation)
        - [Abstraction](#abstraction)
        - [Inheritance](#inheritance)
        - [Polymorphism](#polymorphism)
3. [Git Repositories](#git-repositories)
    - [git init](#git-init)
    - [git status](#git-status)
    - [git add](#git-status)
    - [git commit](#git-commit)
    - [git switch](#git-switch)
    - [git branch](#git-branch)
    - [git log](#git-log)
    - [git merge](#git-merge)
    - [Rebase vs Merge](rebase-vs-merge)
    - [git push](#git-push)
    - [git remote](#git-remote)
4. [Built-in Functions](#built-in-functions)
    - [map()](#mapfunction-iterable-iterables)
    - [filter()](#filterfunction-iterable)
5. [Functional Programming](#functional-programming)
    - [What is Functional Programming?](#what-is-functional-programming)
    - [Goals of Functional Programming](#goals-of-functional-programming)
    - [Functional and OOP Overlap](#functional-and-oop-overlap)
    - [Pure Functions](#pure-functions)
    - [Side Effects](#side-effects)
        - [I/O](#io)
6. [Markdown/HTML Cheatsheet](#markdownhtml-cheatsheet)
    - [Markdown to HTML Conversions](#markdown-to-html-conversions)
        1. [Headings](#headings)
        2. [Paragraphs](#paragraphs)
        3. [Bold Text](#bold-text)
        4. [Italics](#italics)
        5. [Links](#links)
        6. [Images](#images)
        7. [Unordered Lists](#unordered-lists)
        8. [Ordered Lists](#ordered-lists)
        9. [Quotes](#quotes)
        10. [Code](#code)
99. [Excess Definitons](#excess-definitions)

# Data Types
### This section will go over the different ways to create data structures within Python and various tips and tricks involved with them

## Lists
### Creation
1. `my_list = []`
### `.Contains()` Equivalent
1. See Boots Conversation from [Boot.Dev OOP Lesson CH1 C2](https://www.boot.dev/lessons/d0da006c-6562-41c0-a7f6-93e795cf078d)
2. `string in my_list` --> Returns True/False
3. `string not in my_list` --> Returns True/False
4. When comparing objects, for example an instance of a Book Object that has properties book.title/book.author, using the `x in y` method will check if they are the **EXACT SAME** object in memory. Take the following as an example:

```python
# Book Class - only properties are the title and author of the book
class Book:
    def __init__(self, title, author):
        self.title = title
        self.author = author
# Library Class - has a library name and contains a list of Book objects
class Library:
    def __init__(self, name):
        self.name = name
        self.books = []
    # finds any books that contain the search string in the title
    def remove_books(self, book):
        # THE ISSUE IN QUESTION
main(    
library = Library("My Library")
library.books.append(Book("The Road", "Cormac McCarthy"))
book_to_remove = Book("The Road", "Cormac McCarthy")
results = library.remove_books(book_to_remove) # This will pass in an instance of the Book object to find and remove from the Libraries List of Book instances of the Book object
)
```

1. In the case of having an instance of the book object with properties self.title = "The Road" and self.author = "Cormac McCarthy", how could we remove a book from the library's list of books when we pass an instance of the book object with the same title/author properties?
2. Consider we make the following changes to the .find_books() method:

```python
def remove_books(self, book):
    if book in self.books:
        self.books.remove(book)
main()
```
3. In this case, we're using the `in` operator and this will, as stated previously, check if these are the EXACT SAME object in memory. Considering these are their own unique instances of the book object, they are NOT the same object in memory. They are the equivalent of having two physical copies of the same book. They have the same contents, but occupy their own physical space. In order to work past this, consider the following changes to the remove_books() method:

```python
# In this case, we're instead referencing whether the properties of the instance of the object match, rather than whether the instance of the object itself matches. Saying "is this book's title the same as that book's title?" rather than saying "is that copy of that book the one I had in my apartment?" implying they are the same physical book.
def remove_books(self, book):
    for bk in self.books:
    if bk.title == book.title and bk.author == book.author:
        self.books.remove(bk)
main()
```
## Tuples
* Creation `my_tuple = (1, "hello", 3.14)`
* It is important to note that Tuples are an immutable ordered collection of elements. They act similarly to Lists, however unlike Lists, once they have been created they cannot be modified.
    1. You cannot do things such as the following:
    ```python
    my_tuple = (1, "hello", 3.14)
    my_tuple[0] = 2 # This is not possible, you cannot change the contents of a Tuple without creating an entirely new Tuple.
    ```


# Object Oriented Programming
## What is Object Oriented Programming?
1. A programming style (see: paradigm) that organizes software design around "objects," which are entities that contain both data (attributes) and the functions (methods) that operate on that data

## OOP Concepts

### Encapsulation
1. Encapsulation is about hiding internal state. It focuses on tucking implementation details away so no one depends on them.

### Abstraction
1. Abstraction is about creating a simple interface for complex behavior. It focuses on what's exposed.
    - When writing libraries for use by other developers, getting the abstractions right is crucial because changing them later can be disastrous. Imagine if the maintainers of the random module changed the input parameters to the randrange function! It would break code all over the world.

### Inheritance
1. Inheritance allows one class, the "child" class, to inherit the properties and methods of another class, the "parent" class. This powerful language feature helps us avoid writing a lot of the same code twice. It allows us to DRY (don't repeat yourself) up our code.
    - - This notation dictates that Cow is a child class of Animal and can inherit properties from Animal. Example:
    ```python
    class Animal:
    # parent "Animal" class
    class Cow(Animal):
        # child class "Cow" inherits "Animal"
    ```
    - In this example below we can see that when we instantiate a Cow, we are additionally calling the method from the Animal Class to determine the number of legs the Cow has since legs are a property all of our Animals have. Once that is completed, it then sets Cow specific properties in the Constructor.
    - the `super()` method is what allows a child class to call methods and constructors from it's parent class.
    ```python
    class Animal:
        def __init__(self, num_legs):
            self.num_legs = num_legs
    class Cow(Animal):
        def __init__(self, num_udders):
            # call the parent constructor to give the cow some legs
            super().__init__(4)
            # set cow specific properties
            self.num_udders = num_udders
    ```
2. When a child class inherits from a parent, it inherits **everything**. If you only want to share some functionality, inheritance is probably not the best answer. Better to simply share some functions, or maybe make a new parent class that both classes can inherit from.  
    - All cats are animals, but not all animals are cats. In this case cats could inherit the animal class as it's parent.
    - A good child class is a strict subset of its parent class.

### Polymorphism
1. Polymorphism is the ability of a variable, function or object to take on multiple forms. For example, classes in the same hierarchical tree may have methods with the same name but different behaviors:
```python
class Creature():
def move(self):
    print("the creature moves")
class Dragon(Creature):
    def move(self):
        print("the dragon flies")
class Kraken(Creature):
    def move(self):
        print("the kraken swims")
for creature in [Creature(), Dragon(), Kraken()]:
    creature.move()
# prints:
# the creature moves
# the dragon flies
# the kraken swims
```

- The Dragon and Kraken child classes are overriding the behavior of their parent class's move() method.

2. Overriding functions is an extremely important part of Polymorphism.
    - A function signature (or method signature) includes the name, inputs, and outputs of a function or method. in the example below, both Human and Archer have a method `hit_by_fire`. Both methods have the same name, take 0 inputs, and return integers. If any of those things were different, they would have different function signatures.
    ```python
    class Human:
        def hit_by_fire(self):
            self.health -= 5
            return self.health
    class Archer:
        def hit_by_fire(self):
            self.health -= 10
            return self.health
    # These could be called like so:
    health = sam.hit_by_fire()
    health = archer.hit_by_fire()
    ```
    - If you change the function signature of a parent class when overriding a method, it could be a disaster. The whole point of overriding a method is so that the caller of your code doesn't have to worry about what different things are going on inside the methods of different object types.
    - An example of different function signatures can be seen below where the Archer's implementation of `hit_by_fire` has different arguments than the Human's. You can see it takes an additional argument `dmg`:
    ```python
        class Human:
            def hit_by_fire(self):
                self.health -= 5
                return self.health
        class Archer:
            def hit_by_fire(self, dmg):
                self.health -= dmg
                return self.health
    ```
    - If you change the function signature of a parent class when overriding a method, it could be a disaster. The whole point of overriding a method is so that the caller of your code doesn't have to worry about what different things are going on inside the methods of different object types.
3. Operator Overloading - Another kind of built-in polymorphism in Python is the ability to override how an operator works. For example, the + operator works for built-in types like integers and strings.
    -   Available operators can be seen below:
    ```
    | Operation           | Operator | Method       |
    |---------------------|----------|--------------|
    | Addition            | +        | __add__      |
    | Subtraction         | -        | __sub__      |
    | Multiplication      | *        | __mul__      |
    | Power               | **       | __pow__      |
    | Division            | /        | __truediv__  |
    | Floor Division      | //       | __floordiv__ |
    | Remainder (modulo)  | %        | __mod__      |
    | Bitwise Left Shift  | <<       | __lshift__   |
    | Bitwise Right Shift | >>       | __rshift__   |
    | Bitwise AND         | &        | __and__      |
    | Bitwise OR          | |        | __or__       |
    | Bitwise XOR         | ^        | __xor__      |
    | Bitwise NOT         | ~        | __invert__   |
    ```

# Git Repositories
## `git init`
1. This is the first step in creating a project. This initializes a Git repository in the given directory. This will create a hidden .git file
## `git status`
1. Displays the current status of your repo. This tells you which files are untracked, staged, and committed.
## `git add`
1. This is how we can add files to be tracked for a future commit.
    - `git add <path-to-file | pattern>` can be used to stage specific files to a future commit.
    - `git add .` can be used to stage all untracked files to a future commit.
## `git commit`
1. A commit is a snapshot of the repository at a given point in time. It's a way to save the state of the repository, and it's how Git keeps track of changes to the project. A commit comes with a message that describes the changes made in the commit.
    - `git commit -m "your message here"` This is the general purpose way to commit your changes that you will use 99% of the time. This will commit all staged changes to the current repo branch
## `git swtich`
1. This is how we can switch branches. By default you will be working on main, but you can create new branches using `git switch -c "your branch name"` in order to create a branch that doesn't exist when attempting to switch to it.
## `git branch`
1. This command simply tells you which branch you are running on.
2. You can rename a branch by running the following command: `git branch -m oldname newname`
## `git log`
1. This is used to show the log of commits and branch history within a repository. It's often best to use this with the `--oneline` parameter for better viewing.
    - You can also use `git log --oneline --decorate --graph --parents` to view a visual graphing of the projects branching
## `git merge`
1. This is used to merge a feature branch back into another branch (usually main).
2. Fast Forward commits are merges that do not create a merge commit. 
## Rebase vs Merge
1. Rebasing is the practice of bringin branches back inline with the main commit history creating a linear history.
    - When working alone you're much more likely to create a feature branch, write some code, and then simply rebase it back onto main.
2. Merges keep existing branch structure in the commit history. This preserves the true history of the project.
    - Keeping branch history is much more common when working in team environments.
## `git remote`
1. Manages a set of tracked repositories
2. Add a remote named `<name>` for the repository at `<URL>`. Able to set a remote origin repository.
 - `git remote origin <URL>` is how we can initialize our project to reference a Github Repository and then stage and commit changes to it.
## `git push`
1. This is the command we use once we have staged and committed changes to a branch. We can then push them to a remote repository to update it in our hosted version control.
2. `git push <repository> <src>` is how we would push our changes made to our remote repository.
    - `<repository>` would be the defined remote, such as origin.
    - `<src>` would be the name of the branch we would like to push to said remote, such as main, or a feature branch.

# Built-in Functions
## `map(function, iterable, *iterables)`
### Return an iterator that applies function to every item of iterable, yielding the results.
1. If additional iterables arguments are passed, function must take that many arguments and is applied to the items from all iterables in parallel.
2. The iterator that is returned must be acted upon to be a usable type, if you pass in a List, it will not return a List. For example:
```python
my_list = ["one", "two", "three"]
my_func = lambda x: x.upper()
my_list = list(map(my_func, my_list))
# Returns ["ONE", "TWO", "THREE"]
```

## `filter(function, iterable)`
### Construct an iterator from those elements of iterable for which function is true.
1. Iterable may be either a sequence, a container which supports iteration, or an iterator.
2. The iterator that is returned must be acted upon to be a usable type, if you pass in a List, it will not return a List. For Example:
```python
my_list = ["one", 2, "three", 4]
my_func = lambda x: x.isdigit()
my_list = list(filter(my_func, my_list))
# Returns [2, 4]
```

# Functional Programming
## What is Functional Programming?
1. Functional programming is a style (see: paradigm) of programming where we compose functions instead of mutating state (updating the value of variables).
2. More focused on declaring *what* you want to happen rather than *how* you want it to happen
    - This is different from Imperative Code in the sense that you're declaring both **what and how**
    ```python
    # Example of Imperative Code
    car = create_car()
    car.add_gas(10)
    car.clean_windows()

    # Example of Functional Code
    return clean_windows(add_gas(create_car()))
    ```
    - Important distinction between the two of these is that in the **Functional** example, we are never changing the value of the `car` variable.
    - We simply compose functions that return new values, with the outermost function, `clean_windows` in this case, returning the final result.

## Goals of Functional Programming
1. Make data [immutable](#immutable); once a value is created, it *cannot be changed*
2. Aim to be declarative; we prefer to declare *what* we want the computer to do, rather than muck around with the details of *how* to do it

## Functional and OOP Overlap
1. Three main concepts of programming overlap within both Functional Programming and Object Oriented Programming
    - [Encapsulation](#encapsulation)
    - [Abstraction](#abstraction)
    - [Polymorphism](#polymorphism)
2. The concept that **DOES NOT** overlap between the two is [Inheritance](#inheritance). This is exclusively an Object Oriented Programming concept. It is not seen in Functional Programming due to the mutable classes that come along with it

## Pure Functions
1. Pure functions don't do anything with anything that exists outside of their scope.
    - An impure function might access something like a global variable, whereas a pure function would define that value it requires within it's definiton. Example:
    ```python
    # Pure Function Example
    def find_max(nums):
    max_val = float("-inf")
    for num in nums:
        if max_val < num:
            max_val = num
    return max_val

    # Impure Function Example
    global_max = float("-inf")

    def find_max(nums):
        global global_max
        for num in nums:
            if global_max < num:
                global_max = num
    ```
2. As stated, Pure functions should not modify anything outside it's scope, implying that any values that are inherently passed via reference (Lists, Dictionaries, Sets) must not be altered within the function
    - A pure function would recreate this value that was passed in by doing something similar to the following:
    ```python
    def add_item_to_list(original_list, new_item):
        new_list = original_list.copy() # Preserves the state of the original_list that was passed into the function
        new_list.append(new_item) # Edits the new_list we created within the function definition
        return new_list
    ```
    - You can recreate the value passed via reference by iterating through it and reading its values, or simply use something like `.copy()` if it's available for the data type being used
3. Always returns the same values given the same input
4. Running them causes no [side effects](#side-effects)
5. Pure Functions do not perform any I/O operations such as reading from disk, accessing the internet, or writing to the console
6. Pure Functions are always Referntially Transparent
    - Referential Transparency is a function call can be replaced by its would-be return value because it's the same every time
    - Referentially transparent functions can be safely memoized. For example `add(2, 3)` can be smartly replaced by the value `5` 
## Side Effects
Side effects are actions that are performed within a function that effect values outside of it's scope. In the example below we can see the function is editing the global variable `y` that is set to `5`, but following the function call, `y` has been updated due to the function:
```python
# Side Effect example
y = 5
def add_to_y(x):
    global y
    y += x

add_to_y(3)
# y = 8
```

### I/O
1. I/O stands for Input/Output. This refers to anything that interacts with the "outside world", which refers to anything that is not stored within our application's memory (like variables)
    - All I/O is a form of side effects, this includes the following:
        - Reading from or writing to a file on the harddrive
        - Accessing the Internet
        - Reading from or writing to a database
        - Printing to the console
2. I/O is considered *dirty but necessary* in Functional Programming
    - There should be a clear place in your project for I/O to take place, this may be an Init step or a Cleanup step. Your program might take the following steps to keep this clean:
        1. Read a file from the harddrive as the program starts
        2. Run a handful of Pure Functions to analyze the data
        3. Write the results of the alaysis to a file on the harddrive at the end
### No-Op
1. If a function doesn't return anything, it's probably impure. If it doesn't return anything, the only reason for it to exist is to perform a side effect. However, if it doesn't return anything, and doesn't perform a side effect, it *does nothing*, it's a No-Op
```python
# No-Op example
def square(x):
    x * x
```
# Markdown/HTML Cheatsheet
## Markdown to HTML Conversions
### Headings:
1. Markdown
```md
# Heading 1

## Heading 2

### Heading 3

#### Heading 4

##### Heading 5

###### Heading 6
```
2. HTML
```html 
<h1>Heading 1</h1>
<h2>Heading 2</h2>
<h3>Heading 3</h3>
<h4>Heading 4</h4>
<h5>Heading 5</h5>
<h6>Heading 6</h6>
```
### Paragraphs
1. Markdown
```md
This is a paragraph of text.
```
2. HTML
```html
<p>This is a paragraph of text.</p>
```
### Bold Text
1. Markdown
```md
This is a **bold** word. (or __bold__)
```
2. HTML
```html
<p>This is a <b>bold</b> word.</p>
```
### Italics
1. Markdown
```md
This is an *italic* word. (or _italic_)
```
2. HTML
```html
<p>This is an <i>italic</i> word.</p>
```
### Links
1. Markdown
```md
This is a paragraph with a [link](https://www.google.com).
```
2. HTML
```html
This is a paragraph with a <a href="https://www.google.com">link</a>.
```
### Images
1. Markdown
```md
![alt text for image](url/of/image.jpg)
```
2. HTML
```html
<img src="url/of/image.jpg" alt="Description of image">
```
### Unordered Lists
1. Markdown
```md
* Item 1
* Item 2
* Item 3
```
2. HTML
```html
<ul>
    <li>Item 1</li>
    <li>Item 2</li>
    <li>Item 3</li>
</ul>
```
### Ordered Lists
1. Markdown
```md
1. Item 1
2. Item 2
3. Item 3
```
2. HTML
```html
<ol>
    <li>Item 1</li>
    <li>Item 2</li>
    <li>Item 3</li>
</ol>
```
### Quotes
1. Markdown
> This is a quote.
2. HTML
```html
<blockquote>
    This is a quote.
</blockquote>
```
### Code
1. Markdown

~~~md
```<language>
This is a code block
```
~~~
- Python Example:
    ~~~
    ```python
    int1 = 2
    int2 = 4
    print(int1+int2)
    # prints 6
    ```
    ~~~
2. HTML
```html
<code>This is code</code>
```
# Excess Definitions
#### Immutable
- In [Object Oriented Programming](#object-oriented-programming) (OOP) and [Functional Programming](#functional-programming), an immutable object (unchangeable object) is an object whose state cannot be modified after it is created.
#### Encapsulation
- The practice of bundling data (variables) with the methods that operate on that data into a single unit, typically a class, thereby restricting direct access to the data and allowing controlled manipulation through designated methods, essentially hiding the internal implementation details of an object while providing a public interface to interact with it
#### Abstraction
- The process of simplifying complex systems to make them easier to use. The main goal is to handle complexity by hiding unnecessary details from the user. That enables the user to implement more complex logic on top of the provided abstraction without understanding or even thinking about all the hidden complexity.
#### Polymorphism
- Polymorphism in programming is the ability to use the same interface for different types of data or objects. You can access objects of different types through the same interface. Each type can provide its own independent implementation of this interface.
#### No-Op
- If a function doesn't return anything, it's probably impure. If it doesn't return anything, the only reason for it to exist is to perform a side effect. However, if it doesn't return anything, and doesn't perform a side effect, it *does nothing*, it's a No-Op














