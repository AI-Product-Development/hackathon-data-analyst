TITLE: Defining a Dataclass with Type Annotations (Python)
DESCRIPTION: This example demonstrates how to define a dataclass using the @dataclass decorator. The class InventoryItem has fields for name, unit_price, and quantity_on_hand, all with type annotations. The @dataclass decorator will automatically generate an __init__ method, among others.
SOURCE: https://github.com/python/cpython/blob/main/Doc/library/dataclasses.rst#2025-04-21_snippet_0

LANGUAGE: Python
CODE:
```
"from dataclasses import dataclass

@dataclass
class InventoryItem:
    \"\"\"Class for keeping track of an item in inventory.\"\"\"
    name: str
    unit_price: float
    quantity_on_hand: int = 0

    def total_cost(self) -> float:
        return self.unit_price * self.quantity_on_hand"
```

----------------------------------------

TITLE: Calculating Powers in Python
DESCRIPTION: This snippet demonstrates how to use the ** operator to calculate powers in Python.
SOURCE: https://github.com/python/cpython/blob/main/Doc/tutorial/introduction.rst#2025-04-21_snippet_2

LANGUAGE: python
CODE:
```
>>> 5 ** 2  # 5 squared
25
>>> 2 ** 7  # 2 to the power of 7
128
```

----------------------------------------

TITLE: Performing Basic Arithmetic in Python
DESCRIPTION: This snippet demonstrates how to use Python as a simple calculator, performing basic arithmetic operations like addition, subtraction, multiplication, and division.
SOURCE: https://github.com/python/cpython/blob/main/Doc/tutorial/introduction.rst#2025-04-21_snippet_0

LANGUAGE: python
CODE:
```
>>> 2 + 2
4
>>> 50 - 5*6
20
>>> (50 - 5*6) / 4
5.0
>>> 8 / 5  # division always returns a floating-point number
1.6
```

----------------------------------------

TITLE: Enumerating Sequences with Python's enumerate Function
DESCRIPTION: The enumerate function constructs an iterable providing a count and values from a given iterable, starting from the specified index. It requires an iterable object and has an optional start parameter that defaults to zero.
SOURCE: https://github.com/python/cpython/blob/main/Doc/library/functions.rst#2025-04-21_snippet_7

LANGUAGE: python
CODE:
```
>>> seasons = ['Spring', 'Summer', 'Fall', 'Winter']
>>> list(enumerate(seasons))
[(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
>>> list(enumerate(seasons, start=1))
[(1, 'Spring'), (2, 'Summer'), (3, 'Fall'), (4, 'Winter')]
```

----------------------------------------

TITLE: Creating a Virtual Environment with venv in Python
DESCRIPTION: Creates a new virtual environment named 'tutorial-env' using the venv module. This command will generate a directory with a copy of the Python interpreter and supporting files.
SOURCE: https://github.com/python/cpython/blob/main/Doc/tutorial/venv.rst#2025-04-21_snippet_0

LANGUAGE: bash
CODE:
```
python -m venv tutorial-env
```

----------------------------------------

TITLE: List Comprehensions in Python
DESCRIPTION: Shows various examples of list comprehensions including filtering, transforming elements, and working with nested lists.
SOURCE: https://github.com/python/cpython/blob/main/Doc/tutorial/datastructures.rst#2025-04-21_snippet_3

LANGUAGE: python
CODE:
```
squares = [x**2 for x in range(10)]
[(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]
[x*2 for x in vec]
[x for x in vec if x >= 0]
[abs(x) for x in vec]
[weapon.strip() for weapon in freshfruit]
[(x, x**2) for x in range(6)]
```

----------------------------------------

TITLE: Creating Dictionaries in Python
DESCRIPTION: Shows multiple ways to create dictionaries that are functionally equivalent. These examples demonstrate dictionary creation using keyword arguments, braces with key-value pairs, the zip function, list of tuples, and dictionary transformation.
SOURCE: https://github.com/python/cpython/blob/main/Doc/library/stdtypes.rst#2025-04-21_snippet_90

LANGUAGE: python
CODE:
```
>>> a = dict(one=1, two=2, three=3)
>>> b = {'one': 1, 'two': 2, 'three': 3}
>>> c = dict(zip(['one', 'two', 'three'], [1, 2, 3]))
>>> d = dict([('two', 2), ('one', 1), ('three', 3)])
>>> e = dict({'three': 3, 'one': 1, 'two': 2})
>>> f = dict({'one': 1, 'three': 3}, two=2)
>>> a == b == c == d == e == f
True
```

----------------------------------------

TITLE: Nested List Comprehensions in Python
DESCRIPTION: Demonstrates matrix transposition using nested list comprehensions and equivalent loop-based implementations.
SOURCE: https://github.com/python/cpython/blob/main/Doc/tutorial/datastructures.rst#2025-04-21_snippet_4

LANGUAGE: python
CODE:
```
matrix = [
    [1, 2, 3, 4],
    [5, 6, 7, 8],
    [9, 10, 11, 12],
]
[[row[i] for row in matrix] for i in range(4)]
```

----------------------------------------

TITLE: Handling Undefined Variables in Python
DESCRIPTION: This snippet demonstrates the error that occurs when trying to use an undefined variable in Python.
SOURCE: https://github.com/python/cpython/blob/main/Doc/tutorial/introduction.rst#2025-04-21_snippet_4

LANGUAGE: python
CODE:
```
>>> n  # try to access an undefined variable
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'n' is not defined
```

----------------------------------------

TITLE: Creating Fibonacci Module in Python
DESCRIPTION: Defines a Python module with two functions for generating Fibonacci sequences. One function prints the sequence up to n, while the other returns it as a list.
SOURCE: https://github.com/python/cpython/blob/main/Doc/tutorial/modules.rst#2025-04-21_snippet_0

LANGUAGE: python
CODE:
```
# Fibonacci numbers module

def fib(n):    # write Fibonacci series up to n
    a, b = 0, 1
    while a < n:
        print(a, end=' ')
        a, b = b, a+b
    print()

def fib2(n):   # return Fibonacci series up to n
    result = []
    a, b = 0, 1
    while a < n:
        result.append(a)
        a, b = b, a+b
    return result
```

----------------------------------------

TITLE: Concurrent Task Execution with asyncio.gather() in Python
DESCRIPTION: Demonstrates the use of asyncio.gather() to run multiple coroutines concurrently. This example calculates factorials of different numbers in parallel, showcasing how gather can be used for efficient concurrent execution of tasks.
SOURCE: https://github.com/python/cpython/blob/main/Doc/library/asyncio-task.rst#2025-04-21_snippet_10

LANGUAGE: python
CODE:
```
import asyncio

async def factorial(name, number):
    f = 1
    for i in range(2, number + 1):
        print(f"Task {name}: Compute factorial({number}), currently i={i}...")
        await asyncio.sleep(1)
        f *= i
    print(f"Task {name}: factorial({number}) = {f}")
    return f

async def main():
    # Schedule three calls *concurrently*:
    L = await asyncio.gather(
        factorial("A", 2),
        factorial("B", 3),
        factorial("C", 4),
    )
    print(L)

asyncio.run(main())
```

----------------------------------------

TITLE: Using f-strings in Python for String Formatting
DESCRIPTION: Demonstrates formatted string literals (f-strings) in Python, which allow embedding variable values and expressions directly in strings by prefixing with 'f' or 'F' and enclosing expressions in curly braces.
SOURCE: https://github.com/python/cpython/blob/main/Doc/tutorial/inputoutput.rst#2025-04-21_snippet_0

LANGUAGE: python
CODE:
```
>>> year = 2016
>>> event = 'Referendum'
>>> f'Results of the {year} {event}'
'Results of the 2016 Referendum'
```

----------------------------------------

TITLE: Python Module Name Check Condition
DESCRIPTION: A conditional check commonly used to determine if a module is being run directly rather than being imported. This block only executes when the module is used to populate the __main__ namespace.
SOURCE: https://github.com/python/cpython/blob/main/Doc/reference/import.rst#2025-04-21_snippet_5

LANGUAGE: python
CODE:
```
if __name__ == "__main__":
```

----------------------------------------

TITLE: Implementing chain.from_iterable function in Python
DESCRIPTION: Equivalent implementation of the chain.from_iterable method that creates an iterator from a single iterable of iterables, yielding elements from each nested iterable in sequence.
SOURCE: https://github.com/python/cpython/blob/main/Doc/library/itertools.rst#2025-04-21_snippet_4

LANGUAGE: python
CODE:
```
def from_iterable(iterables):
    # chain.from_iterable(['ABC', 'DEF']) → A B C D E F
    for iterable in iterables:
        yield from iterable
```

----------------------------------------

TITLE: Using the @dataclass Decorator (Python)
DESCRIPTION: This example shows different ways to use the @dataclass decorator. The first two are equivalent, with the second explicitly showing the empty parentheses. The third shows the decorator with all of its parameters set to their default values.
SOURCE: https://github.com/python/cpython/blob/main/Doc/library/dataclasses.rst#2025-04-21_snippet_2

LANGUAGE: Python
CODE:
```
"@dataclass
class C:
    ...

@dataclass()
class C:
    ...

@dataclass(init=True, repr=True, eq=True, order=False, unsafe_hash=False, frozen=False,
           match_args=True, kw_only=False, slots=False, weakref_slot=False)
class C:
    ..."
```

----------------------------------------

TITLE: Logging Configuration Example in Python
DESCRIPTION: This Python script sets up logging using the provided JSON configuration. It logs messages at different severity levels and showcases how the logger can be utilized within an application.
SOURCE: https://github.com/python/cpython/blob/main/Doc/howto/logging-cookbook.rst#2025-04-21_snippet_7

LANGUAGE: python
CODE:
```
import json
import logging
import logging.config

CONFIG = '''
{
    "version": 1,
    "disable_existing_loggers": false,
    "formatters": {
        "simple": {
            "format": "%(levelname)-8s - %(message)s"
        }
    },
    "handlers": {
        "stdout": {
            "class": "logging.StreamHandler",
            "level": "INFO",
            "formatter": "simple",
            "stream": "ext://sys.stdout",
            "filters": ["warnings_and_below"]
        },
        "stderr": {
            "class": "logging.StreamHandler",
            "level": "ERROR",
            "formatter": "simple",
            "stream": "ext://sys.stderr"
        },
        "file": {
            "class": "logging.FileHandler",
            "formatter": "simple",
            "filename": "app.log",
            "mode": "w"
        }
    },
    "root": {
        "level": "DEBUG",
        "handlers": [
            "stderr",
            "stdout",
            "file"
        ]
    }
}
'''

logging.config.dictConfig(json.loads(CONFIG))
logging.debug('A DEBUG message')
logging.info('An INFO message')
logging.warning('A WARNING message')
logging.error('An ERROR message')
logging.critical('A CRITICAL message')
```

----------------------------------------

TITLE: Configuring Logging Across Multiple Modules in Python
DESCRIPTION: This snippet demonstrates how to set up logging across multiple modules in a Python application. It shows the configuration in the main module and how to use loggers in an auxiliary module.
SOURCE: https://github.com/python/cpython/blob/main/Doc/howto/logging-cookbook.rst#2025-04-21_snippet_0

LANGUAGE: python
CODE:
```
import logging
import auxiliary_module

# create logger with 'spam_application'
logger = logging.getLogger('spam_application')
logger.setLevel(logging.DEBUG)
# create file handler which logs even debug messages
fh = logging.FileHandler('spam.log')
fh.setLevel(logging.DEBUG)
# create console handler with a higher log level
ch = logging.StreamHandler()
ch.setLevel(logging.ERROR)
# create formatter and add it to the handlers
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
fh.setFormatter(formatter)
ch.setFormatter(formatter)
# add the handlers to the logger
logger.addHandler(fh)
logger.addHandler(ch)

logger.info('creating an instance of auxiliary_module.Auxiliary')
a = auxiliary_module.Auxiliary()
logger.info('created an instance of auxiliary_module.Auxiliary')
logger.info('calling auxiliary_module.Auxiliary.do_something')
a.do_something()
logger.info('finished auxiliary_module.Auxiliary.do_something')
logger.info('calling auxiliary_module.some_function()')
auxiliary_module.some_function()
logger.info('done with auxiliary_module.some_function()')
```

----------------------------------------

TITLE: URL Fetching with ThreadPoolExecutor
DESCRIPTION: Complete example showing how to use ThreadPoolExecutor to fetch multiple URLs concurrently and handle exceptions.
SOURCE: https://github.com/python/cpython/blob/main/Doc/library/concurrent.futures.rst#2025-04-21_snippet_4

LANGUAGE: python
CODE:
```
import concurrent.futures
import urllib.request

URLS = ['http://www.foxnews.com/',
        'http://www.cnn.com/',
        'http://europe.wsj.com/',
        'http://www.bbc.co.uk/',
        'http://nonexistent-subdomain.python.org/']

# Retrieve a single page and report the URL and contents
def load_url(url, timeout):
    with urllib.request.urlopen(url, timeout=timeout) as conn:
        return conn.read()

# We can use a with statement to ensure threads are cleaned up promptly
with concurrent.futures.ThreadPoolExecutor(max_workers=5) as executor:
    # Start the load operations and mark each future with its URL
    future_to_url = {executor.submit(load_url, url, 60): url for url in URLS}
    for future in concurrent.futures.as_completed(future_to_url):
        url = future_to_url[future]
        try:
            data = future.result()
        except Exception as exc:
            print('%r generated an exception: %s' % (url, exc))
        else:
            print('%r page is %d bytes' % (url, len(data)))
```

----------------------------------------

TITLE: PEP 553: Built-in breakpoint() Function in Python 3.7
DESCRIPTION: This entry introduces the new built-in function breakpoint(), aligned with PEP 553, which enhances debugging by allowing developers to easily insert breakpoints in their code.
SOURCE: https://github.com/python/cpython/blob/main/Doc/whatsnew/3.7.rst#2025-04-21_snippet_3



----------------------------------------

TITLE: Encoding Basic Python Objects to JSON in Python
DESCRIPTION: Demonstrates how to encode Python data structures like lists, dictionaries, and strings to JSON format using json.dumps() and json.dump(). Shows handling of special characters and sort_keys parameter.
SOURCE: https://github.com/python/cpython/blob/main/Doc/library/json.rst#2025-04-21_snippet_0

LANGUAGE: python
CODE:
```
>>> import json
>>> json.dumps(['foo', {'bar': ('baz', None, 1.0, 2)}])
'["foo", {"bar": ["baz", null, 1.0, 2]}]'
>>> print(json.dumps("\"foo\bar"))
"\"foo\bar"
>>> print(json.dumps('\u1234'))
"\u1234"
>>> print(json.dumps('\\'))
"\\"
>>> print(json.dumps({"c": 0, "b": 0, "a": 0}, sort_keys=True))
{"a": 0, "b": 0, "c": 0}
>>> from io import StringIO
>>> io = StringIO()
>>> json.dump(['streaming API'], io)
>>> io.getvalue()
'["streaming API"]'
```

----------------------------------------

TITLE: Using range() Function in Python Loops
DESCRIPTION: This example demonstrates various ways to use the range() function in Python loops, including specifying start, stop, and step values.
SOURCE: https://github.com/python/cpython/blob/main/Doc/tutorial/controlflow.rst#2025-04-21_snippet_3

LANGUAGE: python
CODE:
```
>>> for i in range(5):
...     print(i)
...
0
1
2
3
4

>>> list(range(5, 10))
[5, 6, 7, 8, 9]

>>> list(range(0, 10, 3))
[0, 3, 6, 9]

>>> list(range(-10, -100, -30))
[-10, -40, -70]
```

----------------------------------------

TITLE: TYPE_CHECKING Usage Example
DESCRIPTION: Shows how to use TYPE_CHECKING constant for conditional imports in type checking contexts while avoiding runtime overhead.
SOURCE: https://github.com/python/cpython/blob/main/Doc/library/typing.rst#2025-04-21_snippet_88

LANGUAGE: python
CODE:
```
if TYPE_CHECKING:
    import expensive_mod

def fun(arg: 'expensive_mod.SomeType') -> None:
    local_var: expensive_mod.AnotherType = other_fun()
```

----------------------------------------

TITLE: Working with dictionaries in Python
DESCRIPTION: Demonstrates dictionary creation, key-value pair manipulation, and basic operations like adding, deleting, and accessing elements.
SOURCE: https://github.com/python/cpython/blob/main/Doc/tutorial/datastructures.rst#2025-04-21_snippet_11

LANGUAGE: python
CODE:
```
>>> tel = {'jack': 4098, 'sape': 4139}
>>> tel['guido'] = 4127
>>> tel
{'jack': 4098, 'sape': 4139, 'guido': 4127}
>>> tel['jack']
4098
>>> del tel['sape']
>>> tel['irv'] = 4127
>>> tel
{'jack': 4098, 'guido': 4127, 'irv': 4127}
>>> list(tel)
['jack', 'guido', 'irv']
>>> sorted(tel)
['guido', 'irv', 'jack']
>>> 'guido' in tel
True
>>> 'jack' not in tel
False
```

----------------------------------------

TITLE: Named Group Matching with groupdict()
DESCRIPTION: Shows how to use named capture groups and retrieve them as a dictionary.
SOURCE: https://github.com/python/cpython/blob/main/Doc/library/re.rst#2025-04-21_snippet_22

LANGUAGE: python
CODE:
```
>>> m = re.match(r"(?P<first_name>\w+) (?P<last_name>\w+)", "Malcolm Reynolds")
>>> m.groupdict()
{'first_name': 'Malcolm', 'last_name': 'Reynolds'}
```

----------------------------------------

TITLE: Parsing and Formatting Datetime in Python
DESCRIPTION: Demonstrates the use of strptime to parse a date string and strftime to format a datetime object. This example shows how to convert between different date formats using these methods.
SOURCE: https://github.com/python/cpython/blob/main/Doc/library/datetime.rst#2025-04-23_snippet_29

LANGUAGE: python
CODE:
```
>>> datetime.strptime('31/01/22 23:59:59.999999',
...                   '%d/%m/%y %H:%M:%S.%f')
datetime.datetime(2022, 1, 31, 23, 59, 59, 999999)
>>> _.strftime('%a %d %b %Y, %I:%M%p')
'Mon 31 Jan 2022, 11:59PM'
```

----------------------------------------

TITLE: Using the dir Function in Python
DESCRIPTION: The dir function shows a list of attribute names in the local or specified object scope. It's versatile, adapting its output based on whether the input is a module, type or class object, or other object types. The function has an optional parameter, object, whose attributes it attempts to list if provided.
SOURCE: https://github.com/python/cpython/blob/main/Doc/library/functions.rst#2025-04-21_snippet_5

LANGUAGE: python
CODE:
```
>>> import struct
>>> dir()   # show the names in the module namespace  # doctest: +SKIP
['__builtins__', '__name__', 'struct']
>>> dir(struct)   # show the names in the struct module # doctest: +SKIP
['Struct', '__all__', '__builtins__', '__cached__', '__doc__', '__file__', '__initializing__', '__loader__', '__name__', '__package__', '_clearcache', 'calcsize', 'error', 'pack', 'pack_into', 'unpack', 'unpack_from']
```

LANGUAGE: python
CODE:
```
>>> class Shape:
...     def __dir__(self):
...         return ['area', 'perimeter', 'location']
...
>>> s = Shape()
>>> dir(s)
['area', 'location', 'perimeter']
