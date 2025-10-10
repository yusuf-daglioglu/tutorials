# JAVA LAMBDA AND STREAM

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 lambda expression

- `Kalkülüs (or eng:calculus)` hesap anlamına gelmektedir.
- `lambda calculus (or λ-calculus)` matematikte fonksiyonların ifade edilmesi için bir çeşit syntax'tır. programlama dillerindeki aynı yapıya tekabül eder.
- `Kalkülüs` aynı zamanda matematiğin temel pozitif bilimler için gerekli birçok konuyu (türev ve integrali kapsayan matematiksel analitiğin giriş kısmı) kapsayan alanın adıdır.

- `lambda` programlama dünyasında anonim fonksiyon yaratmak için kullanılan özel bir syntax ismidir. örnek `lambda` expression:

```text
(param1) --> { print(param1) }
```

`lambda`, programlama dünyasında `anonim fonksiyon` anlamına gelmemektedir. fakat `anonim fonksiyon` yaratmak için kullanılan syntax'tır. Çünkü `anonim fonksiyon` için tek syntax `lambda` olmayabilir. örneğin `JS`'te şu şekilde de anonim metot yaratılabilir:

```text
function(param1){ print(param1) }
```

Yani; `lambda` expression kullanılmadan da `anonim fonksiyon` yaratılabiliyor.

- `JS`'te `lambda expression` terimi yerine; `arrow function expression` kullanılır.

## 📌 Java dilinde lambda

`Java` 8 ile hem `lambda expression` hem de `fonksiyon arayüz (or function interface)` özelliği gelmiştir.

```java
@FunctionalInterface // bu notasyon isteğe bağlı. hiçbir özelliği yok. ama atılması öneriliyor. Çünkü normal arayüzler ile aynı syntax'ı var aşağıdaki kodun. farkını okuyan görebilmeli. (gelecekte bu iki farklı ama aynı syntax'a ait durumları derleyici veya IDE eklentilerinin ayırt etmesi gerekebilir)
interface MyInterface {

    MyResultObj myMethod(int myParam);
}
```

ile yukarıda tanımlama yaptık. Aşağıda da kullandık:

```java
MyInterface myInterface = realParam -> {

            System.out.println(realParam);

            return new MyResultObj();
};

myInterface.myMethod(3);
```

`myMethod`'u implemente etmiş olduk. arayüzde `myMethod` gibi sadece bir metot olabilir. daha fazla olamaz. bu sebeple metodu implemente ederken hiç `myMethod` `string`'ini hiç kullanmadık.

- `myMethod` iki adet parametre alsaydı, kullanım kısmında şu satır olacaktı:

  ```java
  MyInterface myInterface = (realParam, realParam2) -> {
  ```

- `myMethod` metodumuz isterse parametre döndürebilir. doğal olarak dönüş parametresi kullanım kısmında `myInterface.myMethod(3)` satırındaki `myMethod` metodundan dönüyor olacaktı.

- `Interface`'ler extend edebilirler.

- kullanım kısmında; `System.out.println(3);` etrafındaki süslü parantezler yazılmayabilir. bu durum sadece tek satırlı kodlar için geçerli.

- `java.util.Function` altında birçok hazır `fonksiyon arayüz`leri tanımlanmıştır. örnek: `BiConsumer<T,U>`, `Consumer<T>` gibi. Bunlar hazırdır. Çünkü; çoğu zaman genellikle fonksiyon arayüzleri birkaç adet parametre alır ve birkaç adet parametre döner yada dönmez. Bunlar için sürekli yeni fonksiyon interface tanımlaması yapmamız gerekir. bu sebeple hazırları oluşturulmuştur. örnekler:

  - `Consumer`; herhangi bir parametre alır, ama hiçbir şey dönmez. `JDK` içerisinde source code'a bakarsak:

  ```java
  @FunctionalInterface
  public interface Consumer<T> {
      void accept(T t);

      // Opsiyonel olarak burada farklı utility metotları olabilir.
      // Fakat bu ek metotlar kesinlikle interface'de implemente edilmiş olmalıdır.
      // Çünkü functional interface'ler sadece 1 adet implemente edilmemiş metot içerebilir.
      // Bu metodu da developer, FunctionalInterface'i initialize ederken implemente eder.
  }
  ```

  `Consumer`'a alternatif olan diğer birçok örneği tabloda toplarsak:

  (`T`, `U`, `R` herhangi bir class'ı temsil eder)

| interface adı    | aldığı parametrelerin tipi | döndürdüğü parametre tipi |
|------------------|----------------------------|---------------------------|
| `Consumer`       | T                          | void                      |
| `BiConsumer`     | T U                        | void                      |
| `Function`       | T                          | R                         |
| `BiFunction`     | T U                        | R                         |
| `UnaryOperator`  | T                          | T                         |
| `BinaryOperator` | T T                        | T                         |
| `Predicate`      | T                          | boolean                   |
| `BiPredicate`    | T T                        | boolean                   |
| `Supplier`       | void                       | T                         |
| `ToIntFunction`  | T                          | int                       |
| `DoubleConsumer` | double                     | void                      |
| `ObjIntConsumer` | Object int                 | void                      |

`QueryDSL` ve `JPA Criteria` kütüphanelerinin `Predicate` konusu farklıdır.

## 📌 real example

```java
// sumFunction'ı aşağıdaki ("bu yanlış" yazan satırdaki) gibi tanımlayabilirdik.
// Fakat stream'deki "mapToInt" metodu, parametre olarak "ToIntFunction" kabul ediyor.
ToIntFunction<Car> sumFunction; // bu doğru
Function<Car, Integer> sumFunction; // bu yanlış

// method reference:
sumFunction = Car::getSalePrice;
// lambda expression:
sumFunction = (car) -> car.getSalePrice();
// lambda expression (bazı kaynaklarda "lambda statement" olarak ifade ediliyor):
sumFunction = (car) -> { return car.getNumberOfAdults(); };

int total = carList
                .stream()
                .mapToInt(sumFunction)
                .sum();
```

## 📌 lambda vs closure

`lambda`, `closure` ile aynı anlama gelmiyor. çoğu yerde aynıymış gibi yansıtılıyor. `lambda` bir syntax. `closure` ise return edilen bir fonksiyon aracılığı ile farklı `scope`'tan değer değiştirmemize yarayan metottur. `lambda` desteği olan yazılımda genelde `closure` olabileceği için bu iki terim aynıymış gibi kullanılıyor.

## 📌 metot referansları

tanımlama kısmında, implemente etmiş olduğumuz metot zaten başka bir yerde zaten var ise, başka yerdekini direk referans edebiliriz. örnek;

```java
MyInterface myInterface = AnotherClass::otherMethod; // "otherMethod" is static method or public and the return type of "otherMethod" is compatible with "myInterface".
MyInterface myInterface = anotherObj::otherMethod; // anotherObj is an instance
MyInterface myInterface = AnotherClass::new; // refers the constructor method
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Java stream

`Java` 8 ile gelmiştir.

`Stream` bir kütüphane'dir. Fakat `Lambda` syntax'ına bağlıdır, çünkü `stream` fonksiyonları `lamda` argümanı kabul eder.

`Stream` metotları genelde `stream` döner. ard arda metot çağırma kolaylığı olsun diye böyle yazıldılar.

## 📌 different streams

`stream` genel bir kavram. birçok bağımsız stream vardır:

- `java.util.stream.DoubleStream` (developed for primitive types only)
- `java.util.stream.LongStream` (developed for primitive types only)
- `java.util.stream.Stream` (generic stream)

Yukarıdakiler birbirinin implementasyonu değildir. Ama kolaylık olsun diye, benzer isimde metotlar ve özellikler sunarlar.

Bunlar gibi kendi `stream`'lerimizi de yazabiliriz.

```java
List<Integer> numberList = Arrays.asList(1, 2);

numberList // numberList is java.util.ArrayList

    .stream() 
    /* bu metot bunu çağırıyor:

    java.util.Collection içindeki:

    default Stream<E> stream() { // this is java.util.stream.Stream

          return StreamSupport.stream(spliterator(), false);
    }

    Burada spliterator() ArrayList veya List tarafından implemente edilmiş olmalı.
    spliterator() iterator metodu gibi elemanları dönen bir metot. Stream business'ı ile alakalı kodları içermiyor.

    Yani her collection, stream business'ı ile ilgili kod içermiyor.

    */


    .filter(e -> e != 100) 
    /* Artık collection kodları yok.

    Burada çağrılan metotlar java.util.stream.Stream'in yönettiği kodlar.

    java.util.stream.Stream içinde filter metodu bir interface metodu.
    Bunun implementasyonunu bize stream() metodu dönmüş oluyor.
    Java içinde gelen Stream'ler genelde bu implementasyonlara "Pipeline" diye isimlendiriyor.

    */


    .map(x -> x + 1)
    /*
    "filter" metodu ile aynı yorumlar geçerlidir.
    */


    .collect(Collectors.toList());
    /*
    "filter" metodu ile aynı yorumlar geçerlidir.
    */
```

## 📌 examples

```java
package org.example;

import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.IntStream;
import java.util.stream.LongStream;
import java.util.stream.Stream;

public class TestClass {

public static void main(String[] args) {

    ////////////////////////////////////
    ////////////////////////////////////
    // BASIC EXAMPLE
    List<Integer> numberList = Arrays.asList(1, 2, 3, 99);
    List result1 = numberList
            .stream()
            .filter(e -> e != 100)
            .map(x -> x + 1)
            .collect(Collectors.toList());
    // Collectors.toList() ---> Functional Interface dönüyor.
    // Collectors.toList() yerine toSet() kullansaydık Set dönerdi.

    ////////////////////////////////////
    ////////////////////////////////////
    // PRIMITIVE TYPES
    // LongStream, java.util.stream.Stream implementasyonu değildir.
    // LongStream primitive tip tutar.
    // LongStream'in metot isimleri java.util.stream.Stream'e çok benzerdir.
    // LongStream'i, boxed metodu ile java.util.stream.Stream'e çeviririz.
    LongStream.of(1l, 2l, 3l).
        boxed()
        .collect(Collectors.toList());

    ////////////////////////////////////
    ////////////////////////////////////
    // FLATMAP
    // "map" metodu obje dönerken, flatMap stream dönüyor.
    // Aslında "flatMap" metodunun ismi, "map to stream" de olabilirdi.
    // flatmap; içindeki fonksiyonun döndürdüğü bütün stream'leri, yeni bir stream olarak döner.

    // flatmap örnek 1:
    // Aşağıdaki "flatMap" satırı şu bilgileri içeren stream dönüyor: {a,b,x,y,1,2}
    String[][] arrayOfArray = new String[][]{{"a", "b"}, {"x", "y"}, {"1", "2"}};
    List<String> result3 = Stream.of(arrayOfArray)  // returns: Stream<String[]>
            .flatMap(Stream::of)                    // returns: Stream<String>
            .filter(x -> !"a".equals(x))      // returns: Stream<String>
            .collect(Collectors.toList());

    // flatMap örnek 2:
    List<String> messageList3 = Arrays.asList("Hello", "World", "Maria");
    // Below code prints only the second character of each element.
    messageList3
            .stream()                                   // returns: Stream<String>
            .flatMap(e -> Stream.of(e.charAt(2))) // returns: Stream<Character>
            .forEach(System.out::println);              // returns: void
          // forEach(x -> System.out.println(x)) // same as above.
    
    ////////////////////////////////////
    ////////////////////////////////////
    // REDUCE
    // Ek not: "range" metodu 1, 2, 3 elementlerinden oluşan bir IntegerStream yaratıyor.
    // reduce metodu ise kümülatif işlemler yapmamızı sağlıyor.
    // reduce önce 1 ve 2'yi bizim yolladığımız fonksiyona atıyor.
    // daha sonra sonucu "3" ile yine bizim yolladığımız parametreye yolluyor.
    // ve en sonda sonucu döndürüyor.
    // yani; şu işlemler sırası ile oluyor:

    // (1,2) -> 1+2   (buradan 3 geliyor)
    // (3,9) -> 3+9   (buradan 11 geliyor)

    // Ek not: OptionalInt ==> Optional<Integer> ile aynı davranışları sergiliyor.
    // Sadece Optional<Integer> primitive "int" tutamadığı için 
    // (Java'da generic'ler primitive desteklemediği için)
    // onun yerini tutan basit bir sınıf geliştirildi.

    // OptionalInt=9 olarak dönüyor.
    OptionalInt reduced = IntStream
            .range(1, 4)  // returns: IntStream
            .reduce((a, b) -> a + b);
          // .reduce(Integer::sum); // same as above.

    ////////////////////////////////////
    ////////////////////////////////////
    // MATCH OPERATIONS
    List<String> messageList4 = Arrays.asList("Hello", "Hi", "Maria");

    // noneMatch: her elementi kontrol eder. Belirtilen kriter listede hiçbir elemanda bulunmuyor ise true döndürür.
    boolean result5 = messageList4.stream().noneMatch(message -> message.contains("x"));

    // allMatch: Belirtilen kriter listede tüm elemanlarda bulunuyor ise true döndürür.
    boolean result6 = messageList4.stream().allMatch(message -> message.contains("a"));

    // anyMatch: Belirtilen kriter listede herhangi bir elemanlarda bulunuyor ise true döndürür.
    boolean result7 = messageList4.stream().anyMatch(message -> message.contains("a"));

    // findAny: rastgele bir eleman döndürür.
    // paralel olmayan stream'lerde ilk elemanı dönüyor gibi gözükse de bunun garantisi verilmez.
    // paralel olan stream'lerde hep farklı eleman döner.
    Optional<String> result8 = messageList4.stream().findAny();

    // findFirst: mutlaka ilk elemanı döndürür.
    Optional<String> result9 = messageList4.stream().findFirst();
}
}
```

## 📌 intermediate operations vs terminal operations

`filter()` gibi metotlar yeni bir `stream` objesi döndürür, bu sebeple `intermediate` olarak nitelendirilir. oysa `terminal` operation metotları, `Stream.forEach` gibi, `stream` değilde, herhangi farklı bir obje döndürür.

`intermediate` metotlar 2'ye ayrılır:

- `stateless`

  bir önceki `stream`'den hiçbir `state` tutmazlar. örnek: `filter` and `map`.

- `stateful`

  örnek: `sorted`.

## 📌 parallel vs serial

`Collection` sınıfı içinde `stream()` and `parallelStream()` metotları vardır. sadece buna göre `stream`'lerin paralel yürütülüp, yürütülmeyeceğine karar verilir.

örnek:

```java
List number = Arrays.asList(2,3,4,5);
number.parallelStream().forEach(y->System.out.println(y));
```

Paralel olan `stream`'ler paralel çalışacağı için sıra önemsiz olan kod logic'lerinde kullanılmalıdır. Paralel olanlar daha hızlıdır.

## 📌 java.util.stream.BaseStream

`java.util.stream.Stream`, `BaseStream`'den türemiştir. `BaseStream` ise sadece `java.lang.AutoCloseable`'dan türemiştir. `AutoCloseable` sadece `close` metodu içerir. Fakat her `stream`, `close` metodunu implemente etmek zorunda değildir. `I/O` işlemi yapan `stream`'ler `close`'u implemente etmelidir.

```java
public interface BaseStream<T, S extends BaseStream<T, S>>
```

metotları:

- `Iterator\<T> iterator();`

  `Stream` ile dönülecek data'ları `java.util.Iterator` instance'ına atar ve bu `iterator`'ı döner.

- `Spliterator\<T> spliterator();`

  `java.util.Spliterator`, `java.util.Iterator`'a benzer bir yapıdır. `iterator` gibi çalışıyor, fakat tüm iterasyonu sıra ile yapmak zorunda bırakmıyor. onun yerine parça parça gruplara ayırıp, istersek her parçayı farklı thread'lerde iterate edebilmemizi sağlıyor.

- `boolean isParallel();`

  kullanılan `stream`'in paralel olup olmadığının bilgisi döner.

- `S sequential();`

  `stream` paralel ise, onun paralel olmayan instance'ını döndürür.

- `S parallel();`

  `stream`'in paralel olan instance'ını döndürüyor.

- `S unordered();`

  `sequential()` ile aynı görevi görür. tek farkı sıranın önemsenmemesidir.
  
  örneğin bir array-list'ten `stream` oluşturduk. eğer `unordered()` metodundan dönen `stream`'i kullanırsak, liste random sıra ile karşımıza çıkabilir. ama `sequential()` mutlaka listeyi var olan orjinal sırası ile döndürecektir.

- `S onClose(Runnable closeHandler);`

  `AutoCloseable`'ın metodu değildir. Burası close yapıldığında çalıştırılacak callback metodunu parametre alır.

## 📌 StreamSupport

`Iterable`'larda `stream` dönen metot yok (`iterableInstance.stream()` yok). Bu sebeple özel olarak `StreamSupport` geliştirildi.

```java
Iterable<String> iterable = Arrays.asList("a", "b");

// Dikkat: aşağıdaki "spliterator" metodu "java.util.stream.BaseStream" içindeki bir metot değildir.
// java.lang.Iterable'ın bir metodudur.
List<String> result = StreamSupport.stream(iterable.spliterator(), false)
                                          .map(String::toUpperCase)
                                          .collect(Collectors.toList());
```

`StreamSupport` zaten `Iterable` için geliştirildi. Fakat, dikkat edersek, buna rağmen şunu yapmadık:

```java
List<String> result = StreamSupport.stream(iterable, false)
```

Bunun birkaç sebebi var. Fakat en önemli sebep; `Iterable`'ın paralel olarak elemanları döndürememesidir. Fakat `Stream`'ler paralelde koşabiliyor. Burada uyumsuzluk olacağı için, artık yeni `java.util.Spliterator` sınıfı kullanılıyor. Yani `iterable.spliterator()`, bir `java.util.Spliterator` sınıfı dönüyor.
