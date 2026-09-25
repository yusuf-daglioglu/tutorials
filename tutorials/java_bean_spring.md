# JAVA BEAN SPRING

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Spring Bean

### 📌📌 Spring IoC container

`DI` için gereklidir. 2 çeşittir:

- `Bean Factory`
- `ApplicationContext` (daha gelişmiş özellikler içerir)

#### 📌📌📌 ApplicationContext

```java
ApplicationContext appContext = new ClassPathXmlApplicationContext("Beans.xml");

HelloWorld instance1 = (HelloWorld) appContext.getBean("helloWorld");
```

#### 📌📌📌 ClassPathXmlApplicationContext

`classpath` içindeki dizini istiyor.

#### 📌📌📌 FileSystemXmlApplicationContext

full path istiyor.

#### 📌📌📌 AnnotationConfigApplicationContext

parametre olarak bir class istiyor. bu sınıfın içinde konfigürasyonlar oluyor.

`Spring Bean`'i dışında kalan sınıflar (`Bean` olmayan sınıflar, normal sınıflar) içerisinde `Bean` inject eden `annotation`'lar kullanılamaz. eğer inject etmeye kalkarsa, hata alınmasa bile `null` gelir değerler. örneğin; `@autowired` ile normal bir sınıfta bir `Bean` sınıfını inject ettik. bu sınıf `runtime`'da `null` olur. `Bean`'ler ancak `Bean`'ler tarafından çağrılabilir. yada yukarıdaki gibi `context.getBean()` metodu ile çağrılabilir.

`Runtime` sırasında aynı `Java` uygulamasında birden fazla `ApplicationContext` olabilir. dolayısı ile `ApplicationContext.getBean()` metodu ile hangi `ApplicationContext`'i aldığımıza dikkat etmeliyiz. fakat best practice'lere göre `ApplicationContext`'e bu şekilde erişmek pek önerilmez.

### 📌📌 BeanFactory

- `ApplicationContext`'e alternatif bir kütüphanedir.
- `Spring-beans` modülü içinde gelir.
- `ApplicationContext`, `BeanFactory`'den daha gelişmiştir ve `BeanFactory`'nin tüm özelliklerini zaten içerir.
- `BeanFactory` daha sadedir. bu sebeple mobil gibi uygulamalarda tercih edilebilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

### 📌📌 Bean Scopes

### 📌📌 singleton

tüm `applicationContext`'te tek bir instance olmasını sağlıyor. bu `Spring Bean`'leri için default değerdir.

### 📌📌 prototype

each injection initialize new object

### 📌📌 request

each HTTP request initialize new object. only exist for Web applications.

### 📌📌 session

each HTTP session initialize new object. only exist for Web applications.

### 📌📌 global-session

`portlet`'ler için `session` bazlı `Bean` oluşmasını sağlıyor. bu `Bean` normal `servlet`'ler için kullanılırsa, `session` `Bean` ile aynı mantıkta çalışacaktır.

### 📌📌 WebSocket

`WebSocket`'lerin bağlantısı açık kaldıkça aynı `Bean` sürekli açık kalır

### 📌📌 application

`ServletContext` hayat döngüsü boyunca var olan singleton bir obje. bir `applicationContext` içerisinde birden fazla `ServletContext` olabilir.

Notlar:

- Yukarıdaki hazır scope'lar dışında custom scope'ta yaratılabilir.

- `Bean`'lerin ne scope olursa olsun initialize ve destroy sırasında çağrılacak metotları olabilir. bu metotlar `beans.xml` dosyasında belirtilmek zorundadır.

- `Singleton` `Bean` içerisine `prototype` `Bean` inject edilebilir. `prototype` olan `Bean`, sadece `singleton` olan `Bean` init olduğunda, `singleton` olan `Bean` içerisine inject edilir. artık bir daha bu instance kesinlikle değiştirilmez.

- Farklı scope'larda `Bean`'leri birbirine inject etmek karmaşıklığa yol açabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Bean hayat döngüsü

sırası ile:

- `Bean`'e property'leri set edilir

- `BeanNameAware.setBeanName(String name)`

- `BeanFactoryAware.setBeanFactory(BeanFactory beanFactory)`

- `ApplicationContextAware.setApplicationContext`

- `@PostConstruct` yada `BeanPostProcessor.postProcessBeforeInitialization()`

- `InitializingBean.afterPropertiesSet()`

- a user defined `Bean` init method

- `BeanPostProcessor.postProcessAfterInitialization()`

- `Bean` hazır durumda

- `DisposableBean.destroy()`

- a user defined `bean` destroy method

farklı bir örnek üzerinden gidelim:

`X` `Bean`'imizin hayat döngüsüne bir metot ekleyelim: `X` `Bean`, `BeanFactoryAware`'i implemente eder ve `setBeanFactory` metodunu override ederse, `setBeanFactory` otomatik olarak çalışır. aksi durumda bu event atlanır.

`BeanFactoryAware`'i ilgili `Bean` (`X`) implemente etmeyip aşağıdaki gibi kullanabiliriz. fakat böyle durumlarda aşağıdaki metot tüm `Bean`'ler için çalıştırılmaktadır.

```java
public class MyBeanFactory implements BeanFactoryAware {

    private BeanFactory beanFactory;

    @Override
    public void setBeanFactory(BeanFactory beanFactory) throws BeansException {
        this.beanFactory = beanFactory;
    }

    public void getMyBeanName() {
        MyBeanName myBeanName = beanFactory.getBean(MyBeanName.class);
        System.out.println(beanFactory.isSingleton("myCustomBeanName"));
    }
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Bean property

`XML` tanımı yapılır:

```xml
<Bean id="person" class="com.ornek.Bean.Person">
      <property name="message1" value="Ahmet" />
</Bean>
```

`Java` tarafında artık `Bean` hazırdır. `message1` değeri otomatik `Java EE` veya `Spring` tarafından doldurulacaktır:

```java
public class Person {
   private String message1;
}
```

Yukarıdaki `XML` tanımını yapmasaydık, `Java` kodumuzun içine `annotation` atmak zorundaydık.

### 📌📌 Bean init method

benzer şekilde `destroy` metodu da belirlenebiliyor

```xml
<Bean id="myBean" class="myClass" init-method="myInitMethod"/>
```

### 📌📌 @PostConstruct and @PreDestroy Annotations

`Java EE` standardıdır. bu sebeple `annotation`'lar `javax.annotation.*` içerisindedir. `Bean`'lerde initialize ve destroy sırasında çalışacak metotların üzerine konulur. `beans.xml`'de belirtilmemişse, bu `annotation`'lar kullanılabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 profiles

uygulama içinde arayüzler `@autowired` ederiz. `runtime` sırasında hangi implementasyonun instance olarak ayarlanacağına `Spring` profiller aracılığı ile karar verir.

örnek:

`@autowired` ile `WeatherService` ekledik.

bir implementasyonumuz da bu olsun:

```java
@Service
@Profile({"sunny", "default"})
public class SunnyDayService implements WeatherService {
   ...
}
```

`application.properties`'te;

```properties
Spring.profiles.active: sunny
```

satırı olmalıdır. `default` isimli profil özeldir. `default` aktif hale gelir.

Eğer bir profilde bir component'in hiç scan edilmemesini istiyorsak ünlem işareti kullanırız. örnek:

```java
@Profile({"!sunny","default"})
```

### 📌📌 default implementasyon

`Spring `veya `Java EE`'de bir arayüzü bir sınıfın içinde inject ettiğimiz zaman, framework o nesneye path'te olan bir implementasyonunu atar. path'te en az bir implementasyon yok ise hata fırlatır.

Aşağıdaki örnekte; request o anda isteği yapan request bilgilerini içerir.

```java
@Autowired
private HttpServletRequest request;
```

Bu durum yukarıda bahsi edilen default implementasyonla bağlantılı değildir. Burada instance'ı ele alıyoruz, yukarıda bahsi edilen durum hangi implementasyonun (yani sınıfın) kullanılacağıdır.

Eğer birden fazla implementasyon classpath'te ise, o zaman önceliği vereceğimiz sınıfın tepesine @Primary yazmamız gerekmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 conditional beans

`MyConditions` içerisindeki `matches` metodunun dönüşüne göre `Bean` tanımlanıyor yada tanımlanmıyor.

```java
@Bean
@conditional(MyConditions.class)
MyBean myBean(){
  return new MyBean();
}
```

```java
class MyConditions implements Condition {
  boolean matches( ConditionContext cc, AnnotatedTypeMetadata meta){
      // "ConditionContext" has info about the environment: class loader, beanFactory...
      // "AnnotatedTypeMetadata" has info about the Bean which will be included or not
      if( context.getEnvironment("my_keyword") == null ){
           return false;
      }
      return true;
  }
}

```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 @Configuration

```java
@Configuration
public class MyClassConfig {

    @Bean
    public TransferService transferService() {
        return new TransferServiceImpl();
    }
}
```

artık `TransferService` `Bean`'i uygulamamızın herhangi bir yerinden çağrılmak istendiğinde (context'ten inject edilmek istendiğinde), "new TransferServiceImpl()" instance'ı çağrılacaktır.

Configuration'ın üstüne @Profile("development") gibi profiller tanımlanabilir.

@Bean annotation'u projenin herhangi bir dizininde (componentScan içinde olmak şartıyla) kullanılabilir. ve o metodun dönüş değeri Bean olarak tanımlanacaktır. fakat @configuration annotation'u ile bu Bean'leri gruplamış oluyoruz. ve bazı profillerde istediğimiz Bean'ler aktif olurken, diğer profillerde  Bean'leri aktif edebiliriz. örnek:

```java
public class MainApp {

   public static void main(String[] args) {

      ApplicationContext ctx = new AnnotationConfigApplicationContext(MyClassConfig.class);

      TransferService ts = ctx.getBean(TransferService.class);

      ts.doSomething();
   }
}
```

## 📌 org.springframework.context.annotation.Import sınıfı

`annotation` tanımımızın üstüne `@Import(MyClass.class)` yazabiliriz. bu şekilde o `annotation`'u barındıran sınıfta artık `MyClass`'ta olan her şey tanımlı olacaktır. `MyClass`'ın `@Configuration` barındıran bir sınıf olması şarttır.

`Import` sayesinde configuration'ları kolayca sınıflara gruplandırıp tek bir sınıfta birleştirebiliriz. `@Import`, sadece `@Configuration` ile birlikte çalışabilmek için tasarlanmıştır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
