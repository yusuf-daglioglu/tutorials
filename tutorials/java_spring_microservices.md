# JAVA SPRING MICROSERVICES

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 discovery server

istenilen sayıda micro-servisler instance'ı, merkezi bir discovery server'a kendilerini register ederler. son kullanıcı (yazılım geliştirici) buradan tüm servisleri monitör eder.

discovery'lar: 

- __Eureka server__
- __Consul__
- __Zookeeper__
- __Etcd__

`Zookeeper` ve `etcd` temelde key-value bir storage manager'dır. yazılımcıya config'leri tutacak şekilde optimize edilmiştirler. Aslında, diğer NoSQL ve RDBMS'lere göre, `CAP`  teoremindeki seçimleri/öncelikleri farklıdır. `Zookeeper`, `CAP` teoreminde `P` ve `C`'yi mutlaka sağlamaktadır. Fakat availability'den tölerans gösterir (feragat eder). Bu sebeple genelde "distributed coordination" için ve config'leri tutmak için kullanılırlar:

- açık olan microservice listesini tutarlar
- Kafka gibi sistemlerde topic listesini ve daha birçok bilgiyi tutarlar (Kafka'nın Zookeeper'da neleri tuttuğu Kafka'nın alt-başlığında anlatılıyor)

Çünkü; sistemdeki config'lerin veya servis listesini herkesle paylaşan Zookeeper'ın geç cevap vermesine tölerans gösterilebilir, fakat eski bilgi paylaşması ve bu bilgilerin merkezi yerde tutulmamasına tölerans gösterilemez.  

## 📌 Eureka node types

### 📌📌 Eureka server

diğer instance'ların location'larını tutan ve listesini tutan node'dur.

### 📌📌 Eureka service vs Eureka instance

örneğin; "order-servise" isimli Eureka servisine ait, 10 adet Eureka instance'ı olabilir.

### 📌📌 Eureka client

any application can discover services. kendisini servis(instance) olarak tanıtmak zorunda değildir.

## 📌 config server

tüm diğer servislerin bir micro-servisten config'leri (property'leri) okuması gerekir. bu micro-sunucuya "config server" adı verilir.

## 📌 Zuul

bir microservice olarak çalışır.

@EnableZuulProxy, @EnableZuulServer'a göre daha fazla özelliği aktif eder.

api gateway özelliklerini programatik olarak kullanabilmemizi sağlar.

bu servise register olan sunucuların API'lerini Zuul dışarıya kendisi üzerinden açmaktadır. örnek: user-service'imizin /url1 path'li bir metodu olsun. money-service'imizinde /url1 path'li bir metodu olsun. bunları dışarıdan şu şekilde çağrılmalarını sağlamaktadır. bu iki servis ayrı porttan çalışmaktadır. bu iki API'yi de tek bir porttan açmak istediğimizde Zuul proxy üzerinden açarız.

```yaml
Zuul:
  routes:
    service1:
      path: /user-service/**
      url: http://localhost:8081/user-service/
    service2:
      path: /money-service/**
      url: http://localhost:8082/money-service/
```

yukarıdaki config şunu diyor: path'e gelen istekler (örnek: http://zuul_proxy_ip:zuul_port/zuul_proxy_service/money-service/url1 ) URL'ye yönlendirilsin ( örnek: <http://localhost:8082/money_service/url1> )

Zuul-proxy'nin filter özelliği mevcut. istek önce ZuulServlet'e geliyor. Zuul servlet sırası ile bizim filtrelerimizi işletiyor. filtrelerimiz 3 çeşit:

- prefilter: istek yönlendirilmeden önce

- routing filter: istek burada henüz yönlendirilmedi, fakat httpRequest objesi burada hazırlanabiliyor ve request burada yapılıyor. request'in dönüşü de ilk bu sınıfta karşılanabiliyor.

- postfilter: istek geldikten sonra çalıştırılıyor

- error filter: herhangi bir aşamada exception alındığında bu filter devreye giriyor

## 📌 Circuit Braker (⟷ devre kesici)

bu kütüphane sayesinde hata fırlatan veya cevap vermeyen (timeout'a düşen) metotlar için backup metotlar hazırlanabilir. spring-boot ile de entegrasyonu olan, ama Spring'siz core Java ile de çalışabilen Hystrix buna bir örnek araçtır.

Hystrix örnek kod:

aynı sınıf içinde aşağıdaki iki metot olmalıdır. eğer fallbackMethod yok ise; Hystrix exception fırlatır. zaten bu exception'u fırlatacaktı diye düşünebiliriz. fakat fallback yapması ve timeout'ları da desteklemesi Hystrix'in avantajıdır.

```java
@HystrixCommand(fallbackMethod = "noBooks", ignoreExceptions = { NullPointerException.class })
public String fetchBooks(int a) {
    return restTemplate.get(uri, String.class);
}

public String noBooks(int a) { //aynı değeri buraya atıyor
    return "There is no book available now";
}
```

yukarıdaki fetchBooks metodu için timeout set etmek isteyelim. timeout ayarını bu projenin `application.yaml`'sine eklememiz yeterlidir. bu projeyi restart ettiğimizde ayar devreye girmiş olacaktır.

## 📌 Ribbon

client side load balancer görevi görüyor. Ribbon REST-template yada Feign-client gibi hazır Spring modülleri ile dışarıdaki sunuculara istek yaptığımız zaman, ilgili sunuculara düzenli /testUrl gibi bir path üzerinden sürekli istek yapar. bu istek eğer 200 dönmezse o sunucuyu down olarak varsayar ve bizi diğer sunuculara yönlendirir. /testUrl path'i, timeout, gibi parametreler override/configure edilebilir.

Ribbon kütüphanesi Ribbon işlemi yapılacak client'a eklenmelidir. örnek bir config (`YAML`) içeriğimiz (client uygulamada olmalı bu config'ler):

```yaml
account-service: # this is service name
  Ribbon:
    Eureka:
      enabled: false
    listOfServers: localhost:8090,localhost:9092,localhost:9999
    ServerListRefreshInterval: 15000
```

Yukarıdaki config'de Eureka disabled edildiği için listOfServers'ı elle verdik. eğer Eureka enabled olsaydı, listOfServers otomatik algılanıyor olacaktı.

## 📌 Feign Client

feign Türkçe kelime anlamı: uydurmak

REST istekleri için kolayca client oluşturmamız sağlayan modül. REST server'daki kodların aynısını (body kısmı hariç) Feign-client annotation'lu arayüze kopyalanır. daha sonra REST istek yapılması istenen kod satırında şu şekilde kullanmak yeterli olacak:

```java
MyFeignClientInterface.doPostRequest("Hello");
```

örnek kod:

```java
@FeignClient(name = "account-service")
public interface CHAccountServiceClient {

  @RequestMapping(value = "/accounts/v1/transactions", method = RequestMethod.POST)
  public ServiceResponse<String> transaction(@RequestBody RequestDTO requestDTO);
}
```

## 📌 artifact ID

Feign client ilk sürümünden itibaren https://github.com/OpenFeign/feign repo'sunda geliştiriliyor.

__com.netflix.feign:feign-core__ eski paket ismiydi.

`9.0.0` sürümü ile ismi __io.github.openfeign:feign-core__ oldu.

__org.springframework.cloud:Spring-cloud-starter-openfeign__ jar'ı Spring cloud'un, Openfeign'i depend ederek üzerine ek geliştirmeler ile sunduğu pakettir. kaynak: https://cloud.spring.io/spring-cloud-netflix/multi/multi_spring-cloud-feign.html Bu kaynakta bu yazmaktadır: "Feign is a declarative web service client. It makes writing web service clients easier. To use Feign create an interface and annotate it. It has pluggable annotation support including Feign annotations and JAX-RS annotations. Feign also supports pluggable encoders and decoders. Spring Cloud adds support for `spring-mvc` annotations and for using the same HttpMessageConverters used by default in Spring Web. Spring Cloud integrates Ribbon and Eureka to provide a load balanced HTTP client when using Feign."

Zira __org.springframework.cloud:Spring-cloud-starter-openfeign__ source code içerisinde de Openfeign dependency'si görülmektedir. kaynak: https://raw.githubusercontent.com/spring-cloud/spring-cloud-openfeign/master/spring-cloud-openfeign-core/pom.xml

## 📌 Ribbon entegrasyonu

Feign client'ın Ribbon ile entegrasyonu vardır.

Eğer `@FeignClient(name = "account-service")` yazılırsa, account-service'in adresi Ribbon'dan getirilecektir.

eğer Ribbon kullanılması istenmiyorsa, direk URL verilmelidir:  `@FeignClient(name = "<http://mysite.com/rest/>")`

## 📌 test

Yukarıdaki, CHAccountServiceClient Feign client bir arayüzdür. direk olarak autowired edilir ve implementasyonunu yaratmayız. bu sebeple test, prod, local gibi ortamlar (Spring profiller) için farklı Feign Inject edemeyeceğiz. çünkü profiller runtime sırasında implementasyon arar. Bunun için şöyle bir yol izleyebiliriz:

- Test kodlarımızda farklı URL'li Feign client yaparız. Mockito ile asıl Feign client'ımızdaki her metodu ikinci yazdığımız bu feignClient'a yönlendiririz.

- FeignFactory diye bir interface yaparız. 2 adet te bunun implementasyonu: FeignFactoryProdImpl ve FeignFactoryDevImpl.

```java
@Component

@Profile("prod")

class FeignFactoryProdImpl implements FeignFactory{

  @Autowired
  ProdFeign Feign;
}
```

Benzeri FeignFactoryDevImpl içinde olacak. Artık asıl Feignclient'ı kullanacağız sınıfa @autowired ile FeignFactory'yi inject edeceğiz. Prod profilindeyken runtime'de ProdFeign gelecektir.

Feign client builder'ı ile hem encoder'larımızı değiştirebiliriz hem de diğer client'ları wrap ettirip kullanabiliriz. örnek:

```java
GsonCodec codec = new GsonCodec();

GitHub github = Feign.builder()
                      .encoder(new GsonEncoder()) // buraya JacksonEncoder da gelebilirdi
                      .decoder(new GsonDecoder())
                      .target(GitHub.class, "https://API.github.com");
```

```java
GitHub github = Feign.builder()
                     .client(new OkHttpClient())
                     .target(GitHub.class, "https://API.github.com");
```

## 📌 Spring Cloud Sleuth

`Sleuth` Türkçe kelime anlamı: dedektif.

`Sleuth`, `Spring`'in `cloud`'un bir alt projesidir. `Sleuth`, `Zipkin` gibi takip sistemlerini kapsar.

## 📌 Spring Cloud Netflix

`Spring`'in `cloud`'un bir alt projesidir. şunları içerir:

- `Service Discovery` (`Eureka`)
- `Circuit Breaker` (`Hystrix`)
- `Intelligent Routing` (`Zuul`)
- `Client Side Load Balancing` (`Ribbon`)

## 📌 OpenTracing vs OpenCensus vs OpenTelemetry (⟷ OTel)

`OpenTracing` ve `OpenCensus` projeleri, `OpenTelemetry` isimli projede birleşti.

`OpenTelemetry` projesi tüm bunları içeriyor:

- `span` ve `trace` ve `log` ve `metric` bilgilerinin monitoring tool'a nasıl yollanacağı (network protocol).
- `trace context propagation`: her servis sadece `OpenTelemetry` server'ına değil, aynı zamanda iş akışını devrettiği servise de mesaj atıyor. böylece `trace-id`'yi, `parent-id`'yi işlemi devam ettirecek servis bilsin. buradaki mesajın formatı.

  Piyasadaki diğer `trace context propagation` alternatifleri:

  - `B3 propagation`
  
    `HTTP` `header`'da `X-B3-` ile başlayan `string`'lerdir.

    `Zipkin`, `Spring Cloud Sleuth`, bunu kullanır.
  
  - `W3C Trace Context`

    `OpenTelemetry` bunu kullanır.

  - `jaeger propagation`

    `jaeger` isimli monitoring tool'u tarafından kullanılıyor.

Dolayısı ile `OpenTelemetry` destekleyen herhangi bir monitoring tool'lar arası geçiş için hiçbir ayar veya kod değişikliğine ihtiyacımız olmuyor.

## 📌 Zipkin

kodun nerede olduğunu `trace` (takip) edebilmemizi sağlar:
- custom bir event olduğunda (bir kod bloğuna girdiğimizde)
- servis başka bir servise HTTP isteği attığında
- DB'ye query atıldığında

Tüm bu bilgileri her servisimiz `Zipkin`'e yollar. `Zipkin` bize UI üzerinden bunları gösterir. `Zipkin` sadece `Web UI` değil, aynı zamanda programatik okuyabilmemiz için `Zipkin Query Service` isimli `API` sunar.

Her servisimizde `Zipkin` ile haberleşmesi için bir kütüphane olmalı. bu kütüphanelere `instrumentation library` deniliyor. her microservice'e `reporter` veya `instrumented app` deniliyor.

 sadece microservice'ler değil, aynı zamanda isteği yapan client'ta bu sürecin içine dahil edilebilir. client tarafta isteği yolladığı anda bu bilgiyi `Zipkin` sunucusuna bildirebilir.

`Zipkin`, her microservice'ten bilgi alabilmek için onlara bir API sunar. Bu API'ye `collector` denir.

## 📌 Sleuth dependencies

`Spring cloud`'un `Sleuth` için dependency'leri:

- `Spring-cloud-starter-zipkin` -> `BOM`
- `Spring-cloud-sleuth-stream` -> deprecated oldu
- `Spring-cloud-sleuth-zipkin` -> `Zipkin` server'a bilgi yollayan kütüphane.
- `Spring-cloud-sleuth` -> `Zipkin`'e bilgi yollamaz. bir sonraki service bilgileri taşır. `kaynak: https://github.com/spring-cloud/spring-cloud-sleuth#only-sleuth-log-correlation`

## 📌 span

`Türkçe` kelime anlamı: karış.

bir iş akışı süreci birçok iş akışı parçacığından oluşabilir. bu parçacıkların ne olacağı tamamen bize kalmış.

Spring kütüphaneleri default'ta, her microservice arasındaki geçişler birer `span` olarak kabul eder.

## 📌 trace

trace-ID tüm istek için baştan sona tekildir.

### 📌 parent-ID

Buna `parent-span-id`'de denildiği olur.

isteğin hangi `span-ID`'den sonra yapıldığı bilgisidir.

## 📌 Spring cloud version history

`Spring cloud`, `Spring`'in bir alt projesidir ve altında birçok alt proje barındırır. Bu aile altında olan projeler daha çok distributed sistemlerde kullanılan dependency'leri barındırır. örnekler: `service discovery`, `circuit braker` gibi.

her alt projenin sürümü aslında diğerlerinden bağımsızdır. bu sebeple uygun sürümleri bir araya getirerek bir cloud projesi sürümü oluşturulur. bu projenin sürümü, numara ile gitmez. isim ile gider. isim sırası alfabetiktir. isimler Londra'da bulunan metro istasyonlarının isimleridir.

örnek: `1.` sürüm `Angel`, `2` `Brixton` şeklinde gider.

Yani "Angel" cloud sub-projeleri için bir sürüm kümesidir.

örneğin Feign-client 4üncü sürümünde ve Eureka 3'üncü sürümde olabilir (sayıları uydurdum).

Eğer "Angel" sürümüne kritik ve ufak bir güncelleme gerekiyor ise SR suffix'ini ("service releases") ve sürümü alıyor. örnek: Angel.SR1, Angel.SR2...

| code name         | release date |
|-------------------|--------------|
| Greenwich.RELEASE | Jan, 2019    |
| Greenwich.SR2     | Jun, 2019    |
| Greenwich.SR1     | Mar, 2019    |
| Finchley.RELEASE  | Jun, 2018    |
| Finchley.SR4      | Jun, 2019    |
| Finchley.SR3      | Feb, 2019    |
| Finchley.SR2      | Oct, 2018    |
| Finchley.SR1      | Aug, 2018    |
| Edgware.RELEASE   | Nov, 2017    |
| Edgware.SR6       | May, 2019    |
| Edgware.SR5       | Oct, 2018    |
| Edgware.SR4       | Jul, 2018    |
| Edgware.SR3       | Mar, 2018    |
| Edgware.SR2       | Feb, 2018    |
| Edgware.SR1       | Jan, 2018    |
| Dalston.RELEASE   | Apr, 2017    |
| Dalston.SR5       | Dec, 2017    |
| Dalston.SR4       | Oct, 2017    |
| Dalston.SR3       | Aug, 2017    |
| Dalston.SR2       | Jul, 2017    |
| Dalston.SR1       | May, 2017    |
| Camden.RELEASE    | Sep, 2016    |
| Camden.SR7        | May, 2017    |
| Camden.SR6        | Mar, 2017    |
| Camden.SR5        | Feb, 2017    |
| Camden.SR4        | Jan, 2017    |
| Camden.SR3        | Nov, 2016    |
| Camden.SR2        | Nov, 2016    |
| Camden.SR1        | Oct, 2016    |
| Brixton.RELEASE   | May, 2016    |
| Brixton.SR7       | Nov, 2016    |
| Brixton.SR6       | Sep, 2016    |
| Brixton.SR5       | Aug, 2016    |
| Brixton.SR4       | Jul, 2016    |
| Brixton.SR3       | Jul, 2016    |
| Brixton.SR2       | Jul, 2016    |
| Brixton.SR1       | Jun, 2016    |
| Angel.SR6         | Jan, 2016    |
| Angel.SR5         | Jan, 2016    |

2020 yılı ile Calendar Versioning'e geçildi.

## 📌 Spring Cloud K8s

Spring cloud ile aynı arayüzlerin `K8s` için implementasyonları içeren kütüphaneler'lerdir.

bu kütüphanelerin sundukları:

- discovery server olan `Eureka` kullanmak yerine, servis listesi `K8s`'in API'lerinden çekilmesini sağlayabilir (`Spring-cloud-starter-Kubernetes`)
- config'leri config server'dan çekmek yerine `K8s` ortamındaki ConfigMap'ten veya Secret'lardan çekebilir (`Spring-cloud-starter-Kubernetes-config`)
- `Ribbon` load balancer'ın Kubernetes servisleri aracılığı ile servisin ayakta olup olmadığını kontrol ederek yapabilmesi sağlanabilir (`Spring-cloud-starter-Kubernetes-Ribbon`)

not: `Spring-cloud-starter-kubernetes-all` `JAR`'ı tüm yukarıdaki dependency'leri barındırıyor.

## 📌 Spring Cloud Stream

Message-broker destekleyen (Kafka, RabbitMQ...) altyapılar için orta API. Eskiden her message-broker için ayrı API vardı. Şimdi hepsi tek çatı altında toplandı.

Sadece "Spring for Apache Kafka" kütüphanesi `spring-boot`'a bağımlı değildir. Fakat "Spring Cloud Stream" `spring-boot`'a bağımlı.

"Spring Cloud Stream" tek başına bir işe yaramaz. RUntime'da bir implementasyon olması gerekir. Örneğin Kafka için 2 farklı implementasyon var:

- spring-cloud-stream-binder-kafka (Kafka Streams API ile uyumsuz)
- spring-cloud-stream-binder-kafka-streams (Kafka Streams API ile uyumlu)
