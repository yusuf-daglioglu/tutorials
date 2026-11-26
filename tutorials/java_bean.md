# JAVA BEAN

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 POJO (⟷ Plain Old Java Object)

Sadece `getter` ve `setter`'ları olan sınıflardır. Resmi bir tanım değildir. bu sebeple; bazı yerlerde `serializable`'dan da türeselerde, `POJO` diye adlandırılırlar.

`POJO` temelde en basit `Java` objesi temsil etmek için kullanılır.

## 📌 Java Bean (⟷ Bean)

`Bean` `Java` dünyası için ortak bir terimdir. her framework'ün `Bean` tanımı farklı olabilir:

- `Java SE` kendi `Bean`'lerine `JavaBean` ismini vermiştir.
- `Spring`, `Spring Bean` diye isimlendirir.
- `Java EE`, `EJB` framework'ü `EJB (⟷ enterprise javaBean)` olarak isimlendirir.

sadece `Bean` denildiğinde, o ortamda kullandığımız framework'ün `Bean` tanımına referans ediliyordur.

`Bean` tekrar kullanılabilir `Java` objesi değildir; `Bean` belli kurallara uyan sınıftır. çünkü inject ettiğimiz `Bean`'ler tekrar kullanılabilirdirler, fakat konfigürasyon amaçlı `Bean`'lerde olabilir. config amaçlı olanlar tekrar kullanılamazlar. örneğin aşağıdaki kodu Spring uygulaması için çalıştırırsak `Configuration` suffix'i ile biten sınıfların `Bean` olarak algılandığını göreceğiz. `Configuration` ile biten sınıfların içine baktığımızda tepelerinde `@Configuration` olduğunu görebiliriz. Bunlar tekrar inject edilmek için kullanılmaz. Fakat manuel olarak `Bean` olarak tanımladığımız her sınıf tekrar inject edilebilir olduğu için, `Bean` tanımı çoğu yerde bu şekilde yapılır. Fakat `Bean`'ler en temelde `DI` framework'ümüzün manage ettiği class'lar olarak düşünebiliriz.

```java
public static void main(String[] args) {
    ApplicationContext applicationContext = SpringApplication.run(MyApplication.class, args);

    String[] allBeanNames = applicationContext.getBeanDefinitionNames();
    for(String beanName : allBeanNames) {
        System.out.println("Bean: " + beanName);
    }
}
```

## 📌 JavaBean kuralları

- public ve argüman istemeyen bir constructor olmalı
- her property için `getter` `setter` olmalı
- `java.io.Serializable`'dan implemente etmeli

## 📌 Spring Bean kuralları

`Java EE`'deki gibi belli kuralları yoktur. kullanılacak modül ne şekilde istiyorsa o şekilde tanımlanmalıdır.

## 📌 DI (⟷ Dependency Injection)

Tüm programlama dillerinde kullanılan bir pattern'dir. sınıf içinde kullanacağımız nesneleri `new` operatörü ile yaratmayız. Bunun yerine `annotation` kullanırız (yada farklı daha kısa bir yöntem) böylece bizim için inject işlemi yapılır. aynı zamanda ek özelliklerde sunabilir: `singleton`, `session` bazlı instance'lar yaratabilmemizi gibi.

## 📌 IoC (⟷ Inversion of Control)

Yazılımcılar arasında `DI` ile karşılaştırılır. İkisi çok benzer fakat inject işleminin hayat döngüsünün farklı aşamalarıdır.

Biz direk olarak bir metodu çağırmayız. Onun yerine kullandığımız `IOC` framework'ü bizim için uygun implementasyonun (instance'ın) metodunu çağırır. Biz framework'e metot çağırması için istek yaparız, framework bizim için ilgili implementasyonu bulur ve ilgili metodu çağırır. Bu sebeple ismi bu şekilde verilmiştir.

`Inject etme` kısmı ise `DI` terimine tekabül eder. Oysa farklı implementasyonların framework tarafından bize temin edilmesi `IoC` kavramıdır.

`IoC` ve `DI` tüm programlama dillerinde kullanılan bir terimdir. Fakat özellikle `Spring`, `DI` işlemi için sürekli `IoC` terimini kullanır ve `DI` container'larına `IoC container` adını vermiştir.

`Martin Fowler` bu makalesinde; `DI` ve `IOC` terimlerinin aynı kapıya çıktığını belirtmektedir.

Fakat detaylara inildiğinde `DI`'sız da `IoC` olabileceği görülebilmektedir. `kaynak: https://stackoverflow.com/questions/6550700/inversion-of-control-vs-dependency-injection Garrett Hall'ın mesajı 3üncü paragraf, Tomasz Jaskuλa'nın mesajı ilk paragraf, Premraj'ın mesajında bulunan şekil.`

`Spring` uygulaması, `Weblogic` gibi bir `app server`'a deploy olduğunda, `servlet container` olarak `Weblogic`'in `servlet container`'larını kullanacaktır. `EJB` `annotation`'larını kullanmış isek; `Weblogic`'in `EJB container`'ını kullanacaktır. Fakat `Spring` `IoC` `annotation`'larını kullandıysak (örnek: `@Component`) o zaman kendisinin `IoC container`'ı kullanılacaktır.

## 📌 CDI (⟷ Context and Dependency Injection)

`@javax.inject.Inject` `annotation`'unu kullanır.

`Java`'nın `DI` spesifikasyonudur. `Java SE` de dahi kullanılabilir (ve `CDI container`'ı `runtime`'da olmak zorundadır). `kaynak: https://jakarta.ee/specifications/cdi/2.0/cdi-spec-2.0.html#part_2 title: "13. Bootstrapping a CDI container in Java SE", 1st paragraph.`

Sadece `Java SE`'de, `SeContainer` bir arayüzdür. Bu sebeple runtime'da bir implementasyona ihtiyaç duyar. örnek implementasyonlar:

- `JBoss Weld` (the reference implementation)

  kullanan server'lar:
  - `WildFly` (community based project)
  - `JBoss` (it is fork of `WildFly`. Developed by `Red Hat`.)
  - `GlassFish`
  - `IBM WebSphere Application Server` (`8.5.5` and above)
  - `Oracle WebLogic`
  - istenirse `Java SE` veya `Tomcat`, `Jetty` ile de kullanılabiliyor.

- `Apache OpenWebBeans (⟷ OWB)`

  kullanılan server'lar:
  - `Apache TomEE`
  - `Apache Geronimo`
  - `IBM WebSphere Application Server`
  - `SiwPas`
  - istenirse `Java SE` veya `Tomcat`, `Jetty` ile de kullanılabiliyor.

- `CanDI`

`kaynak: https://ducmanhphan.github.io/2020-01-14-How-to-use-CDI-in-Java-EE/ title: "What is Context and Dependency Injection", 6th paragraph.`

Aşağıdaki teknolojiler `CDI`'dan bağımsız teknolojilerdir. Fakat `CDI` ile entegre çalışabilirler. Entegrasyona bir örnek: bir `servlet`'e bir `EJB`'yi inject edebilmemizdir.

- `Servlets`
- `JSP`
- `JSF`
- Web services

## 📌 CDI version history

- CDI 1.0 in 2009 (Java EE 6)
- CDI 1.1 in 2013 (Java EE 7)
- CDI 1.2 in 2014
- CDI 2.0 in 2019 (Java EE 8)
- CDI 3.0 in 2020 (Java EE 9)

## 📌 EJB version history

- EJB 1.0 (1998-03-24)
- EJB 1.1, final release (1999-12-17)
- EJB 2.0, final release (2001-08-22)
- EJB 2.1, final release (2003-11-24)
- EJB 3.0, final release (2006-05-11)
- EJB 3.1, final release (2009-12-10)
- EJB 3.2, final release (2013-05-28)
- EJB 3.2.6, final release (2019-08-23)
- EJB 4.0, final release (2020-22-05)

## 📌 EJB (⟷ Enterprise JavaBean)

- `@javax.EJB` `annotation`'unu kullanır. `javax.EJB.Singleton` `annotation`'u `EJB` için kullanılır. oysa `javax.inject.Singleton` annotation'u `CDI` `Bean`'leri için kullanılır.

- `CDI` yapısı üzerine kurulu ve daha fazlasını sunan bir `DI` yöntemidir.

- `EJB`'nin açılımı genel olsa da sadece özel bir `DI` kütüphanesi ismidir.

- çeşitleri:

  - `session Bean` (böyle bir `Bean` yok. bu sadece grup ismi)

    - `Stateful`
    
      aynı session için sürekli sabit kalır. her session için bir instance oluşturulur.

    - `Stateless`
    
      durumsuzdur. istemciler istek yaptıkça yeni instance açılır, yada var olanlar paylaştırılır.

  - `Singleton`
  
    sürekli 1 instance kullanılır.

  - `entity Bean`
  
    DB modellemesi için yaratılan `Bean`'ler

  - Message Driven `Bean`
  
    `JMS` için kullanılan `Bean`

## 📌 managed Bean

`JSF`'in hayat döngüsünde kullanılan `Bean`'lere verilen isimdir. `JSF`, `XHTML`'lerden çağrılan `Bean`'lere bu isimleri vermektedir. Bunlara aynı zamanda `Baked Bean` de denir.

## 📌 Bean sınıfı tanımlama (Spring için)

Sınıfın `Bean` olabilmesi için ya `XML`, yada `annotation` ile tepesinde tanımlama yapılması gerekmektedir. Örnek:

- `@Bean` ile herhangi bir sınıfı, ilgili sınıfa `@Component` koymadan, `Bean` ilan edebiliriz. örnek:

```java
@Configuration
// @Configuration annotation'ı kendi içinde @Component barındırır.
// çünkü component-scan'de algılanması gerekmektedir.
// Fakat aynı springBean gibi inject edilip kullanılamazlar.
public class AppConfig {

    // Foo.class and Bar.class have not @Component annotation.

    @Bean
    public Foo foo() {
        return new Foo(bar());
    }

    @Bean
    public Bar bar() {
        return new Bar();
    }
}
```

## 📌 EJB container

uygulamadaki tüm `EJB`'lerin tutulduğu yerdir. birileri bu `EJB`'leri çağırdığında, `EJB container` üzerinden paylaşılmaktadır. `Spring` projelerinde `EJB container` yerine `IoC container` vardır.

## 📌 DI Container

`EJB container`'ın programlama dünyasında kullanılan genel ismidir. Bu container'lar, kendi içlerinde tüm uygulamada bulunan `dependency` listesinin haritasını tutar. böylece bir `dependency` kullanmak istediğimizi de, kullanacağımız `dependency`'nin `dependency`'lerini de bulur ve ona inject eder.

## 📌 @Qualifier

`javax.inject` paketi içerisinde olan bu `annotation` ile inject edilen sınıfın ismi belirtilebilir. böylece aynı isimde olan `Bean` var ise; doğru olanı inject etmiş oluruz. Bu `annotation` inject edilecek variable'ın üstünde `resource`, `inject`, `Autowired` ile birlikte kullanılmalıdır. örnek:

```java
@Autowired
@Qualifier("iceCream")
void setDessert(Dessert dessert){
    this.dessert = dessert;
}
```

Yukarıdaki kod örneğinde; `Dessert` olarak `iceCream` implementasyonu set edilecektir.

Eğer bu `@Qualifier` sınıfın tepesine yazılırsa:

```java
@Qualifier("iceCream")
class MyDessert {
    // any code here
}
```

artık `MyDessert` yerine `iceCream` kullanılır.

## 📌 OSGI (⟷ Open Services Gateway initiative)

`OSGI` `Java` için bir standarttır. belli kalıplarda yazılan `Java` uygulamaları `OSGI` standartlarına uyuyorsa, modüler bir şekilde çalışabilirler. `OSGI` için `jar` paketlerinin içine bazı ek dosyalar eklemek gerekir.

`OSGI`'nin implementasyonları vardır. `Equinox`, `Apache Felix` gibi. `OSGI` standartlarındaki `JAR`'ları kullanabilmek için `OSGI` implementasyonunun (kütüphanesinin) `runtime`'a çalışması gerekir.

Runtime sırasında hiç restart etmeden bir `OGSI`-uyumlu `jar`'ı çağırabiliriz. çünkü bu `jar` içerisinde `start` ve `stop` gibi implemente edilmiş metotlar vardır. aynı zamanda bu çağırdığımız `JAR`'ın bağlı olduğu diğer `dependency`'ler kendi `manifest` dosyasında vardır. aynı zamanda kendi hakkında detayları da `manifest` içerisinde bulabiliriz (sürüm no gibi).

`OSGI` da her `OSGI` uyumlu `jar` birer `bundle`'dır. örneğin `Eclipse` `IDE` kendi altyapısında `Equinox` kullanır. `IDE`'de her eklenti birer `bundle`'dir. fakat `Eclipse` son kullanıcıya uygun olsun diye bunlara eklenti demek zorundadır.

## 📌 OSGI vs CDI

`CDI`, `OSGI` kadar esneklik sağlamaz. Aslında bu iki teknolojiyi karşılaştırmak yanlış. İkisi apayrı mantıkta ve amaçta çalışmaktadır. `OSGI` modüler uygulama geliştirmek için yaratılmıştır. `CDI` ise sınıfların inject edilebilmesini sağlamaktadır.

- `OSGI` `Java` `runtime` sırasında yeni bir paketin `Java` programına entegre edilebilmesini sağlar. restart'a ihtiyaç duymaz. oysa `CDI`'de, sonradan inject edilecek bir sınıfın `classpath`'te `runtime` sırasında her zaman tanımlı olması gerekir.

- `OSGI`'da bağımlılıklar (paket ID + class name olarak verilir). oysa `CDI`'de qualifier veya class name gibi bilgiler ile injection yapılır.

- `OSGI`'de bağımlılıklar versiyon bazında seçilebilir ve birden fazla versiyon aynı anda çalıştırılabilir. oysa `CDI`'da bunların hiçbiri yok, çünkü versiyon mantığı yok.
