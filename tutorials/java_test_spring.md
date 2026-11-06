# JAVA TEST SPRING

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Spring-test

spring'in bir modülünün özel ismidir.

## 📌 @RunWith(SpringJUnit4ClassRunner.class)

JUnit Spring'in özellikleri ile açılmalıdır. aksi durumda Spring özellikleri çalışmaz (yani injection'lar, Bean'ler özelliklerini kaybeder. normal sınıf olarak kullanımda kalırlar.). __SpringRunner__.class parametresi de __SpringJUnit4ClassRunner__'a referans ettiği için tamamiyle aynı işi görüyor.

Bunun yerine JUnit 5 ile artık "__@ExtendWith__" kullanıyoruz. Fakat geriye uyumluluk için __@RunWith__ hala kullanılabiliyor.

## 📌 @ContextConfiguration(locations = {...})

ApplicationContext'in belirlenmesi için yapılıyor. parametre olarak XML dosyasının path'i verilebilir, yada @Configuration sınıfı/sınıfları verilebilir. Her @Configuration içerisinde @ComponentScan yapıp, sadece ayağa kaldıracağımız Bean'lerin paket isimlerini verebiliriz. Böylece tüm Spring uygulaması ayağa kalkmaz.

## 📌 @SpringBootTest

Eğer Junit testi içinde @Configuration kullanılmamışsa, "/src/main/* (test olmayan paketler)" arasında @SpringBootConfiguration arar ve Spring context'i ayağa kaldırır. böylece Bean'ler test süresince kullanılabilir olur.

__SpringJUnit4ClassRunner__ (veya __SpringRunner__) sadece testlerin Bean'ler kullanılarak çalışılacağını aktif eder. fakat application context'te Bean taraması yapmaz. Bean taraması olmadığından pek bir işe yarayamaz. bu sebeple __SpringRunner__ ve __SpringBootTest__ genelde birlikte kullanılır.

__@SpringBootApplication__ içerisinde zaten __@EnableAutoConfiguration__ vardır.

__@SpringBootTest__ default olarak tek başına Spring server'ı (web ortamı kısmını) ayağa kaldırmaz.

Web ortamı oluşturmak için @SpringBootTest'e parametre geçmek gerekli:

```java
@SpringBootTest(webEnvironment.MOCK)
```

webEnvironment aşağıdaki değerleri sunmaktadır:

### 📌📌 MOCK (Default)

ApplicationContext'i kaldırır fakat mock bir web environment'i oluşturur. yani gerçek bir server ayağa kalkmaz. Mock controller'lar olacağı için:

- @AutoConfigureMockMvc (reactive olamayan server'lara bağlanmak için)
- yada @AutoConfigureWebTestClient (reactive olan server'lara bağlanmak için)

gibi annotation'larla kullanılması önerilir.

eğer classpath'te web dependency'leri yok ise; normal ApplicationContext'i init eder.

### 📌📌 RANDOM_PORT

Gerçek server'ı uygulamayı ayağa kaldırır. Rastgele bir port ile dışarıya açar.

### 📌📌 DEFINED_PORT

Gerçek server'ı uygulamayı ayağa kaldırır. port default port yada `application.yaml`'de verilen porttur.

### 📌📌 NONE

ApplicationContext'i, web ortamlarını tamamen ignore ederek ayağa kaldırır.

If your test is @Transactional, it rolls back the transaction at the end of each test method by default. However, if you are using RANDOM_PORT or DEFINED_PORT which provides a real servlet environment, the HTTP client and server run in separate threads and, for that reason, they are separate transactions. Any transaction initiated on the server does not roll back in this case.

## 📌 @WebMvcTest

@WebMvcTest aşağıdaki iki annotation'u devreye sokar:

- __@AutoConfigureWebMvc__

  sadece `Spring-MVC` modülünde bazı component'leri (@Controller, Filter'ları ...) ayağa kaldıracak şekilde Spring context'i ayağa kaldırır. bu şekilde testlerin başlama hızı daha yüksektir. çünkü sistemin sadece bir kısmı ayağa kalkar. Fakat bu durumda HTTP client ile sunucuya gerçek istek atılamaz.

- __@AutoConfigureMockMvc__

  MockMvc Bean'ini inject edebilmemiz sağlar. web slicing testing'de, server, full ayağa kalkmaz. dolayısı ile Controller'lar normal-standart yöntemlerle çağrılamaz. ancak bu işi "MockMvc" objesi yapabilir. MockMvc artık client API'mizdir.

Yukarıdaki iki annotation bilgilerinden yola çıkarak şunu söyleyebiliriz: eğer testlerimize @AutoConfigureWebMvc ve @SpringBootTest yazarsak (@AutoConfigureWebMvc yazılmamışsa) server full ayağa kalkar ve yine testlerimiz mockMVC ile yapabiliriz.

sadece 1 adet controller'ı ayağa kaldırmak için:

```java
@WebMvcTest(HomeController.class)
```

@WebMvcTest test örneği:

```java
@RunWith(SpringRunner.class)
@WebMvcTest(UserVehicleController.class)
public class UserVehicleControllerTests {

    @Autowired
    private MockMvc MVC;

    @MockBean // package: org.springframework.boot.test.mock.Mockito
    private UserVehicleService userVehicleService;

    @Test
    public void testExample() throws Exception {
        given(this.userVehicleService.getVehicleDetails("sboot"))
                .willReturn(new VehicleDetails("Honda", "Civic"));
        this.MVC.perform(get("/sboot/vehicle").accept(MediaType.TEXT_PLAIN))
                .andExpect(status().isOk()).andExpect(content().string("Honda Civic"));
    }
}
```

bu tarz testlere "__slicing test__ (slice kelime anlamı: dilimlemek)" denir. çünkü sadece sistemin bir kısmı kaldırılır ve o kısımlar test edilir. bu test tipi, unit test'in bir türevidir.

slicing test için spring-boot'ta bu annotation'lar sunulmaktadır:

__@WebMvcTest__ - for testing the controller layer
__@JsonTest__ - for testing the JSON marshalling and unmarshalling
__@DataJpaTest__ - for testing the repository layer
__@RestClientTests__ - for testing REST clients

## 📌 @DataJpaTest

full auto-configuration yapmayıp, sadece JPA ortamını ayağa kaldırıyor.

her test metodu sonrası işlemler geri alınır.

repo testleri entity ilişkilerinin doğru kurulup kurulmadığının ve özel yazılan query'lerin düzgün çalışıp çalışmadığını görebilmek için yapılır.

```java
@RunWith(SpringRunner.class)
@DataJpaTest
public class CityRepositoryTest {

    @Autowired
    private CityRepository repository;

    @Test
    public void should_find_all_customers() {

        Iterable<City> cities = repository.findAll();

        assertThat(cities).hasSize(10);
    }
}
```

@DataJpaTest, __TestEntityManager__ autowired edilebilmesini sağlar. TestEntityManager, EntityManager'i wrap etmez. ona alternatiftir. test için uygun olan metotları barındırır ve bazı EntityManager metotlarını barındırmaz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
