# C SHARP

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 C SHARP

In all below examples of this document, only differences from `Java` has been shown.

## 📌 default parameter

```c#
void aboutDefaultParameter(string city = "London"){
    // do something...
}

// If a parameter has default value, it becomes optional.
// For example we can call aboutDefaultParameter without parameter as below:
aboutDefaultParameter(); // this will work.
```

## 📌 for each

```c#
foreach (string car in carArray)
{
    Console.WriteLine(car);
}
```

## 📌 cast

```c#
int myInt = 9;
double myDouble = myInt; // Automatic casting: int to double
int myInt2 = (int) myDouble; // Manual casting: double to int (This one is same as Java)
```

## 📌 primitive type

On the official documentations they call them `value type`. It also named `built-in value type` because they are embedded in the core of the language.

Some `value type` are:

- `bool` (which is alias of `System.Boolean`)
- `byte` (which is alias of `System.Byte`)
- `sbyte` (which is alias of `System.SByte`)
- `char` (which is alias of `System.Char`)
- `decimal` (which is alias of `System.Decimal`)
- `double` (which is alias of `System.Double`)
- `int` (which is alias of `System.Int32`)
- `long` (which is alias of `System.Int64`)

The class type variables are calling "__reference types__". Some "__built-in reference types__" are:

- `object` (which is alias of `System.Object`)
- `string` (which is alias of `System.String`)
- `dynamic` (which is alias of `System.Object`)

The `value type`s are different type in `C#` even they are alias of classes.

## 📌 Alias

String is a class. It is defined as alias: "string". This is defining by compiler (but it's hidden to developers as other "value types" like int, short...) as below:

```c#
using string = System.String
```

int has been also defined in same way:

```c#
using int = System.Int32
```

## 📌 String

Simple String methods:

```c#
// Length is a property of string (not a method)
int lengthOfStr = myStr.Length;

// String Interpolation
string firstName = "Jack";
string lastName = "Barrow";
string name = $"My full name is: {firstName} {lastName}";
Console.WriteLine(name);

// access a character
string myString = "Hello";
Console.WriteLine(myString[0]); // Outputs "H"
```

## 📌 Namespace

```c#
namespace Hello.World.Demo {
  // any code here...
}

// the below code is same

namespace Hello {
  namespace World {
    namespace Demo {
      // do something
    }
  }
}
```

usage the namespace:

```c#
// Import single class
using MyMathClass = Hello.World.DemoSpace.MyMathClass;

// Import all classes under DemoSpace (which is a C# namespace)
using Hello.World.DemoSpace;
```

## 📌 static namespaces

```c#
using static System.Console;

WriteLine("Hello, World!");
```

## 📌 @ character

Has 2 purposes:

- we can create String without escape character:

```c#
var myString = @"c:\myFolder\myfile.txt"

// otherwise we had to write:
var myString = "c:\\myfolder\\myfile.txt"
```

- we can use the reserved words:

```c#
string @class = "Test";
```

## 📌 Named arguments

```c#
void MyMethod(string child1, string child2, string child3)
{
  // do something
}

// Calling above method:
MyMethod(child3: "John", child1: "Jack", child2: "Steve");
```

## 📌 Property vs Field

Properties expose fields.

```c#
class Person
{
  // As default all class member fields are private.
  // But "private" keyword can be used anyway.
  private string myName; // field

  public string Name // property
  {
    get { return myName; }   // get method
    set { myName = value; }  // set method
  }
}

// or (below code is same as above but it's short):
// "Auto-Implemented Property" (C# 3.0 and higher).
class Person
{
  // the only difference is that this example does not have "myName".
  // A private field is creating by compiler. But this field is not accessible
  // even via same class. The field is only accessible via property.
  // "Name" itself is a property.
  // We don't know the name of the field. It is anonymous.
  public string Name  // property
  { get; set; }
}

// or the same code with lambda expression:
class Person
{
  private string myName; // field

  public string Name => myName;
}

// calling anywhere:
Person person = new Person();
person.Name = "Jack"; // we are using the setter method here!
Console.WriteLine(person.Name); // we are calling the getter method here!
```

## 📌 Inherit

```c#
class Dog : Animal
{
  // any code here...
}
```

If a class is "sealed", you can not build a source code which has classes inherited from sealed class. As example the below code will not compile:

```c#
sealed class Animal
{
  // any code here...
}

class Dog : Animal
{
  // any code here...
}
```

## 📌 calling function

## 📌 1- delegate

- Delegates are similar to C++ function pointers.
- Delegate is type-safe.
- Delegate is multi-cast. Which means it can call multiple functions. Example:

```c#
Foo myDelegate = MyFun1;
myDelegate += MyFun2;
myDelegate(); // Will call MyFun1 then MyFun2
```

- Whole example of delegate:

```c#
// this must be defined:
delegate void MyDelegate(int n1, double n2);

// this is the method which we will assign to a variable:
static void Print(int number1, double number2) {
    Console.WriteLine(number1 + number2);
}

// we assign to a variable:
MyDelegate myFunction = Print;
// or the same code:
MyDelegate myFunction = new MyDelegate(Topla);

// now we can invoke the function:
myFunction(100, 200);
```

## 📌 -2 action

When we use "Action" we don't have to define "delegate" method. Example:

```c#
// this is the method which we will assign to a variable:
static void Print(int number1, double number2) {
    Console.WriteLine(number1 + number2);
}

// we assign to a variable:
Action<int, double> myFunction = Print;

// now we can invoke the function:
myFunction(100, 200);
```

## 📌 -3 Func

"Func" is same as "Action". But "Func" supports return type. Example:

```c#
// this is the method which we will assign to a variable:
static bool Print(int number1, double number2) {
    Console.WriteLine(number1 + number2);
    return true;
}

// we assign to a variable:
Func<int, double, bool> myFunction = Print;

// now we can invoke the function:
myFunction(100, 200);
```

## 📌 overriding methods

```c#
class Dog : Animal
{
  public override void animalSound()
  {
    // any code here...
  }
}
```

## 📌 Enum values

Enums has values as default:

```c#
enum Months
{
  January,    // 0
  February,   // 1
  March,      // 2
  April,      // 3
  May,        // 4
  June,       // 5
  July        // 6
}

// usage:
int myNum = (int) Months.April;
```

We can override default values:

```c#
enum Months
{
  January,    // 0
  February,   // 1
  March=6,    // 6
  April,      // 7
  May,        // 8
  June,       // 9
  July        // 10
}
```

## 📌 Destructors

```c#
class Dog
{
  ~Dog() {
     // destroy the class instance.
  }
}
```

## 📌 is instance

on C#:

```c#
if (dog is Dog)
```

corresponding code on Java:

```java
if (dog instanceof Dog)
```

## 📌 Struct

C# has Struct type. The syntax is same as classes. But some rules are different then classes. examples differences:

- Struct cannot have a default constructor (a constructor without parameters) or a destructor.
- Struct cannot be a base class. So, Struct types cannot abstract and are always implicitly sealed.

## 📌 scope

```c#
void DoSomething()
{
    int a;

    {
        // this is a new scope
        int b;
        a = 1;
    }

    a = 2;
    b = 3; // Will fail because the variable is declared in an inner scope.
}
```

## 📌 pointers

The syntax is same with C and C++.

## 📌 Anonymous types

```c#
var carl = new { Name = "Carl", Age = 35 };
```

## 📌 Partial class

```c#
// File1.cs
partial class Foo
{
    // any code here
}

// File2.cs
partial class Foo
{
    // any code here
}
```

## 📌 Object initializers

```c#
var person = new Person
{
    Name = "John",
    Age = 39
};

// Equal to
var person = new Person();
person.Name = "John";
person.Age = 30;
```

## 📌 pass by value ⟷ reference

C#, Java ile aynı şekilde __value type__ (Java'daki primitive'e denk geliyor) tipteki parametreleri "__pass by value__", class'ları ise "__pass by reference__" tekniği ile çalıştırarak yollar.

Eğer "__ref__" keyword'ünü kullanırsak, artık __value type__ olan parametre, "__pass by reference__" tekniği kullanılarak fonksiyona aktarılır. Artık bu __value type__ değere yeni bir değer atadığımızda bu yeni değer fonksiyon bittiğinde de geçerli olacaktır.

"__out__" keyword'ü de ref ile apaynı görevi görüyor. fakat bir fark var: ref değeri null bir değer yollanamazken, out null yollanabilir.

## 📌 const vs readonly

Java'daki final'a benzer mantıkta çalışır.

__const__ olarak belirlenen değer compile sırasında init edilmek zorundadır. Bu sebeple fonksiyon çağıramaz. Yani aşağıdaki olamaz:

```c#
const int a = calculateA();
```

Sadece aşağıdakine benzer compile sırasında hesaplanabilen bir değer olabilir:

```c#
const int a = "Hello" + "World";
```

__Readonly__ ise fonksiyon çağırabilir. Çünkü runtime'da çağrıldıklarında init edilirler.

__Const__ tanımlanan değer otomatik olarak static olur. Oysa __readonly__'de böyle bir durum söz konusu değildir.

## 📌 method keywords

__protected__ method can only be access by inherited classes.

__virtual__ methods can be override by inherited classes.

__abstract__ methods must be override by inherited classes.

__private__ methods only can be accessible via itself

## 📌 System.Object

Tüm sınıflar __System__ namespace'indeki __Object__ sınıfından türemiştir.

## 📌 Language Integrated Query (⟷ LINQ)

A technology which is using to:

- grouping
- filtering
- ordering
- transform data (from SQL table to XML output)

from different data source like:

- DB tables
- XML documents
- Native C# language arrays and collections

Example code:

```c#
// Specify the data source.
int[] scores = { 97, 92, 81, 60 };

// Define the query expression.
IEnumerable<int> scoreQuery =
    from score in scores
    where score > 80
    select score;

// Execute the query.
foreach (int i in scoreQuery)
{
    Console.Write(i + " ");
}

// Output: 97 92 81
```

LINQ does not invoke before it calling via for-each. source: <https://docs.microsoft.com/en-us/dotnet/csharp/linq/> title:"Query expression overview", paragraph:4, writes: "A query is not executed until you iterate over the query variable, for example, in a foreach statement.".

At compile time, LINQ syntax will be converted to methods. LINQ is more human-readable then methods. See below example:

```c#
string sentence = "the quick brown fox jumps over the lazy dog";
// Split the string into individual words to create a collection.
string[] words = sentence.Split(' ');

// Using query expression syntax:
var query = from word in words
            group word.ToUpper() by word.Length into gr
            orderby gr.Key
            select new { Length = gr.Key, Words = gr };

// Using method-based query syntax:
var query2 = words.
    GroupBy(w => w.Length, w => w.ToUpper()).
    Select(g => new { Length = g.Key, Words = g }).
    OrderBy(o => o.Length);
```

Some query operations, such as `Count` or `Max`, have no equivalent query expression clause and must therefore be expressed as a method call. Method syntax can be combined with query syntax in various ways.

## 📌 Attributes

It is the same concept of Java's "annotation".

Example syntax:

```c#
// technically OperationContractAttribute is a method.
// OperationContractAttribute is inherits from "System.Attribute" class of C#.

[OperationContractAttribute(IsOneWay=true)]
public void Method1 (int x)
{
  // do something...
}

// If we will not sent any parameter to OperationContractAttribute method, we don't need parentheses.
[OperationContractAttribute]
public void Method2 (int x)
{
   // do something...
}
```

## 📌 Question mark

Has different meaning depending on the context:

- Null-conditional operators

It returns `null`, even if `myObject` or `Items` is `null`. It does not throw exception.

```c#
Console.Write(myObject?.Items?[0].ToString());
```

- The Conditional Operator/Ternary Operator

Same as Java.

```c#
isTrue ? "Valid" : "Lie";
```

- Null Coalescing Operator

```c#
// Example 1:
// if Answer1 is not null return Answer1. Otherwise return Answer2;
string Answer = Answer1 ?? Answer2;

// Example 2:
// Same as above one. But it also checks Answer2 is null or not. If Answer2 is null, it return Answer3...
string Answer = Answer1 ?? Answer2 ?? Answer3 ?? Answer4;
```

- Nullable value or reference types

Normally value types has default value. They can not set as null. But with question mark, it is possible to set null on value typed variables:

```c#
// This is valid.
bool? result1 = null;

// This is same as above line:
Nullable<bool> result2 = null;

// below line is invalid:
bool result2 = null;
```

For reference types,

## 📌 Null vs not-initialized variable

On C# they are different:

```c#
// Example of not-initialized variable:
int? b;
System.Console.WriteLine(b == null); // throws exception.

// Example of null variable:
int? b = null;
System.Console.WriteLine(b == null); // it works! returns true.
```
