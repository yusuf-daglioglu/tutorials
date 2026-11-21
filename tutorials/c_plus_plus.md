# C PLUS PLUS

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 C++

`C++` ile bazı konular `C` konuları altında anlatılıyor.

```c++
#include <iostream> // includes all functions of this lib on this file.
using namespace std;

namespace myCustomNameSpace
{
  int val = 30;
}

int val = 10; // Global variable.

#define NAME_SIZE 50 // Defines a macro

class Person {
  int id; // all members are private by default
  char name[NAME_SIZE];
  bool active; // boolean

  public:
   void aboutMe(){

    cout << "Im a person";

    // eğer en yukarıda "using namespace std;" yazmasaydık,
    // o zaman bunu yazmak zorundaydık:
    // std::cout << "I'm a person";

    // nameSpaceIdentifier::varInsideNameSpace
  }

  int anotherFunction(){
    // this is also public function
    return 0;
  }
};

class Student : public Person {
  public: void aboutMe(){
    cout << "Im a student";
  }
};

int main(){
  Student * p = new Student();
  p -> aboutMe(); // prints "I'm a student"
  delete p; // allocated memory

  // comma operator example:
  int b = 0;
  int a = (b=3, b+2);
  // above code flow:
  // 1- b=3 (b is 3)
  // 2* b+2
  // a = 5

  int x = 5;
  int y = 6;
  duplicate(x, y);

  // arrays
  int arrX[5] = { 10, 20, 30 }; // other 2 variables are 0.
  // or different syntax:
  int arrY[5] { 10, 20, 30 };

  // C++ standart lib --> include <array> have custom array:
  // array<int,3> myarray {10,20,30};
  // for (int i=0; i<myarray.size(); ++i) {
  //     ++myarray[i];
  // }

  return 0;
}
```

## 📌 & on parameters

```c++
void degerDegistir(int &x) {
    
    x = 10;

    // bu satırda x'in adresi şudur:
    // &x
}

int main() {
    int a = 5;

    degerDegistir(a);
    
    // burada a = 10

    return 0;
}
```

## 📌 const

`const` `Java`'daki `final`'a denk geliyor. opsiyoneldir.

## 📌 inline

`inline` compiler'a bu kodun çağrılan yere kopyalanabileceğini belirtiyor. yani `inline` olan bir fonksiyon başka yerden çağrılıyorsa, build sırasında orada direk o fonksiyon fiziksel olarak yaratılır. böylece çağırma hızı artar. fakat `EXE` boyutu büyür.

compiler, bazı durumlarda (config'e bağlı) developer `inline` yazmasa bile, kendisi `inline` yapabiliyor.

```c++
inline void duplicate(const int& a, int& b) {
  a*=2;
  b*=2;
}

// yukarıdaki fonksiyonu böyle çağırmamız gerek:
int var1 = 3;
int var2 = 8;
duplicate(var1, var2);
// bu şekilde çağıramayız. hata verir:
duplicate(3, 8);
```

## 📌 default value for parameters

```c++

void function99(int a = 2){
   
}
```

## 📌 template

```c++
template <class T, int X>
T sum(T a, T b)
{
  T result;
  result = (a + b) * X;
  return result;
}

// usage:
int x = sum<int, 3>(1, 2);
int y = sum<double, 3>(1.1, 2.2);
```

## 📌 scope

```c++
int main () {
  int x = 10;
  int y = 20;
  {
    int x;   // ok, inner scope.
    x = 50;  // sets value to inner x
    y = 50;  // sets value to (outer) y
  }
  // x = 10
  // y = 50
}
```

## 📌 using keyword

```c++
#include <iostream>
using namespace std;

namespace first
{
  int x = 5;
  int y = 10;
}

namespace second
{
  double x = 3.1416;
  double y = 2.7183;
}

int main () {
  using first::x;
  using second::y;
  cout << x << '\n'; // prints the x of first
  cout << y << '\n'; // prints the y of second
  cout << first::y << '\n';
  cout << second::x << '\n';
  return 0;
}
```

## 📌 Operator overloading

`function overloading`'den farklıdır. `function overloading` aynı isimdeki fonksiyonun farklı parametreler alarak çalışabilmesidir. oysa `operator overloading` dildeki gömülü olan bir operatörün (`+`, `-` gibi) kullanımını override edebilmemizi sağlayan özelliktir.

`C++` birçok operatörün değiştirilmesine izin veriyor. bazı override edilebilen operatörler:

```
+, -, *, /, %, ^, !, =, ==, new, delete, --, <<=, &&, ||
```

Fakat bazılarını overload edemiyoruz:

```
., ::, ?:, sizeof
```

Bazı operatörlerin `unary` ve `binary` versiyonları vardır. Yani; bu operatörü kullanırken hem sağ hem solunda bir instance olabilir. bunlar ayrı ayrı overloading edilirler. örnek:

```
&, *, +, -.
```

## 📌 insertion operator (<<)

```c++
// "cout" console output'un kısaltmasından gelir.
// cout writes to console-output-stream.

cout << "hello1";
// same with:
cout.operator<<("hello1");

// example 2:
cout << "hello2" << "world";

// extraction operator (>>)
// cin: console input'un kısaltmasından gelir.
// cin reads from console-input-stream.
char charArr[100];
cin >> charArr;
```
