# JAVA VERSION

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Java EE version history

- 2EE 1.2 (1999)
- J2EE 1.3 (2001)
- J2EE 1.4 (2003)
- Java EE 5 (2006)
- Java EE 6 (2009)
- Java EE 7 (2013)
- Java EE 8 (2017)
- Jakarta EE 9 (2020) (`javax.*`'dan `jakarta.*`'ya namespace geçişi)
- Jakarta EE 9.1 (2021)
- Jakarta EE 10 (2022)
- Jakarta EE 11 (2024) (LTS)

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Java version history (high level)

eskiden JDK ile bütünleşik geliyordu, bu sebeple ilk isimleri JDK olarak geçmektedir.

| Sürüm      | Yıl  | LTS   | Öne Çıkan Özellikler             |
|------------|------|-------|----------------------------------|
| JDK 1.0    | 1996 |       |                                  |
| JDK 1.1    | 1997 |       |                                  |
| J2SE 1.2   | 1998 |       |                                  |
| J2SE 1.3   | 2000 |       |                                  |
| J2SE 1.4   | 2002 |       |                                  |
| J2SE 5.0   | 2004 |       |                                  |
| Java SE 6  | 2006 |       |                                  |
| Java SE 7  | 2011 |       |                                  |
| Java SE 8  | 2014 |       | Lambda, `Stream` API, `Optional` |
| Java SE 9  | 2017 |       | modülerlik                       |
| Java SE 10 | 2018 |       | `var` keyword                    |
| Java SE 11 | 2018 |       | `HTTP` 2, `WebSocket`            |
| Java SE 12 | 2019 |       |                                  |
| Java SE 13 | 2019 |       |                                  |
| Java SE 14 | 2020 |       |                                  |
| Java SE 15 | 2020 |       |                                  |
| Java SE 16 | 2021 |       |                                  |
| Java SE 17 | 2021 | `LTS` |                                  |
| Java SE 18 | 2022 |       |                                  |
| Java SE 19 | 2022 |       |                                  |
| Java SE 20 | 2023 |       |                                  |
| Java SE 21 | 2023 | `LTS` | `virtual thread`                 |
| Java SE 22 | 2024 |       |                                  |

## 📌 java version history (detailed)

## 📌 Java 8 news

- __lambda expressions + function interface__ (konular başka başlıkta anlatılıyor)

- __stream__

- JavaFX tümüyle JDK içinde entegre gelmektedir.

- __Nashorn__ (konu başka başlıkta anlatılıyor)

- "__Optional__" sınıfı geldi (konu başka başlıkta anlatılıyor)

- __Java Mission Control__: Java yazılımlarınızı kontrol edebileceğiniz bir tool. Mesela programınız ne kadar CPU, hafıza ve thread kullanıyor.

- __Permgen__ politikası değiştirildi

- new Date-Time API. "__java.time__" isimli yeni kütüphane paketi eklendi. eski sürümlerde hem java.util.Calendar hem de java.util.Date kullanılıyordu. hala `JVM`'lerde geriye uyumlu çalışabilmesi için kaldırılmadılar.

- __default__ keyword: arayüzde metot önüne "default" keyword'ü atılıp o metodun body'si yazılabiliyor. Eğer yazılırsa artık implementasyonlarda o metot override edilse bile arayüzdeki metot çağrılıyor. Yani abstract sınıftakilerden farklı bir durum var.

  Burada Java zorunlu implementasyon yaptırmak istemiyordu. sadece arayüzde metot implemente edilmesi isteniyordu. fakat zorunlu implementasyon geriye uyumluluğu sağlamak için eklendi. şu sebepten:  örnek üzerinden gidelim: List arayüzüne bir metot eklenmek isteniyor. bu metodu JDK eklerse tüm güncel yazılımlar artık kullanılamaz hale gelecektir. çünkü tüm yazılımdaki list implementasyonları çalışmaz olacaktır. çünkü arayüzdeki tüm metotları implemente etmek zorundalar. fakat artık implemente etme zorunlulukları olmadığı için bu metotlar sorunsuz çalışıyor olacaktır.

- java.util.Base64 ile base64 işlemleri birçok implementasyon destekleyecek şekilde tasarlandı (başka yerde anlatılıyor)

## 📌 Java 9 news

- __Java REPL (⟷ JSHELL ⟷ Java Shell)__

- __modülerlik__. artık her jar, kendi descriptor'unda hangi paketleri dışarıya açılacağını (dışarıdan görülebilir olacağını) ve hangi modüllere depend ettiğini belirtebilmektedir.

- __Reactive Streams__ (başka yerde anlatılıyor)

- \@Deprecated (forRemoval=true , since="9") ile artık hangi versiyondan sonra bu metodun var olmayacağını dökümante edebiliyoruz.

## 📌 Java 10 news

- __type safe__ olmayan nesneler tutulabiliyor:

```java
var list = new ArrayList<String>();
```

## 📌 Java 11 news

- `HTTP` 2 ve `WebSocket` geldi. Eski sürümlerde üçüncü parti kütüphanelerle kullanılıyordu. `Java EE` ve `Spring` kendi içinde üçüncü parti kütüphaneleri gömülü getiriyor ve onları kullanıyordu. bu sebeple eski sürüm `Java` kullanılsa bile, `Java EE` veya `Spring` kullanarak WebSocket ve `HTTP` 2 kullanılabiliyordu.

  Aslında bu modül `incubator` olarak Java 9 ile geldi.

  `incubator`'dan çıkışı 11'inci sürüm oldu.

## 📌 Java 12 news

- `10K`, `10 thousand` gibi gösterimler yapabilmek için Number-formatter geldi (date-time-formatter gibi).

## 📌 Java 13 news

- escape karakteri kullanımına gerek olmadan String oluşturulabiliyor:

```java
// Bu syntax çok yeni olduğu için şu anda ekranda yanlış renklendirme ile gösteriliyor olabilir.

String jsonString = """
{
    "name" : "jack",
    "website" : "https://www.mycompany.com/"
}
""";
```

## 📌 Java 14 news

- null-pointer hataları artık daha detaylı. aynı satırda birçok instance olduğunda, hangi instance'ın null olduğunu da yazıyor.

- artık sadece ihtiyacımız olan modülleri içine alan, OS bazlı executable üretebiliyoruz.

- instanceof'ta opsiyonel syntax değişikliği oldu.

eski Java ile kod:

```java
if (person instanceof Employee) {
    Employee employee = (Employee) person;
    Date hireDate = employee.getHireDate();
    //...
}
```

yeni Java ile kod:

```java
if (person instanceof Employee employee) {
    Date hireDate = employee.getHireDate();
    //...
}
```

## 📌 Java 15 news

- SealedInterface sınıfının sadece `TCP` ve Http tarafından super class olabileceğini belirtmiş oluyoruz. Yani artık SealedInterface'ten türeyen bir sınıf olursa, sadece ve sadece SealedInterface'i super class olarak kullanabilir. birden fazla class'ı implemente edemez.

not: "__sealed__" yerine "final" keyword olsaydı, o zaman SealedInterface'ten hiçbir sınıfı türetilemeyecekti. bu 2si farklı kavramlar.

```java
public sealed interface SealedInterface permits Tcp, Http {

    // any code here
}
```

- __hidden class__: `class loader` ile bulunamayan bir class yaratılabiliyor. bu class'ı sadece ilgili thread yaratıyor ve o thread o anda kullanılabiliyor. bu özellikle framework'ler için iyi. tamamen private class yaratılabilmesini sağlıyor. eskiden private class yaratsak bile, reflection veya `class loader`'lar aracılığı ile erişme ihtimalimizi olabiliyordu. şu anda o ihtimal tamamen kalktı.

- 8'inci sürüm ile gelen Nashorn JS engine kaldırıldı.

## 📌 java 16

- __Record__

  çoğu özelliği standart class'lar gibidir. tek farkı immutable field'ları vardır. Bizim için otomatik şunları yaratıyor:

  - `getter` (`setter` yaratmıyor),
  - `constructor`
  - `equals()`
  - `hashCode()`
  - `toString()`

```java
public record User(int id, String password) {

    // we can write custom method or we can override constructor here.
};
```

## 📌 Java 18 news

- __simple web server__. it's not production-ready. its only test purpose.

run it via command line:

```sh
jwebserver -b 0.0.0.0 -p 8000
```

or via API:

```java
var server = SimpleFileServer.createFileServer(new InetSocketAddress(8000), Path.of("/home/java"), OutputLevel.VERBOSE);

server.start();
```

- __UTF-8 character set by default__. Before 18, `JVM` set locale of OS.

- __Internet-Address Resolution SPI__: artık domain'den IP bulmaya çalıştığımızda, OS'un native API'sini kullanmak zorunda değiliz. custom da kullanabileceğiz.

- __Java.lang.Object.finalize() deprecated__ olarak işaretlendi.

Garbage collector referanssız instance'ları RAM'den silerken, finalize metodunu çağırıyordu o class'ın. ama artık bunu çağırmasın, developer kendi çağırmasını bilmeli deniliyor. bazı sebepler:

- finalize metodunun GC tarafından ne zaman çağrılacağı belirsizdir. bu güvenlik veya kaynak tüketimi sıkıntı yapabiliyor.
- secure by design koda sürüklemek developer'ı.
- kaynak kapatılması için gerekli kodların nerede çalışıyor olduğunun bilinmesi, developer'ı okuyan ve debug eden kişi tarafından daha net bir süreç sağlıyor.

## 📌 java 21 news

- `Virtual Thread`

  OS'un göremediği, sanal hafif thread'lerdir.

## 📌 modülerlik hakkında

- Java 9 ile artık "java.se" modülü JRE içindeki kütüphaneleri barındırıyor.
- Java 9 ile java.se artık JAXB gibi Java EE'ye ait olduğu düşünülen kütüphaneleri deprecated olarak işaretlendi.
- Java 9 ile JAXB gibi Java EE'ye ait olduğu düşünülen kütüphaneler default classpath'te gelmiyorlar fakat JDK içerisinde yüklü geliyorlar. Yani development yaparken ilgili jar'ları hala bulabiliyoruz fakat projemizde ufak config yapmak gerekiyor.
- JDK 11 ile JAXB gibi Java EE'nin olduğu düşünülen paketler JDK içinden tamamen kaldırıldı.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
