# Python

## Index

## Notes on basics

### Functions

#### Docstring

The string to document a function. The convention on the
structure is as follows:

```python
def func(arg1, arg2):
    '''
    On this first paragraph a brief description is provided

    Args:
        arg1 (list): a list of numeric values
        arg2 (int): an integer value representing something

    Returns:
        res (float): a value rounded to two decimal places
    '''
    # Code
```

### Arbitrary arguments

Allow functions to accept any number of arguments. Use keyword
```*args```.
```python
# Arbitrary positional arguments
def func(*args): # Puts the arguments into a touple
    # Code
func(12, 13, 43, 24)

# Arbitrary keyword arguments
def func2(**kwargs) # Puts the arguments into a dictionary
    # Code
func2(a=15, b=29, c=4, d=22)
func2(**{'a':15, 'b':29, 'c':4, 'd':22})
```

#### Lamda functions

It is an anonymous function, which doesn't require a name or
needs to be saved as a variable. It is used for simple tasks
or functions that need to be performed once.

* Convetion is to use ```x``` for a single argument
* No ```return``` statement is required.
* The ```expression``` is the equivalent of the function body
* For iterable elements, use ```map()``` to apply the function
  to all elements.
* To use it, it has to be in parenthesis, followed by the
  arguments/inputs in parenthesis

```python
# Structure
lambda argument(s): expression

# EXAMPLES:
# Simple
(lambda x: sum(x) / len(x))([2, 4, 5])

# Two args
(lambda x, y: x**y)(2, 3)

# Iterable
names = ['john', 'sally', 'leah']
list(map(lambda x: x.capitalize, names))
```

#### Imoprting data

```python
# Reading flat file
with open('file_name.txt', 'r') as file: # 'r' is for read
    text = file.read()

# Excel files with pandas
data = pd.ExcelFile('file_name.xlsx') # stores the excel file
print(data.sheet_names) # prints sheet names
df1 = data.parse('Sheet1') # stores sheet1 info in df
df2 = data.parse(0) # stores data from first sheet in df
```

#### Importing sql

