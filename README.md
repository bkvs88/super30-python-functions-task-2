# super30-python-functions-task-2
Advanced Function Challenges

## Table of Contents
1. [*args](#args)
2. [**kwargs](#kwargs)
3. [Lambda](#lambda)
4. [map()](#map)
5. [filter()](#filter)
6. [Recursion](#recursion)
7. [Local/Global Scope Variables](#localglobal-scope-variables)

---

## *args

### Overview
`*args` allows a function to accept a variable number of **non-keyword arguments**. The asterisk (`*`) unpacks the arguments into a tuple.

### Syntax
```python
def function_name(*args):
    # args is a tuple
    pass
```

### Key Points
- `args` is just a convention; you can use `*numbers`, `*items`, etc.
- Arguments are passed as a **tuple** inside the function
- Useful when you don't know how many arguments will be passed
- The `*` operator unpacks the arguments

### Example
```python
def print_args(*args):
    print(f"Type: {type(args)}")  # <class 'tuple'>
    for i, arg in enumerate(args):
        print(f"Argument {i}: {arg}")

print_args(1, 2, 3, "hello")
# Output:
# Type: <class 'tuple'>
# Argument 0: 1
# Argument 1: 2
# Argument 2: 3
# Argument 3: hello

def sum_all(*args):
    return sum(args)

print(sum_all(1, 2, 3, 4, 5))  # Output: 15
```

### When to Use
- Functions that need to accept varying numbers of arguments
- Creating flexible APIs where users can pass multiple values
- Functions like `print(*objects)`, `max(*args)`, `min(*args)`

---

## **kwargs

### Overview
`**kwargs` allows a function to accept a variable number of **keyword arguments**. The double asterisks (`**`) unpack the arguments into a dictionary.

### Syntax
```python
def function_name(**kwargs):
    # kwargs is a dictionary
    pass
```

### Key Points
- `kwargs` is just a convention; you can use `**options`, `**settings`, etc.
- Arguments are passed as a **dictionary** inside the function
- Keys are the parameter names, values are the arguments passed
- Useful for handling named parameters flexibly

### Example
```python
def print_kwargs(**kwargs):
    print(f"Type: {type(kwargs)}")  # <class 'dict'>
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_kwargs(name="Alice", age=25, city="New York")
# Output:
# Type: <class 'dict'>
# name: Alice
# age: 25
# city: New York

def create_user(**kwargs):
    user = {}
    for key, value in kwargs.items():
        user[key] = value
    return user

result = create_user(name="Bob", email="bob@example.com", role="admin")
print(result)
# Output: {'name': 'Bob', 'email': 'bob@example.com', 'role': 'admin'}
```

### When to Use
- Functions that accept optional named parameters
- Configuration functions where you pass settings as key-value pairs
- Creating flexible APIs with dynamic attributes

---

## Combining *args and **kwargs

You can use both together in a function:

```python
def flexible_function(*args, **kwargs):
    print("Positional arguments:", args)
    print("Keyword arguments:", kwargs)

flexible_function(1, 2, 3, name="Alice", age=30)
# Output:
# Positional arguments: (1, 2, 3)
# Keyword arguments: {'name': 'Alice', 'age': 30}
```

---

## Lambda

### Overview
A lambda is a **small anonymous function** defined with the `lambda` keyword. It's useful for short, simple operations that you use only once or pass as arguments.

### Syntax
```python
lambda arguments: expression
```

### Key Points
- Lambda functions can take any number of arguments but only one expression
- The expression is automatically returned
- No `return` keyword needed
- Useful with higher-order functions like `map()`, `filter()`, `sorted()`
- Cannot contain multiple statements or complex logic

### Example
```python
# Simple lambda
square = lambda x: x ** 2
print(square(5))  # Output: 25

# Lambda with multiple arguments
add = lambda x, y: x + y
print(add(3, 4))  # Output: 7

# Lambda with default arguments
greet = lambda name="Guest": f"Hello, {name}!"
print(greet())  # Output: Hello, Guest!
print(greet("Alice"))  # Output: Hello, Alice!

# Sorting with lambda
students = [("Alice", 85), ("Bob", 90), ("Charlie", 78)]
sorted_by_score = sorted(students, key=lambda student: student[1])
print(sorted_by_score)
# Output: [('Charlie', 78), ('Alice', 85), ('Bob', 90)]
```

### When to Use
- One-time use functions
- As arguments to `map()`, `filter()`, `sorted()`, `max()`, `min()`
- Simple transformations or conditions
- Callbacks in data processing

### When NOT to Use
- Complex logic (use `def` instead)
- Code that needs documentation
- Functions used multiple times throughout your code

---

## map()

### Overview
`map()` applies a function to every item in an iterable and returns a map object (which can be converted to a list, tuple, etc.).

### Syntax
```python
map(function, iterable)
```

### Key Points
- Returns a **map object** (lazy evaluation), not a list
- The function is applied to each element
- Commonly used with lambda functions
- Can accept multiple iterables

### Example
```python
# Basic map with lambda
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x ** 2, numbers))
print(squared)  # Output: [1, 4, 9, 16, 25]

# Map with built-in function
strings = ["1", "2", "3", "4"]
converted = list(map(int, strings))
print(converted)  # Output: [1, 2, 3, 4]

# Map with custom function
def double(x):
    return x * 2

numbers = [1, 2, 3, 4]
doubled = list(map(double, numbers))
print(doubled)  # Output: [2, 4, 6, 8]

# Map with multiple iterables
list1 = [1, 2, 3]
list2 = [4, 5, 6]
result = list(map(lambda x, y: x + y, list1, list2))
print(result)  # Output: [5, 7, 9]
```

### When to Use
- Transforming all items in a collection
- Converting between data types (e.g., string to int)
- Applying a function to multiple lists simultaneously

---

## filter()

### Overview
`filter()` creates an iterator that filters elements from an iterable based on a function that returns `True` or `False`.

### Syntax
```python
filter(function, iterable)
```

### Key Points
- Returns a **filter object** (lazy evaluation), not a list
- The function should return a boolean value
- If function is `None`, items that are truthy are kept
- Commonly used with lambda functions

### Example
```python
# Filter with lambda
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
even_numbers = list(filter(lambda x: x % 2 == 0, numbers))
print(even_numbers)  # Output: [2, 4, 6, 8, 10]

# Filter with custom function
def is_positive(x):
    return x > 0

numbers = [-3, -1, 0, 2, 5]
positive = list(filter(is_positive, numbers))
print(positive)  # Output: [2, 5]

# Filter with None (keeps truthy values)
values = [0, 1, False, True, "", "hello", None, 42]
truthy_values = list(filter(None, values))
print(truthy_values)  # Output: [1, True, 'hello', 42]

# Filter strings
words = ["apple", "a", "banana", "cat", "elephant"]
long_words = list(filter(lambda word: len(word) > 3, words))
print(long_words)  # Output: ['apple', 'banana', 'elephant']
```

### When to Use
- Selecting elements that meet certain criteria
- Removing unwanted items from a collection
- Data validation and cleaning

---

## Recursion

### Overview
Recursion is a technique where a function calls itself to solve a problem. A recursive function must have a **base case** (when to stop) and a **recursive case** (when to call itself).

### Key Components
1. **Base Case**: The condition that stops the recursion
2. **Recursive Case**: The function calling itself with modified arguments
3. **Return Values**: Must eventually return a value

### Example
```python
# Simple recursion: factorial
def factorial(n):
    # Base case
    if n <= 1:
        return 1
    # Recursive case
    else:
        return n * factorial(n - 1)

print(factorial(5))  # Output: 120 (5 * 4 * 3 * 2 * 1)

# Fibonacci sequence
def fibonacci(n):
    # Base cases
    if n <= 0:
        return 0
    elif n == 1:
        return 1
    # Recursive case
    else:
        return fibonacci(n - 1) + fibonacci(n - 2)

print(fibonacci(6))  # Output: 8 (0, 1, 1, 2, 3, 5, 8)

# Sum of list elements
def sum_list(lst):
    # Base case
    if len(lst) == 0:
        return 0
    # Recursive case
    else:
        return lst[0] + sum_list(lst[1:])

print(sum_list([1, 2, 3, 4, 5]))  # Output: 15

# Binary search
def binary_search(arr, target, left=0, right=None):
    if right is None:
        right = len(arr) - 1
    
    # Base case: not found
    if left > right:
        return -1
    
    mid = (left + right) // 2
    
    # Base case: found
    if arr[mid] == target:
        return mid
    
    # Recursive cases
    elif arr[mid] > target:
        return binary_search(arr, target, left, mid - 1)
    else:
        return binary_search(arr, target, mid + 1, right)

print(binary_search([1, 3, 5, 7, 9, 11], 7))  # Output: 3
```

### Advantages
- Elegant solution for naturally recursive problems (trees, graphs, divide-and-conquer)
- Code is often cleaner and more intuitive
- Mirrors the problem structure

### Disadvantages
- Can be slower than iteration
- Uses more memory (call stack)
- Risk of stack overflow with deep recursion
- Can be harder to debug

### When to Use
- Tree and graph traversal
- Divide-and-conquer algorithms
- Problems naturally described recursively
- Dynamic programming (with memoization)

### When NOT to Use
- Simple iteration is better
- Working with very large datasets (stack overflow risk)
- Performance-critical code

---

## Local/Global Scope Variables

### Overview
**Scope** determines where a variable can be accessed. Python has different levels of scope: **Local**, **Enclosing**, **Global**, and **Built-in** (the LEGB rule).

### Scope Levels
1. **Local**: Inside a function
2. **Enclosing**: In an outer function (for nested functions)
3. **Global**: At the module level (top-level)
4. **Built-in**: Built-in Python functions and constants

### Key Points
- **Local variables** are created inside functions and only exist within that function
- **Global variables** are created at the module level and can be accessed anywhere
- Use `global` keyword to modify a global variable inside a function
- Use `nonlocal` keyword to modify an enclosing scope variable inside a nested function

### Example
```python
# Global variable
global_var = "I'm global"

def outer_function():
    # Enclosing variable
    enclosing_var = "I'm enclosing"
    
    def inner_function():
        # Local variable
        local_var = "I'm local"
        
        print(local_var)        # Works (local scope)
        print(enclosing_var)    # Works (enclosing scope)
        print(global_var)       # Works (global scope)
    
    inner_function()

outer_function()

# Accessing global variable
print(global_var)  # Output: I'm global

# This will cause an error - local variables don't exist outside function
# print(local_var)  # NameError


# Using the 'global' keyword
counter = 0

def increment():
    global counter
    counter += 1
    print(f"Counter: {counter}")

increment()  # Output: Counter: 1
increment()  # Output: Counter: 2
print(counter)  # Output: 2


# Using the 'nonlocal' keyword
def outer():
    value = 10
    
    def inner():
        nonlocal value
        value += 5
        print(f"Inside inner: {value}")
    
    inner()
    print(f"Outside inner: {value}")

outer()
# Output:
# Inside inner: 15
# Outside inner: 15


# Variable shadowing (local hides global)
name = "Global Name"

def test_shadowing():
    name = "Local Name"  # This is a different variable
    print(name)  # Output: Local Name

test_shadowing()
print(name)  # Output: Global Name
```

### LEGB Rule (Order of Variable Lookup)
Python searches for variables in this order:
1. **Local** - Inside the current function
2. **Enclosing** - In outer functions (for nested functions)
3. **Global** - At module level
4. **Built-in** - Python's built-in names

```python
x = "Global"

def outer():
    x = "Enclosing"
    
    def inner():
        x = "Local"
        print(x)  # Output: Local (found in Local scope)
    
    inner()
    print(x)  # Output: Enclosing (found in Enclosing scope)

outer()
print(x)  # Output: Global (found in Global scope)
```

### Best Practices
- **Minimize global variables** - they can make code harder to understand and debug
- **Use function parameters** instead of global variables when possible
- **Use `global` sparingly** - it can make code confusing
- **Prefer passing data as function arguments** - makes dependencies clear
- **Use `nonlocal` for closures** - when you need to modify outer scope variables

---

## Summary Table

| Topic | Type | Use Case | Example |
|-------|------|----------|---------|
| *args | Function Parameter | Variable positional arguments | `func(1, 2, 3)` |
| **kwargs | Function Parameter | Variable keyword arguments | `func(name="Alice", age=30)` |
| Lambda | Anonymous Function | One-liner function | `lambda x: x ** 2` |
| map() | Higher-order Function | Transform all items | `map(lambda x: x*2, [1,2,3])` |
| filter() | Higher-order Function | Select items by condition | `filter(lambda x: x>5, [1,8,3,9])` |
| Recursion | Function Technique | Self-referential problems | `factorial(n) = n * factorial(n-1)` |
| Scope | Variable Concept | Variable accessibility | `global`, `nonlocal`, local |

---

## Practice Challenges

1. Create a function using `*args` that calculates the average of any number of arguments
2. Write a function using `**kwargs` that builds a dictionary of settings
3. Use `lambda` with `sorted()` to sort a list of dictionaries by a specific key
4. Use `map()` to convert a list of temperatures from Celsius to Fahrenheit
5. Use `filter()` to find all prime numbers in a list
6. Write a recursive function to flatten a nested list
7. Demonstrate the difference between local and global scope with an example

---

## Resources
- [Python Official Documentation - Functions](https://docs.python.org/3/tutorial/controlflow.html#defining-functions)
- [Real Python - *args and **kwargs](https://realpython.com/python-args-kwargs/)
- [Python Lambda Functions](https://realpython.com/lambda-functions/)
- [Recursion in Python](https://realpython.com/python-recursion/)
