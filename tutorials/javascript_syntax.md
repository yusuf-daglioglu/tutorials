# JAVASCRIPT SYNTAX

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 variable defining syntax

```js
var var1 = "hello", var2, var3 = 99;
```

equals:

```js
var var1 = "hello";
var var2;
var var3 = 99;
```

## 📌 multi equal character on same line

```js
var a = b = c;
a = b = c;
z = a = b = c;
```

Yukarıdaki örnekte sırası ile: "b = c", "a = b" çalıştırılıyor. en sondaki örnekte ek olarak; en sonda "z = a" çalıştırılıyor.

## 📌 null vs undefined

farklıdır. undefined hiç değer almamış anlamına gelir. örneğin;

```js
var a1;

var a2 = null;

typeof a1; //prints undefined

typeof a9; //prints undefined. a9 is not even typed before.

typeof a2; //prints null;
```

## 📌 data types

- JS'te number, boolean ve string birer primitive type'tır.

- fonksiyonlar (aşağıda detaylı anlatım var), typeof operatörü ile bakıldığında "fonksiyon" döndürürler. bu tipte olmalarının sebebi daha hiç new instance'ı ile oluşturulmamış olmasıdır.

- diğer data tiplerinin tümü (null dahil) objedir. array'ler objeden türemiş birer objedir. dolayısı ile objedir.

## 📌 Accessing Object Properties

- objectName.propertyName

- objectName["propertyName"]

## 📌 global vs local variable

local değerler metotların sadece içinden erişilebilir. oysa global değerler tüm program tarafından erişilebilir. bir metot içerisinde "var" ile variable tanımlaması yapılmadan bir bilgiye değer atanırsa; o değer otomatik olarak global olarak tanımlanır. fakat böyle bir şeyin yapılması kesinlikle tavsiye edilmez.

## 📌 === vs ==

- == karşılaştırılan değerlerin eşit olup olmadığına (valueOf() metodu ile) bakarken, === ek olarak tiplerinde eşit olup olmadığına bakar.

- zorunlu kalmadıkça == kontrolü yapmamak gerekli. mantığı biliyor olsak da, başkasının okumasını zorlaştırıyoruz.

- === kontrolü daha doğal ve basit. JS bilgisi dahi gerektirmiyor. tek istisna NaN'da oluyor. NaN === NaN -> false dönüyor.

- == için kurallar sırasıyla işletilmelidir:

  1- aynı tipteler ise === karşılaştırması yapılıyor. farklı tipler için aşağıdaki kurallar geçerlidir:

  2- null ve undefined eşittir

  3- sayı ve string karşılaştırılınca string önce sayıya çevrilir. JS içinde Number(string) metodu mevcut.

  4- en az bir operand boolean ise; true->1, false->0'ya çevirir ve tüm kuralları tekrar işletir.

  5- obje karşısında string yada number varsa, obje.valueOF çağrılır ve kurallar tekrar işletilir.

  6- diğer tüm durumlar false döner

## 📌 NaN

__Not-A-Number__ anlamına gelmektedir. isNaN gibi metotlar bazen beklenmedik sonuçlar verebilir. örneğin " " değeri 0 a eşit sayıldığından isNaN false döner.

NaN bilgisayar bilimlerinde bir sayıdır. örneğin 0/0 tanımlanamaz ve bu sebeple NaN döndürmelidir. bu sebeple JS'te typeof ile bakarsak; NaN, "number" tipindedir.

JS beklenmedik sonuçlar dönebilmektedir. birçok alışılmadık durumları buradan inceleyebiliriz: jsfuck.com

## 📌 debugger; keyword

bu keyword'ü içeren satırlar debug mode'da olan IDE tarafından durdurulurlar.

## 📌 Regular expressions

objeden türemişlerdir. JS'te özel syntax'ı vardır. tırnak içerisine almaya gerek yoktur.

## 📌 object (⟷ nesne ⟷ tr:obje) vs instance

instance kelime anlamı: 1. örnek 2. hal/durum

"class", programlama dillerinde "nesne" tanımlayabilmek için gerekli bir tiptir. "User" eğer class tipindeyse nesnedir. User bir arayüzde olabilirdi.

Instance ise bellekte ayrı yeri olan klon nesnedir.

bu başlıkta ve bu notlarda, obje ve nesne terimlerini kullanırken doğru olmasına hiç dikkat edilmedi. Ikisi de birbiri yerine kullanılmıştır.

## 📌 JS function vs JS class

JS'te class yoktur (JS'te class Ecmascript 6 ile geldi). JS, sınıf olmamasına rağmen nesne tabanlı bir dildir. "nesne" yerine "prototip" kavramı vardır.

aşağıdaki person fonksiyonu bize objeyi döndürmek için kullanılan fonksiyondur. nesne döndürdüğü için constructor da denir. (hatta nesne döndürdüğü için class'ta denir fakat bu doğru değildir).

```js
function person(first) {

    this.firstName = first;

    this.lastName = null;

    this.changeLastName = function (name) {

        this.lastName = name;
    };
}

var myFather = new person("John"); // (inside "person" function) "this" (scope) is a new scope.
print(myFather.firstName);
print(typeof myFather); // object

var myMother = person("Esra"); // (inside "person" function) "this" (scope) is the current "window" object.
print(myMother); //prints undefined. If we had "return "ahmet" on last line of person function, myMother would be "Ahmet".
print(window.firstName);//prints Esra.
```

Ecmascript'in güncel standartlarında class'lar vardır. fakat geriye uyumluluk adına herkes tarafından kullanılması tercih edilmeyebiliyor. örnek:

```js
class User {
  constructor(name) { this.name = name; }
  sayHi() { alert(this.name); }
}

alert(typeof User); // function (yeni sürüm JS'teki "class" olmasına rağmen)
```

Yeni sürüm JS class'ları ile, eski sürüm JS function'ları tamamiyle aynı şeylerdir. 'Class' yapısı arka planda aynı işlevi bizim için yerine getirmektedir, yani syntactical sugar'dır. kaynak: (source-id: 313) title: "Defining a class", writes: "JavaScript classes, introduced in ECMAScript 2015, are primarily syntactical sugar over JS's existing prototype-based inheritance. The class syntax does not introduce a new object-oriented inheritance model to JavaScript.".

Eski tip Function'larda extend işlemi yapılamaz. Function'un türettiği objede extend yapılabilir ("JS object" başlığında anlatılıyor). Oysa yeni JS'teki class'larda extend işlemi Java'daki syntax gibi yapılır.

## 📌 JS object

aşağıdaki (nesne2) tanımlama ile fonksiyonsuz bir şekilde obje türetmiş oluyoruz.

```js
var nesne2 = {

    Ad: 'Melih',

    Soyad: 'Orhan',

    AdSoyad: function () {

        return this.Ad + ' ' + this.Soyad
    }
}
```

Aşağıdaki şekilde inheritance yapabiliriz:

```js
// 2'inci parametrede descriptor'lar yollanıyor (başka başlıkta anlatılıyor).

var obj1 = Object.create(nesne2, {

    prop1: {
      writable: true,
      configurable: true,
      value: 'value1'
    },

    prop2: {
      writable: true,
      configurable: true,
      value: 'value2'
    },

});
```

Yukarıda nesne2 yerine "Object.prototype" yazarsak boş objeden türetmiş oluruz.

## 📌 descriptor

her property'nin descriptor'ları vardır. bir obje içine bir property eklediğimizde JS'in aşağıdaki metodunu kullanabiliriz. bu metot üzerinden gidersek descriptor'ları daha iyi anlayabiliriz.

```js
Object.defineProperty(obj, prop, descriptor)
```

örnek:

```js
Object.defineProperty(myObject, 'myInteger, {
  value: 99, // myInteger'ın değeri
  writable: true, // myInteger = 1; ile değiştirilip değiştirilemeyeceği
  enumerable: true, // "for loop" ile dönüldüğünde bu değerin de döngüye girip girmeyeceği
  configurable: true, // property'nin silinip silinmeyeceği veya tipinin değiştirilip değiştirilmeyeceği
  get: function() { return val; },
  set: function(newValue) { val = newValue; }
});
```

Artık myObject.myInteger bize 99 u verecektir.

Dikkat edilirse defineProperty'nin aldığı tüm JSON elementine "descriptor" deniliyor. yani her biri birer descriptor değil. (bazen karışıklığa sebep olabiliyor. o yüzden bu cümleyi not aldım).

ES2017 ile getOwnPropertyDescriptors metodu gelmiştir (başka yerde anlatılıyor).

## 📌 own-property

bir nesnenin süper variable'ları bu kümeye dahil değildir. __hasOwnProperty("propertyToCheck")__ ile sorgulayabiliriz.

## 📌 fonksiyon ve obje içinde gelen property ve metotlar

### 📌📌 fonksiyon properties

- __length__

  fonksiyon kaç parametreli olduğunu tutar.

  ```js
  function func2(a, b) {}
  console.log(func2.length);
  // output: 2
  ```

- __arguments__

  (deprecated or not standart) fonksiyon çağrılınca ona yollanan argümanlar dizi olarak tutulur. bu argümanlar bu property'de tutulur.

- __arity__

  (deprecated or not standart) length ile aynı görevi görür. fakat length standartlarda olduğundan length kullanılmalıdır.

- __caller__

  (deprecated or not standart) fonksiyon o anda çağıran fonksiyon referansı.

- __display name__

  (deprecated or not standart) default olarak metodun kendi ismidir, fakat developer bunu istediği gibi ezebilir.

- __name__

  fonksiyon adının string halini tutar.

- __prototype__

  tipi JSON objesidir. array değildir. JS objesi olduğundan saf JS syntax'ında olduğu gibi buraya bir çok değer atayabiliriz:

  > myFunction.prototype.newProp = 99;

  prototype objesi, ilgili fonksiyondan üretilecek veya daha önceden üretilmiş her instance'a "__\_\_proto\_\___" property'si referans geçilir. bu sebeple buraya atadığımız her değer diğer eski ve yeni yaratılacak her instance'ın \_\_proto\_\_'sunda vardır.

  Aynı zamanda önceden veya sonradan oluşturulan instance'lara prop olarak kopyalanır. örnek:

  ```js
  myFunction.prototype.newProp = 99;

  instanceOfFunc.newProp; // 99

  instanceOfFunc.__proto__.newProp; // 99

  instanceOfFunc.prototype; // objelerin prototype'ı olmaz. sadece function'ların prototype'ı vardır.
  ```

  prototype ile yeni prop eklemek çok kolaydır. fakat var olan prop'u güncellemek daha karmaşıktır. zira instance'ı üretmek için kullanılan constructor, zaten prop'u override edecektir. Bu durumu aşağıdaki örnekten anlayabiliriz:

  ```js
  let f = function () {
    this.a = 1;
    this.b = 2;
  }
  let o = new f();

  f.prototype.b = 3;
  f.prototype.c = 4;
  // do not set the prototype f.prototype = {b:3,c:4}; this will break the prototype chain. because prototype has also super prototype.

  // o.__proto__ ---> has properties b and c
  // o.__proto__.__proto__ --> is Object.prototype
  // o.__proto__.__proto__.__proto__ --> is null.

  console.log(o.b); // 2
  // Is there a 'b' own property on o? Yes, and its value is 2.
  // The prototype also has a 'b' property, but it's not visited.
  // This is called Property Shadowing.
  ```

### 📌📌 fonksiyonlar

- __call(thisArg, arg1, arg2 ...)__

  ilgili fonksiyonu execute eder. args ile execute ederken göndereceğimiz parametreleri bir dizide göndeririz. thisArgs ise, fonksiyonun içindeki "this" scope'unu ezmek için kullanırız.

- __apply(thisArg, args)__

  "call" ile aynı görevi görür. sadece parametre alışı farklıdır. args bir dizi olmak zorundadır.

- __bind(thisArg, arg1, arg2 ...)__

  "call" ile aynı görevi görür. tek farkı; "bind", metodu execute etmez. ona parametreleri geçer. metot farklı satırda execute edilebilir. eğer argümanlar(args1, args2...) yollanmazsa; fonksiyon bir sonraki kod bloklarında execute edildiğinde de verilebilir. bind metodunda argümanlar verilirse; ileride execute ederken vermek gerekmez.

- __isGenerator()__

  (deprecated or not standart) fonksiyon içerisinde "yield" keyword'u bulunup bulunmadığını döndürür. (yield başka başlıkta anlatılıyor)

- __toSource()__

  (deprecated or not standart) fonksiyon içindeki kod bloğunu string olarak döndürür.

- __toString()__

  toSource() ile aynı görevi görür. fakat toString daha standarttır.

### 📌📌 objelerin içerdiği fonksiyonlar

- __valueOf()__

  o objenin primitive değerini döndürmelidir. == işaretinin yaptığı karşılaştırma için kullanılır. bu sebeple unique bir primitive döndürmelidir. valueOf() metodu objenin tipinden bağımsız bir eşitlik kontrolüdür. (başka başlıkta == vs === anlatılıyor)

  istisna olarak bir array'in valueOf'u array'in kendisini döner.

  eğer valueOf, developer tarafından hiç override edilmediyse JSON objesi döner. bu JSON objesinde ne olduğuna örnek üzerinden bakarsak:

  ```js
  function person(first) {

      this.firstName = first;

      this.lastName = null;

      this.changeLastName = function (name) {

          this.lastName = name;
      };
  }

  var myFather = new person("John");

  print(myFather.firstName);
  // output:
  // {
  //   firstName: John,
  //   lastName: null,
  //   changeLastName = function (name) {
  //         this.lastName = name;
  //   },
  //   __proto__: {}
  // }
  ```

## 📌 accessor property

Özel bir syntax'tır. "get a()" satırı, "object.a" dediğimizde döndürülecek değeri belirler. benzer şekilde "set a(v)" satırı, onu set ettiğimizde çalıştırılacak metodu çalıştırır. örnek:

```js
var o = {

  n: 1,

  get a(){ return this.n; },

  set a(v){ this.n = v*v; }
};

o.a // prints 1

o.a = 4;  //set ettiğimiz satır

o.a // prints 16
```

## 📌 JS'te bir syntax örneği

```js
(  function(name) {
              console.log("hello " + name)
}) ("ahmet")
```

Burada, "name" parametresi alan anonim bir fonksiyon tanımlanıyor. Bu fonksiyonu oluşturuyoruz (parantez içerisinde) ve hemen yanında metot çağırdığımızda yaptığımız gibi parantez içinde parametrelerini veriyoruz ve anonim fonksiyonu execute ediyoruz. sonuç olarak console'da "hello ahmet" çıktısını alıyoruz.

## 📌 REST operator

burada kullanılan "..." (üç nokta) REST operatörü olarak adlandırılır. fonksiyonlara sınırsız parametre yollanabilmesini sağlar.

```js
function f(x, y, ...arr1) {

  // arr1 is simple JS array.
}

// calling above method
f("a", "b", 1, 2, 3, 4);
```
