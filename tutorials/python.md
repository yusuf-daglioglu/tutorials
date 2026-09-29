# PYTHON

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Python

- Python dili `JVM` gibi runtime'a ihtiyaç duyar. garbage collector de mevcuttur.
- open source'tur.
- basit ve üst seviyeli bir dildir.
- Python'un gömülü (npm gibi) paket yöneticisi vardır. ismi: "pip".
- komut satırında interactive arayüz sunar.
- gömülü birçok data tipi mevcuttur ve bunlar için API'lar sunar.
- dinamik tipler mevcut (tipinin deklere etmeye gerek kalınmayan instance'lar oluşturulabiliyor)

## 📌 Diğer interpreter dillerle karşılaştırması

- Özellikle Java ile karşılaştırıldığında yukarıda yazan son 4 özellik Python'da daha iyi ve çok erken gelmişti.
- JS zaten (sonradan) NodeJS ile masaüstü dünyasında çalışabilir duruma geldi. Python bu sebeple masaüstü uygulamalarda erken yaygınlaştı.
- özellikle data-tiplerine olan native/built-in desteği sayesinde de "data science" (data mining, machine learning, big data, yapay zeka, data analizi) konularında birçok kütüphane Python için yazıldı.

## 📌 Python market share

Python birçok Linux dağıtımında yüklü gelir. çünkü Linux'ta birçok uygulama Python ile yazılmıştır.

Default yüklü gelen sistemsel uygulamalar Python'un yüklü gelen versiyonunu kullandığı için, Python sürümünü değiştirmek bazen sıkıntılara yol açabilir.

## 📌 Runtime implementations

Python çalıştırmak için runtime'a ihtiyaç duyulur. 

Burada bazı alternatiflerin listesi yazılmıştır: <https://www.python.org/download/alternatives/> 

Bu linkte aynı zamanda "Python.org" sitesinde dağıtılan (traditional) implementasyonun (takma adı CPython'dır) kaynak kodları ile tekrar derleme yapmış, bu derlemeye ek modüller eklemiş veya çok ufak değişiklikler yapıp, paket olarak sunan farklı otoritelerin de listesi yazılmıştır.

## 📌 pip

PIP, NPM'deki gibi bağımlılıkları proje içinde bir klasörde tutmaz. PIP'te her dependency globaldir. sadece dependency isimleri, projenin içinde bir dosyada tutulur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Basic syntax

On Python indentation and new-character matter.

```python
# This is a comment
print("Hello, World!")

"""
Since Python will ignore string literals that are not assigned to a variable. Therefore some developers uses this as multiline comment.
"""

x = 5
y = "John"
y2 = 'John' # this is same as above
print(type(x)) # prints "int"
print(type(y)) # prints "string"

Y = "John" # Variable names are case-sensitive. So this is another variable. It is different then "y" variable.

my_Variable2 = 9 # variables also can have underscore and numbers. (but the variable can not start with number)

# x is Orange
# y is Banana
# z is Cherry
x, y, z = "Orange", "Banana", "Cherry"
# same with below
fruits = ["Orange", "Banana", "Cherry"]
x, y, z = fruits

# all below variables are Orange
x = y = z = "Orange"
```

## 📌 scope and functions

global keyword refers that the "x" is in the global scope (not function scope).

def is a keyword to define function. functions can return any type of value. but the return type is not on the signature.

```python
def myFunction():
  global x
  x = "hello"
```

Functions can be defined inside functions (in function scope):

```python
def myFunction1(n):

    x = "Hello"
    # any code here...
    def func2(n):
        return n+n
    m = func2(n) # func2 can be used only inside myFunction1 (in myFunction1 scope).
    # any code here...

myFunction1(99)
```

## 📌 lambda

```python
x = lambda a, b : a * b
x(5, 2) # calling the lambda. returns 10.
```

## 📌 built-in types

```python
x = "Hello World" # str
x = 'Hello World' # str
x = a = """This is
very long multiline
string"""
x[1] # x[1] is also string

x = 20 # int

x = 20.5 # float
# float can be define via e (which mean ^10).
x = 35e3
y = 12E4
z = -87.7e100

x = 1j # complex numbers

# there is no array on Python. List is the only similar data-structure.
x = ["apple", "banana", "cherry"] # list

# eng:tuple (⟷ tr:demet)
x = ("apple", "banana", "cherry") # this is a tuple.
# tuple is same as list. the only diff is that tuple is immutable.

x = range(6) # range

x = {
  "name" : "John",
  "age" : 36
} # dict (dictionary)

x = {"apple", "banana", "cherry"} # set

x = frozenset({"apple", "banana", "cherry"}) # frozenset

x = True # bool (boolean)

x = b"Hello" # bytes

x = bytearray(5) # bytearray

x = memoryview(bytes(5)) # memoryview

x = None # NoneType

# json, date, RegEx should be imported from modules.
```

Also we can define the same variables with functions:

```python
x = str("Hello World") # str
x = int(20) # int
x = list(("apple", "banana", "cherry")) # list
```

Type casting also is available via those below methods:

```python
myInt = int(20)
myFloat = float(myInt)

# cast to boolean
# - Almost any value is evaluated to True if it has some sort of content.
# - Any string is True, except empty strings.
# - Any number is True, except 0.
# - Any list, tuple, set, and dictionary are True, except empty ones.
bool("abc") # return true
bool(123) # return true
bool(0) # return false
```

## 📌 for loop for lists

```python
routers = []
routers.append(("a", "c", 99))
routers.append(("x", "y", 88))

for var1, var2, var3 in myList:

  # use var1 var2 var3 here
  # any code here...
```

## 📌 String

- for each

```python
for x in "banana":
  print(x)
```

- "in" or "not in"

```python
txt = "Hello world"
"Hello" in txt # return true

# We can use as below:
if "John" not in txt:
    print("No, 'John' is not present.")
```

- string slice

```python
b[:5] # return the characters from position 2 to position 5

b[:5] # return characters from the start to position 5

b[2:] # return  characters from position 2, and all the way to the end

txt = "123456789"
b[-5:-2] # returns 567
```

- string merge

```python
txt = "hello" + " " + "world"
```

## 📌 operators

- Arithmetic Operators

```python
# Exponentiation
x ** y

# Floor division
x // y
# example:
10 / 3 # equals 3.3
10 // 3 # equals 3 (it rounds the result)
```

- Logical and Bitwise operators

```python
# logical operations:
# and
# or
# not
# example:
x < 5 and  x < 10

# bitwise:
# &
# |
# ^
# ~
# <<
# >>
```

## 📌 list

- change the items

```python
list1 = ["1", "2", "3", "4", "5", "6"]
list1[1:3] = ["x", "y"]
```

- access

```python
list1[1] # first element
list1[-2] # sondan ikinci eleman
list1[2:5] # from second to fifth element
```

## 📌 conditions

```python
if b > a:
  print("b is greater than a")
elif a == b:
  print("a and b are equal")
else:
  print("a is greater than b")
```

- shorthand 1

```python
if a > b: print("a is greater than b")
```

- shorthand 2

```python
print("A") if a > b else print("B")
```

- shorthand 3

```python
print("A") if a > b else print("=") if a == b else print("B")
```

## 📌 multiple arguments

```python
def my_function(*kids):
  print("The youngest child is " + kids[2])

my_function("Emil", "Tobias", "Linus")
```

## 📌 Keyword arguments

```python
def my_function(child3, child2, child1):
  print("The youngest child is " + child3)

my_function(child1 = "arg1", child2 = "arg2", child3 = "arg3")
```

## 📌 Arbitrary Keyword Arguments

```python
def my_function(**kid):
  print("His last name is " + kid["lname"])

my_function(fname = "Tobias", lname = "Refsnes")
```

## 📌 default parameter

```python
def my_function(country = "Norway"):
  print("I am from " + country)

my_function("Sweden") # valid
my_function() # valid
```

## 📌 pass

pass keyword'ü tamamen boş olan

- if (condition kısmı değil, condition doğru olunca yapılacak olan kısım)
- fonksiyon

larda kullanılabilir.

```python
def myFunction1():
  pass
```

## 📌 Class

Python is object-oriented.

```python
class Person:

  # __init__ is a special function.
  def __init__(self, name, age):
    self.name = name
    self.age = age

  def myfunc(self):
    # "self" is not a keyword. we can renamed it as we want.
    print("Hello my name is " + self.name)

p1 = Person("John", 36)

print(p1.name)
p1.myfunc()
p1.age = 40
del p1.age # deleting age property.
del p1 # deleting complete person instance.
```

## 📌 inheritance

```python
class Dog(Animal):
  pass
```

## 📌 module

```python
import mymodule # this is "mymodule.py" file.

x = mymodule.globalFunc()
```

or

```python
import mymodule as module1 # this is "mymodule.py" file.

x = module1.globalFunc()
```

any variable inside a module can be called:

```python
from myLib import request

# access "request" directly:
mine = request()

# or

import myLib.request

# access "request" via myLib:
mine = umyLib.request()
```

## 📌 exceptions

```python
try:
  # any code here
except:
# if we cant to specify the exception type:
# except IOError:
  # this block will run if only any exception will handle.
  # if we want to get the details of exception
  # we should use sys.exc_info().
else:
  # this block will run if only any exception will not throw.
finally:
  # this block will run in all cases
```

throw an error:

```python
raise Exception("Sorry")
```

## 📌 annotations

annotation python derleyicisi tarafından ignore edilir. fakat IDE'lerin ve bazı derleme öncesi tool'lar ile validasyonlar yapılabilmesini sağlar.

```python
# below examples are function definitions

# without annotation:
def findAgeOfPerson(name, id):

  # any code here inside function...


# with annotation:
def findAgeOfPerson(name: string, id: int = 12345) -> int:

  # any code here inside function...
```

Class içinde veya fonksiyon içinde local variable tanımlarken de benzer syntax kullanılabilir:

```python
# without annotation:
name = "initial variable for 'name' variable"

# with annotation:
name: string = "initial variable for 'name' variable"
```
