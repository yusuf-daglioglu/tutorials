# LANGUAGE

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Memoization

Memoization performance'ı arttırmak için kullanılan bir tekniktir. Memoization tekniğinde; çağrılan fonksiyonların sonuçlarını bir yerde saklarız. bu şekilde tekrar aynı parametrelerde fonksiyon çağrılmak istendiğinde, sonucu sakladığımız yerden okuruz.

## 📌 dynamic programming (⟷ dinamik programlama)

bir çeşit matematiksel optimizasyon tekniği olduğu için bazı kaynaklarda __dinamik optimizasyon (⟷ dynamic optimization)__ olarak geçer.

dynamic programming ile yazılan kodlar recursive veya iterative olabilir.

dynamic programming, problemi, sub-problemlere bölerek çözme yöntemidir. fakat bunu yaparken örtüşen (overlapping) olan bölümleri de ilişkilendirip performance'tan kazanmaya çalışan çözümler olması şarttır. örneğin; Fibonacci'yi iterative kod ile memoisation ile çözersek, dynamic programming yapmış oluruz.

not: dynamic programming ve Memoization terimi birçok kaynakta yanlış anlamda kullanılıyor. Bazı kaynaklar top-down design veya bottom-up design yerine kullanıyorlar. fonksiyon implemente edildikçe top-down design veya bottom-up design ile aynı kapıya çıktığı doğru, fakat terminolojik olarak kök sebepleri farklı.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Özyinelemeli (⟷ Recursive) solution vs Tekrarlamalı (⟷ Iterative) solution

recursive kod daha fazla CPU ve RAM harcar çünkü arka planda program her çağrılan fonksiyon için yeni bir call-stack açar. iterative kod ise görünürde daha uzundur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Syntactic sugar

kolaylaştırılmak üzere geliştirilmiş syntax'a verilen isimdir. örneğin; C dilinde:

```c
a[i]
```

buna denk gelmektedir:

```c
*(a + i)
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 implicit vs explicit

implicit anlamı: üstü kapalı.

explicit anlamı: açık/belirgin.

implicit, framework'ün arka planda söylemeden bir işlemi yapmasıdır.

explicit ise; yazılımcının kendi halletmek durumunda kaldığı işlemlere denir.

### 📌📌 Örnek 1

Java will provide us default constructor implicitly. Even if the programmer didn't write code for constructor, he can call default constructor.

### 📌📌 Örnek 2

```java
// explicit Locking
Student student = entityManager.find(Student.class, id);
entityManager.lock(student, LockModeType.OPTIMISTIC);
```

### 📌📌 Örnek 3

Explicit casting:

official name: __widening conversion__

```java
double x = 10.5;
int y = (int) x;
```

Implicit casting:

officially name: __narrowing conversion__

```java
int x = 10;
double y = x;
```

### 📌📌 Örnek 4

Mockito kütüphanesi mock instance'larımızı tüm çağrılan kodlardaki field'lara implicit şekilde inject eder:

```java
@InjectMocks
Dependency1 d1;
```

Oysa annotation kullanmazsak, elle bu işlemi yaparsak explicit şekilde set etmiş oluruz:

```java
Dependency1 d1 = Mockito.mock(Dependency1);
myService.setD1(d1);
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 subset (⟷ altküme)

subset'in tersi: __superset__

if A is subset of B; then B-compiler can compile A code. subset/superset concepts are only about language syntax.

TypeScript is a superset of JS. That means JS is a subset of Typescript. fakat; Her typescript compiler'ı, JS kodunu derleyebilecek diye bir kaide yok. bunun birçok sebebi olabilir. bazı sebepler:

- syntax uyabilir, ama native gelen primitive objeler uymak zorunda değil.
- default API'ler farklı olabilir.
- paket isimleri farklı olabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 header file (⟷ copybook)

programla dillerinde genelde dosyanın tepesinde bazı kütüphaneler import edilir. eğer kütüphane import edilirse ilgili kütüphane dosyaları, ilgili dosyamızın içine kopyalanır. bu durumda kopyalanan bu dosyalar header dosyalarımız olur. C/C++ buna en iyi örnektir.

Java C# gibi dillerde header file yoktur. çünkü sadece ilgili kütüphanenin referansı tutulur. bunlara da __dynamic library symbols__ denir.

## 📌 symbol (⟷ sembol)

symbol bazı programlama dillerinde __identifier__ yada __atom__ olarak ta isimlendirilir.

symbol programlama dillerinde: metot, instance, sınıf, field gibi tekil şeylerin referanslarıdır... Sembolün tüm programlama dillerinde global bir tanımı yoktur. Dilin kendisine kalmıştır.

## 📌 type

sembolün tipidir. Örneğin bir field var. Bu field bir semboldür ama tipi bir Student sınıfına tekabül ediyordur. Student bu field'ın tipi olur.

## 📌 type safe

Dilin tipini compile'dan önce belirtmek zorundaysak o dil-type safe'tir.

Bazı diller hem type-safe hem de type-unsafe olabilir. Örneğin; Java'nın 10'uncu sürümünde "var" keyword'ü geldi. Fakat hala eskisi gibi bir değişkene tip verebiliyoruz. Dolayısı ile Java'nın 10'uncu sürümü ile artık hem type-safe hem de type-unsafe destekliyor. Java eskiden sadece type-safe'ti.

## 📌 Type Inference

Inference kelime anlamı: sonuç/anlam çıkarma.

Bir değişkenin tipi, dil tarafından compile ve/veya runtime sırasında otomatik belirleniyorsa, o dilde "Type Inference" vardır.

"var" (Java, C#, JS), "auto" (C++) keyword'leri ile yapılırlar.

## 📌 Static typing language (⟷ Statically typing language) vs Dynamic typing language (⟷ dynamically typing language)

Eğer bir değişkenin tipi belirlenmiş ise, daha sonra ona başka bir tip atayabiliyorsak bu dil "Dynamic typing language"tir. Eğer bir değişkene atanmış tipi, tekrar değiştiremiyorsak, o dil "Static typing language" tipindedir.

Tipin otomatik mi algılanıp set edileceği, yoksa yazılımcının mı mutlaka tanımlaması gerektiği konusu ayrı bir konudur ("Type Inference" konusudur).

Diller üzerinden örnekler:

- Swift'te, eğer ilerideki kod satırlarında aynı referansa farklı bir tipte değer set etmeye çalışırsak runtime'da hata alırız. kaynak: <https://docs.swift.org/swift-book/LanguageGuide/TheBasics.html> title: "Type Safety and Type Inference".
- JS'te integer bir değere, hemen alt satırında String olarak set edebiliyoruz. fakat TypeScript Static typed'tır.
- `script language` yapıları genelde `Dynamically typed`'tır.

Trade-off'lar:

- Static typing compile time süresini uzatır.
- sadece static typing diller, runtime'da tip kontrolü olmadığından dolayı özellikle eklentiler vs güvenlik açıklarına yol açabilir.
- runtime'da tip kontrolü, runtime'ı yavaşlatır.
- static typing dillerde tip olması developer'ların daha az bug olan kodlar yazmasını sağlar.

## 📌 strongly typed vs weakly typed (⟷ loosely typed)

Bu 2 terim farklı kaynaklarda farklı anlamlarda kullanılıyor. Definition için kesin ve ortak bir netlik yok.

Genelde şu anlamda kullanılıyor: Bazı diller string bir değeri eğer içindeki sayı ise gerektiğinde sayı'ya kendisi cast edebiliyor. JS weakly typed'tır. Strongly typed ise bunun tam tersi.

Aşağıda görüldüğü gibi JS weakly typed'tır bu sebeple aşağıdaki kod bloğu sorunsuz çalışacaktır:

```js
value = 99;
value = value + "hello";
console.log(value);
```

Yukarıdaki kod örneğinde; "Artı" karakteri, aslında bir fonksiyona denk geliyor. Bu fonksiyonun ilk parametresi String. String yollamıyoruz fakat String'e cast işlemi yapılabiliyorsa, JS String'e otomatik cast ediyor. Bu durum beklenmedik hatalara sebep olabilir.

## 📌 static binding (⟷ early binding) vs dynamic binding (⟷ late binding)

Dog d1=new Dog();  --> static binding

Animal a=new Dog();  --> dynamic binding

## 📌 Compile Time Polymorphism vs Runtime Polymorphism

polimorfizm bir nesnenin farklı şekillerde davranabilmesidir. bu sebeple iki farklı polimorfizm gözlemlenebilir:

compile time --> Static binding veya Method overloading olan durumlarda geçerlidir

runtime --> Dynamic binding veya Method overriding olan durumlarda geçerlidir

## 📌 Type Erasure

Type bilgisinin compile sırasında silinmesi durumudur.

Örneğin; Java'da Generic'ler (generic'teki sınıf bilgisi) compile time sırasında silinmektedir. Bu sebeple IDE ve compiler eğer uyuşmazlık varsa hata verir, fakat runtime'da `JVM` hata fırlatmaz. kaynak: <https://docs.oracle.com/javase/tutorial/java/generics/erasure.html>

Java'daki bu durum, bu repo'da örneklendirilmiştir: <https://github.com/yusuf-daglioglu/java-generics-type-erasure>

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Bağlaşım (⟷ Coupling)

sınıfların birbirine bağımlı olma durumlarıdır. X sınıfında değişiklik yapınca Y'de de değişiklik yapılması gerekiyor ise Y, X'ye bağlıdır.

## 📌 Gevşek bağlılık (⟷ Loosely Coupled ⟷ Low Coupling) vs Sıkı bağlılık (⟷ Tightly Coupled)

sıkı bağlılığa örnek:

```java
class Traveller {

    Car c = new Car();

    Public void startJourney() {
        c.move();
    }
}
```

gevşek bağlılığa örnek:

```java
class Traveller {

    Arac c; // burada injection şart ve bir interface olması önemli.

    Public void startJourney() {
        c.move();
    }
}
```

## 📌 Cohesion

Türkçe kelime anlamı: yapışkanlık

Bir sınıf Single responsibility principle çerçevesinde yalnızca tek bir görevi gerçekleştirmelidir. Eğer bir sınıf içerisinde birden fazla değişik iş yapan yordamlar ya da bağımsız veri alanları bulunuyorsa, bu iş grupları farklı sınıflar halinde ayrıştırılmalıdır. Bir sınıf içerisindeki yordamlar ortak veri alanlarını kullanmıyorlarsa, muhtemelen ayrı sınıflarda olmaları gerekir.

Yapışkanlık derecesi düşük olması, sınıf içerindeki fonksiyonların birbirinden alakasız olduğunu belirtiyor.

__LCOM (⟷ Lack of Cohesion Methods)__ ilgili sınıfta kaç farklı daha gruplama yapılabileceğimizi (yani kaç farklı sınıfa ayırabileceğimizi) temsil eden sayıdır.

LCOM4 ise bunu hesaplamak için kullanılan özel bir algoritma/yöntem ismidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 method/function invoke vs call

Ikisi de kelime anlamı olarak aynı. resmi bir farklılık yok. fakat çoğu zaman "invoke" kelimesi otomatik olarak çalıştırılan metotlar için kullanılırken kullanılır. örneğin; sınıf oluşturduk, içindeki before ve constructor metotları otomatik çalıştırıldı. oysa "call" ibaresi daha çok elle bir fonksiyonu çağırdığımızda kullanılır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 memory-safe

RAM'de tutulan verileri kullanmadıkça silen, böylece ekstra güvenlik sağlayan programlama dillerine denir.

Örnek diller: Java, Rust.

Bu konuda özellikle Rust çok başarılı. Çünkü Java'da runtime'da garbage-collector'ın ne zaman devreye gireceği belirsizken, Rust dili bu konularda çok katı.

C ve C++ gibi dillerde ise bir variable tamamen manuel kod satırı yazarak siliniyor. Bu diller memory-safe değildir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Metot Aşırı Yükleme (⟷ Method Overload)

- yazılım dünyasında kullanılan genel bir terimdir.
- her programlama dili desteklemez.
- bir metot aynı isimde, aynı sınıfta/scope'ta olabilir. fakat aldığı parametrelerin:
  - sayısı farklı 
  - veya sayısı aynı fakat tipleri farklı olabilir.
  
  bu durumda o metod aşırı yüklenmiştir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 pure vs impure function

- pure fonksiyonlar sadece bir hesaplama yapıp bir sonuç geri döndürürler.
- uygulamanın içinde yada dış kaynaklarda hiçbir şeyi değiştirmezler.
- hiçbir şeyi değiştirmedikleri gibi hiçbir şey de okumazlar, okusalar bile, okudukları bu değeri hiç kullanmamalıdırlar (zaten bu durumda o okuma boşuna yapılmış olur).
- bu şekilde sonuç olarak; her zaman aynı girişe karşı aynı çıktıyı verirler.
- deterministiktirler.
- pure olmayan fonksiyonları çağıramazlar. pure olmayan fonksiyonlara örnek: Date.now(), Math.random().

impure metotlar ise bu kurala uymayan tüm metotlar için geçerli bir sıfattır.

## 📌 side effect

bir fonksiyonun kendi içindeki değerler haricinde dışarıda bazı kaynaklarda değişiklik yapmasıdır. yani dış dünyaya etkisi olmasıdır. örneğin; dosyaya yazması. console.log() metodu dahi dış kaynakları kullanır. console'u milyon saniyede bir de olsa durdurur ve o kaynağın başkası tarafından kullanılmasını engellemiş olur. DB'den veri okumak bile side effect yaratır. okunan veri için dışa kaynaklarda soket açılır, belki o soketler başkası tarafından kullanılacaktı. DB sistemi ayarları sebebi ile biri okuduğunda başkasının o veriler üzerinde değişiklik yapmasını engelleyebilir. bu da bir etkidir (side effect'tir).

## 📌 Referential transparency (⟷ referential opacity)

kodun bir satırında bir metot çağırdık. çağırdığımız bu metodu kaldırıp yerine döndürdüğü değeri yapıştırdığımızda, tüm yazılım yine aynı şekilde çalışmaya devam ediyorsa, bu metot için "Referential transparency" özelliğinden bahsedilebilir demek oluyor. matematikte her fonksiyon için kesinlikle Referential transparency vardır. fakat yazılımda kesinlikle bu durum var diyemiyoruz. çünkü çağırdığımız metot pure bir metot değilse, "Referential transparency" özelliğinden bahsedilemez.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 method vs function

metot; bir objenin fonksiyonudur.

fonksiyon ise; sınıflardan bağımsızdır.

Object-oriented dünyasında "metot" ismi kullanılır. Bunun çok detaylı bir sebebi yok. "metot" terimi, "yöntem" terimi ile aynıdır. detaylı düşünüldüğünde; bir yöntem/metot ile bir akışı başlatmak için o yöntemi uygulayan bir varlığın (kod içerisinde: sınıfın) olması şarttır. Örneğin Canlı arayüzünden türemiş aslan ve insan sınıflarının (varlıklarının) "koş" isimli metot'ları düşünülebilir. oysa "fonksiyon"; matematik'te de olduğu gibi bir varlığa ait değildir. tüm sistemden bağımsızdır.

object-oriented dillerde, her fonksiyon birer metottur. object-oriented olmayan dillerde ise hepsi fonksiyondur. istisna olarak object-oriented'lerdeki static metotlar vardır. bunlar sınıfın fonksiyonlarıdır fakat sınıfa bir bağlılıkları yoktur. bu sebeple her zaman tartışma konusu olmuşlardır.

bu tanımların farkı yazılımcılar arasında bilinmediğinden; çoğu zaman yanlış yerlerde kullanılırlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
