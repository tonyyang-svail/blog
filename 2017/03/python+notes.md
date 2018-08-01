# Python Notes

```python
## Define function in the runtime
# import sys
#
# for line in sys.stdin:
#     line = line.rstrip()
#     if int(line) > 0:
#         def foo():
#             print line, 'positive'
#     else:
#         def foo():
#             print line, 'negative'
#
#     foo()

## Pit falls of closure
# from __future__ import print_function
# def buggy_maker(my_list):
#     acts = []
#     for i in my_list:
#         acts.append(lambda: print(i))
#     return acts
# for act in buggy_maker([1,2,3]):
#     act() # always print 3, instead of 1, 2, 3
#
# def buggy_maker2():
#     acts = []
#     i = 1
#     acts.append(lambda: print(i))
#     i = 2
#     acts.append(lambda: print(i))
#     return acts
# for act in buggy_maker2():
#     act() # always print 2, instead of 1, 2

## Usage of global
# def foo():
#     global s
#     s = None
#     print s
#
# foo()
# print s
```

## Naming

| Identifier Type | Format            | Example                       |
|-----------------|-------------------|-------------------------------|
| Class           | Camel case        | `class StringManipulator():`  |
| Function        | Words joined by _ | `def multi_word_name(words):` |
| Constant        | All uppercase     | `SECRET_KEY = 42`             |

Basically everything not listed should follow the variable/function naming conventions
of ‘Words joined by an underscore’.

## Control flow

```python
# Avoid repeating variable name in compound if statement
is_generic_name = name in ('Tom', 'Dick', 'Harry')

# Use else to execute code after a for loop concludes
for user in get_all_users():
    print ('Checking {}'.format(user))
    for email_address in user.get_all_email_addresses():
        if email_is_malformed(email_address):
            print ('Has a malformed email address!')
            break
else:
    # The else clause is executed after the iterator is
    # exhausted, unless the loop was ended prematurely
    # due to a break statement
    print ('All email addresses are valid!')
```


## Generator

```python
import random

def get_data():
    """Return 3 random integers between 0 and 9"""
    return random.sample(range(10), 3)

def consume():
    """Displays a running average across lists of integers sent to it"""
    running_sum = 0
    data_items_seen = 0

    while True:
        data = yield
        data_items_seen += len(data)
        running_sum += sum(data)
        print('The running average is {}'.format(running_sum / float(data_items_seen)))

def produce(consumer):
    """Produces a set of values and forwards them to the pre-defined consumer
    function"""
    while True:
        data = get_data()
        print('Produced {}'.format(data))
        consumer.send(data)
        yield

if __name__ == '__main__':
    consumer = consume()
    consumer.send(None)
    producer = produce(consumer)

    for _ in range(10):
        print('Producing...')
        next(producer)
```

## Class
```python
from __future__ import print_function

class Foo(object):
    def __init__(self):
        self.name = 'name'
        self._name = '_name'
        self.__name = '__name'

class Bar(Foo):
    def __init__(self):
        super(Bar, self).__init__()

foo = Foo()
bar = Bar()

print(foo.name)       # name
print(foo._name)      # _name
print(foo.__name)     # ValueError
print(foo._Foo__name) # field __name has been changed to _Foo__name
print(bar.name)       # name
print(bar._name)      # _name
print(bar.__name)     # ValueError
print(foo._Foo__name) # field __name has been changed to _Foo__name
```

## Data Structure
```python
# List
#  Avoid using '', [], and {} as default parameters to functions
def f(a, L=[]):
    L.append(a)
    return L
print(f(1))
print(f(2))
print(f(3))
# This will print
# [1]
# [1, 2]
# [1, 2, 3]


# Dict:
log_severity = None
if 'severity' in configuration:
    log_severity = configuration['severity']
else:
    log_severity = 'Info'
# is equivalent to
log_severity = configuration.get('severity', 'Info')

# Set: the mathematical set operations
A & B # Intersection
A | B # Union
A - B # The set of elements in A but not B, not necessarily equal to B -A
A ^ B # Symmetric Difference
```

## Decorator
### Timeit
```python
from __future__ import print_function
from timeit import default_timer as timer

def timeit(F):
    def FF(*args, **kwargs):
        start = timer()
        F(*args, **kwargs)
        end = timer()
        print(F.__name__ + ':', end - start)

    return FF

# This is equivalent to F = timeit(F)
@timeit
def computation_func(x):
    count = 0
    for i in xrange(x):
        count += i

class Foo(object):
    @timeit
    def computation_func(self, x):
        count = 0
        for i in xrange(x):
            count += i

computation_func(2**8)        # 2.78949737549e-05
Foo().computation_func(2**8)  # 2.40802764893e-05
```
### Registry
```python
from __future__ import print_function

registry = {}
def register(obj):
    registry[obj.__name__] = obj
    return obj

@register
def spam(x):
    return (x ** 2)

@register
def ham(x):
    return (x ** 3)

print('Registry:')
for name in registry:
    print(name, '=>', registry[name], type(registry[name]))
# Registry:
# ham => <function ham at 0x7fd0de4b48c0> <type 'function'>
# spam => <function spam at 0x7fd0de4b4848> <type 'function'>

print('\nRegistry calls:')
for name in registry:
    print(name, '=>', registry[name](2)) # Invoke from registry
# Registry calls:
# ham => 8
# spam => 4
```

```
## Naming

| Identifier Type | Format            | Example                       |
|-----------------|-------------------|-------------------------------|
| Class           | Camel case        | `class StringManipulator():`  |
| Function        | Words joined by _ | `def multi_word_name(words):` |
| Constant        | All uppercase     | `SECRET_KEY = 42`             |

Basically everything not listed should follow the variable/function naming conventions
of ‘Words joined by an underscore’.

## Control flow

```python
# Avoid repeating variable name in compound if statement
is_generic_name = name in ('Tom', 'Dick', 'Harry')

# Use else to execute code after a for loop concludes
for user in get_all_users():
    print ('Checking {}'.format(user))
    for email_address in user.get_all_email_addresses():
        if email_is_malformed(email_address):
            print ('Has a malformed email address!')
            break
else:
    # The else clause is executed after the iterator is
    # exhausted, unless the loop was ended prematurely
    # due to a break statement
    print ('All email addresses are valid!')
```


## Generator

```python
import random

def get_data():
    """Return 3 random integers between 0 and 9"""
    return random.sample(range(10), 3)

def consume():
    """Displays a running average across lists of integers sent to it"""
    running_sum = 0
    data_items_seen = 0

    while True:
        data = yield
        data_items_seen += len(data)
        running_sum += sum(data)
        print('The running average is {}'.format(running_sum / float(data_items_seen)))

def produce(consumer):
    """Produces a set of values and forwards them to the pre-defined consumer
    function"""
    while True:
        data = get_data()
        print('Produced {}'.format(data))
        consumer.send(data)
        yield

if __name__ == '__main__':
    consumer = consume()
    consumer.send(None)
    producer = produce(consumer)

    for _ in range(10):
        print('Producing...')
        next(producer)
```

## Class
```python
from __future__ import print_function

class Foo(object):
    def __init__(self):
        self.name = 'name'
        self._name = '_name'
        self.__name = '__name'

class Bar(Foo):
    def __init__(self):
        super(Bar, self).__init__()

foo = Foo()
bar = Bar()

print(foo.name)       # name
print(foo._name)      # _name
print(foo.__name)     # ValueError
print(foo._Foo__name) # field __name has been changed to _Foo__name
print(bar.name)       # name
print(bar._name)      # _name
print(bar.__name)     # ValueError
print(foo._Foo__name) # field __name has been changed to _Foo__name
```

## Data Structure
```python
# List
#  Avoid using '', [], and {} as default parameters to functions
def f(a, L=[]):
    L.append(a)
    return L
print(f(1))
print(f(2))
print(f(3))
# This will print
# [1]
# [1, 2]
# [1, 2, 3]


# Dict:
log_severity = None
if 'severity' in configuration:
    log_severity = configuration['severity']
else:
    log_severity = 'Info'
# is equivalent to
log_severity = configuration.get('severity', 'Info')

# Set: the mathematical set operations
A & B # Intersection
A | B # Union
A - B # The set of elements in A but not B, not necessarily equal to B -A
A ^ B # Symmetric Difference
```

## Decorator
### Timeit
```python
from __future__ import print_function
from timeit import default_timer as timer

def timeit(F):
    def FF(*args, **kwargs):
        start = timer()
        F(*args, **kwargs)
        end = timer()
        print(F.__name__ + ':', end - start)

    return FF

# This is equivalent to F = timeit(F)
@timeit
def computation_func(x):
    count = 0
    for i in xrange(x):
        count += i

class Foo(object):
    @timeit
    def computation_func(self, x):
        count = 0
        for i in xrange(x):
            count += i

computation_func(2**8)        # 2.78949737549e-05
Foo().computation_func(2**8)  # 2.40802764893e-05
```
### Registry
```python
from __future__ import print_function

registry = {}
def register(obj):
    registry[obj.__name__] = obj
    return obj

@register
def spam(x):
    return (x ** 2)

@register
def ham(x):
    return (x ** 3)

print('Registry:')
for name in registry:
    print(name, '=>', registry[name], type(registry[name]))
# Registry:
# ham => <function ham at 0x7fd0de4b48c0> <type 'function'>
# spam => <function spam at 0x7fd0de4b4848> <type 'function'>

print('\nRegistry calls:')
for name in registry:
    print(name, '=>', registry[name](2)) # Invoke from registry
# Registry calls:
# ham => 8
# spam => 4
```

# 40. Metaclasses

# 19. Advanced Function Topics
```python
>>> sys.getrecursionlimit() # 1000 calls deep default
1000
>>> sys.setrecursionlimit(10000) # Allow deeper nesting
```
