# JAVA SPRING

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Proxy

Her spring-bean, ayrı bir proxy sınıfı tarafından wrap edilir.

```java
AccountService accountService = (AccountService) applicationContext.getBean(AccountService.class);
String accountServiceClassName = accountService.getClass().getName();
logger.info(accountServiceClassName);
```

The output will be:

```text
INFO : transaction.TransactionProxyTest - $Proxy13
```

Örneğin @Cacheable, @Transactional annotation'ları bu proxy bean'indeki kodlarda değişiklik yapar. Dolayısı ile; bir metod, aynı spring-bean'indeki bir metodu çağırdığında proxy üzerinden çağırmaz. Bu sebeple de bu annotation'lar tekrar devreye girmez.

## 📌 Technology for Proxy

Java'da 2 proxy yapabilmek için farklı teknoloji var:

- JDK-based dynamic proxy
  
JDK içinde gömülü gelir. Programatik Proxy yapabilmemizi sağlar:

```java
// Example:
// We proxy the java.util.Map class.

Map proxyInstance1 = (Map) Proxy.newProxyInstance(
      getClassLoader(), 
      new Class[] { Map.class }, 
      new CustomHandler());

proxyInstance1.put("key1", "value1");
```

The handler class for above example:

```java
public class CustomHandler implements InvocationHandler {

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
    
        print("Invoked method: {}", method.getName());

        return 1; // we return fake variable
    }
}
```

- Dynamic proxy based on Cglib

Projenin özel ismini "Code Generation Library" den türetmişlerdir.

Runtime'da programatik olarak JVM'in anladığı bytecode oluşturmaya yaramaktadır.

## 📌 Spring ne kullanıyor

JDK-based dynamic proxy eğer proxy yapılacak sınıfın interface'i varsa kullanılabiliyor. Bu sebeple spring kullanabildiği zaman JDK-based dynamic proxy kullanır.

Fakat eğer bir Spring Bean'in interface'i yok ise, o zaman Spring, Cglib ile proxy oluşturur. Çünkü Cglib için interface zorunluluğu yoktur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 dependency yönetimi

Maven-pom'da şu eklenirse:

```xml
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>Spring-boot-starter-parent</artifactId>
  <version>1.5.2.RELEASE</version>
</parent>
```

Spring'in 1.5.2 versiyonunun tüm dependency'leri artık tanımlanmış (eklenmemiş,sadece tanımlanmış!) olur. Artık aşağıdaki gibi bir kütüphane ekleyebiliriz:

```xml
<dependencies>
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>Spring-boot-starter-web</artifactId>
  </dependency>
</dependency>
```

Spring-boot-starter-web'in sürümünü belirtmeye gerek yok çünkü zaten parent'ta tanımlanmıştı.

Spring-boot-starter-web projesi default olarak embedded Tomcat kullanır. biz bunun yerine jetty kullanmak istersek aşağıdakini yazmamız yeterli olacaktır:

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>Spring-boot-starter-web</artifactId>
        <exclusions>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>Spring-boot-starter-tomcat</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>Spring-boot-starter-jetty</artifactId>
    </dependency>
</dependencies>
```

## 📌 Starters

Spring-boot-starter-XYZ formatında yazılırlar. XYZ uygulamamızın tipidir.

örneğin;

Spring-boot-starter-web; Spring-MVC ve Tomcat embedded getirir.

Spring-boot-starter-jersey; web'e bir alternatiftir. Spring-MVC yerine Apache Jersey kullanır.

1 adet starter, birçok spring modülünü bir arada barındırıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 org.springframework.web.filter.OncePerRequestFilter

Klasik filter ile aynı özellikleri içerir. Fakat bu filter, her request için en fazla 1 kere çalıştırılacağını garanti eder.

bir request'i başka bir servlet'e redirect edebiliriz. eğer redirect ettiğimiz servlet'in bir filter'ı var ise, o zaman o filter'larda devreye girecektir. işte böyle bir durumda; eğer OncePerRequestFilter'dan türemiş bir filter var ise, ve bu filter birden fazla servlet'in önünde ise, birden fazla çalışması durumu olmayacaktır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Spring-retry

modülün dependency'si eklendikten sonra:

```java
@Configuration
@EnableRetry
public class AppConfig { ... }
```

```java
@Service
public class MyService {

    @Retryable(
              // Spring will try same method if the method throws SQLException
              value = { SQLException.class },
              // Spring will try 2 times
              maxAttempts = 2,
              backoff = @Backoff(delay = 5000))
    void retryService(String sql) throws SQLException{
       // any code here
    }

    // Recover is optional. Spring will call this method if 2 attempt will not work above.
    @Recover
    void recover(SQLException e, String sql){
       // any code here
    }
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 java.util.Properties

Java Map sınıfından türetilmiştir. "Properties" formatlı dosyalardan okumak için tasarlandı.

## 📌 System Environments from Java

- environment değerleri okumak için: 

```java
System.getEnv();
```

- JVM argümanlarına geçilen parametreleri okuma:

```java
System.getProperties("myParam1") kullanılır.
```

- program argümanları okuma:

```java
main(string args[])
```

## 📌 Spring framework Externalized Configuration

Spring uygulaması başlarken onlarca sınıftan data okur. okuma öncelik sırasına göre aşağıdaki gibidir:

- Devtools global settings properties in the $HOME/.config/Spring-boot folder when devtools is active.

- @TestPropertySource annotations on your tests.

- properties attribute on your tests. Available on @SpringBootTest and the test annotations for testing a particular slice of your application.

- Command line arguments.

- Properties from SPRING_APPLICATION_JSON (inline JSON embedded in an environment variable or system property).

- ServletConfig init parameters.

- ServletContext init parameters.

- JNDI attributes from java:comp/env.

- Java System properties (System.getProperties()).

- OS environment variables.

- A RandomValuePropertySource that has properties only in random.*.

- Profile-specific application properties outside of your packaged jar (application-{profile}.properties and YAML variants).

- Profile-specific application properties packaged inside your jar (application-{profile}.properties and YAML variants).

- Application properties outside of your packaged jar (application.properties and YAML variants).

- Application properties packaged inside your jar (application.properties and YAML variants).

- @PropertySource annotations on your @Configuration classes. Please note that such property sources are not added to the Environment until the application context is being refreshed. This is too late to configure certain properties such as logging.*and Spring.main.* which are read before refresh begins.

- Default properties (specified by setting SpringApplication.setDefaultProperties).

kaynak: (source-id: 361) "2. Externalized Configuration" başlığı.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 SpringApplicationBuilder

Spring boot uygulamamız başlatılmadan önce ayarlar set etmemizi sağlar. kullanımı direk @SpringBootApplication atılan sınıfın içinde olmalıdır. örnek:

```java
@SpringBootApplication
public class WebApplication extends SpringBootServletInitializer {

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {

        application = new SpringApplicationBuilder();

        application.parent(new Object[]{"classpath:file1.xml", "classpath:file2.xml"})

       .profiles("abc")

       .properties("key1:test1", "key2:test2")

      .showBanner(false)

      .logStartupInfo(true)

       .headless(true)

      .application()

      .run();

        return application;
    }

    public static void main(String[] args) throws Exception {

        SpringApplication.run(WebApplication.class, args);
    }
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Spring MVC

Rest support'u bu modül içindedir. ModelAndView objesi de bu modül içindedir.

### 📌📌 ModelAndView

Rest Controller'ımızı normal yazıyor ve gelen request'e cevap olarak ModelAndView döndürüyoruz.

```java
@Controller
class RegistrationController {

  @RequestMapping(value = "/register", method = RequestMethod.GET)
  public ModelAndView showRegister() {
    ModelAndView mav = new ModelAndView("register");
    mav.addObject("user", new User("Ahmet"));
    return mav;
  }
}
```

ModelAndView içinde 'model' ve 'JSP dosya path'i bilgisini barındırır. Spring "ViewResolver" sınıfında belirlendiği ayarlarla JSP'yi bulmaya çalışır. Viewresolver config örneği;

```java
@Configuration
@EnableWebMvc
class WebConfig extends WebMvcConfigurerAdapter {

  @Bean
  public ViewResolver viewResolver() {
        InternalResourceViewResolver resolver = new InternalResourceViewResolver();
        resolver.setPrefix("/WEB-INF/views/");
        resolver.setSuffix(".jsp");
        resolver.setExposeContextBeansAsAttributes(true);
        return resolver;
  }

  @Override
  public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {

        configurer.enable();
  }
}
```

Viewresolver'da belirtilen View'ı (JSP'yi) model ile doldurup response olarak yolluyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Spring batch

temel olarak 3 işlevi yerine getirmek için geliştirilmiş altyapıdır:

- reading data
- processing data
- writing data

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 JavaEE dezavantajlar

- application server specific deployment descriptors

- lots of XML files even the most basic EJB - Güncel sürümlerde XML yerine annotation kullanılıyor. Spring bu durumu daha önceden çözdüğü için Spring projesi ortaya çıkmıştı. JavaEE'nin güncel sürümlerde bu eksiklik giderildi.

- sunucu ayağa kalkma süresi (çünkü tüm kütüphaneler sunucu içerisinde) - Güncel sürümlerde sunucular çok hızlandı ve Spring daha büyüdüğü için kısmen daha yavaşladı.

  not: Spring sadece servlet container'a ihtiyaç duyuyor. Oysa JavaEE bir spesifikasyon, app tarafından kullanılmasa bile, tüm kütüphaneler sunucu içerisinde.

- sunucuya bağımlılık

## 📌 Spring dezavantajlar

- app sever üzerinde çalıştırılırsa, app sever şirketinden ticari destek alınamaz.

- app server'sız çalıştırıldığında, monitoring tool'ları genelde JavaEE sunucularına uyumlu olduğundan, monitoring işlemlerinde uyumsuzluklar yaşanabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 stereotype

kelime anlamı: klişe, basmakalıp.

bu terim JavaEE ve Spring'de kullanılan bir terimdir. Java core'a ait değil.

Anlamından da çıktığı gibi standart (basmakalıp) olan tanımlara/pattern'leri temsilen bu terim kullanılır. Örneğin Spring ve JavaEE'deki kullanımına bakarsak, genel DI ve programlama dünyasında kullanılan standart sınıfları işaretlemek amaçlı atıldığını görürüz.

- JavaEE'de; bu bir annotation'dur: "javax.enterprise.inject.Stereotype". JavaEE tarafından yazılmış bir (custom) annotation'dur. bu annotation JavaEE'nin kendi yazdığı bazı annotation'ların içinde kullanılır. bu annotation'un bir işlevi yoktur. amaç sadece gruplandırmak (mark etmektir).

- Spring'de; bu bir annotation değildir. Aşağıdaki annotation'lar "org.springframework.stereotype" paketi altındadır. amaç (JavaEE'dekine benzer olarak) sadece gruplandırmaktır.

  - Component
  - Controller
  - Indexed
  - Repository
  - Service

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Spring framework

## 📌 SpringSource

Spring framework'ünü geliştiren firmanın eski ismi. Firma ismi daha sonra Spring oldu.

## 📌 Spring Tools (or STS or Spring Tools Suite)

Eclipse için Spring eklentisidir. Eclipse'e yüklü şekilde de direk indirilebiliyor.

## 📌 Spring modules

Spring birçok modül sunuyor. Bu modüller gruplandırılmış durumda ve her modül, farklı gruptan olsa da, başka bir modüle depend ediyor olabilir.

Aşağıdaki listede en üst seviyedeki başlıklar (örnek: "Data access & Integration", "Core Container") sadece gruplamak için yazılmıştır. bir artifact'ı temsil etmiyorlar. Oysa alt seviyedeki her madde birer artifact (jar) dosyasıdır ve Maven'a eklendiklerinde ihtiyacı olan tüm bağımlılıkları çeker.

- Data access & Integration
  - __spring-jdbc__
  - __spring-orm__
  - __spring-oxm__: Object/XML mapping implementations for JAXB and other library
  - __spring-tx__: transaction support
  - __spring-jms__: message queue support
- Web & Remoting
  - __spring-web__: Servlets
  - __spring-webMVC (spring web MVC) (old name: Web-Servlet)__: Spring-web & REST support'u (örnek @Controller) & ModelView desteği.
  - __spring-websocket__
  - __spring-webMVC-portlet (old name: __web-portlet__)
  - __spring-webflux__: reactive web support
- Core Container
  - __spring-core__: IoC özelliği bu modülün içinde tanımlıdır.
  - __spring-beans__: BeanFactory özelliği bu modülün içinde tanımlıdır.
  - __spring-context__: ApplicationContext özelliği bu modülün içinde tanımlıdır.
  - __spring-expression (or spEL)__: "Expression Language" modülüdür. string üzerinde kelime işleme özellikleri barındırmaktadır. aynı zamanda Bean property'lerine variable atayabilmek için özel syntax kullanımını sağlıyor.
- Aspect
  - __spring-aop__: method-interceptors (Spring's own Aspect library)
  - __spring-aspects__: AspectJ ile entegrasyonu sağlamaktadır (alternative of Spring-aop)
- Others
  - __spring-test__: JUnit + TestNG support.
  - __spring-instrument__: support for class loader implementations

## 📌 Maven artifact-ID isimlendirmesi

Spring yukarıdaki modüller hariç de birçok özellik sunuyor. Yukarıda yazan modüller temel seviyede olduklarından;

- group-ID: org.springframework
- artifact-ID: Spring-* (örnek Spring-webMVC)

şeklinde dağıtılıyorlar. yukarıda olup "core container" grubu içerisinde olanlar:

- group-ID: org.springframework
- artifact-ID: org.springframework.* (örnek org.springframework.context)

şeklinde dağıtılıyor. Yukarıda olmayan modüller (örnek cloud, boot, batch) ise şu şekilde dağıtılıyor:

- group-ID: org.springframework.* (örnek: org.springframework.batch)
- artifact-ID: org.springframework.* (örnek Spring-batch-core)

Spring her paketi paralelde OSGI uyumlu şekilde sunar. O paketlerin isimleri farklıdır. OSGI uyumlu paketler Enterprise Bundle Repository (EBR) üzerinden dağıtılmaktadır. Örnek:

- group-ID: org.springframework
- artifact-ID (Maven central): Spring-core
- artifact-ID (Spring EBR): org.springframework.core

kaynak: (source-id: 362) "Spring modules on the Maven repository başlığı". yazarlar: "Rob Harrop", "Clarence Ho", kitap adı: "Pro Spring 3"

## 📌 Spring version naming

(Spring cloud versiyonları başka başlıkta anlatılıyor.)

- GA (or General availability): stable versiyon.

- RC (or Release candidate): beta versiyon.

- M (or Milestone build): nightly versiyon.

Yukarıdaki sürüm terimleri genel olarak tüm yazılım sürüm politikalarınca kullanılmaktadır. sadece Spring'e özgü terimler değildir.

## 📌 Spring initializr

<https://start.Spring.io> sitesinin altyapısı olan projedir. bu site ile web arayüzden Spring modülleri seçebiliyoruz, paket ismi vs form girişi yaptıktan sonra bizim için Gradle/Maven, Java/Groovy gibi hazır boş Spring projesi oluşturmaktadır. bu projeyi anında download etmemizi sağlıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 application.properties

Spring başladığında default olarak proje içindeki application.properties dosyasını okur. farklı dosya verilmek istenirse;

> -Dspring.profiles.active=dev -Dspring.config.location=file:C:/application.yml

profillerde dosya içinde --- (üç çizgi) ile ayrışmalıdır:

```yml
Spring:
  profiles: dev

prop1: val1
prop2: val2

---

Spring:
  profiles: prod

prop1: val1
prop2: val2

```

3 çizgi YML formatının bir özelliğidir. kaynak: (source-id: 443) ("4.3.2. Document Boundary Markers" başlığı)

## 📌 bootstrap.yml vs application.yml

bootstrap dosyası mikroservisler ile karşımıza çıktı. config server'dan her servis config'lerini aldığı için, Spring boot uygulaması ayağa kalkmadan bazı property'leri tutması gerekli: örneğin; config server'ın portu ve IP bilgisi. mikroservis; config'den aldığı port numarası bilgisi ile kendisini hizmete açacaktır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Spring Boot

2 özellik sunar. diğer tüm özellikler buradan türer.

## 📌 Örnek 1

spring-boot-starter-data-jpa ekledik projemize. Bu durumda:

- Spring Boot Starters

  otomatik olarak hibernate JAR'ını ekler.

- @EnableAutoConfiguration

  DB IP ve port bilgisi girmezsek, H2 in-memory ayağa kaldırır ve ona bağlanır.

## 📌 Örnek 2

spring-boot-starter-web ekledik projemize. Bu durumda:

- Spring Boot Starters

  otomatik olarak tomcat JAR'ını ekler.

- @EnableAutoConfiguration

  HTTP Server port bilgisi girmezsek, 8081 portu ve context-path'i "/" olarak tomcat'i başlatır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 @SpringBootApplication

bu annotation şu 3'ünü içeriyor:

- @Configuration (başka başlıkta anlatılıyor)

- @EnableAutoConfiguration (başka başlıkta anlatılıyor)

- @ComponentScan (eğer parantez içinde parametre vermezsek, sadece bulunduğu sınıfın paketindeki ve alt paketlerdeki sınıfları Spring ayarlarına kabul ediyor.)

## 📌 AutoConfiguration

tüm oto configuration sınıfları tek bir jar ile dağıtılır: Spring-boot-autoconfigure-{version}.jar

Bu sınıflar condition içerir. eğer biz önceden config Bean'lerimizi oluşturmadıysak, condition'lar sayesinde AutoConfiguration Bean'leri load edilecektir.

örnek: org.springframework.boot.autoconfigure.JDBC.DataSourceAutoConfiguration sınıfında

```java
@Configuration

// SQL lib are in classpath
@ConditionalOnClass({ DataSource.class, EmbeddedDatabaseType.class })

// default properties will be bind, if datasource properties are written in application.properties
@EnableConfigurationProperties(DataSourceProperties.class)

// other depended configurations
@Import({ Registrar.class, DataSourcePoolMetadataProvidersConfiguration.class })

public class DataSourceAutoConfiguration
{
```

some other auto config classes:

- org.springframework.boot.autoconfigure.web.DispatcherServletAutoConfiguration
- org.springframework.boot.autoconfigure.orm.jpa.HibernateJpaAutoConfiguration
- org.springframework.boot.autoconfigure.data.jpa.JpaRepositoriesAutoConfiguration
- org.springframework.boot.autoconfigure.jackson.JacksonAutoConfiguration
- org.springframework.boot.autoconfigure.mongo.MongoAutoConfiguration

Eğer bean tanımı olmazsa aşağıdan NoSuchBeanDefinitionException hatası alırız:

```java
applicationContext.getBean(MongoTemplate.class);
```

Aslında MongoTemplate bir class, fakat @Component içermiyor. Bu sebeple onun class-filed'larını uygun şekilde set eden ve yeni instance'ını, oluşturan Config class'ları olmalı. İşte MongoAutoConfiguration bunu yapan bir class.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Aware

Türkçe kelime anamı: farkında, tetikte, haberdar

ServletContextAware gibi birçok aware arayüzü vardır. ServletContextAware örnek olarak alınırsa, bu sınıftan implemente ettiğimiz her Bean ServletContext'i inject etmesine gerek kalmaz. örnek:

```java
@RestController("/mycontroller")
public MyController implements ServletContextAware {

    private ServletContext context;

    @Override
    public void setServletContext(ServletContext context) {

        //setServletContext ServletContextAware arayüzünün bir metodu. "context" private değerine istediğimiz ismi verebiliriz.
        this.context = context;
    }
}
```

ServletContextAware hiç kullanmasaydık, aynı işlemi, setter-based injection kullanarak da yapabilirdik.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Webjars

Maven'a aşağıdaki eklendiğinde:

```xml
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>jquery</artifactId>
    <version>3.1.1</version>
</dependency>
```

HTML'lerimizden get isteği ile script'i çekebilmemizi sağlar:

```html
<script src="/webjars/jquery/3.1.1/jquery.min.js"></script>
```

Konfigürasyonu bu şekildedir:

```java
@Configuration
@EnableWebMvc
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry
          .addResourceHandler("/webjars/**")
          .addResourceLocations("/webjars/");
    }
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
