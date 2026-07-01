# JAVA SPRING AND JAVA EE

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 front controller

genel bir server uygulama pattern'idir. bir server'a gelen istekleri yakalayan ilk sınıftır/servlet'tir. Bu servlet'in görevi ilgili yere bu isteği taşımaktadır.

## 📌 FacesServlet

`JSF`'in `front controller`'ıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Tomcat variants

`Tomcat`: `servlets` + `JSP`

`Tomee`: `Tomcat` + `JSF` + `JPA` + `CDI`

`Tomee-JAX-RS`: `Tomee` + `JAX-RS`

`Tomee+`: `Tomee-JAX-RS` + `JAX-WS`

detailed comparison:

|                                                   | Tomcat | TomEE | TomEE JAX-RS (~ Microprofile) | TomEE+ | TomEE PluME | OpenEJB |
|---------------------------------------------------|--------|-------|-------------------------------|--------|-------------|---------|
| Java Servlets                                     | +      | +     | +                             | +      | +           |         |
| JSP                                               | +      | +     | +                             | +      | +           |         |
| JSF                                               |        | +     | +                             | +      | +           |         |
| JTA                                               |        | +     | +                             | +      | +           | +       |
| JPA                                               |        | +     | +                             | +      | +           | +       |
| CDI                                               |        | +     | +                             | +      | +           | +       |
| JAAS                                              |        | +     | +                             | +      | +           | +       |
| Java Authorization Contract for Containers (JACC) |        | +     | +                             | +      | +           | +       |
| JavaMail API                                      |        | +     | +                             | +      | +           | +       |
| Bean Validation                                   |        | +     | +                             | +      | +           | +       |
| Enterprise JavaBeans                              |        | +     | +                             | +      | +           | +       |
| JAX-RS                                            |        |       | +                             | +      | +           | +       |
| JAX-WS                                            |        |       |                               | +      | +           | +       |
| Java EE Connector Architecture                    |        |       |                               | +      | +           | +       |
| JMS                                               |        |       |                               | +      | +           | +       |
| EclipseLink                                       |        |       |                               |        | +           |         |
| Mojarra                                           |        |       |                               |        | +           |         |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Tomcat version history

| version | JSP | WebSocket spec | servlet spec | Java version                        | release date | Latest release | Latest version release date |
|---------|-----|----------------|--------------|-------------------------------------|--------------|----------------|-----------------------------|
| 2.0     |     |                |              |                                     | 1998         |                |                             |
| 3.0     |     |                |              |                                     | 1999         | 3.3.2          | 2004-03-09                  |
| 4.1     | 1.2 |                | 2.3          | 1.3 and later                       | 2002-09-06   | 4.1.40         | 2009-06-25                  |
| 5.0     | 2.0 |                | 2.4          |                                     | 2003-12-03   | 5.0.30         | 2004-08-30                  |
| 5.5     |     |                | 2.4          | 1.4 and later                       | 2004-11-10   | 5.5.36         | 2012-10-10                  |
| 6.0     | 2.1 |                | 2.5          | 5 and later                         | 2007-02-28   | 6.0.53         | 2017-04-07                  |
| 7.0     | 2.2 | 1.1            | 3.0          | 6 and later (WebSocket için min 7)  | 2011-01-14   | 7.0.96         | 2019-07-29                  |
| 8.0     | 2.3 | 1.1            | 3.1          | 7 and later                         | 2014-06-25   | 8.0.53         | 2018-07-05                  |
| 8.5     |     | 1.1            | 3.1          | 7 and later                         | 2016-06-13   | 8.5.43         | 2019-07-09                  |
| 9.0     |     | 1.1            | 4.0          | 8 and later                         | 2018-01-18   | 9.0.22         | 2019-07-09                  |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 servlet version history

| Servlet API version | Released       | Specification    | Platform                                                  | Important Changes                                                           |
|---------------------|----------------|------------------|-----------------------------------------------------------|-----------------------------------------------------------------------------|
| Servlet 4.0         | Sep 2017       | JSR 369          | Java EE 8                                                 | `HTTP` 2                                                                      |
| Servlet 3.1         | May 2013       | JSR 340          | Java EE 7                                                 | Non-blocking I/O, `HTTP` protocol upgrade mechanism                           |
| Servlet 3.0         | December 2009  | JSR 315          | Java EE 6, Java SE 6                                      | Pluggability, Ease of development, Async Servlet, Security, File Uploading  |
| Servlet 2.5         | September 2005 | JSR 154          | Java EE 5, Java SE 5                                      | Requires Java SE 5, supports annotation                                     |
| Servlet 2.4         | November 2003  | JSR 154          | J2EE 1.4, J2SE 1.3                                        | web.xml uses XML Schema                                                     |
| Servlet 2.3         | August 2001    | JSR 53           | J2EE 1.3, J2SE 1.2                                        | Addition of Filter                                                          |
| Servlet 2.2         | August 1999    | JSR 902, JSR 903 | J2EE 1.2, J2SE 1.2                                        | Becomes part of J2EE, introduced independent web applications in .war files |
| Servlet 2.1         | November 1998  | 2.1a             |                                                           | First official specification, added RequestDispatcher, ServletContext       |
| Servlet 2.0         | December 1997  | N/A              | JDK 1.1                                                   | Part of April 1998 Java Servlet Development Kit 2.0                         |
| Servlet 1.0         | December 1996  | N/A              | Part of June 1997 Java Servlet Development Kit (JSDK) 1.0 |                                                                             |

## 📌 servlet 3.1 non-blocking IO

```java
public void doGet(request, response) {
    final AsyncContext asyncContext = request.startAsync();

    asyncContext.start(new Runnable() {
        @ Override
        public void run() {
            // do some work here which is on a new thread. that makes free the blocking HTTP thread.

            // burada aslında aşağıdaki gibi bir şey yapmazsak neredeyse hiçbir anlamı kalmaz
            asyncHttpCLient.sendPostRequest().
                                    than(response -> {
                                                       log(response);
                                                       response.getOutputStream().print(response);
                                                       asyncContext.complete();
                                    })
        }
    });
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Catalina

`Tomcat`'in `servlet container`'ın ismidir.

## 📌 Jasper

`Tomcat`'in `JSP` derleme motorudur. `JSP` derlenir ve bir servlet'e dönüştürülür.

## 📌 $CATALINA_HOME

`Tomcat` runtime dosyalarının bulunduğu dizindir.

## 📌 $CATALINA_BASE

birden fazla `Tomcat` instance'ı aynı anda çalıştırılabilir. her instance'ın bulunduğu config'ler `CATALINA_BASE` içinde olmalıdır.

eğer `CATALINA_BASE` set edilmemişse; `CATALINA_HOME` değeri ona set edilecektir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Eclipse IDE Tomcat config files

`Eclipse`, `Tomcat`'i başlatırken config dosyaları için workspace içinde otomatik olarak `/Eclipse-workspace/Servers/Tomcat v6.0 Server at localhost-config` isminde bir dizin oluşturuyor. bu dizindeki config'ler `Tomcat` dizini içindeki config'leri override ediyor.

`Tomcat` start edilirken ona zaten tüm config parametreleri (config dosyalarının path'leri, örnek `server.xml`) komut satırından geçilebiliyor. `Eclipse`'te bu config dosyalarını, `Tomcat` binary'sine otomatik olarak parametre geçiyor. yani gerçek `Tomcat` dizinini override etmemiş oluyor. böylece tek bir `Tomcat` dizini birden fazla `Eclipse` workspace'i tarafından kullanılabiliyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Tomcat directory structure

- `bin`: executable files and "start Tomcat", "run as service" like scripts.

- `lib`: `jar` available for all `webapps`

- `logs`

- `work`: sadece `JSP` dosyaları, `servlet`'e çevrildiklerinde, burada o `servlet`'ler saklanır.

- `conf`: `server.xml` gibi config dosyalarının bulunduğu dizin

  - `catalina.policy`:

    authorization tanımları bu dosyadadır. örnek:

    permission `java.util.PropertyPermission`, `java.version`, `read`.

  - `catalina.properties`

    Java class paketleri bazında ve `jar` bazında hangi projenin hangilerini okuyabileceğinin bilgisinin yer aldığı dosya.

  - `server.xml`

    sunucunun tüm ayarları buradan yapılabiliyor. detaylı anlatım aşağıda mevcut. bu dosyadan her `war` dosyasının içinde olamaz.

  - `web.xml`

    bu dosya her `war` içindeki `web.xml` parse edilmeden önce işletilir. daha sonra `war` içindeki `web.xml` okunur.

- `webapps`

  - `docs`: dökümantasyon içeren bir proje

  - `examples`: örnek `JSP` sayfaları ve kaynak kodlarını gösteren bir site

  - `ROOT`: `context-path`'i olmayan bir proje. burada sadece `Tomcat`'e hoşgeldiniz ve diğer `Apache.org` projelerine linkler verilmiş bir site bulunmaktadır.

  - `manager`: `Tomcat` üzerinde `JVM` memory detayları, yüklü olan projeleri `context-path`'leri gibi bir çok detay monitör edilebilir ve deploy/undeploy işlemleri yapılabilmesini sağlar.

  - `host-manager`: `Virtual Host` özelliğini monitör etmek için kullanılmaktadır. `Virtual host` birden fazla `webapp` klasörü yaratılabilmesini her her `webapp` içindeki projelerin birer domain-name'e karşılık gelmesini sağlamaktadır. Örneğin; `Google.com` ve `Amazon.com` sitelerine yapılan istekler aynı `Tomcat` instance'ımıza gelecek ve fakat farklı `webapp` dizinlerindeki projelere yönlendirilebilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Tomcat Realm

realm Türkçe kelime anlamı: diyar, krallık, alan, alem

A Realm is a "database" of usernames and passwords that identify valid users of a web application. this DB includes also roles of users.

Tomcat içinde belirlediğimiz uygulamalar, güvenlik katmanlarında isterlerse bu kullanıcıları kullanabilirler. Böylece tüm uygulamalar için ortak kaynak belirlemiş oluruz.

Tomcat içinde gelen "manager" uygulaması, varsayılan olarak Realm'daki user'lara göre kullanıcıların login olup arayüzde işlem yapmasına izin vermektedir.

Realm kaynağı kullanmak için org.Apache.catalina.Realm'dan türemiş bir implementasyona ihtiyacımız var. Tomcat içerisinde bunlar yüklü gelmektedir:

- `JDBCRealm`

JDBC aracılığı ile DB'ye bağlanır. tabloları bizim oluşturmuş olmamız gereklidir.

`/Tomcat/conf/server.xml` dosyasına da buna benzer bir tanımlamayı eklememiz gereklidir:

```xml
<Realm
    className="org.Apache.catalina.realm.JDBCRealm"
    driverName="org.gjt.mm.mysql.Driver"
    connectionURL="JDBC:mysql://localhost/authority?user=dbuser&amp;password=dbpass"
    userTable="users" userNameCol="user_name" userCredCol="user_pass"
    userRoleTable="user_roles" roleNameCol="role_name"/>
```

DB'de yapılan satır değişiklikleri `Tomcat`'e anında yansımaktadır. fakat bir kullanıcının rolü değişirse, yada kullanıcı kaldırılırsa, kullanıcı logout olana kadar session'ı kullanabilecektir.

eğer `servlet`'lerde biri protected bir kaynağa erişmek isterse `Tomcat` otomatik olarak `org.Apache.catalina.realm.JDBCRealm.authenticate()` metodunu çağıracaktır.

- `DataSourceRealm`

`JNDI` tanımı aracılığı ile DB'ye bağlanmaktadır.

```xml
<Realm
   className="org.Apache.catalina.realm.DataSourceRealm"
   dataSourceName="JDBC/authority"
   userTable="users" userNameCol="user_name" userCredCol="user_pass"
   userRoleTable="user_roles" roleNameCol="role_name"/>
```

- `JNDIRealm`

`JNDI` tanımı aracılığı ile `LDAP` sunucusundan kullanıcı bilgilerini çeker.

- `UserDatabaseRealm`

`JNDI` aracılığı ile XML'den kullanıcı bilgilerini okunduğu `realm`'dır. varsayılan olarak `/tomcat/conf/tomcat-users.xml` dosyasıdır. örnek `tomcat-users.xml`:

```xml
<tomcat-users>
  <user name="tomcat" password="tomcat" roles="tomcat" />
  <user name="role1"  password="tomcat" roles="role1"  />
  <user name="both"   password="tomcat" roles="tomcat,role1" />
</tomcat-users>
```

- `MemoryRealm`

DEMO için geliştirilmiş bir implementasyondur. `UserDatabaseRealm` ile aynıdır (aynı `XML`'i okur), tek eksisi `XML`'de yapılan değişiklikler restart edilene kadar algılanmaz.

- `JAASRealm`

istek yapan kullanıcıları `JAAS` ile onaylayan `Realm`'dır.

- `CombinedRealm`

birden fazla sub `realm` kullanabilmemize imkan tanıyan `Realm`'dır.

```xml
<Realm className="org.Apache.catalina.realm.CombinedRealm" >

   <Realm className="org.Apache.catalina.realm.UserDatabaseRealm"
          resourceName="UserDatabase"/>

   <Realm className="org.Apache.catalina.realm.DataSourceRealm"
          dataSourceName="JDBC/authority"
          userTable="users" userNameCol="user_name" userCredCol="user_pass"
          userRoleTable="user_roles" roleNameCol="role_name"/>
</Realm>
```

- `LockOutRealm`

`CombinedRealm`'dan türemiştir. kullanıcıları, fazla şifre denemesi isteği yaptıkları durumda onları engelleyebilmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 context path

`context`; Tomcat'te bir `war` dosyasına denk geliyor (`server.xml` mantığından da anlaşılacağı gibi).

`context path`; URL'de `host` ve `port`'tan sonra gelen web projesinin ilk seviye path'idir.

Webapp altındaki her proje URL'den `http://host/directoryName` olarak çağrılırlar. tek istisna ROOT folder'ıdır. o direk `http://host/` ile çağrılır. root içerisindeki bir web projesi, örnek `/tomcat/webapps/ROOT/webapp1`, `http://host/webapp1` olarak çağrılır.

`TOMCAT/WEBAPPS/demo#v1#myfeature/` projesi, URL'den bu şekilde erişilir: `http://host/demo/v1/myfeature`.

`##` (çift diyez) örnek: `foo##2.war` sürüm olarak algılanmaktadır. sürüm numaraları URL'ye (`context path`'e) yansıtılmaz.

`/tomcat/conf/server.xml` dosyası sadece Tomcat restart edildiğinde işlenir, bu sebeple `context`'lerin `server.xml` dosyasına yazılmaması önerilir.

## 📌 context.xml

bu dosya `server.xml` içindeki host elementinin içine yazılan kısmı taşıyabilir. bir sistemde birden fazla context belirteci olabilir. aynı sistemde birden fazla belirteç yazılmış olabilir. bunlar birbiri ile çakıştıklarında öncelik sırası parantez içerisinde verilmiştir (önceliği yüksek olan 1 numara):

- (1) `/tomcat/conf/context.xml`

- (1) `tomcat/conf/server.xml` içindeki context elementi

- (2) `/tomcat/conf/[ Engine_name ]/[ Host_name ]/war-name.xml`

- (2) `/tomcat/conf/context.xml`

- (3) `war` içindeki `/META-INF/context.xml`

Tek istisna: `tomcat/conf/server.xml` içindeki `context` elementinde `override=true` yok ise; sistemin herhangi bir yerine yazdığımız `context` değeri algılanır.

## 📌 META-INF

Bu dizin `war` dosyalarının içindedir. `war` export edilirken, bu dizinin içindekiler `tomcat/webapps/app1/` içerisine taşınır.

## 📌 server.xml

```xml
<Server>
  <Service name="MyService1">
    <Connector port="8443"/>
    <Connector port="8444"/>
    <Engine>
      <!-- default appBase: webapps -->
      <Host name="yourhostname" appBase="/webapps2">
        <!-- file1 path is: TOMCAT/webapps2/file1.war -->
        <Context path="/webapp1" docBase="file1.war" />
        <Context path="/webapp2"/>
      </Host>
    </Engine>
  </Service>
</Server>
```

- Görüldüğü üzere her bilgi `server` içerisine yazılıyor.

- örnekte 1 adet service içerisinde birden fazla `connector` (bunlar sadece port'u dinleyip yönlendirme yapmaya yarıyor), `webapp1` vs `webapp2`'ye yönlendirme yapıyor.

- 2 ayrı port tek bir `engin`'e yönlenmiş durumda. `webapp1` ve `webapp1` bir adet `engine`'e bağlanmış durumdalar.

- `engine` burada `Catalina`'ya denk geliyor. `Catalina` ise bir `servlet container`'dır.

- `Service`, `engine` gibi gruplamalar log'lardan da takip edilebildiği için büyük kolaylık sağlamaktadır.

- 80'e gelen istekleri, eğer client SSL bağlantısı kurma isteği yapıyorsa, 443'e yönlendirebiliriz:

```xml
<Connector port="80" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="443" />
```

Bu `connector` tanımı yetmeyecektir. Çünkü son durumda 443'ün tanımı yoktur. 443'ün tanımını da yapmalıyız:

```xml
<Connector port="443"
           protocol="org.Apache.coyote.http11.Http11Protocol"
           SSLEnabled="true"
           scheme="https"
           secure="true"
           sslProtocol="TLS"
           keystoreFile="/path/to/keystorefile"
           keystorePass="my_keystore_password"/>
```

Bazı `Connector` parametreleri:

- `address`

  bir sunucu birden fazla IP alıyor olabilir. bunlardan hangisinin o connector tarafından listening'de olacağını belirtir. default olarak tüm IP'ler dinlenir.

- `maxPostSize`

  post request'inin max boyutu. varsayılan 2 MB.

- `scheme`

  protocol'ün ismidir. URL'deki değer değildir. default http.

- `secure`

  Java kodumuzda `request.isSecure()` true/false dönüşü yapsın istiyorsak bu değeri `true` olarak set etmeliyiz.

- `URIEncoding`

  URL'nin encoding'idir. default `ISO 8859-1`.

- `useBodyEncodingForURI`

  eğer request'in header'ında `contentType` varsa, `URIEncoding` değerini hiçe sayar ve `contentType`'a göre URL'yi parse eder. default değeri false.

Yukarıda protocol ile class ismi (implementasyon) belirtmezsek Tomcat otomatik olarak bir implementasyon seçecektir. SSL standarttır. bu sebeple bu implementasyonlar SSL protocol'ünün işleyişinde bir değişiklik yapmıyorlar, bunlar sadece konfigürasyonları fark ettiriyor.

- `JNDI`

Direk `server` içerisine bu tanımlama yapılabilir. bu şekilde tüm uygulamalar ismi ile `JDNI`'a erişebilir olur.

```xml
<GlobalNamingResources>
  <Resource name="UserDatabase" auth="Container"
            type="org.Apache.catalina.UserDatabase"
            description="the user information"
            factory="org.Apache.catalina.users.MemoryUserDatabaseFactory"
            pathname="conf/tomcat-users.xml" />
</GlobalNamingResources>
```

Bu `JNDI`'ı kullanacak war dosyaları kendi `web.XML` dosyalarında bunu belirtmek zorundalar:

```xml
<resource-ref>
    <description>
          DB of Tomcat
    </description>
    <res-ref-name>
          UserDatabase
    </res-ref-name>
    <res-type>
          org.Apache.catalina.UserDatabase
    </res-type>
    <res-auth>
          SERVLET
    </res-auth>
</resource-ref>
```

Yukarıdaki `res-aut` `CONTAINER` da olabilirdi.

- Environment variables

Direk `server` içerisine bu tanımlama yapılabilir. bu şekilde tüm uygulamalar için environment variable atamış oluruz.

```xml
<Environment name="simpleValue" type="java.lang.Integer" value="30"/>
```

- `catalina` alternatives

`catalina` dışında bir `container` kullanmak istersek:

```xml
<Service name="MyService1" className="com.my.class.Name" >
```

Eğer `className` verilmezse default olarak `org.Apache.catalina.Service` arayüzü kullanılır.

- `Listeners`

Birçok event `listener` mevcut. Bunlar default olarak `catalina`'ya assign edilmiş durumdadır.

```xml
<Listener className="org.Apache.catalina.startup.VersionLoggerListener" />
```

## 📌 shutdown

aşağıdaki satır `JVM`'in 8005'ten port dinlemesi ve `SHUTDOWN` string'i o porta yollandığı takdirde kendini kapatmasını belirtmektedir.

```xml
<Server port="8005" shutdown="SHUTDOWN">
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 deployment descriptors

| file                   | platform        | directory           |
|------------------------|-----------------|---------------------|
| application.xml        | Java EE         | META-INF            |
| application-client.xml | Java EE         | META-INF            |
| beans.xml              | CDI             | META-INF or WEB-INF |
| ra.xml                 | JCA             | META-INF            |
| EJB-jar.xml            | EJB             | META-INF or WEB-INF |
| faces-config.xml       | JSF             | WEB -INF            |
| persistence.xml        | JPA             | META-INF            |
| validation.xml         | Bean Validation | META-INF or WEB-INF |
| web.xml                | Servlet         | WEB-INF             |
| web-fragment.xml       | Servlet         | WEB-INF             |
| webservices.xml        | SOAP            | META-INF or WEB-INF |

## 📌 "Java EE deployment descriptors" vs "runtime deployment descriptors"

`Java EE` implementasyonları (`app server`'lar) kendi içlerinde ek olarak `XML` istiyor olabilir. bunlara `runtime deployment descriptors` denir. örnekler:

- `sun-application.xml`
- `sun-web.xml`

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 dependency Injection types

### 📌📌 Constructor-based

```java
public class Service {

  private Collaborator collaborator;

  Service(Collaborator collaborator) {

    this.collaborator = collaborator;
  }
}
```

### 📌📌 Setter-based

```java
public class Service {

  private Collaborator collaborator;

  @Required
  public void setCollaborator(Collaborator c) {
    this.collaborator = c;
  }
}
```

### 📌📌 field-based

```java
public class Service {
  @AUtowired
  private Collaborator collaborator;
}
```

## 📌 Avantajlar ve dezavantajlar

- `field-based` daha sade ve okunabilirdir. diğerlerinde kod blokları daha uzundur.
- `field-based` olmayan injection'larda, instance'ları kontrol etme ve kontrol sonrası duruma göre aksiyon alabilmemiz mümkündür. ne gibi kontroller yapabiliriz:
  - inject edilen sınıfların null gelme durumunu kontrol edebiliriz (log basarız veya null gelen yerine NullObjectPattern uygulayabilirz).
  - inject edilen sınıfların içindeki default variable değerlerini kontrol edebiliriz.
  - Eklenti altyapımız varsa, ek güvenlik için inject edilen sınıfların hangi class referansı olduğu durumunu kontrol etmek isteyebiliriz. çünkü Spring duruma göre (profillere, classpath'e göre) farklı çeşit class'lar inject edebilir.

- `Constructor-based`'de tüm instance'ları tek bir metotta kontrol edebiliriz. bu şekilde bir instance'ın özel bir durumu diğerini etkileyebilir. oysa `setter`-based'de sadece ilgili dependency için aksiyon alabiliriz.

- bir sınıfımıza gereğinden fazla field eklediğimizi düşünelim. bu sınıfı parçalara bölmek genelde doğru bir çözüm olmaktadır. eğer `constructor-based` injection kullanırsak, static code analizi yapan eklentiler bizi constructor çok fazla eleman kabul ediyor diye uyaracaktır. fakat `field-based` ve `setter`-based injection'larda bu uyarıları alamayız.

- `Field-based` injection yapılan bir sınıfı, Spring'i, bean container'sız init etme şansımız yok (tek şansımızı reflection kullanmak). Bu sebeple Spring container'sız çağrılabilecek sınıflar (bu durum bazen Spring kullanılmayan unit testlerimizde olabiliyor) var ise o sınıfların içinde `field-based` constructor kullanmamalıyız.

- `Java`'da filed'ın önüne annotation yazabiliyoruz fakat bu farklı dillerde olmayabilir veya `DI` framework'ü bunu desteklemiyor olabilir. Örneğin; `constructor-based` annotation'da, specific bir dependency'yi `@Lazy` yapamayabiliriz. Aşağıdaki örnek hata verir:

```java
@Service
public class MyService {

  private Dependency1 d1;
  private Dependency2 d2;

  @Autowired
  MyService(Dependency1 d1, @Lazy Dependency2 d2){
      this.d1 = d1;
      this.d2 = d2;
  }
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 circular dependency

`A` Bean'i, `B` bean'ini inject ediyor ve `B` bean'i, `A` bean'ini inject ediyorsa, runtime sırasında `BeanCurrentlyInCreationException` hatası alırız.

Bu durum bir anti-pattern'dir. Mimari yeniden düzenlenmesi önerilir. Fakat redesign yapılamazsa birçok workaround çözümleri vardır. bazıları:

- circular dependency yaratan Bean'lerin inject edildiği yerlere `@Lazy` annotation'u koymak.
- `setter`-based injection kullanmak.

Bu workaround çözümler temelde şunu yapar: Spring, `circular dependency` problemi çıkaran Bean'leri aynı fazda initialize etmez. Spring birini sonradan initialize eder. böylece `circular dependency` problemi olmaz. Örneğin; `constructor-based` injection yapıldığında tüm field'ları Spring aynı anda initialize etmeli ki, constructor çağrılabilsin. Bu sebeple `constructor-based` injection'da `circular dependency` problemini çözemeyiz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 app server (⟷ uygulama sunucusu) vs web server (⟷ web sunucusu)

`Web server` yazılımına bir istek geldiğinde, sunucu bu isteği basitçe en iyi karşılayabilecek programa aktarır. kısacası; yapılan çağrıları ilgili yerlere aktarmaktır ve dönen cevabı döndürmektir. load balancing, caching, gibi bir çok ek işlevi de yapabilir.

örnek sunucular: `Apache HTTP Server`, `nginx`

`app server` ise; iş mantığını yürüten (işleten, yani, üzerinde uygulamacıklar çalıştıran) ve API sunan bir motordur.

örnek sunucular: `GlassFish`, `Weblogic`, `jboss`, `WildFly`, `IBM WebSphere`

## 📌 java dünyası için "app server" tanımı

Java dünyasının standartları, genel bilişim standartlarından farklı.

Java diyorki; `EJB Container` içermeyen hiçbir yazılım `app server` olamaz. Bu tanıma göre;

- `Tomcat` ve `Jetty` birer `web server` sınıfındadır.
  - `Tomcat`, `Servlet` ve `Filter` destekler ama `Dependency Injection` yapamaz.
- `TomEE` bir `app server`'dır.

## 📌 Java'da bir web uygulaması neden app server'a ihtiyaç duyuyor?

Aslında zorunlu değiliz. Fakat Java EE'nin `HTTP filter` ve `HTTP Servlet` gibi özelliklerini, `Dependency Injection` yapısı ile uyumlu çalışan tüm kütüphaneleri desteklemek istiyorsak, Java EE standartındaki `app server` ile uyumlu çalışmak zorundayız. Yani; `Servlet Container` bulundurmak zorundayız. Zaten bunu hazır yapan bir çok açık kaynaklı paket var.

`Servlet container` sayesinde web projesi ayağa kaldırdığımız için, `web container` olarak ta adlandırılır.

notlar:
- `Spring` framework'ün `servlet container` modülü yoktur. Bu sebeple web uygulaması yazacak isek;
  - Java EE sunucusu üstünde çalışmaldır.
  - veya `spring-boot` ile gömülü `Java EE` sunucusu kullanılmalıdır.
- Oysa Java EE bir spesifikasyon ve komple tüm kütüphaneler sunucu içerisinde.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 javax.servlet.Servlet

jar groupId: `javax.servlet`

jar artifactId: `javax.servlet-API`

paketi sadece arayüzlerini içeriyor.

runtime sırasında bu kod direk implementasyonları çağırıyor.

`javax.servlet.Servlet` arayüzünün birçok implementasyonu var: 

- `FacesServlet`
- `GenericServlet`
- `HttpServlet`

`javax.servlet.Servlet` şu metotları barındırıyor:

- `destroy()` servlet artık hiç kullanılmayacağı zaman çağrılıyor.
- `init(ServletConfig config)` uygulamamızın hayatı boyunca 1 kere çağrılıyor. `ServletConfig` buraya otomatik `Java EE` sunucusu tarafından pass ediliyor.
- `service(ServletRequest req, ServletResponse res)` her request'te çağrılıyor
- `getServletConfig()` servlet ayarlarında verdiğimiz config'leri çekebilmemizi sağlıyor. `init` metodunda verilen `ServletConfig`'i çekebilmemizi sağlıyor.

`javax.servlet.http.HttpServlet` ise, `javax.servlet.Servlet`'ten extend eder ve bu metotları da içerir:

- `doGet(HttpServletRequest req, HttpServletResponse resp)`
- `doHead(HttpServletRequest req, HttpServletResponse resp)`
- `doPost(HttpServletRequest req, HttpServletResponse resp)`
- ve dahası

## 📌 javax.servlet.ServletConfig

`Servlet`'e pass edilen interface. İçerisinde aşağıdaki metotlar var.

```java
public String getServletName();
public ServletContext getServletContext();

// init params are key-value pairs that we give on deployment and runtime descriptors.
public String getInitParameter(String name);
public Enumeration<String> getInitParameterNames();
```

## 📌 javax.servlet.ServletContext

`ServletConfig` içerisindeki bir metot sayesinde, `Servlet` sınıfı `ServletContext`'e erişir. `ServletConfig` sınıfı ile `Servlet` specific bilgi bulundurabiliriz. Fakat `ServletContext` tüm web uygulamasında globaldir ve her `Servlet` aynı `ServletContext`'i okur. `ServletContext` bu sebeple aşağıdaki gibi metotlar barındırır:

```java
public ClassLoader getClassLoader();
public int getSessionTimeout();
public void setSessionTimeout(int sessionTimeout);

// below method return configs from deployment or runtime descriptors.
public String getRequestCharacterEncoding();
```

Yani; herhangi bir `servlet` içinde, `servlet`'in kendi config'lerine erişmek için bu `ServletConfig` kullanılır:

```java
ServletConfig servletConfig = getServletConfig(); //getServletConfig javax.servlet.Servlet sınıfında zaten tanımlıdır.
String servletName = servletConfig.getInitParameter("ServletName");
```

Fakat tüm `servlet`'lerde kullanılacak olan bilgiler `ServletContext`'ten çekilir:

```java
ServletContext servletContext = getServletContext();
String projectOptions = servletContext.getInitParameter("projectOptions");
```

## 📌 request.getSession().getServletContext() vs getServletContext()

`getServletContext()`, `Servlet` class'ından extend eden sınıflarda direk kullanılabilir bir metottur. oysa `request` objesi herhangi bir kod satırında kullanılabilir. `request`'ten de `ServletContext`'e erişilebilir. İkisi aynı instance'ı döndürürler.

## 📌 RequestContextListener

Bu sınıftan implemente etmiş bir sınıfı Spring'e tanıttığımızda request oluştuğundaki event'lerimizi tanımlayabiliriz.

## 📌 DispatcherServlet

Dispatch Türkçe kelime anlamı: sevk etmek. yazılım dünyasında metoda/fonksiyona mesaj(parametre) yollama(sevk etme) anlamında kullanılır.

`Spring-MVC`'de `front controller`'ın ismidir.

`DispatcherServlet`, `HandlerMapping` sınıfına başvurarak ilgili Controller bilgisine ulaşır. Varsayılan olarak, `BeanNameUrlHandlerMapping` handler olarak kullanılır.

her `servlet`'in bir URL'si vardır. alt-URL'ler 1 adet `servlet` sınıfının kodunun içinde `if` blokları ile dağıtılır. fakat `spring-mvc` bunu bizim için otomatik yapar. `spring-mvc`'nin request'i ilk karşılayan `servlet`'i `org.springframework.web.servlet.DispatcherServlet` sınıfıdır.

## 📌 Web XML

örnek `web.xml` dosyamızda, `spring-mvc`'nin config'lerini değiştiriyoruz:

```xml
<web-app>

  <listener>
      <!-- Burada application context'in hangi framework loader'ı ile ayağa kaldırılacağını belirtiyoruz. ContextLoaderListener default olarak /WEB-INF/applicationContext.xml dosyasını okur. fakat biz bu örnekte bunu, <context-param> ile override ettik. -->
      <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>

  <context-param>
      <!-- root-context'imiz: applicationContext. app-context içerisinde Bean'lerin tanımları mevcut.-->
      <param-name>contextConfigLocation</param-name>
      <param-value>/WEB-INF/app-context.xml</param-value>
  </context-param>

  <!--
  Yukarıda dikkat edilirse XML based bir tanımlama yaptık. Eğer Bean'lerimizi annotation ile tanımlamak istersek:
  <context-param>
      <param-name>contextClass</param-name>
      <param-value>
        org.springframework.web.context.support.AnnotationConfigWebApplicationContext
      </param-value>
  </context-param>
  -->

  <servlet>
      <servlet-name>app</servlet-name>
      <!-- bu servlet'imiz bizim elle tanımladığım değil, Spring'in Spring-MVC (Spring web MVC) modülünde sunduğu front-controller pattern'ine karşılık gelen servlet'idir. bu servlet exception-handling, view-resolver, request mapping (for redirection of sub-URL's to other servlets), MultipartResolver gibi görevleri yapabiliyor. -->
      <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
      <init-param>
          <!-- child-context'imiz: webApplicationContext. dispatcher-servlet-context-1.xml içerisinde Bean'lerin tanımları mevcut. bu Bean'ler sadece bu DispatcherServlet'in yönettiği Servlet'ler tarafından kullanılabilecektir. -->
          <param-name>contextConfigLocation</param-name>
          <param-value>/WEB-INF/dispatcher-servlet-context-1.xml</param-value>
      </init-param>
      <load-on-startup>1</load-on-startup>
  </servlet>

  <servlet-mapping>
      <!-- servlet-name bir ID'dir. detayları yukarıda tanımlı. burada sadece hangi URL'yi karşılayacağı yazıyor. -->
      <servlet-name>app</servlet-name>
      <url-pattern>/app/*</url-pattern>
  </servlet-mapping>

</web-app>
```

Yukarıdaki tanımları servlet ayağa kaldıracaksak yapmak zorundayız. Çünkü;

1- web projeleri, `war` dosyalarından oluşur. `app server`'lar, `war` dosyalarını okurken (standartlar gereği), önce `web.xml` dosyasını okur. bu sebeple; `servlet` sınıfımızın, Spring (`DispatcherServlet`) tarafından yönetildiğini `app server`'a belirtmemiz şarttır.

2- `dispatcher-servlet-context-1.xml` tanımı olmazsa `dispatcher-servlet-context-1.xml`'de belirtilen Bean'lerimizi ilgili `servlet`'imizde kullanamayız. `Servlet`'ler birer `Spring Bean` olmadıkları için onları inject edebilmemiz için ek olarak `dispatcher-servlet-context-1.xml` tanımlaması şarttır.

Yukarıda 2'inci maddede belirtildiği gibi normal koşullarda `servlet`'ler `Bean` değildir. fakat `Bean` olarak tanımlanırlarsa, `spring-boot` bunları `servlet container`'a otomatik ekler. `kaynak: https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-embedded-container-servlets-filters-listeners-beans "Registering Servlets, Filters, and Listeners as Spring Beans" başlığı, 1inci paragraf.`

Yukarıdaki gibi birçok `DispatcherServlet`'i aynı projede kullanabiliriz. Her `DispatcherServlet`'in URL'si farklı olacaktır.

Not: 1 `dispatcher-servlet` yada 1 `servlet`, birden fazla `WebApplicationContext`'e sahip olabilir. `kaynak: https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html "1.1.1. Context Hierarchy" 2. paragraf.`

## 📌 WebApplicationContext

Spring içinde olan bir sınıftır. Bize `javax.servlet.ServletContext` ile bütünleşik çalışabilmemizi sağlar. Bütünleşik çalışmadan kasıt şudur: Her Spring `Bean`'i isterse `javax.servlet.ServletContext` sınıfına erişebilecektir. Bu şu şekilde olur:

`WebApplicationContext` aracılığı ile ayağa kaldırılan her Bean, `ServletContextAware`'ten implemente ettiği takdirde, `javax.servlet.ServletContext` sınıfına erişebilecektir.

`ServletContextAware` koduna bakarsak; `ServletContext`'in inject ediliğini görürüz:

```java
package org.springframework.web.context;

public interface ServletContextAware extends Aware {
     void setServletContext(ServletContext servletContext);
}
```

`WebApplicationContext` genelde 1 uygulamada 1 adet olur. Tabi bu durumda repository, business-logic-service'lerimizde `WebApplicationContext` içerisinde olur. Çünkü `servlet`'e gelen istekten bu diğer `Bean`'leri çağırmamız gerekecektir. kaynak: https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html "1.1.1. Context Hierarchy" 1. ve 2. paragraf.

## 📌 spring-boot embedded servers (like: Tomcat, jetty)

`Tomcat` gibi `app server`'lar, `servlet container` barındırır. Bu sebeple `Spring` özel bir `ApplicationContext` türevi kullanır: `ServletWebServerApplicationContext`. Bu sınıf aynı zamanda `WebApplicationContext` türevidir. Bu sınıf otomatik olarak `ServletWebServerFactory` `Bean`'lerini bulur. Bu `Bean`'lerin implementasyonları da runtime sırasında `TomcatServletWebServerFactory`, `JettyServletWebServerFactory` gibi karşımıza çıkar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
