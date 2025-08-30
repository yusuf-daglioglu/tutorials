# JAVA LAMBDA AND STREAM

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 lambda expression

- __Kalkülüs (or eng:calculus)__ hesap anlamına gelmektedir.
- __"lambda calculus (or λ-calculus)"__ matematikte fonksiyonların ifade edilmesi için bir çeşit syntax'tır. programlama dillerindeki aynı yapıya tekabül eder.
- Kalkülüs aynı zamanda matematiğin temel pozitif bilimler için gerekli birçok konuyu (türev ve integrali kapsayan matematiksel analitiğin giriş kısmı) kapsayan alanın adıdır.

- lambda programlama dünyasında anonim fonksiyon yaratmak için kullanılan özel bir syntax ismidir. örnek lamdba expression:

```text
(param1) --> { print(param1) }
```

lambda, programlama dünyasında "anonim fonksiyon" anlamına gelmemektedir. fakat anonim fonksiyon yaratmak için kullanılan syntax'tır. Çünkü "anonim fonksiyon" için tek syntax lambda olmayabilir. örneğin Javascript'te şu şekilde de anonim metot yaratılabilir:

```text
function(param1){ print(param1) }
```

Yani; lambda expression kullanılmadan da anonim metot yaratılabiliyor.

- Javascript'te "lambda expression" terimi yerine; "__arrow function expression__" kullanılır.

## 📌 Java dilinde lambda

Java 8 ile hem __lambda expression__ hemde __fonksiyon arayüz (or function interface)__ özelliği gelmiştir.

```java
@FunctionalInterface // bu notasyon isteğe bağlı. hiçbir özelliği yok. ama atılması öneriliyor. Çünkü normal arayüzler ile aynı syntax'ı var aşağıdaki kodun. farkını okuyan görebilmeli. (gelecekte bu iki farklı ama aynı syntaxa ait durumları derleyici vey akelnetilerin ayırt etmesi gerekebilir)
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

myMethod'u implemente etmiş olduk. arayüzde myMethod gibi sadece bir metot olabilir. daha fazla olamaz. bu sebeple metotu implemente ederken hiç "myMethod" string'ini hiç kullanmadık.

- myMethod iki adet parametre alsaydı, kullanım kısmında şu satır olacaktı:

  ```java
  MyInterface myInterface = (realParam, realParam2) -> {
  ```

- myMethod metotumuz isterse parametre döndürebilir. doğal olarak dönüş parametresi kullanım kısmında myInterface.myMethod(3) ‘teki myMethod metotundan dönüyor olacaktı.

- Interface ‘ler extend edebilirler.

- kullanım kısmında; System.out.println(3);  etrafındaki süslü { } parantezler yazılmayabilir (tek satırlı kodlar için geçerli.)

- java.util.Function altında birçok hazır fonksiyon arayüzleri tanımlanmıştır. örnek: BiConsumer\<T,U>, Consumer\<T> gibi... Bunlar hazırdır. Çünkü; çoğu zaman genellikle fonksiyon arayüzleri 1-2-3 parametre alır ve bir parametre döner yada dönmez. Bunlar için sürekli yeni fonksiyon interface tanımlaması yapmamız gerekir. bu sebeple hazırları oluşturulmuştur. örnekler:

  - Consumer (tüketici), yani herhangi bir parametre alır, ama hiçbir şey dönmez. JDK içerisinde source code'a bakarsak:

  ```java
  @FunctionalInterface
  public interface Consumer<T> {
      void accept(T t);

      // Opsiyonel olarak burada farklı utility metodları olabilir.
      // Fakat bu ek metodlar muhahkak interface'de implemene edilmiş olmalıdır.
      // Çünkü functional interface'ler sadece 1 adet implemente edilmemiş metod içerebilir.
      // Bu metodu da developer, FunctionalInterface'i initialize ederken implemente eder.
  }
  ```

  Consumer'a alternatif olan diğer birçok örneği tabloda toplarsak:

  (T, U, R herhangi bir class'ı temsil eder)

| interface adı  | aldığı parametrelerin tipi | döndürdüğü parametre tipi |
|----------------|----------------------------|---------------------------|
| Consumer       | T                          | void                      |
| BiConsumer     | T U                        | void                      |
| Function       | T                          | R                         |
| BiFunction     | T U                        | R                         |
| UnaryOperator  | T                          | T                         |
| BinaryOperator | T T                        | T                         |
| Predicate      | T                          | boolean                   |
| BiPredicate    | T T                        | boolean                   |
| Supplier       | void                       | T                         |
| ToIntFunction  | T                          | int                       |
| DoubleConsumer | double                     | void                      |
| ObjIntConsumer | Object int                 | void                      |

"Predicate" konusu başka başlıklarda anlatılıyor (QueryDSL ve JPA Criteria konularına bakılabilir).

## 📌 real example

```java
// sumFunction'ı aşağıdaki ("bu yanlış" yazan satırdaki) gibi tanımlayabilirdik.
// Fakat stream'deki "mapToInt" metotu, parametre olarak "ToIntFunction" kabul ediyor.
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

lambda closure ile aynı anlama gelmiyor. çoğu yerde aynıymış gibi yansıtılıyor. lambda bir syntax. closure ise return edilen bir fonksiyon aracılığı ile farklı scope'tan değer değiştirmemize yarayan metottur. lambda desteği olan yazılımda mutlaka closure olabileceği için bu iki terim aynıymış gibi kullanılıyor.

## 📌 metot referansları

tanımlama kısmında, implemente etmiş olduğumuz metot zaten başka bir yerde zaten var ise, başka yerdekini direk referans edebiliriz. örnek;

```java
MyInterface myInterface = AnotherClass::otherMehod; // "otherMehod" is static method or public and the return type of "otherMehod" is compatible with "myInterface".
MyInterface myInterface = anotherObj::otherMehod; // anotherObj is an instance
MyInterface myInterface = AnotherClass::new; // refers the constructor method
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Java stream

Java 8 ile gelmiştir.

Stream bir kütüphane'dir. Fakat Lamdda syntax'ına bağlıdır, çünkü stream fonskiyonları lamda argümanı kabul eder.

Stream metotları genelde stream döner. ardarda metod çağırma kolaylığı olsun diye böyle yazıldırlar.

## 📌 different streams

"stream" genel bir kavram. birçok bağımsız stream vardır:

- __java.util.stream.DoubleStream__ (developed for primitive types only)
- __java.util.stream.LongStream__ (developed for primitive types only)
- __java.util.stream.Stream__ (generic stream)

Yukarıdakiler birbirinin implementasyonu değildir. Ama kolaylık olsun diye, benzer isimde metodlar ve özellikler sunarlar.

Bunun gibi kendi stream'lerimizi de yazabiliriz.

```java
List<Integer> numberList = Arrays.asList(1, 2);

numberList // numberList is java.util.ArrayList

    .stream() 
    /* bu metod bunu çağrıyor:

    java.util.Collection içindeki:

    default Stream<E> stream() { // this is java.util.stream.Stream

          return StreamSupport.stream(spliterator(), false);
    }

    Burada spliterator() ArrayList veya List tarafından implemente edilmiş olmalı.
    spliterator() iterator metodu gibi elemanları dönen bir metod. Stream business'ı ile alakalı kodları içermiyor.

    Yani her collection, stream business'ı ile ilgili kod içermiyor.

    */


    .filter(e -> e != 100) 
    /* Artık collection kodları yok.

    Burada çağrılan metodlar java.util.stream.Stream'in yönettiği kodlar.

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
    // LongStream'in metod isimleri java.util.stream.Stream'e çok benzerdir.
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
    // Ek not: "range" metotu 1, 2, 3 elementlerinden oluşan bir IntegerStream yaratıyor.
    // recude metotu ise kümülatif işlemler yapmamızı sağlıyor.
    // reduce önce 1 ve 2'yi bizim yolladığımız fonksiyona atıyor.
    // daha sonra sonucu "3" ile yine bizim yolladığımız parametreye yolluyor.
    // ve en sonda sonucu döndürüyor.
    // yani; şu işlemler sırası ile oluyor:

    // (1,2) -> 1+2   (buradan 3 geliyor)
    // (3,9) -> 3+9   (buradan 11 geliyor)

    // Ek not: OptionalInt ==> Optional<Integer> ile aynı davranışları sergiliyor.
    // Sadece Optional<Integer> primitive "int" tutamamadığı için 
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
    // paralel olmayan stream'lerde ilk elemanı dönüyor gibi gözüksede bunun garantisi verilmez.
    // paralel olan stream'lerde hep farklı eleman döner.
    Optional<String> result8 = messageList4.stream().findAny();

    // findFirst: mutlaka ilk elemanı döndürür.
    Optional<String> result9 = messageList4.stream().findFirst();
}
}
```

## 📌 intermediate vs terminal operations

filter() gibi metotlar yeni bir stream objesi döndürür, bu sebeple intermediate olarak nitelendirilir. oysa terminal operation metotları, "Stream.forEach" gibi, stream değilde, herhangi farklı bir obje döndürür.

intermediate metotlar 2'ye ayrılır:

- stateless

  bir önceki stream'den hiçbir state tutmazlar. örnek: filter and map.

- stateful

  örnek: sorted

## 📌 parallel vs serial

Collection sınıfı içinde stream() and parallelStream() metotları vardır. sadece buna göre stream'lerin paralel yürütülüp, yürütülmeyeceğine karar verilir.

örnek:

```java
List number = Arrays.asList(2,3,4,5);
number.parallelStream().forEach(y->System.out.println(y));
```

Paralel olan stream'ler paralel çalışacağı için sıra önemsiz olan kod logic'lerinde kullanılmalıdır. Paralel olanlar daha hızlıdır.

## 📌 java.util.stream.BaseStream

__java.util.stream.Stream__, __BaseStream__'den türemiştir. __BaseStream__ ise sadece __java.lang.AutoCloseable__'dan türemiştir. __AutoCloseable__ sadece "__close__" metotu içerir. Fakat her stream, __close__ metotunu implemente etmek zorunda değildir. I/O işlemi yapan stream'ler __close__'u implemente etmelidir.

```java
public interface BaseStream<T, S extends BaseStream<T, S>>
```

metotları:

- __Iterator\<T> iterator();__

  Stream ile dönülecek data'ları __java.util.Iterator__ instance'ına atar ve bu iterator'ı döner.

- __Spliterator\<T> spliterator();__

  __java.util.Spliterator__, __java.util.Iterator__'a benzer bir yapıdır. iterator gibi çalışıyor, fakat tüm iterasyonu sıra ile yapmak zorunda bırakmıyor. onun yerine parça parça gruplara ayırıp, istersek her parçayı farklı thread'lerde iterate edebilmemizi sağlıyor.

- __boolean isParallel();__

  kullanılan stream'in paralel olup olmadığının bilgisi döner.

- __S sequential();__

  stream paralel ise, onun paralel olmayan instance'ını döndürür.

- __S parallel();__

  stream'in paralel olan instance'ını döndürüyor.

- __S unordered();__

  __sequential()__ ile aynı görevi görür. tek farkı sıranın önemsenmemesidir.
  
  örneğin bir array-list'ten stream oluşturduk. eğer __unordered()__ metodundan dönen stream'i kullanırsak, liste random sıra ile karşımıza çıkabilir. ama __sequential()__ mutlaka listeyi varolan orjinal sırası ile döndürecektir.

- __S onClose(Runnable closeHandler);__

  AutoCloseable'ın metotu değildir. Burası close yapıldığında çalıştırılacak callback metotunu parametre alır.

## 📌 StreamSupport

__Iterable__'larda __stream__ dönen metot yok ( iterableInstance.stream() yok ). Bu sebeple özel olarak __StreamSupport__ geliştirildi.

```java
Iterable<String> iterable = Arrays.asList("a", "b");

// Dikkat: aşağıdaki "spliterator" metodu "java.util.stream.BaseStream" içindeki bir metod değildir.
// java.lang.Iterable'ın bir metodudur.
List<String> result = StreamSupport.stream(iterable.spliterator(), false)
                                          .map(String::toUpperCase)
                                          .collect(Collectors.toList());
```

__StreamSupport__ zaten __Iterable__ için geliştirildi. Fakat, dikkat edersek, buna rağmen şunu yapmadık:

```java
List<String> result = StreamSupport.stream(iterable, false)
```

Bunun birkaç sebebi var. Fakat en önemli sebep; __Iterable__'ın paralel olarak elemanları döndürememesidir. Fakat __Stream__'ler paralelde koşabiliyor. Burada uyumsuzluk olacağı için, artık yeni "__java.util.Spliterator__" sınıfı kullanılıyor. Yani "__iterable.spliterator()__", bir "__java.util.Spliterator__" sınıfı dönüyor.
