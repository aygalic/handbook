# Python and Computer Science Fundamentals

## Data Structures in Python

### Lists vs. Tuples
Lists and tuples are both sequence types in Python, but they have crucial differences:

* **Mutability**
    - Lists are mutable: Elements can be modified, added, or removed after creation
    - Tuples are immutable: Once created, elements cannot be modified
    
* **Syntax**
    - Lists use square brackets: `my_list = [1, 2, 3]`
    - Tuples use parentheses: `my_tuple = (1, 2, 3)`

* **Memory and Performance**
    - Tuples generally consume less memory than lists
    - Tuples are slightly faster to access and iterate over
    - Lists require extra memory for potential growth

* **Use Cases**
    - Lists: When you need a collection that will change (e.g., growing dataset)
    - Tuples: When data shouldn't change (e.g., coordinates, database records)

### Sets and Dictionaries

#### Sets
* Unordered collections of unique elements
* Useful for:
    - Removing duplicates
    - Set operations (union, intersection, difference)
    - Membership testing (faster than lists)
* Example:
```python
my_set = {1, 2, 3}
other_set = {3, 4, 5}
union_set = my_set | other_set  # {1, 2, 3, 4, 5}
intersection = my_set & other_set  # {3}
```

#### Dictionaries
* Key-value pairs with O(1) lookup time
* Keys must be immutable (strings, numbers, tuples)
* Common operations:
```python
# Creation and access
my_dict = {'a': 1, 'b': 2}
value = my_dict['a']  # Direct access
value = my_dict.get('c', 0)  # Safe access with default

# Methods
keys = my_dict.keys()
values = my_dict.values()
items = my_dict.items()
```

## Iteration and Generators

### Iterators
* An iterator is an object that implements:
    - `__iter__()`: Returns the iterator object itself
    - `__next__()`: Returns the next value or raises StopIteration
* Used in for loops and comprehensions
* Example:
```python
class CountUpTo:
    def __init__(self, max_value):
        self.max_value = max_value
        self.current = 0
        
    def __iter__(self):
        return self
        
    def __next__(self):
        if self.current >= self.max_value:
            raise StopIteration
        self.current += 1
        return self.current

# Usage
for num in CountUpTo(3):
    print(num)  # Prints 1, 2, 3
```

### Generators
* Special type of iterator created using functions with `yield`
* Memory efficient: Values generated on-the-fly
* State is preserved between calls
* Example:
```python
def fibonacci(n):
    a, b = 0, 1
    for _ in range(n):
        yield a
        a, b = b, a + b

# Usage
for num in fibonacci(5):
    print(num)  # Prints 0, 1, 1, 2, 3
```

### List Comprehensions vs. Generator Expressions
```python
# List comprehension (creates full list in memory)
squares_list = [x**2 for x in range(1000)]

# Generator expression (generates values on demand)
squares_gen = (x**2 for x in range(1000))
```

## Memory Management

### Reference Counting and Garbage Collection
* Python uses reference counting for memory management
* Objects are deallocated when reference count reaches zero
* Circular references handled by garbage collector
* Example:
```python
# Reference counting
x = [1, 2, 3]  # ref count: 1
y = x  # ref count: 2
del x  # ref count: 1
del y  # ref count: 0, list is deallocated
```

### Memory Model
* Everything is an object
* Variables are references to objects
* Assignment creates new references
* Example:
```python
a = [1, 2, 3]
b = a  # Both variables reference the same list
b.append(4)  # Modifies the list through either reference
print(a)  # [1, 2, 3, 4]
```

## Object-Oriented Programming

### Classes and Objects
* Classes define blueprints for objects
* Objects are instances of classes
* Key concepts:
    - Encapsulation
    - Inheritance
    - Polymorphism
* Example:
```python
class DataProcessor:
    def __init__(self, data):
        self.data = data
    
    @property
    def size(self):
        return len(self.data)
    
    def process(self):
        raise NotImplementedError("Subclasses must implement process()")
```

### Special Methods (Magic Methods)
* Define behavior for built-in operations
* Common methods:
    - `__init__`: Constructor
    - `__str__`: String representation
    - `__len__`: Length
    - `__call__`: Make object callable
* Example:
```python
class Dataset:
    def __init__(self, data):
        self.data = data
    
    def __len__(self):
        return len(self.data)
    
    def __str__(self):
        return f"Dataset with {len(self)} records"
    
    def __getitem__(self, idx):
        return self.data[idx]
```

## Common Interview Topics

### Time and Space Complexity
* Big O notation basics
* Common complexities:
    - O(1): Constant time
    - O(log n): Logarithmic
    - O(n): Linear
    - O(n log n): Log-linear
    - O(n²): Quadratic

### Common Algorithms
* Sorting algorithms
    - Quick sort: Average O(n log n)
    - Merge sort: Guaranteed O(n log n)
    - Bubble sort: O(n²)
* Search algorithms
    - Binary search: O(log n)
    - Linear search: O(n)

### Threading vs. Multiprocessing
* Threading:
    - Shares memory space
    - Good for I/O-bound tasks
    - Limited by GIL for CPU-bound tasks
* Multiprocessing:
    - Separate memory spaces
    - Good for CPU-bound tasks
    - Higher memory overhead

### Context Managers
* Used with `with` statement
* Ensures proper resource management
* Example:
```python
class DataConnection:
    def __enter__(self):
        print("Opening connection")
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        print("Closing connection")

# Usage
with DataConnection() as conn:
    # Work with connection
    pass  # Connection automatically closed
```

### Decorators
* Modify or enhance functions
* Common uses:
    - Logging
    - Timing
    - Access control
* Example:
```python
def timer(func):
    def wrapper(*args, **kwargs):
        import time
        start = time.time()
        result = func(*args, **kwargs)
        print(f"Function took {time.time() - start:.2f} seconds")
        return result
    return wrapper

@timer
def slow_function():
    import time
    time.sleep(1)
```

## Best Practices for Interviews
1. Always discuss trade-offs between different approaches
2. Explain your thought process clearly
3. Consider edge cases
4. Be prepared to explain time and space complexity
5. Have examples ready for each concept
6. Be familiar with Python-specific implementations
7. Understand how concepts relate to data science tasks

Remember to practice implementing these concepts and be ready to explain them in the context of real-world data science problems.