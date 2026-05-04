# SECURITY AUTHENTICATION MECHANISMS

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

# PAM (⟷ Pluggable Authentication Modules)

`PAM` sadece temel seviye kurlaları belirleyen bir `RFC`'si var. Fakat bu `RFC` implementasyonun her detayını açıklamıyor.

Bazı implementasyonlar:

- `Linux` ailesi için: `Linux PAM`
- `BSD` vs `MacOS` ailesi için: `OpenPAM`
- `MS Windows`: `PAM` mimarisini temel alan bir yapı kullanmıyor. Fakat benzer şekilde runtime servis'leri ile authentication API'leri sunuyor.

`PAM`, aslında bir `C` `API`'dir. Kütüphane dosyası direk `OS` içinde gömülü oluyor. Böylece, `OS` user'ları gibi kaynakları baz alınarak authentication yapmak isteyen bir yazılım olursa, bu `API`'yi çağrıyor. `sudo`, `su`, `shhd` (`ssh` server), `gdm`, `lightdm` gibi yazılımlar `PAM` `API`'yi çağırır.

`C` ile yazıldığı için en kolay `C` ve `C++` üzerinden çağrılmaktadır.

`API`'yi çağıran her yazılım, kendi isminide parametre olarak bu `API`'ye geçiyor. `OS` içerisinde `/etc/pam.conf` dosyasında, `OS` admin'i, hangi yazılımın, hangi kaynaktan doğrulama yapacağına karar verebiliyor. Yani; `PAM` `API`, `OS` user'larına mı, `LDAP` user'larına mı, veya farklı bir kaynağa mı bakılacağı konusunda bir standart içermiyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 SASL (⟷ Simple Authentication and Security Layer)

`SASL` sadece authentication sağlar. authorization mekanizmasını içermez.

Hem client hem de server'ın `SASL` desteklemesi şart.

`SASL` client ile server arasındaki haberleşme protocol'üdür.

Client authentication isteği yollar. Server desteklediği mekanizmaları yollar. Client kendisinin de desteklediği bir mekanizma ile süreci başlatır. İşte bu seçmek için haberleşme kısmını `SASL` üstlenir.

Mekanizma seçildikten sonra mekanizmanın nasıl çalışacağı da `RFC`'lerde yazmamaktadır. Aslında mekanizma `SASL`'dan bağımsızdır fakat `SASL` süreci baştan itibaren wrap ettiği için mekanizma sürecinde ufak değişiklikler olabilir. Bu sebeple her mekanizmanın ayrı `RFC`'si vardır.

Bazı mekanizmalar sadece `SASL` tarafından kullanılır. Çünkü ilk defa `SASL` tarafından ortaya atılmıştır.

Server ve client'ın hem `SASL` hemde devamındaki süreçte kullanacakları mekanizmayı desteklemeleri gerekir. Her `SASL` destekleyen yazılım, tüm `SASL` mekanizmalarını desteklemek zorunda değildir.

- `ANONYMOUS`

  doğrulamaya gerek kalmadan giriş yapılır.

- `PLAIN`

  şifre açık şekilde yollanır.
  
  `SSL` kullanılması opsiyonel.

- `DIGEST-MD5`

  özel bir protocol. güvensiz.

- `GSSAPI`

  `GSSAPI` uyumlu herhangi bir protocol'ü başlatmıyor. Sadece Kerberos version 5 destekliyor. Bu sebeple isim kafa karıştırıcı olabilir.

- `KERBEROS_V4`

  Sadece `Kerberos` version 4 çalıştırmak için. (bu yeni `SASL` `RFC`'sinde yok. Eski `SASL` `RFC`'de vardı sadece.)

- `GS2-*`

  `GSSAPI` uyumlu herhangi bir protocol'ü yürütebilir.

  Yıldız yerine bir `GSSAPI` implementasyonu seçmek zorundayız.

  `GS2` denilmesinin sebebi `GSSAPI` mekanizmasına bazı özelliklerin eklenmesi ve round-trip'lerin azaltılmasıdır.

- `EXTERNAL`

  `RFC` içerisinde bağımsız bir mekanizmadan yararlanılmasından bahsedilmiş. Örneğin `SSL`. Fakat username şifre gibi bir bilgi server'a yollanmıyor.
  
  Fakat hangi external mekanizması olduğu 2 tarafın önceden bilmesi gerekli. güvensiz bir yöntem.

Others:

https://www.iana.org/assignments/sasl-mechanisms/sasl-mechanisms.xhtml

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 JAAS (⟷ Java Authentication and Authorization Service)

`JVM SE` içinde gelen bir pakettir.

`javax.security.auth` paketi altındaki sınıfları kullanır.

örnek kullanım:

`jaas1.config` dosyamızın içeriği

```conf
KafkaClient {
  org.apache.kafka.common.security.plain.PlainLoginModule required
  username="myuser"
  password="mypassword";
};
```

`Java` uygulamamızı `VM args` ile başlatırız:

```sh
-Djava.security.auth.login.config=src/main/resources/jaas/jaas1.config
```

Artık `Kafka` kütüphanesi otomatik olarak güvenlik için gerekli login işlemlerini yapacaktır.

`Kafka` kütüphanesi `JAAS`'ın `login` fonksiyonunu çağırıyor. İstersek bizde `JAAS`'ın `login`'ini manuel çağrabiliriz ama çoğunlukla buna gerek kalmıyor.

`JAAS`'ın `login` metodu, config ismini parametre olarak istiyor. Bizdeki örnekte, bu değer `KafkaClient`. Bu tamamen developer'ın verdiği random bir ID. Fakat Kafka kütüphanesi default olarak `KafkaClient` ID'li config'i otomatik bulacak şekilde hard-coded tasarlanmış.

`JAAS` `LoginModule`'leri (bir diğer değiş ile implementasyonları) (bizdeki örnekte `org.apache.kafka.common.security.plain.PlainLoginModule`) istenilen herhangi bir `Java` kodunu çalıştırabiliyor. Dolayısı ile `JAAS` protocol'den bağımsızdır. Yani server tarafın `JAAS` destekliyor olma gibi bir durumu söz konusu değildir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 JNDI (⟷ Java Naming and Directory Interface)

`Java` `API`'sidir.

property dosyalarından okurmuş gibi obje yaratabilmemizi sağlar. Bu objeyi authentication gibi amaçlarla kullanabilir.

`JNDI` bilgisi genelde `Tomcat` gibi server'lar tarafından uygulamamıza inject edilir. Böylece uygulamamızda direk `JPA` gibi `API`'lere authentication yapmak için kullanırız.

Örnek `JNDI` tanımlama kodu:

```java
import javax.naming.Context;
import javax.naming.InitialContext;
import javax.sql.DataSource;
import org.h2.jdbcx.JdbcDataSource;
import java.util.Hashtable;

public class JndiBinder {
    public static void main(String[] args) throws Exception {
        
        Hashtable<String, String> env = new Hashtable<>();

        // JNDI'ın nereye kaydolacağını belirleyen kütüphanenin sınıfı.
        env.put(Context.INITIAL_CONTEXT_FACTORY, "com.sun.jndi.fscontext.RefFSContextFactory");

        // JNDI nerey kaydedilecek.
        // Bu value yukarıdaki kütüphanenin standartına göre belirlenmeli.
        // şu anda local file'a yazıyoruz.
        env.put(Context.PROVIDER_URL, "file:/directory1/jndi");


        Context context = new InitialContext(env);

        // artık burada istediğimiz sınıfı oluşturup JNDI'a ekleyebiliriz.
        // Bu örnekte JDBC DataSource oluştur (örnek: H2)
        JdbcDataSource dataSource = new JdbcDataSource();
        dataSource.setURL("jdbc:h2:mem:testdb");
        dataSource.setUser("username1");
        dataSource.setPassword("password1");

        // JNDI name ile bind ederiz.
        context.bind("jdbc/myDataSource", dataSource);
    }
}
```

Örnek `JNDI` kullanma kodu:

```java
import javax.naming.Context;
import javax.naming.InitialContext;
import javax.sql.DataSource;
import java.sql.Connection;

public class JndiLookup {
    public static void main(String[] args) throws Exception {

        Context context = new InitialContext();
        DataSource ds = (DataSource) context.lookup("jdbc/myDataSource");

        try (Connection conn = ds.getConnection()) {
            System.out.println("connected");
        }
    }
}
```

Yani server tarafın `JNDI` destekliyor olma gibi bir durumu söz konusu değildir. `JNDI`'ı client kütüphanesi bile desteklemesi zorunda değildir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Generic Security Service Application Program Interface (⟷ GSS-API ⟷ GSSAPI)

Programlama dillerinden bağımsız bir `API`'dir. Her metodun ne yapacağı `RFC`'de yazılmıştır. Bu `API` sayesinde, authentication yapacak mekanizmanın akışlarını bilmeden, authentication olabiliriz.

Hem server hem client tarafın `GSSAPI` destekliyor olması şarttır.

Sadece authentication yapar. authorization özellikleri yoktur.

`GSSAPI` kendi başına bir işe yaramaz. Bir implementasyona ihtiyacımız var. Örneğin `Kerberos` implementasyonu ile `GSSAPI` çalıştırabiliriz.

Farklı versiyonları vardır.

`Java SE`'de `GSSAPI` ve onun `Kerberos` implementasyonu default gelir.

Örnek kod:

```java
import org.ietf.jgss.*; // It comes with Java SE.
import java.net.http.*;

public class Test1 {

    public static void main(String[] args) throws Exception {

        // Kerberos specific configurations
        System.setProperty("java.security.krb5.conf", "/path/to/krb5.conf");
        System.setProperty("javax.security.auth.useSubjectCredsOnly", "false");
        String servicePrincipal = "HTTP/your.server.com@YOUR.REALM";
        
        // GSSAPI code
        GSSManager manager = GSSManager.getInstance();
        GSSName serverName = manager.createName(servicePrincipal, GSSName.NT_HOSTBASED_SERVICE);

        // Oid is class of GSSAPI.
        // This long ID is the reference of Kerberos version 5.
        Oid krb5Oid = new Oid("1.2.840.113554.1.2.2");

        GSSContext context = manager.createContext(
                serverName,
                krb5Oid,
                null,
                GSSContext.DEFAULT_LIFETIME
        );

        context.requestMutualAuth(true);
        context.requestConf(true);

        byte[] token = new byte[0];
        token = context.initSecContext(token, 0, token.length);
    }
}
```

Artık bu `token`'ı istediğimiz yere manuel yollayabiliriz. Örneğin `HTTP` client'ın her header'a eklemesini ayarlayabiliriz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Lightweight Directory Access Protocol (⟷ LDAP)

Bu protocol'ü destekleyen sunucular:

- `Microsoft Active Directory (⟷ Microsoft AD)` (veya sadece `Active Directory (⟷ AD)`)

  `SMB` protocol'ünü de destekliyor.

  `Kerberos` ile login'i de native olarak destekler.

- `OpenLDAP`
- `Apache Directory Server`
- `Red Hat Directory Server`
- ve daha birçoğu.

Farklı versiyonları vardır:

- 2
- 3 (geriye uyumlu. fakat sunucu isterse, eski sürüm client'ları reddedebiliyor.)

### 📌📌 LDAP akışı

Client sunucuya query atıp, yetkisi dahilinde tüm sistemdeki

- user'ları
- user detaylarını (kişisel bilgiler gibi)
- grupları
- hangi user hangi hizmete/path'e erişebilir (naming service)
- her user için içini bizim doldurduğumuz sabit key-value'lar (bu bilgi sayesinde authorization yapabiliriz)

görebilir.

Fakat query atabilmesi için öncesinde `LDAP`'a doğru şifre ile login olması gereklidir. Bu login aşaması `LDAP` dünyasında `Bind` olarak adlandırılır. Tam olarak bu login işlemini saf `LDAP` ile yapabiliriz. Fakat istersek sadece bu login aşaması için `Kerberos` protocol'ü de kullanılabilir.

Aşağıdaki aşamalarda `LDAP`, sadece `LDAP` server'a atılan isteklerde kullanılır. Diğer isteklerde `LDAP` ile iglili birşey yoktur.

`Aşama 1:` Client --> `LDAP` server

Client, `LDAP` server'a istek atıp, `X`'e bağlanıp bağlanamayacağını öğrenir. bu response saf bir bilgidir. token dönmez. session based bir bilgi yoktur burada.

`Aşama 2:` Client --> `X`

Client, `X`'e işlem yapmaya kalkışır. Bu istekte `Aşama 1` deki request veya response'tan taşınan hiçbir bilgi yoktur.

`Aşama 3:` `X` --> `LDAP` server

`X`, `LDAP` server'a bu client bana gelebilir mi diye sorgu atar. Hatta, `LDAP` server'dan alacağı user'a bağlı key-value'lara bakarak authorization için karar verebilir. `Aşama 3:` zorunlu bir aşamadır. Oysa `Aşama 1` opsiyoneldir. Client direk `X`'e istek atıyor olabilirdi. Client `LDAP`'a sorgulamak zorunda değildi.

### 📌📌 LDAP kaynak referansları

| Kısaltma | Anlamı                      | Örnek                                         |
|----------|-----------------------------|-----------------------------------------------|
| CN       | Common Name                 | CN=Jack Jones                                 |
| OU       | Organizational Unit         | OU=BilgiIslem                                 |
| DC       | Domain Component            | DC=myCompany                                  |
| DN       | Distinguished Name          | CN=Ali Veli,OU=BilgiIslem,DC=myCompany,DC=com |

## 📌 SMB

dosya paylaşım için authorization protocol'üdür.

farklı sürümleri vardır.

## 📌 Samba

`SMB` ve `LDAP` protocol'lerini destekleyen cross-OS yazılımdır.

`Kerberos` ile login'i de native olarak destekler.

`SMB` authorization protocol'üdür. Login için kendi basit bir mekanizma destekler, ama istenirse `LDAP` ile login olunmasını da sağlayabilir.

## 📌 Kerberos

Sadece authentication protocol'üdür.

Hem client hem server'ın `Kerberos` desteklemesi şarttır.

Farklı versiyonları var. Version 5 standart oldu, çünkü version 4'te güvelik açıkları bulundu.

Client, `ABC` servisine login olmak istesin. Client, `Kerberos` standarında belirtilen `KDC (⟷ Key Distribution Center)` sunucusuna gider ve kimliğini doğruladığına dair `ticket` alır. Bu `ticket` ile `ABC` servisine istek yapar. `ABC` bu şifreli `ticket`'ı açar ve client'ı doğrular.

### 📌📌 Kerberos kullanırken client tarafta olan dosyalar

- `krb5.conf` dosyası

  burada `KDC`'nin `IP` adresi gibi bilgiler vardır.

- `keytab` dosyası

  anahtar dosyamız.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •