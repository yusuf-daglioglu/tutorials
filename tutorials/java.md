# JAVA

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 java.util.concurrent.atomic package

`AtomicIntegerArray`, `AtomicInteger`, `AtomicLong` gibi birçok sınıf barındırır. Tümü multi-thread uygulamalarda variable'ları yönetmek için kullanılır. Örnekler:

- AtomicInteger class --> public int addAndGet(int delta)

integer'ı değere set eder ve yeni değeri döndürür. Eğer AtomicInteger olmasaydı bunu bu şekilde kullanacaktık:

```java
int a = 1;
a = a + delta;
return a;
```

Aynı işlemi iki satırda yaptığımız için, "a" variable'ı başka thread'lerden kullanılıyor olsaydı, işlem atomic olmayabilirdi. Bu tarz durumları engellemek için "AtomicInteger.addAndGet()" metodunu kullanmalıyız.

Normal koşullarda bu kod dahi multi-thread uygulamalarda hatalı çalışabilir:

```java
private int counter;

public int getNextUniqueIndex() {
    // Burası tek satırda olsa da, arka planda birden fazla CPU operation'ı içeriyor olabilir.
    // Bu sebeple burası bazen hatalı çalışabilir.
    // Kesin olarak bildiğimiz şey şudur ki; counter++ işlemi, en az iki işlemden meydana gelmektedir:
    // int tmp = this.counter;
    // this.counter = tmp + 1;
    return counter++;
}
```

Yukarıdaki kodu bu şekilde yapmak şart:

```java
private AtomicInteger counter;

public int getNextUniqueIndex() {
    return counter.getAndIncrement();
}
```

- AtomicInteger class --> public boolean compareAndSet(int expect, int update)

set edeceği yeni değer ("update" parametresi), expected ile aynı ise set etmiyor boşuna. böylece daha az lock'lanma oluyor.

- AtomicInteger class --> public int decrementAndGet()

1 azaltıp değeri döndürüyor.

- AtomicInteger class --> public double doubleValue()

integer'ı double'ye cast edip döndürüyor. Bu metod multi-thread için özel tasarlanmamış. sadece ek yardımcı bir metot.

- AtomicInteger class --> public void lazySet(int newValue)

Eventually sets to the given value. Farklı thread'ler aynı anda değeri güncelliyor ise, diğerlerine öncelik verir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Access level modifiers

modifier kelime anlamı: 1.düzenleyici 2.niteleyici

## 📌 modifiers for: class

Classes can only get both below modifiers:

- public: can be access by any where
- no modifier: can be access only by classes of same package

```java
// Above rule is written only for MyClass modifiers.
public class MyClass {

  int a;
}
```

## 📌 modifiers for: sub-class ⟷ internal members of class

- public
- private
- protected
- no modifier

```java
// Above rule is written only for "a" and "Abc" modifiers.
public class MyClass {

  int a;

  class Abc {

  }
}
```

On the above example, Abc class is only a definition. Is not an instance. There is no meaning to "access" them from another class (it is not logical). But to create a new instance from another class is to make them static:

```java
public class MyClass {

  int a;

  static class Abc {

  }
}

public class AnotherClass {

  new MyClass.Abc(); // this is valid.

  // Note:
  // If "static class Abc" is private we can not create an instance from other classes.
  // Actually, the "access modifiers" of "static class Abc" apply for the rules to create new instance of it.
}
```

On the official `Java` doc of `Oracle`, no-modifier is defined as __package-private (⟷ no explicit modifier)__.

Class member access table:

| Modifier/access from | Class itself | other classes on the same package | Subclass (extend) in same package | other packages |
|----------------------|--------------|-----------------------------------|-----------------------------------|----------------|
| public               | Y            | Y                                 | Y                                 | Y              |
| protected            | Y            | Y                                 | Y                                 | x              |
| no modifier          | Y            | Y                                 | x                                 | x              |
| private              | Y            | x                                 | x                                 | x              |

> Y = can be accessed

> x = can not be accessed

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Annotation based vs Java based (JavaConfig) vs XML based configuration

Spring ile şu şekilde config yapabiliriz:

- Annotation based (Spring'in kendi standartları)

```java
import org.springframework.stereotype.Service;

@Service
public class MyService {
    public String getMessage() {
        return "Hello from MyService!";
    }
}
```

- XML based

"beans.xml" file:

```xml
<bean id="myService" class="com.example.MyService" />
```

- Java based (JSR based - Java standartları)

```java
import javax.inject.Named;

@Named
public class MyService {
    public String getMessage() {
        return "Hello from MyService!";
    }
}
```

Oysa Java EE dökümanlarında:

- Annotation based
- Java based

terimleri aynı şeye denk gelmektedir. çünkü Java EE zaten sadece JSR kütüphanelerini kullanmaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 variable scope

Java standartlarında aşağıdaki terimler kullanılmaktadır. Fakat aynı terimler farklı programlama dillerinde farklı seviyeler için kullanılıyor olabilir.

### 📌📌 Local variables

kod bloğu tamamlandığında, yok edilen değerlerdir.

### 📌📌 Instance variables

garbage collector'ün temizlemesine girme durumu olan variable'lar.

### 📌📌 Class variables

tüm program sonlanana kadar kullanılan değerlerdir.

```java
public class MyClass {

  final static int MAX_LENGTH = 5;  // class variable
  int length;                       // instance variable

  public void method() {
    int inches = 40;                // local variable
  }
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 template engine

Java için açık kaynaklı template engine'ler:

- Thymeleaf
- Freemarker

- her ikisi de sadece HTML dosyalarını değil, herhangi bir dosyayı da model ile doldurabilmektedirler.
- her ikisini de özellikle Spring ile de opsiyonel kullanılabilen entegrasyon modülleri vardır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Apache's Commons-BeanUtils BeanUtils class vs org.springframework.beans.BeanUtils

`Spring`'in `BeanUtils`'i Spring Bean'lerinin instance'larında değişiklik yapabilmek için tasarlanmıştır. Oysa `Apache`'ninki sadece `getter` ve `setter` içeren `Java` objelerinin instance'ları üzerinde işlem yapabilmek için tasarlanmıştır.

`Apache`'s `Commons-beanutils` `BeanUtils` example:

```java
Course course = new Course(); // Course has "String name" and "List codes".

String name = "Computer Science";
List<String> codes = Arrays.asList("CS", "CS01");

PropertyUtils.setSimpleProperty(course, "name", name);
PropertyUtils.setSimpleProperty(course, "codes", codes);
```

`Apache`'s `Commons-beanutils` `BeanUtils` example:

```java
// copies from different types of classes, if fields are same name.
BeanUtils.copyProperties(courseEntity, course);
```

`org.springframework.beans.BeanUtils` example:

```java
// copies from different types of classes, if fields are same name.
BeanUtils.copyProperties(courseEntity, course);
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 LLVM

dilden bağımsız bir derleyicidir. LLVM, klasik VM'ler ile (Java VM gibi) benzerlikler göstermektedir. Bu sebeple LLVM ismi, "Low Level Virtual Machine" teriminden esinlenerek verilmiştir. Dolayısı ile "Low Level Virtual Machine" kısaltması değildir. LLVM özel bir isimdir.

C ve C++ için full support'u vardır. Rust dili de LLVM ile çalışacak şekilde tasarlanmıştır.

LLVM programatik olarak kullanılabilecek bir API'ye de sahiptir. Programcı bir Java dili için JIT yazıyor olsun (örneğin `JVM`'e alternatif bir VM geliştiriyor olsun). Programcı, Java kodunu önce  __intermediate representation (⟷ IR)__'a, daha sonrada makine koduna çevirebilir. Bu 2 işi yapmak içinde LLVM'in sunduğu programatik API'leri kullanabilir.

LLVM-frontend saf kodu IR'a çevirirken, IR'ı native binary'ye LLVM-backend ile çevrilir.

LLVM-optimiser ise IR'a çevrilmiş kodu optimize eder. örneğin kullanılmayan kod akışlarını siler gibi.

__intermediate representation (⟷ IR ⟷ Intermediate code ⟷ Intermediate language)__ genel olarak programlama dünyasında; derlemenin öncesinde, kodun çevrildiği bir biçimdir. birçok IR standardı vardır.

Aslında bakıldığında `JIT`'lerin ve `VM`'lerin kabul ettiği native kodlar (örneğin `JVM` `Bytecode`) bir `IR`'dır.

__Common Language Infrastructure (⟷ CLI)__ Microsoft'un  `.Net` platformu için geliştirdiği `IR` ve bunu işletecek `JIT` standartlarının tümünün ismidir. __Common Intermediate Language (⟷ CIL ⟷ old-name:Microsoft Intermediate Language ⟷ old-name:MSIL ⟷ old-name:Intermediate Language ⟷ old-name:IL)__ ise bu standartlara uygun olarak implemente edilmiş bir IR'dır. Şekil üzerinden bu durumu göstermek gerekirse:

https://upload.wikimedia.org/wikipedia/commons/thumb/8/85/Overview_of_the_Common_Language_Infrastructure.svg/800px-Overview_of_the_Common_Language_Infrastructure.svg.png

## 📌 GraalVM

`Oracle` tarafından geliştirilen OpenJDK türevi VM'dir. İsminde sadece VM ifadesi olsa da, aslında SDK ve VM bir aradadır. OpenJDK'ya ek olarak sunduğu özellikler:

- OpenJDK'nın JIT compiler'ına göre bazı durumlarda avantaj sağlayacak ek özellikleri/optimizasyonları var.

- LLVM toolchain

  Birçok farklı dilde yazılmış kodun 'LLVM bitcode'una çevrilmesi için derleyici içeriyor.

  C'de yazdığımız bir kodu aşağıdaki komut ile 'LLVM bitcode'una çevirebiliyoruz:

  ```sh
  $LLVM_TOOLCHAIN/clang my_c_code.c -o "my_llvm_bitcode_binary"
  ```

  Bazı diller için direk olarak support gelmemektedir. Onları "GraalVM Updater" ile kurmamız gerekiyor:

  ```sh
  gu install python
  ```

- `LLVM`-based diller için `runtime` içeriyor. Yani; `LLVM bitcode` formatında olan dosyalardan native executable üretebiliyor.

  aşağıdaki komut ile bunu yapabiliriz:

  ```sh
  lli "my_llvm_bitcode_binary"
  ```

- `polyglot` `API` (`Interoperability`)

  `Bu kısım için kaynak: <https://www.graalvm.org/latest/reference-manual/llvm/Interoperability/>`

  `LLVM`'e çevrilebilen birçok programlama dili için, yine LLVM kodunu çağırabilmemizi sağlayan API yazılmış. Ek olarak Java, LLVM'e çevrilmemesine rağmen, oda LLVM destekleyen kodları çağırabiliyor veya LLVM'e çevrilebilen dillerden çağrılabiliyor. Örnekler:

  LLVM kodunu execute edebilen API sağlamak sanıldığı gibi kolay olmayabiliyor. Örneğin; JS'te diğer objelere erişirken isim ile erişiyoruz. JS kodunu LLVM'e çevirdiğimizde, LLVM'in isimleri tutuma gibi bir özelliği olmadığı için, JS ile Interoperability'yi kaybediyoruz. Fakat GraalVM, burada meta-data (debug information) tutarak bunun üstesinden gelmemizi sağlıyor.

  - C kodumuzda şu kütüphaneyi kullanarak:

    ```c
    #include <graalvm/llvm/polyglot.h>
    ```

    Java kodumuzu vererek, Java kodumuzdaki nesnelere ve fonksiyonlara erişebiliyoruz.

  - Java kodumuzda şu kütüphaneyi kullanarak C kodunu çağırabiliyoruz:

    ```java
    import org.graalvm.polyglot.*;
    ```

- Java kodlarını native derleme

  - Bunu yapmak için Java kodları önce LLVM'e çevrilip, sonra native'e çevriliyor.

  - Ahead-of-time compiler olduğu anlamına gelir. Her OS'a bağımlı native executable üretiyor.

  - GraalVM Java kodunu native derlendiğinde, derlemesi uzun sürüyor. Çünkü sadece çağrılabilecek sınıfları embedded hale getiriyor. Bunun için tüm kodlarda özel tarama yapması gerekiyor (Static Analysis yapıyor. yani "reachable code" ları sadece executable içine gömüyor.).

    Bu taramayı yaparken (build sırasında) aynı zamanda bazı sınıfları execute edebiliyor. Örneğin static initializer'ları execute ediyor. Bu şekilde initialiser'ların runtime'da işletmekle zaman kaybetmemiz oluyoruz.

  - GraalVM reflection yapamıyor. Fakat Java kodumuzda reflection varsa şöyle 3 yöntem ile çözebiliyoruz:
  
    - Çözüm 1

      Build aşamasından önce reflection ile çağrılabilecek bütün metotların imzalarını bir JSON dosyasında saklıyoruz. Build aldığımızda bu JSON dosyasındaki her metoda birer özel referans atanıyor. Sadece bu metotlar artık runtime'da programatik olarak reflection ile çağrılabilir hale geliyorlar.

    - Çözüm 2:

      biz manuel yazdığımız kodların hangileri reflection ile çağrılabileceğini biliyoruz. Bunları JSON'a tek tek yazabiliriz. Fakat framework'lerin kendi içinde neyi çağırıyor olacağını bilemeyiz. Burada framework'ün desteği şarttır. Örneğin;

      - Spring, `spring-boot-starter-parent` içerisinde pom.xml'i içerisinde "native" isimli bir profil tanımlamış. Bunu "mvn -Pnative" parametresi ile çağırmamız şarttır.
      - Gradle projesi kullanırken eğer "org.graalvm.buildtools.native" plugin'i ekli ise, build sırasında Gradle'daki "bootBuildImage" metodu tetikleniyor. Bu metod zaten `spring-boot-starter-parent` tarafından da desteklendiği için otomatik olarak Spring devreye giriyor. Dolayısı ile Gradle kullanınca, özel bir profil execute edeceğimizi parametre olarak geçmeye gerek kalmıyor.

    - Çözüm 3:

      Eğer framework'ün desteği yok ise o zaman şu şekilde ilerliyoruz: <https://www.graalvm.org/latest/reference-manual/native-image/metadata/AutomaticMetadataCollection/> Burada özel bir komut ile `JVM` üzerinde (native olmayacak şekilde) projeyi run ediyoruz. Daha sonra runtime'da uygulamamızı son kullanıcı olarak kullanıyoruz. Kullandıkça çağrılan her reflection, bir JSON dosyaya kaydediliyor. Bu şekilde hangi sınıfların reflection ile çağrıldığını bilebilmemizi sağlıyor.

  - GraalVM bir class'ın önüne runtime'da dinamik proxy oluşturamıyor. Reflection sorununu nasıl çözdüysek, aynı 3 metodolojiyi kullanarak bu sorunu da çözebiliyoruz.

  - GraalVM ile "property" dosyaları runtime'da Java class'larına map'lenemiyor. Bu sebeple manuel build başlamadan önce, property sınıflarımızı bir JSON dosyasında belirtiyoruz. Build sırasında Java-native-compiler, bunları hard-code Java sınıflarımıza map'liyor.

  - GraalVM içinde gelen 'native-image' isimli executable ile compile edilen kodlar, herhangi bir OS'ta native çalışmaktadır (hangi OS'ta derlenirse, o OS için executable oluşturulur). Fakat native kod'da çalışması için reflection gibi teknolojileri kullanmamak gerekli. Örneğin Spring framework reflection kullandığı için şu anlık desteklenemiyor (yıl 2019 sonu). Bu sebeple; direk native derlenebilmesi için uygun __Quarkus__ ve __Micronaut__ projeleri geliştirildi. Bu projeler enterprise framework ve kütüphanelerden oluşan birbirlerine alternatif projelerdir.

  - Java ile yazılan kodlarda garbage-collector'un arkada çalışması şarttır. bu sebeple native executable'ın içerisinde ufak bir virtual-machine gömülüyor. `JVM`'e göre çok hafif ve daha az yeteneği var. Bu VM'e '__SubstrateVM__' ismi verilmiş.

## 📌 Quarkus

```xml
<dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>io.quarkus</groupId>
        <artifactId>quarkus-universe-bom</artifactId>
        <version>1.1.1.Final</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
    </dependencies>
  </dependencyManagement>
```

Daha sonra buradan istediğimiz extension'u dependency olarak eklememiz gerekmektedir: https://code.quarkus.io/ Bu listedeki her bir extension ayrı bir Maven dependency'dir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Eclipse IDE compiler Java (⟷ ECJ)

- `Eclipse` IDE'nin `Java` compiler'ının ismidir.
- `Java`'nın default compiler'ından farklıdır.
- en temel farklar:
  - kod düzgün derlenmemiş olsa bile, onu paketleyebilir (derleyebilir, `jar` veya `war` oluşturur). örnek bir class fiziksel olarak yoksa fakat bir başka class içinden referans edilmişse, `JDK` normalde hata verirken, `Eclipse` IDE bu hatayı es geçebilir. dolayısı ile runtime'da classNotFound gibi bir hata alırız.
  - kısmi derlemelere imkan sunar. kodun sadece bir kısmını değiştirdiğimizde sadece o kısımları derler.
- `Tomcat`'te `JSP`'leri derlerken `ECJ` kullanır. bu şekilde en ufak hatada tüm sistemi derlememezlik yapmaz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Java package names

> java.lang.* 

buradaki tüm paketler otomatik import edilir.

> java.*

JRE içinde olan core paketler

> javax.*

JRE içinde gelen fakat genelde extension (daha çok core pakete girmesi zorunlu olmayan: örnek JAXB) sınıflarını içermektedir.

Buradaki bazı paketler `java.*` altına taşınmıştır.

## 📌 önerilen isimlendirme

> com.company_name.project_name.package_purpose

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 InputStream

reads bytes from any type of stream.

## 📌 OutputStream

writes bytes to any type of stream.

## 📌 flush

Türkçe kelime anlamı: 1. su ile temizlemek. "The toilet doesn't flush." gibi kullanılıyor. 2. utançtan yüzü kızartmak.

OutputStream'de olan bir metottur. bazı stream'ler write ile yazdığımız bilgiyi karşıya hemen yollamaz. bunun bir çok sebebi olabilir:

- cache
- buffer
- protocol standartları gereği belli bir boyuta gelmesi beklenir.

`Flush` metodu stream'e/protocol'e yazma sinyali göndermektedir. öncelikle stream'in flushable olması gereklidir. flushable olsa dahi bu metot işlemin fiziksel olarak karşıya gidip gitmeyeceğini garanti etmez. örneğin `TCP` soket'e yazarken flush metodu kesinlikle bunun garantisini vermiyor, çünkü `TCP` standartları belli data'nın bir byte'a gelmesini bekliyor. ancak `TCP` soket kapatılırken, data kesinlikle yollanıyor.

## 📌 comparison table

| Class Name          | Buffer Support | Reads                               |
|---------------------|----------------|-------------------------------------|
| InputStreamReader   | No             | Character                           |
| DataInputStream     | No             | Any primitive types                 |
| BufferedInputStream | Yes            | Byte                                |
| Reader              | No             | Character                           |
| BufferedReader      | Yes            | Character                           |
| Scanner             | No             | Primitive types or String via regex |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 generic programming

tüm programlama dilleri için bir terimdir. bu terimin `Türkçe`'si ve eş anlamlıları:

- `Jenerik programlama (⟷ generic programming)`
- `Şablon programlama (⟷ Template Programming)`

## 📌 generics in Java

örnek generic `Java` kodu:

- kullanıldığı kod:

```java
List<String> c = new ArrayList<String>(); //Java 8 ile birlikte artık sağ tarafta generic-tipini belirtmeye gerek yoktur.

c.add(new Object()); // compile-time error fırlatır
```

- tanımlandığı kod:

```java
public class Entry<K, V> {

    private final K key;
    private final V value;

    public Entry(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public K getKey() {
        return key;
    }

    public Object method1(){
        return new Collection()<? extends K>;
    }
}
```

K, V harfleri istenilen karakter yada string olabilir. Genelde alışkanlık olduğundan, T (type), E (Element), V (value) and K (key) kısaltmaları kullanır.

Yukarıdaki örnekte V yerine gerçek bir sınıf ismi yazmaya gerek yoktur. zaten o sınıf belli ise (fix ise) o zaman generic yapmaya gerek yoktur. Bu sebeple oraya ne yazarsak yazalım sınıf ismi değil, özel bir keyword olarak algılanacaktır.

Yukarıdaki örnekte new Collection()<? extends K>; satırındaki "extends" tahmin edilen anlamından farklı bir anlama geliyor. K'nın subclass'ı olduğu herhangi bir sınıf gelmeli buraya anlamına geliyor. eğer tersi olursa runtime'da hata alırız. extend yerine super yazarsa, aşağıdaki satırda hata almayız. çünkü süperi "String" olan bir sınıf anlamına gelecekti:

```java
Box<? super String> list = new Box<String>(); //super'i String olan bir sınıfı gelecek anlamına geliyor

list.add(new String("Hello World"));
```

Oysa yukarıda super yerine extend yazsaydı daha compile sırasında hata alacaktık.

## 📌 PECS (⟷ Producer Extends Consumer Super)

Yukarıdaki super ve extend'li kod satırlarına dikkat edersek; super yazdığımız zaman aşağıdaki satırlarında listeye ekleme yapabiliyoruz. çünkü ne ekleyeceğimizi biliyoruz. oysa extends'de ne ekleyeceğimizi bilmediğimizden, ekleme yapamıyoruz sadece okuma yapabiliyoruz. Bu sebeple extend keyword'ü ile işlem yaptığımızda kodumuz "Producer" görevi görüyor. bu sebeple producer'lar extend etmeli terimi ortaya çıkmıştır: "Producer Extends". Benzer şekilde "Consumer Super" terimi ortaya atılmıştır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 static initializer

```java
class Person {
   static {
           int a = 10;
           System.out.print("this block is 'static initializer' ")
   }

   {
          int b = 20;
          System.out.print("this block is 'instance initializer' ")
   }
}
```

kod çalışma sırası:

1- static initializer

2- instance initializer

3- constructor

Yukarıdaki kodda int a ve b class'ın field'ları değildir. block'un local field'larıdır.

## 📌 instance initializer vs constructor

yukarıda bahsedilen set etme olayı neden constructor'da yapılmıyor?

1- instance initializer'ın sınıfın 'final' değişkenlerini set etme özelliği de bulunuyor.

2- birden fazla constructor olduğunda hepsinde aynı set etme işlemini yapmak zorunda kalabiliriz. bunun yerine tüm constructor'lar için ortak olan kodu instance initializer'a atmakta yarar vardır.

3- Anonymous class'larda constructor desteklenmez. onun yerine instance initializer kullanırız.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 objenin metotları

aşağıda açıklaması geçmeyen metotlar başka yerde anlatılıyor.

- toString

- wait

- notify

- notifyAll

- finalize

  object `JVM` tarafından destroy edilmeden önce çağrılır.

- clone

  tüm variable'ların aynı olduğu yeni bir obje yaratır. bunu user'ın kendisinin override etmesi gerekir. eğer user override etmediyse direk CloneNotSupportedException fırlatacak şekilde default implemente edilmiştir.

- getClass

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 annotation

Türkçe kelime anlamı: dipnot

annotation'lar birer @interface annotation'undan türemiş objelerdir. örnek:

tanım:

```java
@Target(ElementType.METHOD) // annotation nerelerin önüne koyulabilir
@Retention(RetentionPolicy.RUNTIME) // annotation hangi adımlarda kullanılacak (runtime, source, class olabilir)
@interface Todo {

 public static enum Status {STARTED, NOT_STARTED}

 String author() default "Ahmet";
 Status status() default Status.NOT_STARTED;
}
```

kullanım:

```java
@Todo(author = "Ahmet", status = Todo.Status.STARTED)
public void myMethod() {

    // Any code here...
}
```

## 📌 annotation kendi içinde nasıl business logic uyguluyor?

- RetentionPolicy.RUNTIME

Runtime sırasında iş yapan annotation'lar: Runtime sırasında annotation'u sunan framework, reflection kullanarak hangi sınıflarda annotation varsa a onları buluyor. bunlara göre manuel işlem yapıyor.

- `RetentionPolicy.SOURCE`

@Override, @SuppressWarnings gibi sadece IDE'lerin görmesi beklenen, derleme sırasında önemsenmeyen ve derlenmeyen annotation'lardır.

IDE veya eklentisi manuel olarak bunları buluyor ve işlem yapıyor.

- `RetentionPolicy.CLASS`

derleme sırasında kullanılırlar. fakat annotation'un kendisi, derlenip `bytecode` olarak `JAR` içine atılmazlar.

örnekler:

- compile sırasında dökümantasyon oluşturmak için kullanılabilirler.
- mapstruct code generate eder

CLASS annotation'ları derleme sırasında Java derleyicisi tarafından bulunuyorlar. Ve her annotation "Java source code generator" yazılımına yollanıyor.

eğer farklı bir generator (processor) kullanmak istiyorsak Java compiler'ına bunu belirtmek gerekiyor.

örnek:

- komut satırından:

```sh
javac -processor com.MyProcessor Person.java
```

- Maven'dan:

```xml
<build>
  <plugins>
      <plugin>
          <groupId>org.Apache.Maven.plugins</groupId>
          <artifactId>Maven-compiler-plugin</artifactId>
          <configuration>
              <annotationProcessors>
                  <annotationProcessor>
                      com.MyProcessor
                  </annotationProcessor>
              </annotationProcessors>
          </configuration>
      </plugin>
  </plugins>
</build>
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 java.lang.Class sınıfı

`MyService.jav`a gibi bir sınıftır. Bu sınıf her sınıfın içinden şu şekillerde çağrılabilir:

- `myClassInstance.getClass()`
- `MyClass.class` (static field)

bu iki değerin referansı birbirine eşittir.

Class sınıfı, `MyClass`'ın özelliklerini yansıtan metotlar içermektedir. örneğin; myClass'ın ismi, paket ismi gibi.

## 📌 Class objesinde olan bazı metotlar/property'ler

- `static forName(String className)`: metottur. Bu metoda herhangi bir sınıfın ismi string olarak gönderilebilir. bize gönderdiğimiz sınıf için Class objesini döndürecektir.

  Örnek: `Class myCarClass = Class.forName("Car");`

- `getAnnotation(Class < A > annotationClass)`: parametre aldığı annotation sınıfından ilgili sınıfta var ise o annotation'u döner. `java.lang.annotation.Annotation` döner. bu sınıfı tüm annotation'lar extend eder.

- `Annotation[] getAnnotations()`: o sınıfta kullanılan tüm annotation'ları döndürür.

- `ClassLoader getClassLoader()`: ilgili sınıfın `class loader`'ını dönüyor.

- `diğer metotlar`: `enum` olup olmadığı, field'lar, anonim olup olmadığı gibi bilgileri döndürürler.

## 📌 reflections (⟷ yansımalar)

java.lang.reflections paketleri altında olan bu sınıflar, metotlar ve sınıflar hakkında meta bilgileri alabilmemiz ve hatta gerektiğinde ismi ile aşağıdaki örnekte görüldüğü gibi metotları execute edebilmemizi sağlayabilmektedir.

```java
Method m = valueObject.getClass().getMethod(methodName, new Class[] {});

Object ret = m.invoke(valueObject, new Object[] {}); //m isimli metoda parametre yolluyoruz
```

Çok sade bir örnekle Java'da reflection'ları incelersek:

```java
Class insanClass = Insan.class;
Field surnameField = insanClass.getField("surname"); // surnameField sadece meta bilgidir. bir instance değildir.

Insan ahmet = new Insan("ahmet", "kara"); // name: ahmet, surname: kara

String surname = surnameField.get(ahmet); // ahmet instance'ının surname'ini çağırdık. bize dönüş değeri yine instance'tır.

surnameField.set(ahmet, surname); // ahmet instance'ının, surname değerini set ediyoruz (tekrar aynı değerle set etmiş oluyoruz. örnek olsun diye yapıldı.)
```

## 📌 Java arrays

Java'daki her native-array, java.lang.Obje sınıfından türemiştir. fakat runtime'de dinamik oluşturulurlar. örnek:

```java
// ornek 1
Integer[] array1 = new Integer[3];
Object array2 = new Integer[3];

// ornek 2
Integer[][] array3 = new Integer[3][3];
Object array4 = new Integer[3][3];

// Hatta java.lang.Object'in metotlarını override ettiklerini de görebiliriz:
array3.getClass();
array3.hashCode();
array3.toString();
array3.notify(); // IDE ile notify metodunun içerisine bakmak istediğimizde java.lang.Obje'nin içindeki notify metoduna yönlendirdiğini de göreceğiz.
```

"toString" metodu şu şekilde print eder:

```java
int[] x = new int[3];

System.out.println(x.getClass().getName()); //ekrana [I basar.
// int[][] olsaydı [[I basardı.
// [[I sınıfı dinamik oluşturulur.
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Jrebel

`JVM` için yazılmış bir bir `Class loader` extension'udur. var olan `class loader`'a ek olarak kendi logic'lerini çalıştırır. Birçok IDE için yazılmış eklentisi de vardır.

`Jrebel` ile deploy edilen uygulama (app server içerisinde çalışsa dahi) hot-deploy yapılabiliyor. sadece değişen Java sınıfları deploy ediliyor ve uygulama restart istemiyor. normalde Java'da hot deploy özelliği var fakat metot imzası değişmesi gibi durumları desteklemiyor. bu eklenti ile daha fazla durumda hot deploy yapılabilmesi sağlanıyor.

`ASM` ve `Javassist` framework'leri, `Bytecode`'u runtime sırasında manipüle edebilmek için kullanılan projelerdir. Jrebel da bunlara alternatif bir yazılım geliştiriyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 WAR (Web Application Archives) vs EAR (Enterprise Archives) vs JAR (⟷ Java archive)

- JAR sadece Java sınıfları ve kaynak dosyalarını barındırır.

- WAR, JAR'a ekstradan, servlet ve JSP dosyalarını barındırır.

- EAR ise servlet (WAR) + "EJB modülü" olarak ayarlanmış JAR dosyaları bulundurması gerektiğinde kullanılır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 JVM memory spaces

- __Heap Space__

  tuttuğu bilgiler:
  - Static olmayan tüm variable'lar (yani Heap'teki referansların gerçek değerleri burada tutulur)

  Heap space'in alt gruplarının olmasının sebebi şu: Garbage collector yeni yaratılan variable'lardan artık kullanılmayanları daha sıkça temizliyor. Oysa eskiler, artık muhtemelen hep kullanılacak düşüncesi ile daha az sıklıkla temizlenir.

  - __Young Generation__

    - __Eden Space__

    Eden kelime anlamı: Havva ve Ademin yaşadığı cennet.

    'new' keyword'ü ile oluşturulan her nesne bu alanda tutulur.

    - __Survivor Space__

    garbage collector (⟷ GC), "eden space" alanını gezip ölü nesneleri yok eder. yaşayanları bu alana taşır.
  
  - __Old Generation__

    garbage collector, "Survivor space" alanını gezip ölü nesneleri yok eder. yaşayanları bu alana taşır.

- __Non-Heap (⟷ Off-heap)__
  
  - __Permanent Generation (⟷ PermGen)__

    Permanent kelime anlamı: kalıcı.
  
    Tuttuğu bilgiler:

    - sınıflar ve meta bilgileri (metot imzaları gibi)
    - static variable'lar.

    Java version 8 Update: __PermGen__ is replaced with __Metaspace (⟷ Method Area)__ which re-sizes dynamically at runtime. Options __PermSize__ and __MaxPermSize__ have also been removed in JDK 8.

  - __Thread Memory (⟷ Stack Memory)__

    Her thread'in kendi Stack memory'si vardır.

    tuttuğu bilgiler:
    - primitive değerler.
    - nesnelere ait referanslar (nesnelerin kendisi burada değildir! gerçek değerler heap'te tutuluyor.)

  - __Code Cache__
  
    it can be Virtual or reserved.
  
    Hotspot kod kısımlarının, native kod'a çevrildiğinde saklandığı cache bölgesi.

  - __Garbage Collector Data__

## 📌 Different architectures on different JVMs

The `IBM` `WebSphere` `JVM` mimarisinde:

- `old generation`, __Tenured Space__ (kelime anlamı: imtiyazlı),
- `Young Generation`, __Nursery space__ (kelime anlamı: kreş),

olarak kullanılıyor ve yapılarında ufak farklılıklar var.

```text
The layout of the generational heap (see Figure 2.9) is slightly different from that of the Oracle HotSpot JVM. The WebSphere nursery is equivalent to the HotSpot young generation, and the WebSphere tenured space is equivalent to the HotSpot old generation. The nursery is divided into two parts of equal size—allocate and survivor areas. Objects are always allocated in the nursery and copy garbage collection is used to copy surviving objects to the survivor area. After a successful GC cycle, the former survivor area becomes the nursery.
```

`source: <https://www.dynatrace.com/resources/ebooks/javabook/the-three-jvms/>`

## 📌 -XX:MetaspaceSize vs -XX:MaxMetaspaceSize

Default'ta `Metaspace` sınırsızdır. Fakat `MaxMetaspaceSize` buna limit getirebiliriz. `MetaspaceSize` ise şuna yarıyor: Eğer runtime'da `Metaspace` boyutu `MetaspaceSize`'de belirttiğimiz sayıyı geçerse o zaman `GC` sıkça devreye girmeye başlıyor. `Metaspace`'te yer açmak için class unload işlemi yapılıyor.

## 📌 -Xms vs -Xmx

Eğer bu değerler verilmemiş ise; `JVM` default olarak bu değerleri verir. Her `JVM` şunlara göre default değerlere karar verir: OS, RAM ve `JVM` implementasyonu farklı default değere karar verebilir.

Bu değerler Heap space'in size'ını belirler. -Xms (min size'ın baş harfleri) initial RAM değerini, -Xmx (max size'ın baş harfleri) maximum değeri belirler.

-XX:InitialRAMPercentage ve -XX:MaxRAMPercentage parametreleri -Xms ve -Xmx parametreleri ile tamamen aynı görevi görür. Sadece absolute değer (30GB gibi) değil, yüzdelik cinsinden değer (RAM'i %30'u gibi) ayarlayabilmemizi sağlamaktadır.

## 📌 commited memory

JVM'e bellek kullanımı için garanti edilen bellek miktarıdır. Bunun farkını net bir şekilde bulamadım. Fakat muhtemelen max manuel developer tarafından verilebilirken, commited OS tarafından verilen bir değerdir. Developer 100GB lik yer harcayabilsin diye parametre geçebilir (max değeri), fakat OS bunun tümünü atayamayabilir (commited değeri).

## 📌 Minor GC vs Major GC vs Full GC

These terms aren't clearly defined in the `JVM` specification. Here, we consider "Major GC" and "Full GC" to be equivalent.

Minor GC performs when Eden space is full. It removes all dead objects in young generation and moves live objects to from Eden space to s1 of survivor space, or from s1 to s2.

Full GC or major GC does garbage collection in the entire heap. Full GC can also collect parts like metaspace and direct memory, which can be cleaned only by full GC.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Apache commons

birçok Java kütüphanesi barındıran projenin ismidir. altındaki kütüphaneler birçok  amaç için yazılmıştır. sadece ihtiyaç duyulan kütüphanenin jar dosyası ayrı ayrı indirilmektedir. alt modüllerin bazıları:

- commons-codec: General encoding/decoding algorithms (example: phonetic, base64, URL).

- commons-lang: extra feature for java.lang (core Java library)

- commons-cli: command line argument parser

- commons-collection: data structures library

- commons-dbcp: JDBC üzerinde pool'ların yönetimini yapmamızı sağlayan kütüphane. ORM kütüphanesi değildir.

## 📌 Google Guava

Apache commons gibi birçok farklı iş için geliştirilen Java kütüphaneleri grubudur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 modes of JVM

client ve server mod var. iki mode'da `JIT` kullanır. client mode, kodları işletirken çok çok az optimizasyon yapar. Bu da onun daha az `RAM` harcamasını ve ilk açılışının hızlı olmasını sağlar. Bu mode bu sebeple `GUI` uygulamalarında tercih edilir. Oysa server mode,önceden execute edeceği kod bloklar için ön-inceleme yapıp, çok fazla optimizasyon yaptığı için, bu optimizasyon için `RAM` tüketir. Genelde uzun süre çalışacak server uygulamalarında bu mod tercih edilir.

yeni olarak `tiered compilation` mode'u gelmiştir. bu mod hem client hem de server karışımıdır. bu mod; uygulamanın başlangıcında client mod temelli çalışır, daha sonraki zamanlarda server mod temelli çalışır.

`java -version` komutu ile `JVM`'in çalıştığı `mode` çıkıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Garbage Collector

Yaptığı işlemlere teknik özel isimler verilmiştir. `JVM` parametresi geçmek gerektiğinde, yada `Garbage Collector`'nin algoritmaları araştırıldığında bu terimlere ihtiyaç duyulabilir.

## 📌 mark

scan and marking all reachable (kullanılan) objects

## 📌 sweep

scan and free unmarked objects

## 📌 copy collection

copying collection to another memory block. example: from eden to survivor space.

## 📌 paralel collection

`Garbage Collector` uses multiple threads for itself

## 📌 concurrent collection

`Garbage Collector` uses concurrent threads for itself

## 📌 compacting

moves objects inside memory for defrag

## 📌 stop the world

`Garbage Collector` pause all app threads to finish it's job

## 📌 minor collection

`Garbage Collector` for young generation

## 📌 major collection

`Garbage Collector` for old generation

## 📌 Types of GC

`Garbage Collector` kullandığı algoritmalara göre ayrılmaktadırlar:

- `Serial collector`
  - tek `CPU`'lu cihazlara uygun
  - `Garbage Collector` işe başladığında tüm sistemi durdurur
- `Parallel collector (⟷ throughput collector)`
  - multiple `CPU`'lar için uygun
  - `Garbage Collector` paralel thread'ler açarak işlem yapıyor
  - `Garbage Collector` işe başladığında tüm sistemi durdurur
- `concurrent collector (⟷ CMS collector ⟷ concurrent-mark-sweep collector)`
  - old generation için arka planda sürekli `Garbage Collector` işlemi yapar. bazen sistemi durdurur bazen durdurmaz. bu mantığa low-pause collection deniliyor.
  - young generation için hep sistemi durduruyor
  - `Garbage Collector` kendi içinde concurrent çalışmaktadır (paralel'den farklı mantıktır. başka başlıkta anlatıldı.)
- `G1 collector (⟷ garbage first collector)`
  - `CMS` ile benzer mantıkta çalışmaktadır.
  - çok büyük heap size verilmiş sistemler için uygun.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 JVM components

## 📌 class loader

class'ları bellekten okuyarak `JVM`'e aktarır. `class loader`'da bir Java sınıfıdır yada sınıflar grubudur. aynı `JVM`'de her app farklı `class loader` kullanabilir. `Tomcat` ve `Spring`'in kendi `class loader`'ları var.

- `JVM`'in içindeki default `class loader`'a `bootstrap class loader (⟷ primordia class loader)` ismi verilmiştir. bunun asıl amacı `/lib` (`$JAVA_HOME\lib\rt.jar` ve diğerleri) dizinindeki core JRE kütüphanelerini import etmektir.

- `Extension Class Loader` ise bootstrap'in bir child'ıdır. bootstrap tarafından çağrılır. `\lib\ext\` içini yükler.

- `Application Class Loader (⟷ System Class Loader)` ise `Extension Class Loader` tarafından çağrılır. `Extension Class Loader`'ın child'dıdır. `JVM` başlatılırken, `JVM`'e parametre geçilen path'lerden class'ları yüklemekle görevlidir.

## 📌 execution engine

ya `JIT` yada interpreter'a iletilerek kodun çalıştırılmasını sağlar.

## 📌 garbage collector

boş referanslı verileri temizlemektedir.

## 📌 security manager

private metotları çağrılmasını engellemek gibi güvenlik duvarı görevi görmektedir. `JVM`'e parametre geçilerek her şey serbest bırakılabilmektedir.

## 📌 Tomcat class loader

`Tomcat` sırası ile bunları işletir:

- `JVM`'s `bootstrap class loader` (with it's child-class loaders)

- System `class loader`

  `$CLASSPATH` variable'ında olan sınıfları yükler. fakat `/tomcat/bin/catalina.sh` veya `/tomcat\bin\catalina.bat` `script`'i `CLASSPATH` değerini ignore eder ve `CLASSPATH`'e kendisi `/tomcat/bin/` içerisindeki bazı `lib`'leri yükler.

- Common `class loader`

  Aşağıdakileri load eder:

  - unpacked classes and resources in `$CATALINA_BASE/lib`
  - unpacked classes and resources in `$CATALINA_HOME/lib`
  - `JAR` files in `$CATALINA_BASE/lib`
  - `JAR` files in `$CATALINA_HOME/lib`

- `WebappX`

  sadece bu class loader'dan yüklenen class'lar webapp'a özeldir. diğer web uygulamaları bunları göremez. bu dizinleri sırası ile yükler:

  - `/WEB-INF/classes`
  - `/WEB-INF/lib`

`Tomcat`'in loader konfigürasyonunda aşağıdaki belirtildikçe yukarıdaki sıra geçerlidir. eğer aşağıdaki belirtilmişse sıra aşağıdaki gibi olur:

```xml
<Loader delegate="true"/>
```

- bootstrap of `JVM`
- `WebappX`
- System
- Common

`Tomcat`'te default açık olmayan `class loader`'lar:

- `Server`: sadece `Tomcat`'in görebildiği kütüphaneler.
- `Shared`: tüm web uygulamalarından görülebilen kütüphaneler. buradakilerin tekrar yüklenmesi için `Tomcat`'in restart edilmesi gerekir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 JVM parametreleri

| Parametre Tipi     | Her `JVM`'de Farklı anlama gelebilir |
|--------------------|------------------------------------|
| `-X` ile başlamayan  | False                              |
| `-X` ile başlayanlar | True                               |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 exceptions

## 📌 try with resource

try parantezinin içinde olan BufferedReader, java.lang.AutoCloseable'dan implemente edilmiştir. bu şekilde derleyici onu hata olursa otomatik kapatacak şekilde koda çevirir. (yani `JVM` runtime'da close metodunu çağırmaz).

```java
try( BufferedReader reader = new BufferedReader(new FileReader(path)) ){

   // business here
   return reader.readLine();

} catch (Exception e) {
  //no need to close the "BufferedReader reader".
}
```

Try içinde birden fazla kaynak olabilir.

Kod derlenirken şekilde logic oluşturulur:

- Eğer "BufferedReader br" hata fırlatırsa bu hata dışarı fırlatılmaz, ignore edilir.
- Diyelim; "business here" BusinessException fırlatsın. Eğer "BufferedReader br" CloseException fırlatsın. Bu durumda CloseException, BusinessException'ın içine suppressed-exception olarak kaydedilir. Böylece log'larda CloseException'un detaylarını görebilmiş oluruz.

## 📌 class hiyerarşisi

- Object
  - Throwable __(Checked)__
    - Error __(Unchecked)__
      - VirtualMachineError
        - InternalError
        - OutOfMemoryError
        - StackOverflowError
        - UnknownError
      - AssertionError
      - ThreadDeath
      - AnnotationFormatError
    - Exception __(Checked)__
      - RuntimeException __(Unchecked)__
        - NullPointerException
        - ArithmeticException
      - IOException __(Checked)__
        - FileNotFoundException

- Tüm throwable nesneleri throw/catch edilebilirdir.

- Error ve altındaki objeler manuelde "throw new OutOfMemoryError()" şeklinde fırlatılabilirler. fakat catch edilmek zorunda dillerdir.

- Error'lar ne kadar da genel VirtualMachine sorunları gibi gözükseler de, Exception'lar gibi belirli bir satırdan fırlatılırlar. Örneğin OutOfMemoryError tam olarak bu satırdan fırlatılabilir:

```java
byte[] array1 = new byte[999999999]
```

İstersek OutOfMemoryError'yi catch edip, thread'e devam edebiliriz. Throw edilen satırdaki "array1" için bellekte yer ayırtılmamış olacaktır. Bu sebeple, hala memory'de boş alanımız var anlamına gelecektir.

JVM'e opsiyonel parametre geçerek, OutOfMemoryError fırlatılması yerine, `JVM`'in kill edilmesini sağlayabiliriz. Hatta `JVM` yine ek parametre alarak, OutOfMemoryError fırlatıldığında istediğimiz shell script'ini execute edebilmemize de olanak sağlıyor.

- Eğer herhangi bir thread'den exception catch edilmez ise, sadece o thread kill edilir.

## 📌 Checked exception vs unchecked exception

bazı Throwable'lar catch edilmek zorundadır. yoksa kod compile edilemez. catch edilmek zorunda olan Throwable'lar "checked" olarak işaretlidir.

RuntimeException ve Error ve bunların subclass'ları haricindeki tüm Throwable'lar checked'tır.

Eğer unchecked exception yaratmak istiyorsak, yeni exception'umuzu unchecked exception'lardan türetmeliyiz.

Checked exception'lar handle edilmesi beklenen durumlarda fırlatılmalıdır. İlk bakışta her exception handle edilmeli gibi düşünülebilir fakat önemli olan husus, hadle edilip yeni bir aksiyon alınabilecek mi? Örneğin NullPointerException, ArithmeticException checked değildir. Çünkü bu hatayı alan kişinin bir çözüme gitmesi beklenmez. ArithmeticException alan bir algoritma düşünelim, 0'a bölüm işlemi yaptığında hata alıyorsa, farklı bir yoldan gitmesi beklenmez. Oysa dosyaya yazma işleminde hata alınan bir durumda, permission'larla ilgili bir son kullanıcıdan yetki isteği yapabilir veya yetkisi olduğundan emin olunan geçici bir yere yazmayı deneyebilir gibi. Bu sebeple dosya yazmalardaki yetki işlemlerindeki hatalar (IOException) checked'tır.

## 📌 finally block on java

finally metodu her türlü çalışır. thread'in try veya exception bloğuna girmiş olması, hatta önceden "return" çağrılmış olması bir şey ifade etmez. örnek:

```java
public static void main(String[] args) {
  System.out.println(test1());
}

public static int test1() {
  try{
      System.out.println("try");
      return 1;
  }catch (Exception e){
      // any code here...
  }finally {
      System.out.println("finally");
      return -1;
  }
}
```

prints:

```text
try
finally
-1
```

## 📌 best practices

- istisnalar dışında; sadece checked olan Throwable'lar fırlatılmalıdır. örneğin; ArithmeticException (unchecked) fırlatılmamalıdır. çünkü zaten unchecked exception'lar `JVM` tarafından fırlatılıyor. Tekrar yazmanın bir anlamı yok. kod kalabalığı yaratırız. örnek:

  ```java

  if( a == 0){
    throw new ArithmeticException();
  }
  return b/a;
  ```

  Yukarıdaki kod sadece kod kalabalığı yaratmaktadır. zira arithmetic exception bu metodu çağıran yazılımcı tarafından yakalanmak zorunda değildir.

- her çeşit exception ayrı ayrı fırlatılmalıdır. hepsi gruplanıp fırlatılmamalıdır. böylece yakalan metot ne olduğu bilinir ve her exception çeşidi için ayrı ayrı ne yapacağına karar verebilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Java Specification Requests (⟷ JSR)

Java ve Java EE için toplulukların yayımladığı spesifikasyonlardır.

## 📌 JDK Enhancement Proposal (⟷ JEP)

OpenJDK için toplulukların yayımladığı spesifikasyonlardır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 @transient

Core Java annotation'ıdır. serialize edilirken bu objenin es geçilmesini sağlar. Bunu ignore edip etmemek kütüphanenin kendisine kalmıştır. Örneğin;

- GSON ve diğer birçok serialize etme kütüphaneleri bu değeri kullanır.
- ORM kütüphaneleri bu field'ı DB'ye yazmaz, ve entity DB'den okunduğunda buraya mapping yapmaz. Genelde bu field'a ait getter fonksiyonu logic içerir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 remote debug

Java uygulaması çalıştırıldığında debug parametresi ile başlatılır ise; `JVM` bir port üzerinden "debugger" uygulamasının kendisine bağlanmasına izin verir. debugger uygulaması genelde bir IDE'dir. IDE uygulamaya bağlanır. IDE üzerinde önceden proje tanımlanmıştır, bu şekilde source code ile Java uygulaması arasında ilişki kurulabilir.

bir uygulamayı lokalde IDE ile debug ettiğimizde, IDE arka planda bu işlemi yapar fakat yazılımcıya fark ettirmez. remote=localhost olur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Java Serializable

bir sınıfın içindeki anlık değerler ile byte olarak kaydedilmesidir. De-Serialization işlemi ise byte dizisinden Java sınıfına aktarılmasıdır. byte'a çevrilecek sınıf  Serializable sınıfının implemente etmek zorundadır.

## 📌 serialVersionUID

bu değer Serializable'dan implemente edilmiş sınıflarda olmalıdır. bu değer byte'a çevrilen sınıfın içine gömülür. byte dizisinden De-Serialization yapıldığında eğer sınıfın serialVersionUID'si aynı ise De-Serialization uyumlu bulduğu tüm değerleri sınıf değerlerine doldurur. eğer serialVersionUID farklı ise, direk olarak hata alınacaktır. bu sebeple yazılımı geliştirirken serialVersionUID'leri manuel atamakta yarar var. aksi durumda derleme sırasında bu değerler rastgele atanacak ve dosyadan De-Serialization işlemlerinde aynı serialVersionUID olmayacağı için, yeni alan eklenmiş sınıflara dahi De-Serialization olumlu sonuçlanmayacaktır.

## 📌 Eclipse'in sunduğu otomatik serialVersionUID seçenekleri

1- default serialVersionUID: 1L (long tipinde 1 değeri) atanır. serialVersionUID değeri önemsiz olan sınıflara atılması için önerilir.

2- oto generated serialVersionUID: sınıfın içindeki tüm değişkenlere bakılır. tüm bu değerlere göre bir hash üretilir. bu hash ile serialVersionUID oluşturulur. bu algoritma ile aynı aynı olan tüm sınıfların serialVersionUID'leri aynı olacaktır. bu şekilde başka bir yazılım tarafından okunan serialize edilmiş veri başarılı şekilde okunabilecektir. Java sınıfımızda bir değişiklik yaptığımızda serialVersionUID otomatik değişecektir. artık modelimiz serialize edilmiş dosyadan okuyamayacaktır. zaten beklenen de budur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 inheritance of class fields

bu Java'da mümkün değil. örnek üzerinden gidelim:

```java
class Animal {
    
    String name = "unknown";
    
    String run(){
        return "Animal is running";
    }
}

class Dog extends Animal {
    
    String name = "dog";
    
    String run(){
        return "Dog is running";
    }
}

class HelloWorld {
    public static void main(String[] args) {
        Animal instance1 = new Dog();
        // prints: unknown & Dog is running
        System.out.println(instance1.name + " & " + instance1.run());
        
        Dog instance2 = new Dog();
        // prints: dog & Dog is running
        System.out.println(instance2.name + " & " + instance2.run());
    }
}
```

Super class'taki bir field'ı override edemeyiz. Zaten etmemeliyiz de. Çünkü super class o filed'ı kendi içinde tamamen özel bir logic için kullanıyor olabilir.

Eğer biz o field'a erişmek istiyorsak, `super.name` veya `getter` ve `setter` metotları aracılığı ile erişmemiz daha doğrudur. Inheritance'ta sadece davranışları override etmeliyiz, yani metotları.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Java SE (⟷ Java standart edition ⟷ old-name:J2SE) vs Java EE (⟷ Jakarta EE ⟷ Java enterprise edition ⟷ Java 2 Platforms ⟷ Java Platform ⟷ old-name:j2ee)

ortalama bir desktop bilgisayar ihtiyaçlarını karşılayan platformlar için:

- standart kütüphaneler
- `JVM`
- `Java` Language

`Java EE`, `Java SE`'yi kullanan ekstra kütüphaneler içerir. runtime sırasında hangi kütüphanelerin olup olmayacağına çalışan uygulama (sunucu) belirler. ek kütüphaneler `JAR`'ın içinde yada classpath'te bulunmalıdır. bu kütüphaneleri barındıran sunucular `GlassFish`, `Weblogic`'tir.

## 📌 JDK (⟷ Java development kit)

`JDK` içinde `Java EE` kütüphaneleri bulunmaz.

`JDK` içinde;

- `Wsimport` (web service yaratma tool'u)
- `Keytool` (sertifika import ve export tool'u)
- gibi birçok utility
- `JRE`
  - `Java` app starter (`java` command)
  - `Java` web starter (`Javaws` command)
  - `javaw` command (wraps the `java` command, and starts the application on a new terminal interpreter)
- compiler (`javac` command)

## 📌 Oracle JDK (⟷ old-name:Sun JDK) vs OpenJDK

(not: `JDK`'lar kendi içlerinde kendi `JVM`'lerini içermektedirler.)

`OpenJDK`, Java SE'nin 7'inci sürümü ile artık referans implementasyon olmuştur. yani sadece desktop makinalar için `JVM` implementasyonu olarak OpenJDK gösterilir. yani; artık Oracle JRE'si, OpenJDK üzerine kuruludur. eskiden de OpenJDK ve Oracle JDK birbirleri ile ortak kodları kullanarak ilerliyorlardı. fakat 7inci sürüm ile artık kesin olarak Oracle JDK, OpenJDK implementasyonu olacağı garantisi verilmiştir.

Oracle JDK ile OpenJDK arasındaki farklar şunlardır:

- Oracle JDK'nın lisansı farklıdır

- Java Plugin and Java WebStart

- JRockit Mission Control (detaylar başka yerde yazıyor)

- font'lar

- bazı ek kütüphaneler

- Oracle JDK 11 ve `JVM` 11 artık ücretli olmuştur. her canlıya atılan paket için Oracle'a ücret ödenmelidir.

Başka bir başlıkta bu konu detaylandırıldı.

## 📌 IcedTea

OpenJDK'yı geliştiren community tüm OS'ları desteklememektedirler. bu sebeple farklı bir topluluk OpenJDK'yı diğer OS'lara entegre eder. yani OpenJDK'yı farklı platformlar için build eder.

## 📌 IcedTea-Web

OpenJDK'da olmayan bir ek modüldür. bu modül web tarayıcıları için Java eklentisi sunar. Java-plugin'e açık kaynaklı alternatiftir.

## 📌 Eclipse IDE Enterprise for Java (⟷ EE4J)

`Java EE` `Oracle` tarafından geliştiriliyordu. 2017'de `Oracle` `Eclipse` vakfına geliştirmeyi bıraktı. `Eclipse` ise `Java EE`'yi, `Jakarta EE` olarak değiştirdi. `Jakarta EE` ve onunla ilişkili diğer tüm projeler için root proje olarak `EE4J` ismi verildi.

## 📌 WebStart vs Java plugin

Java plugin; web tarayıcıya kurulan eklentinin ismidir.

WebStart; Applet'lerin bir sonraki adımı gibi düşünülebilir. local siteme uygulama (Applet benzeri ufak uygulama) indirme, çevirim dışı daha sonra açmak için OS'a kısa yol atama, kurulan uygulamaları yönetebilme, sürüm güncelleme gibi işlevlerin yapılmasını sağlar. bu uygulamalar tarayıcıdan direk olarak kurulabilir. bu uygulamacıklarda izinler çok kısıtlıdır.

## 📌 Java Applet

Java runtime'a ihtiyaç duyar. client tarafta tarayıcı üzerinden Java-plugin aracılığı ile uygulama çalıştırır.

## 📌 JavaFX

masaüstü uygulaması yazılmasını sağlıyor. ek olarak webview kullanımını kolaylaştırmıştır. WebView içerisini düzenleme ve üzerinde farklı syntax'lar ile düzenlemeler yapma özellikleri (entegrasyon) sunmaktadır. WebView içerdiğinden, birçok GUI elementi HTML elementi olacağından, Java'daki diğer GUI kütüphanelerine göre daha cross-platform olduğu söylenebilir.

## 📌 Hotspot

JVM spesifikasyonunu implemente eden birçok `JVM` mevcuttur. "Hotspot", Oracle'ın resmi `JVM`'idir. piyasada onlarca `JVM` implementasyonu mevcut. (başka başlıkta örnekler var)

Oracle'ın `JVM` ismi olan Hotspot, içerdiği Hotspot teknolojisinden gelmektedir. sun `JVM`, Oracle tarafından satın alınmadan önce JRockit olarak adlandırılıyor.

Hotspot ilk bu isimle çıktığında, özel bir teknoloji barındırıyordu. bu teknoloji; sık kullanılan kod bloklarını, daha hızlı erişebileceği bir bellekte tutmakta ve o kod blokları için optimizasyonu arttırmaktadır. kodlar hakkında istatistikler tutmaktadır. bu istatistikler sonucunda;

- sık kullanılan kod blokları native makine koduna çevrili şekilde hazır tutulmaktadır

- gereksiz bazı kod blokları yürütülmemektedir

- bazı kod bloklarındaki sık girilen if-blokları başa taşınmakta, az kullanılan if-condition'ları alta taşınmaktadır

- ve daha birçok teknik kullanılmaktadır

"Hotspot" kelimesi sık kullanılan bölgeleri (sıcak bölgeleri/noktaları) belirtmek için kullanılan genel bir terimdir. `JVM`'in ismi de buradan gelmektedir.

Java dili eskiden interpreter gerektiren bir dildi. Artık güncel sürümlerde Hotspot teknolojisinin gelişmesi ile bazı kod blokları, native kod olarak saklandığı için, tekrar interpreter tarafından işletilmiyor. direk olarak makinada run ediliyor (native app gibi). dolayısı ile yeni `JVM` sürümleri yarı interpreter yarı native kod çalıştırır durumdadır.

java -version ile komut satırından bilgi alındığında "mixed mode" çıktısı; JIT'in de kullanıldığı anlamına gelmektedir. isteğe bağlı bu özellik devre dışı bırakılabilmektedir.

## 📌 native lib

Java Hotspot ile sık girilen kod bloklarını native makine koduna çevirip saklamaktadır. fakat önceden yazılmış default kütüphaneler (JVM içerisinde default gelen sınıflar) önceden makine koduna derlenmiş saklanabilmektedirler. bunlara native lib denmektedir.

`Nativelib` içerisindeki kütüphaneler `CPU`'ların özelliklerinden de yararlanmaktadırlar. örneğin `System.arraycopy()` metodu array kopyalamayı `CPU`'nun özelliğinden yararlanarak yapar. bu tarz metotlardan faydalanmak uygulama hızını çok etkilemektedir.

## 📌 Technology Compatibility Kit (⟷ TCK)

`JVM` olabilmek için belirlenmiş standartlardır. Tüm `JVM`'ler TCK'da belirtilen kurallara uymak zorundadırlar.

## 📌 OpenJDK builds

OpenJDK stabil olsa da, production-ready build'leri resmi olarak süreklilik garantisi verilerek dağıtılmamaktadır. dolayısı ile production-ready değildir. fakat production ortamında kullananlar vardır. bu sebeple; open-JDK kullanmak istemeyenler farklı `JVM` implementasyonları kullanırlar. zaten neredeyse tüm `JVM`'ler OpenJDK'yı referans alır ve üzerine eklemeler/çıkarmalar yapılarak dağıtılırlar.

- OpenJDK, Oracle tarafından geliştirildiği için OpenJDK'nın sitesinde <https://OpenJDK.java.net/install/> indirme linkleri Oracle JDK'yı indirir. çünkü OpenJDK türevi olan production-ready örneği olarak Oracle kendini gösterir. dolayısı ile download linkleri buraya yönlenir: <https://www.oracle.com/technetwork/java/javase/downloads/index.html>

- OpenJDK build'leri ise buradan indirilebilir: <https://JDK.java.net/archive/>

- <https://adoptopenjdk.Net> projesi OpenJDK'yı direk olarak saf haliyle derleyip sunmaktadır. projenin arkasında güçlü destekçiler vardır.

- Ubuntu "apt install OpenJDK-8-JRE" komutu ile OpenJDK'yı kurar.

## 📌 Oracle JDK vs AdoptOpenJDK

detaylı bilgiye buradan ulaşılabilir: https://adoptopenjdk.net/migration.html

| Oracle JDK 8 proprietary component         | Alternative component               | OpenJDK 8         | OpenJDK 11        |
|--------------------------------------------|-------------------------------------|-------------------|-------------------|
| Java Web Start                             | IcedTea-Web                         | yes               | no                |
| JavaFX                                     | OpenJFX                             | no                | no (coming soon)  |
| T2K font rendering engine                  | Freetype                            | yes               | yes               |
| Monotype Lucida fonts                      | Relicensed Lucida fonts             | no (coming soon)  | no (coming soon)  |
| Ductus 2D renderer                         | Pisces/Marlin                       | yes (Pisces)      | yes (Marlin)      |
| Kodac Color Matching System (KCMS) library | LCMS                                | yes               | yes               |
| SNMP                                       | Use JMX (⟷ SNMP4J)                 | yes (not bundled) | yes (not bundled) |
| Sound drivers                              | Use MS Windows sound drivers | yes (not bundled) | yes (not bundled) |
| Java Flight Recorder (JFR)                 | Java Flight Recorder                | no (coming soon)  | yes               |
| Java Mission Control (JMC)                 | Use JDK Mission Control             | no (coming soon)  | no (coming soon)  |

Not: JavaFX, JFR, and JMC, Java 8 ile Oracle tarafından OpenJDK projesine dahil edildi.

## 📌 JVM implementations

| `JDK` Dağıtımı    | Şirket      | Açıklama                                                               |
|-------------------|-------------|------------------------------------------------------------------------|
| `OracleJDK`       | `Oracle`    |                                                                        |
| `Red Hat OpenJDK` | `Red Hat`   |                                                                        |
| `Corretto`        | `Amazon`    |                                                                        |
| `Zulu`            | `Azul`      |                                                                        |
| `Zing`            | `Azul`      | `Zulu`'nun özel `GC` ve derleyiciye sahip ticari sürümü                |
| `Semeru`          | `IBM`       | `OpenJ9` tabanlı. yalnızca `IBM` tarafından geliştiriliyor             |
| `Temurin`         | `Eclipse`   | Proje adı: `Adoptium`                                                  |
| `AdoptJDK`        | `Community` | `HotSpot` ve `OpenJ9` sürümleri vardı. Durduruldu, `Temurin`'e geçildi |
| `Liberica`        | `BellSoft`  |                                                                        |

Burada bu konu hakkında güzel bilgiler yer almaktadır: <https://github.com/whichjdk/whichjdk.com>

## 📌 OpenJ9

`OpenJDK` ve `Oracle`'da gelen `HotSpot` teknolojisine alternatif bir yapıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Look And Feel

sadece tema değil, aynı zamanda efektler ve objeler davranış biçimlerini de kapsayan bir UI terimidir. Sadece tema terimi kullanıldığında, son kullanıcının UI'a karşı tüm hissiyatını kapsamamaktadır. Bu sebeple "`Look And Feel`" terimi daha geniş bir terimdir.

## 📌 AWT (⟷ Abstract MS Windows Toolkit) vs SWING

`AWT`, `OS`'un native `GUI` objelerini kullanır. Fakat hepsini ortak bir Interface üzerinden yönettiği için, sadece ortak özellikler kullanılabilmektedir. AWT her platformda farklı göründüğünden, `Look And Feel` aynı olmamaktadır.

`SWING` ise, pixel pixel ekrana kendi `GUI` objelerini basmaktadır ve tüm platformlarda aynı görünüme ve davranışlara sahiptir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 HTTP clients for java

Client'lar sundukları özelliklere göre değerlendirilir. genel olarak 3'ye gruplayabiliriz (abstraction level):

- Response ve request'leri mapping yapanlar (high level)
- Response ve request'leri mapping yapmayanlar (low level).
- bazı client'lar ise (aşağıdaki listede "very high" olarak belirtildi) diğer tüm decoder/encoder'ları, HTTP client'ları wrap etmemizi sağlıyor. örneğin Feign-client bu kategoride ve bu şekilde setup'ı yapılabilir:

```java
BookClient bookClient = Feign.builder()
  .client(new OkHttpClient())
  .encoder(new GsonEncoder())
  .decoder(new GsonDecoder())
  .logger(new Slf4jLogger(BookClient.class))
  .logLevel(Logger.Level.FULL)
  .target(BookClient.class, "http://localhost:8081/API/books");
```

Yukarıdaki builder'a geçebileceğimiz parametreler iyi bir görsel ile burada gösterilmiştir: https://github.com/OpenFeign/feign. Görseldeki bazı kısımlar buraya kopyalandı:

- clients
  - `java.net.URL`
  - `Apache` `http`
  - `Apache` `HC5`
  - `Google` `HTTP`
  - `Java` 11 `HTTP` 2
  - `OKhttp`
  - `Ribbon`
- async clients
  - `java.net.URL`
  - `Apache` `HC5`
- contracts
  - `Feign`
  - `JAX-RS`
  - `JAX-RS` 2
  - `Spring` 4
  - `SOAP`
  - `spring-boot` (3rd party)
- encoders and decoders
  - `GSON`
  - `Jackson` 1
  - `Jackson` 2
  - `Jackson` `JAXB`
  - `Sax`
- metrics
  - `Dropwizard metrics` 5
  - `Micrometer`
- extras
  - `Hystrix`
  - `SL4J`
  - `Mock`

| name                                                                                                                                                                                                    | abstraction level | comment                                                                                                                                                                                                                                                                                                          |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `Apache` groupId:`commons-httpclient` artifactId:`commons-httpclient`                                                                                                                                   | low               | Proje sonlandı.                                                                                                                                                                                                                                                                       |
| groupId:`org.apache.httpcomponents` artifactId:`httpclient`                                                                                                                                             | low               | `Apache` `commons-httpclient` replaced by this project                                                                                                                                                                                                                                                           |
| `java.net.http.HttpClient`                                                                                                                                                                              | low               | `Java` 10 ile geldi. `java.net.HttpURLConnection` API'si vardı. Fakat eski API yeni `HTTP` 2 sürümlerini desteklemiyor ve API'leri yeni metodolojileri desteklemiyordu ve async çalışmıyordu.                                                                                                                    |
| `Google-http-Java-client`                                                                                                                                                                               | high              | `Android` için kütüphanelerinde kullanmak amaçlı önerilir                                                                                                                                                                                                                                                        |
| groupId:`org.asynchttpclient` artifactId:`async-http-client`                                                                                                                                            | ?                 |                                                                                                                                                                                                                                                                                        |
| `RestTemplate` sınıfı. `spring` kullanıyorsak: groupId:`org.springframework` artifactId:`spring-web` `spring-boot` kullanıyorsak groupId:`org.springframework.boot` artifactId:`spring-boot-starter-web` | high              | `java.net.HttpURLConnection` (`Java` 11 öncesinde var olan `API`) tabanlı. Yakın zamanda deprecated olacak. çünkü sadece sync işlemleri destekliyor. `Spring`, yeni olarak `org.springframework.web.reactive.client.WebClient`'ı çıkardı. `WebClient` hem sync hem async destekliyor. |
| `Feign` or `OpenFeign`                                                                                                                                                                                  | very high         | Declarative olarak client tanımlamamızı sağlıyor (Interface-based implementation). kendi içinde `java.net.HttpURLConnection` (`Java` 11 öncesinde var olan API) ve reflection'lardan yararlanıyor.                                                                                    |
| `Retrofit`                                                                                                                                                                                              | very high         | `Feign` gibi Declarative client oluşturabilmemizi sağlıyor.                                                                                                                                                                                                                                                      |
| `OKHttp`                                                                                                                                                                                                | ?                 | ?                                                                                                                                                                                                                                                                                                                |
| `Unirest`                                                                                                                                                                                               | ?                 | ?                                                                                                                                                                                                                                                                                                                |

## 📌 JAX-WS (⟷ Java API for XML Web Services) vs JAX-RS (⟷ Java API for RESTful Web Services)

`JAX-WS`; `Java EE`'nin `SOAP` web servisi spesifikasyonudur.

`JAX-RS`; `Java EE`'nin `REST` spesifikasyonudur. 

Sadece `API`'dir. Bu `API`'leri aşağıdaki dependency ile projemize ekleyebiliriz.

```xml
<dependency>
    <groupId>javax</groupId>
    <artifactId>javaee-API</artifactId>
    <version>7.0</version>
    <scope>provided</scope>
</dependency>
```

Runtime sırasında ise `Java EE` implementasyonumuz (server'ımız örnek: `tomee`) bunu implemente eden paketi sunacaktır.

## 📌 JAX-WS vs JAX-RS implementasyonları

`Java EE`'de default implementasyon kavramı yoktur. hangi sunucuya atarsak (`jboss`, `Tomcat` gibi) kendi implementasyonunu sunar. üzerinde çalıştırdığımız sunucununkinden farklı implementasyon istersek, biz spesifik `jar`'ları yükleyip konfigürasyon yapmak gerekecektir.

| name         | `JAX-WS` | `JAX-RS` | Installed Default                           |
|--------------|----------|----------|---------------------------------------------|
| `Apache CXF` | yes      | yes      | `TomEE`, `IBM WebSphere Application Server` |
| `Axis`       | yes      | no       |                                             |
| `Axis2`      | yes      | yes      |                                             |
| `Jersey`     | no       | yes      | `GlassFish Server`, `Payara Server`         |
| `RESTEasy`   | no       | yes      | `JBoss`, `WildFly`                          |
| `Restlet`    | yes      | ?        |                                             |

Yukarıdaki bazı implementasyonlar, hem server hem de client tarafını da sunmaktadır. Örnek: `RESTEasy`.

`Spring` framework, `spring-mvc` modülünde bu hizmeti sunmaktadır. `JAX-RS` implementasyonu değildir.

`Spring` ile `Spring-MVC` kullanılmak zorunda değiliz. `Spring-MVC` alternatiflerini de, `Spring` ile kullanabiliriz. örnek: `Spring-boot-starter-jersey`.

## 📌 wsimport

`JDK` içinde yüklü gelen komut satırı uygulaması. `WSDL`'den, `SOAP` web service client `Java` kodu generate etmemize yarıyor.

## 📌 Eclipse GUI client generator

`elipse` `IDE`'de herhangi bir projeye sağ tıklayıp, `New` menüsünden `Web Service Client` seçeneğine tıklayıp, `WSDL`'i veriyoruz, `IDE` bize `Java` kodu üretiyor. burada `Eclipse` `IDE` arka planda `Axis`'in generator'ını kullanıyor.

## 📌 Axis vs Axis2

`Axis`'te büyük değişiklikler yapıldı ve yeni proje olarak fork edildi. artık `REST`'te destekliyor. `Axis2` projenin yeni adıdır. `Axis2`'nin yanına sürüm numarasını gelecektir.

## 📌 RPC (⟷ Remote procedure call)

Uzak makinede yada aynı makinede farklı process'ler arasında metot çağırıp işletme mekanizmasıdır. bu bir konsepttir. protocol değildir. herkes uzaktaki bir metodu çağırıp dönüş alabilir. genelde programlama dilleri kendi protocol'lerini/standartlarını kullanıyor. bu sebeple cross-language değillerdir.

## 📌 Remote Method Invocation (⟷ RMI)

`Java`'nın kullandığı `RPC`'dir.

## 📌 XML-RPC

bir çeşit `RPC` standardıdır.

## 📌 WSDL içinde geçen terimler

`WSDL` `2.0` sürümü `HTTP` isteklerini kısmen desteklemektedir.

aşağıdaki her tanımın `name` isminde property'si mutlaka olmalıdır.

tüm `WSDL` `XML`'inin root elementi `<definition>` etiketindedir.

| `1.1`       | `2.0`       | AÇIKLAMA                                                                                                                                        |
|-----------|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| port      | Endpoint  | gidilecek olan isteğin `URL`'sidir (path ile birlikte)                                                                                            |
| service   | service   | her endpoint'i birer binding'le eşleştiren mapping'dir. örnek: `X` servisi, `A` binding'i ile eşleşir.                                               |
| binding   | binding   | operation'lar kümesidir. + istek hakkında meta bilgiler içerir                                                                                  |
| portType  | Interface | operation'lar kümesidir. + her operation içinde gidip gelecek olan DTO'ları da tanımlamaktadır. + error durumunda dönecek DTO tanımı buradadır. |
| operation | operation | her metot bir operation'dur.                                                                                                                    |
| message   | (yok)     | bir request yada response'un tüm DTO'dur. her message bir type kümesidir. message etiketi içinde her type "part" isimli etiket ile tanımlanır.  |
| types     | types     | her DTO bir type'tır. type tüm request yada response olmak zorunda değildir. bir kısmı da olabilir.                                             |

## 📌 Subset WSDL (⟷ SWSDL)

sadece bir metot kümesini kapsar. SWSDL alınıp client üretilirse, SWSDL içindeki metotlar çağrılabilir.

## 📌 marshalling (⟷ marshaling) vs serialization

Genel bilişim terimleridir.

marshalling işlemi; bir sınıfın instance'ının, uzaktaki bir ortama transferi için:

- serialization
- ve eğer gerekiyorsa bazı metadata'ların (sınıfın package name'i gibi. Java'da "codebase" de alınır.)

bilgilerinin byte array veya herhangi bir formata çevrilmesidir.

## 📌 codebase

- RMI uzaktan Java metodu çağırır. dönüşünde ise bir sınıf alır. dönüşte alacağı sınıfı client bilmeyebilir. client sadece o sınıfın arayüzünü bilir. fakat dönecek implementasyonu bilemez. böyle durumlarda RMI çağırdığı metottan dönen codebase bilgisine bakar. codebase sınıfın ID'sini barındırır. eğer aynı ID'de bir sınıf lokalde varsa; client lokaldeki sınıfı kullanır. fakat eğer lokalde yoksa; uzaktan o sınıfı download eder.

- codebase kelimesi yazılım dünyasında source-code'un tümüne verilen isimdir. genelde generated-file'ları barındırmaz. sadece insanların yazdığı kodların tümüne denir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Java stack yapısı

stack; sadece thread'e bağlı değerleri tutan veri yapısıdır. her thread için ayrı stack vardır. main thread'de buraya dahildir. primitive değerler ve objelerin referansları, stack deposunda tutulur. fakat objeler heap'te tutulur.

thread'de (stack'te) her metot, kendi içinde ona yollanan edilen tüm verileri de tutuyor. yani stack içinde her metot için ayrı bir bölge var. bu sebeple;

```java
metot1(){

     User user1 = new User("ahmet");

     metot2(user1);

     print( user1.name ); // burası ekrana ahmet basacaktır.

}

metot2(User user){

    // burada; metot2 bölgemizde; sadece user diye bir variable var.

    user = new User("Ayşe");  // Bu satırda user variable'nin üzerine yeni bir variable atamış oluyoruz. sadece metot2 bölgesinde bir değişiklik yaptık. eski instance yaşamaya devam ediyor.

}
```

## 📌 call by value (⟷ pass by value) vs call by reference (⟷ pass by reference)

genel programlama dillerinde kullanılan terimlerdir.

Java primitive ve objeler için (yani her zaman) için call-by-value yöntemini kullanır. burada bir konuyu netleştirelim: Java'da objenin value'si geçilirken, aslında objenin heap'teki referansı geçilir. buraya kadar her şey normal. fakat geçilen bu referans'ın kopyası (clone'u), stack içindeki sadece o metoda ait yerde tutulur. yani hem dallandığımız metot içerisinde hem de metodu çağırdığımız metot içerisinde 2 adet kopya olmuş olur.

yani; 'reference' değilde, 'reference value' yollamış oluyoruz. aslında reference yollanıyor fakat kopyası yollanıyor. yukarıdaki kod örneğinde bu kopya değerinin üzerine yeni objemizin (Ayşe) adresini yazıyoruz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Java debugger features

`Eclipse` IDE ve `IntelliJ IDEA` debug mode'undayken aşağıdaki button'ları sunar:

`force` prefix on `IntelliJ IDEA`: `IntelliJ IDEA`'de neredeyse her özellik `force` kombinasyonu ile de sunuluyor. `force` ile bir işlem yapınca yapmak istediğimiz işlemin içerisinde debug point varsa, o debug point'ler ignore ediliyor.

### 📌📌 step into

`Eclipse` simgesi: `↴` (rightwards arrow with corner downwards)

`IntelliJ IDEA` simgesi: `Eclipse` IDE ile aynı fakat altı çizgili.

ilgili metodun içindeki ilk satıra gider.

ek not: `IntelliJ IDEA` default olarak bazı metotların içine girmez. Bunlar `JRE`'nin içindeki bazı metotlardır (örneğin System class'ı). Bu davranış biçimi ayarlardan değiştirilebilir.

### 📌📌 smart step into

bu sadece `Intellij IDEA`'de var. `Eclipse` IDE'de yok.

`IntelliJ IDEA` simgesi: `⇅` (upwards arrow leftwards of downwards arrow)

`step into` ile aynı. fakat 1 satırda bazen birden fazla fonksiyon olabiliyor. örnek:

```java
print(count());
```

`IntelliJ IDEA` her fonksiyonu renklendirip hangisine girmek istediğimizi soruyor.

### 📌📌 force step into

bu sadece `Intellij IDEA`'de var. `Eclipse` IDE'de yok.

`IntelliJ IDEA` simgesi: `step into` ile aynı.

`step into` eğer girdiğimiz metot 3üncü parti bir `jar` içinde ise ignore eder ve bir sonraki satıra geçer. fakat `force step into` 3üncü parti `jar` içine de olsa girer.

### 📌📌 step over

`Eclipse` IDE simgesi: `↷` (clockwise top semi-circle arrow)

`IntelliJ IDEA` simgesi: `Eclipse` IDE ile aynı fakat altı çizgili.

ilgili satırı işletir ve sonraki satırda yine beklemede kalır.

### 📌📌 Step out

`Eclipse` IDE name: __step return__

`IntelliJ IDEA` name: __step out__

`Eclipse` IDE simgesi: `↱` (upwards arrow with tip rightwards)

`IntelliJ IDEA` simgesi: `↑` (underlined upwards arrow)

o anda bulunduğumuz metodun execute edilip sonlanmasını sağlıyor. farklı bir değiş ile: bulunduğumuz metod normal şekilde execute edilip bitmesini sağlıyoruz. Daha sonra parent stack'teki bir sonraki satırda duruyor (debug point olmasa bile duruyor). örnek:

```java
mainMethod(){
  method1();
  method2();
}

method1(){
  innerMethod1(); // if we click "step over" here, the debugger will execute innerMethod1 and innerMethod2, and then it will stop on method2 (even if we don't have debug point on method2).
  innerMethod2();
}
```

### 📌📌 resume

simgesi: `->` (rightwards arrow)

### 📌📌 run to cursor

`Eclipse` IDE name: __run to line__

`IntelliJ IDEA` name: __run to cursor__

`Eclipse` IDE simgesi: `->|` (rightwards arrow and cursor)

`IntelliJ IDEA` simgesi: `Eclipse` IDE ile aynı.

thread durdurulmuşken buna bastığımızda, caret'in (imleç'in) gösterdiği satıra kadar hiç durmadan gider. Yani kod execution'ı satır satır devam eder. Ancak bizim bulunduğumuz satırda yine beklemeye alınır.

### 📌📌 drop frame

`Eclipse` IDE name: __drop to frame__

`IntelliJ IDEA` name: __drop frame__

`Eclipse` IDE simgesi: `☰->` (triagram and small rightwards arrow)

`IntelliJ IDEA` simgesi: `x☐` ("x" and empty box)

o anda bulunduğumuz metodun tekrardan işletilmesini sağlıyor. bunun için:
- o metodun içinde yaratılmış bütün kaynakları (string, int...) yok ediyor
- debug noktasını metodun en başına taşıyor

bu durumda metodun aldığı parametrelerin referanslarında değişiklik yapmışsak, o değişiklikler kalmaya devam ediyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 StringBuilder vs StringBuffer

aynı işi gören iki kütüphanedir. tek farkı; `StringBuffer`'ın `thread-safe` olmasıdır. bu da `StringBuilder`'ı daha performanslı yapıyor.

`thread-safe` konusu `StringBuilder` ile başka başlıkta anlatılıyor.

## 📌 string vs StringBuilder

Bazen compiler aşağıdaki çevrimleri yapabiliyor:

```java
"b" + "a"; // compiler makes it: "ba"

"b" + variable; // compiler manage it with StringBuilder.

// but sometimes compiler can not catch all cases.
```

`StringBuilder` yerine tırnak ile `string` birleştirme arasında şu fark var:

- `StringBuilder` daha az `string` nesnesi yaratır. çünkü `string` birleştirmeyi kendi yönetir. fakat tırnak açarak `string` birleştirme yaparsak, sürekli yeni `string` nesnesi (immutable'dır `string`'ler) yarattığımız için daha çok bellek tüketmiş oluruz.
- `StringBuilder` daha hızlıdır. bunun bazı sebepleri:
  - sürekli yeni `string` oluşturmayacağı için garbage collector daha az işlem yapacaktır.
  - `StringBuilder` `string` yerine char-array ile kendi içinde daha optimize yöntemlerle `string` birleştirme yapar. sırf bu iş için tasarlanmıştır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 immutable types

- tersi mutable'dır.
- immutable‘ın Türkçe kelime anlamı: durağan, sabit, değişmeyen.

init dildikten sonra, nesnenin içindeki state değiştirilemez olduğunda, o nesne immutable'dır.

örneğin unmodifiable collection'lar immutable'dır. çünkü içindeki referans değişkenler (liste elemanları) değiştirilemezler. burada karıştırılmaması gereken nokta;

```java
List<Character> immutableList = Collections.unmodifiableList(list); // immutable yaptık

immutableList.add("new value"); // burası hata vermeli

immutableList = list2; // burası hata vermemeli. çünkü immutable list kavramı nesnenin kendisinin bir özelliğidir, nesnenin referansı konu dışıdır.
```

User nesnesi içindeki TC değişmemeli (kanun gereği kesin olduğunu varsayarsak); User immutable mı olur?  Hayır. Çünkü User içindeki ad, soyad her zaman değişebilir. User nesnesinin içindeki hiçbir değişken değiştirilemez olursa ancak immutable olur.

## 📌 immutability avantajları (Kod örnekleri)

- multi-thread uygulamalarda hata olması durumunu azaltır.
- immutable nesne başka bir nesne içinde field olarak bulunduğunu düşünelim. böyle bir durumda bu immutable objemizi dışarıya `getter` ile açarız. `getter` ile açılan instance'ımızı kimseni update(manipüle) edemeyeceğini biliyoruz. Bu yazılımcıya daha rahat bir ortam sunar. Her `getter` metodunu çağıran thread'in veya logic'in ne yaptığını kontrol etmek zorunda değilizdir.
- immutable bir class'ın hashcode'unu kendi içinde sabit olarak tutarız. böylece hash hesaplamasını sadece 1 kere yaparız, sonra hep o değeri kullanılır (çünkü tekrar hesaplamaya gerek olmayacak).
- Programda akış takibi/debug'ı daha kolay olur: çünkü side effect olmadığından eminiz. örnekler:
  - gelen request-DTO'muzu immutable yaparsak, onun 10 fonksiyon sonra dahi değişmediğine emin olup, debug'a devam edebiliriz.
  - command pattern ile çağrılan bir fonksiyondaki "command" immutable yapılmalıdır.
  - Kafka'dan okunan mesajlar immutable yapılmalıdır. çünkü artık değiştirilemezler.
- append-only data okuduk. bu okunan data'yı immutable yaparız ki, ilgili data'nın değiştirelemeyeceğinden emin olmuş oluruz. örnek append only data'lar:
  - cassandra event-store
  - IDE, text ediyor gibi UI uygulamalarında Undo/Redo bilgileri
  - list of "next page" and "back page" buttons of the web browsers.
  - okunan config `YAML` data'sı.
- backup data read-only okunur. ama yine de immutable yapılmalıdır ki, kod içerisinde farkında olmadan bi manipülasyon yapamamış oluruz.

## 📌 immutability avantajları (mimari örnekler)

- immutable distro:

  örnek `NixOS`, `Fedora Silverblue`
  
  daha güvenli, stabil, atomic rollback olmasını sağlıyor.

- containers:

  örnek `Docker`.
  
  daha güvenli, stabil, atomic rollback olmasını sağlıyor.

- immutable package managers

  örnek: `flatpak`, `snap`

  daha güvenli, stabil, atomic rollback olmasını sağlıyor.

- Kafka kendi içinde topic altındaki mesajları immutable tutar.

- backup data read-only okunur. ama yine de immutable yapılmalıdır ki, kod içerisinde farkında olmadan bi manipülasyon yapamayız.

- `file system` levels/snapshot

  örnekler:

  - `UnionFS` (used by `Docker`)

    `container`'da çalıştırdığımız her komut sonrası, `file system`'in immutable bir fork'u alınır. böylece eski `file system` snapshot'larına dönebilir veya onları başka bir `container` için kullanabiliriz.

  - `BTRFS` `file system` snapshot feature

    `BTRFS` herhangi bir dizinin snapshot'ını almamızı ve istediğiniz zaman ona dönebilmemizi sağlıyor.

- `Immutable Infrastructure (⟷ IaC)`

  bir sunucu kurulduktan sonra değişiklik yapılmadığını garanti eder. Güncelleme ihtiyacı varsa, sıfırdan yeni bir sunucu oluşturulur ve eskisiyle yer değiştirilir. günceleme yapmaz.

## 📌 immutable vs thread-safe

ikisi tamamen apayrı kavramlardır. Immutable olan sınıfın her metodu `thread-safe` olmayabilir. Örnek:

```java
// CustomStringAppender is immutable.
// Because there is no way to change the state of CustomStringAppender after it's initialization.
public class CustomStringAppender {

    // internalStr is immutable.
    private final String internalStr;

    // constructor
    public CustomStringAppender(String initStr) {
        this.internalStr = "initiated as: " + initStr;
    }

    public void appendTo(Appendable appendable) {
        // append method uses StringBuilder internally.
        // StringBuilder is non thread-safe class.
        // That means "append" method is not thread-safe.
        // That means "appendTo" method is not thread-safe.
        // That means "CustomStringAppender" is not thread-safe.
        appendable.append(internalStr);
    }
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 java.util.Optional

Java 8 ile gelmiştir.

NullPointer kontrollerinin gerekmediği daha temiz kod yazılabilmesini sağlamaktadır. Kod mantığında "if" bloğu olmamasını gösteren en basit örnek:

```java
String message = null;

Optional<String> opt = Optional.ofNullable(message); // ofNullable null değer alırsa NullPointer fırlatmaz.

opt.filter(m -> m.length() > 5) // filter, filtreleme yapmamızı sağlayan ek bir metottur.
      .ifPresent(System.out::println); // eğer filtre sonucundaki değeler null değilse, bu kod bloğu işletilir. böylece null kontrolü yapmamış oluyoruz.
```

Optional class'ının sunduğu metotlara örnekler:

```java
///////////////////
// CREATOR METHODS
///////////////////
Optional<Double> opt = Optional.empty(); // boş bir Optional instance'ı oluşturur. isPresent'i false'tur.

Optional<String> opt = Optional.of("Merhaba"); // eğer null yollasaydık NullPointerException fırlatırdı.

Optional<Integer> opt = Optional.ofNullable(null); // eğer null değer alırsa hata fırlatmaz.

///////////////////
// VALUE CHECK METHODS
///////////////////

opt.isEmpty(); // value var ise false döner.

opt.isPresent(); // yukarıdakinin tersidir. boş ise false döner.

///////////////////
// GET METHODS
///////////////////

int value = opt.orElse(0); // o5 içindeki value null ise, 0 döndürür. null değil ise optional içindeki value'yu döndürür.

int value = opt.orElseThrow(RuntimeException::new); // opt null ise parametrede geçeceğimiz exception'u fırlatır. eğer null değil ise optional içindeki değeri döndürür.

int value = opt.orElseThrow( () -> {
  return new Exception("custom error");
}); // opt null ise parametrede geçeceğimiz metodun içinden üretilen exception'ı fırlatır. eğer null değil ise optional içindeki değeri döndürür.

int value = opt.orElseGet(() -> { return 999}); // orElseGet metodu, orElse ile aynı mantıkta çalışmaktadır. tek bir obje yerine fonksiyon parametre alması. eğer opt null ise; parametre geçtiğimiz fonksiyonumuzun döndürdüğü değeri döndürür.

int value = opt.get(); // If a value is present, returns the value, otherwise throws NoSuchElementException.

int value = opt.or( () -> {
  return new Optional(9999);
} ); // If a value is present, returns the value. otherwise return the optional value from the function.

///////////////////
// IF PRESENT METHODS
///////////////////

opt.ifPresentOrElse(value -> { 
  // do something with value. 
  }, () -> {
    // do something if value does not exist.
  }); ​

// eğer optional içindeki değer null değilse, ifPresent metodu koşar. yoksa koşmaz.
// Preset kelime anlamı: mevcut,     önceden ayarlanmış.
opt.ifPresent(value -> {
    System.out.println(value);
});

///////////////////
// JAVA OBJECT CLASS METHODS
///////////////////

opt.toString(); // "Optional" prefix'i ile asıl value'nun toString'ini döner.

opt.hashCode(); // asıl value'nun hashCode metodunu döner.

opt.equals(); // asıl value'ların equals'larını kullanır.

///////////////////
// MAPPER METHODS
///////////////////

// aşağıda price() metodu direk olarak integer döndüğünü varsayıyorum.
Optional<Integer> opt2 = opt.map(value -> { return value.price(); } ); // if value is null, then returns empty optional. opt and opt2 don't have to store same class type.

// Yukarıdaki price metodu direk integer değilde, Optional<Integer> dönseydi, o zaman aşağıdaki gibi bir durumla karşı karşıya kalırdık:
Optional<Optional<Integer>> opt2 = opt.map(value -> { return value.price(); } ); 

// İşte bu sebeple; eğer price, Optional<Integer> dönseydi, flatMap kullanmamız mantıklı olurdu:
Optional opt2 = opt.flatMap(value -> { return value.price(); } );

///////////////////
// OTHER METHODS
///////////////////

Optional opt2 = opt.filter(value -> value.price() > 5); // basit filtre yapar. Eğer filtre'den boş dönerse, boş bir Optional instance'ı dönülür.

opt.ostream(); // tek 1 elemanlı stream döner. eğer value null ise, boş stream döner.
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Nashorn

`Java` ile yazılmış, `JS` motorudur.

`JS` komutu ile `JS` dosyası execute edilebiliyor (`NodeJS` teki gibi).

## 📌 Java'dan JS çağırma örneği

- JS kodu:

```js
var fun1 = function(name) {
    print('Hi there from JS, ' + name);
    return "greetings from JS";
};
```

- Java kodu

```java
ScriptEngine engine = new ScriptEngineManager().getEngineByName("nashorn");

engine.eval(new FileReader("script.js"));

Invocable invocable = (Invocable) engine;

Object result = invocable.invokeFunction("fun1", "Peter Parker");

System.out.println(result);

System.out.println(result.getClass()); //prints java.lang.String
```

## 📌 JS tarafından Java kodu çağırma örneği

- Java kodu:

```java
static String fun1(String name) {

    System.out.format("Hi there from Java, %s", name);

    return "greetings from java";

}
```

- JS kodu:

```js
var MyJavaClass = Java.type('my.package.MyJavaClass');

var result = MyJavaClass.fun1('John Doe');

print(result);
```

## 📌 object types

`JS` tarafından `Java`'ya obje parametresi geçerken;

- çevrilebilen değerler (`JSON` standartındakiler) `java.lang` paketi içindeki `String`, int, boolean gibi sınıflara çevriliyor.

- objeler Java tarafından bilinmeyen objeler ise (örneğin bir `JSON` objesi) ; o zaman `JDK.nashorn.internal.scripts.JO4` Sınıfına cast ediliyor. Bu sınıftan `get("myJsValue")` gibi metotlarla sınıf objelerini çekebiliyoruz.

- `MyJavaClass.fun2(new Date());` gibi genel olarak kullanılabilecek sınıfları `JDK.nashorn.internal.objects.NativeDate` gibi sınıflara çeviriyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 JMapper vs ModelMapper vs Orika vs Dozer

mapping için alternatif kütüphanelerdir.

çoğu reflection kullanıyor ve runtime'da devreye giriyorlar.

## 📌 Mapstruct

`Mapstruct` compile sırasında, mapping'i temsil eden interface'lerin implementasyonlarını target dizini altında oluşturuyor.

`Mapstruct` tanımlarımızdaki logic hatalarını:

- Compile sırasında maven hata verir.
- `IDE`'lerdeki eklentiler sayesinde compile etmeden de hataları görebiliriz.

## 📌 Mapstruct Sping integration

Aşağıdaki annotation'ı elersek, `MapStruct` implementasyonun tepesine sadece `@Component` annotation'unu ekliyor. Böylece Spring otomatik bunu Bean olarak algılıyor ve istediğimiz yerden Autowired edebiliyoruz.

```java
import org.mapstruct.Mapper;

@Mapper(componentModel = "Spring")
public interface UserMapper {
  // mapping functions are here
}
```

## 📌 Mapstruct advantages

`MapStruct`'ın implementasyonları saf `Java` kodları ve IDE'den direk görebiliyoruz. Böylece:
- implementasyonları kopyalayıp projemize yapıştırırsak, `MapStruct`'ı tamamen (dependency olarak) projeden silebiliriz.
- Mapping'ler bazen çok karışık olabiliyor (map yapılan `DTO`'larda iç içe objeler, abstract sınıflar olabiliyor). `MapStruct`'ın runtime sırasında ne yapacağında şüphe duymadan, direk implementasyonu debug edebiliriz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Enum

```java
public enum Age {

  // Java class'ı ile bakıldığında tek fark, burada (enum syntax'ında)
  // instance'ları baştan constructor çağırarak init
  // ediyor olmamızdır.
  NEW(1, "new_age"),
  VERY_OLD(2, "very_old_age", true),
  OLD(3, "old_age"); // burası aşağıdaki constructor'ı çağırıyor.

  private Integer id;
  private boolean dead;
  private String code;

  // standart constructor
  Age(Integer id, String code) {
      this.id = id;
      this.code = code;
  }

  // standart constructor
  Age(Integer id, String code, boolean dead) {
      this.id = id;
      this.code = code;
      this.dead = dead;
  }
}
```

## 📌 methods of JVM

```java
Age[] allAge = Age.values();

Age age = Age.valueOf("VERY_OLD");
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Jeasy

`Java` için birçok birbirinden bağımsız kütüphaneler geliştiren projenin ismidir.

`j-easy`, `Github` ve `www.jeasy.org`'u kullanırlar.

## 📌 easy-random

`Jeasy`'nin bir kütüphanesidir.

`Easy-random`'un API'sine bir `Java` sınıfı veririz, `Easy-random` bu `Java` sınıfının içindeki değerleri random bilgilerle doldurulmuş (değerleri set edilmiş) şekilde döndürür.

`4.0` sürümü öncesi, projenin ismi `Random-beans`'tı. `4.0` ile `Easy-random` yapıldı. Aynı sürümde `io.github.beans` paket ismindeki kodlar, `org.jeasy` altına taşındı.

Bu kütüphane test yazımında büyük kolaylık sağlar.

örnek:

```java
EasyRandomParameters parameters = new EasyRandomParameters()
   .seed(123L)
   .randomizationDepth(3)
   .charset(forName("UTF-8"))
   .timeRange(nine, five)
   .dateRange(today, tomorrow)
   .collectionSizeRange(1, 10)
   .excludeField(named("age").and(ofType(Integer.class)).and(inClass(Person.class)));

EasyRandom easyRandom = new EasyRandom();

Person person = easyRandom.nextObject(Person.class);
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Apache Camel

`EIP`'nin implementasyonlarını sağlayan `Java` framework'ü. Kolaylaştırması açısından birçok farklı DSL desteklemektedir:

`DSL` for core `Java`:

```java
from ("file:/tmp").to("jms:aQueue");
```

`DSL` for `Spring`:

```xml
<route>
  <from uri="file:/tmp"/>
  <to uri="jms:aQueue"/>
</route>
```

`DSL` for `Scala`:

```scala
from "file:/tmp" -> "jms:aQueue"
```

`Camel` genel olarak kaynaktan bilgi okuma, kaynağa bilgi yazma, data transformation gibi işlemleri çok kolay (sadece `DSL` tanımlamaları ile) yapabilmemizi sağlayan bir framework'tür:

```java
from("direct:DistributeOrderDSL")
   .choice()
      .when(header("order").isEqualTo("oil"))
         .to("direct:oil")
      .when(header("order").isEqualTo("milk"))
         .to("direct:milk")
      .otherwise()
         .to("direct:d");
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 package.info file

`javadoc` oluşturduğumuz zaman bu dosya okunur ve bu dosyadaki bilgilere göre ilgili projede bazı metinler oluşturulur.

örnek dosya içeriği:

```java
package com.mycompany.app1;
```

örnek dosya içeriği 2:

```java
@NonNullApi
package com.mycompany.app2;

import org.springframework.lang.NonNullApi;
```

2inci dosyada `API` metodlarının hiçbir zaman `null` olmayacağı dökümanda yazacaktır. Eğer `null` olabilen bir durum varsa, `IDE`'nin bu konuda uyarı yapması opsiyoneldir. Ama bu bilginin dökümantasyonda kesinlikle yazması gerekir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Jmeter

web servislerinin yük testini yapmak için gerekli desktop UI tool'u.

kendi configurasyonlarını `jmx` uzantılı bir dosyaya kaydediyor.

## 📌 JavaMelody

`Java` uygulamalarını (uzak sunucuda olsa da) runtime sırasında monitör etmek için kullanılan yazılım. `Java` kütüphanesi olarak, incelenecek ve run edilecek projenin içine eklenmelidir. Çalışan bu kütüphane runtime sırasında bir port açıyor ve browser üzerinden istatistikleri sunuyor. Uygulamaya yapılan `HTTP` request'ler hakkında istatistikler, DB'ye yapılan `SQL`'ler hakkında birçok istatistik sunuyor.

## 📌 Jvisualvm

sadece `JDK` paketinin içinde bulunan (`bin` dizininde) performans monitör etme yazılımıdır. `GUI`'si vardır. (başka başlıkta anlatılıyor)

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Liferay

- `Liferay`, bir portal platformudur ve Ücretsiz (Community Edition) ve Ücretli (Enterprise Edition) olmak üzere iki şekilde sunulmaktadır.
- `Liferay`, `Java`'da yazılmıştır. `OSGI` kullanarak modüler bir yapıdadır.
- `Liferay`, bir `content management system (⟷ CMS ⟷ içerik yönetim sistemi`). örnek CMS'ler: `Joomla`, `Drupal`, `WordPress`.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
