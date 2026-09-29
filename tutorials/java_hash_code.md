# JAVA HASH CODE

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Nesne eşitliği

## 📌 Nesne eşitliğini bulma

`Java`'da `==` ifadesi iki nesnenin referanslarının aynı olup olmadığına bakar. eğer karşılaştırılan nesneler `primitive` ise (`int`, `boolean` gibi), sadece değerleri karşılaştırılır.

`Java`'da her nesne, `object` sınıfından türemiştir ve `equals(Object)` metodu içerir. Bu metot ile iki obje karşılaştırılabilir. Bu karşılaştırmada metodunu yazılımcı override ettiği için istediğini döner. fakat asıl yapılması (beklenen) dönüş; sınıfın içindeki unique değerlerin kontrol edilerek dönüş almaktır. Tabi sadece unique değerler karşılaştırılarak olmaz, zira karşılaştırılan sınıflarda aynı tipte olmalıdır.

Eğer `equals` metodu override edilmez ise; `object` sınıfındaki metoda göre referansları aynı olan sınıflar için true dönüşü alırız. Yani; override edilmezse `==` ile aynı işleme denk geliyor.

`hashCode` veya `equals` kullanırken tüm elemanları göz önünde bulundurmak zorunda değiliz.

## 📌 toString() default implementation

```java
public String toString() {
    return getClass().getName() + "@" + Integer.toHexString(hashCode());
}
```

## 📌 hashcode() default implementation

bu başlık için kaynak: https://srvaroa.github.io/jvm/java/openjdk/biased-locking/2017/01/30/hashCode.html

`java.lang.Object.hashcode()` implementasyonu `Java` değil, native dilde yazılmıştır. Bu sebeple `JRE`'nin kendisi ne istiyorsa onu döndürür. `Hashcode` metodu specification gereği tüm `runtime` süresi boyunca aynı instance için aynı değeri döndürmesi beklenir. Bu sebeple instance'ın `RAM`'deki adresini dönmeyi tercih etmez. Çünkü `garbage collector` arada temizlik yapmak için çalıştığında, `RAM`'deki instance'ların yerini değiştirebilir.

Native kodlar incelendiğinde; bu dönen sayı ne olursa olsun, önce bu değeri instance'ın meta bilgilerinde sakladığı görülür. Çünkü eğer bir `JRE` implementasyonu `RAM` adres'ini döndürmeye kalkarsa, ilgili instance için önceden alınmış `hash code` varsa onu döndürür, yoksa yeni `hash code` üretir.

Yeni `hash code` üretirken ise her sürüm kendi yöntemini kullanır. Seçenekler şunlardır:

- A randomly generated number.
- A function of memory address of the object.
- A hardcoded 1 (used for sensitivity testing.)
- A sequence.
- The memory address of the object, cast to int.
- Thread state combined with Xorshift (<https://en.wikipedia.org/wiki/Xorshift>)

`JDK`'lar ise şunu yaparlar:

- `OpenJDK` 6 - 1inci seçenek
- `OpenJDK` 7 - 1inci seçenek
- `OpenJDK` 8 - 5inci seçenek
- `OpenJDK` 9 - 5inci seçenek

## 📌 hashcode method

`Equals` ile benzer mantıkta olan `hashCode()` bir `integer` ile unique bir değer döndürmeli.

`hashtable` gibi veri yapılarında, bir sınıfın `hash` değerini saklamak gerekir. bu sebeple bu metot hazır olmalıdır.

`hashcode`'un `Integer` dönmesinin sebebi; eşitlik kontrolünün `equals`'a göre daha hızlı yapılmasını sağlamaktır (sebebi `bucket` konusunda daha detaylı anlatılıyor). Yani; `equals` ile aynı amaçta kullanılır fakat `hashCode`, `Integer` olduğundan, `equals` gibi her zaman güvenebileceğimiz bir bilgi değildir.

`Hashcode`'un dönüş değeri `integer` olduğundan, kısıtlı çıktı kümesi vardır. Bu sebeple aslında; unique değildir. Bu sebeple bazı instance'ların `hashcode`'ları çakışır (`hash collision`). Buna en basit örnek `string`'lerdir. Bir `string`'in sonsuz farklı instance'ını yaratabiliriz. Bu durumda `hashcode`'lar illaki çakışacaktır. Örneğin `Java`'da;

- `Siblings`
- `Teheran`
- `Aa`
- `BB`

`string`'leri aynı `hashcode` döndürür. Bu sebeple `Java`'daki `HashMap` sınıfı önce `hashCode`'a bakar, eğer `hashcode`'u aynı olan nesneler tutuyor olursa, `equals` veya `referans` (`==`) kontrolüne tabi tutar. Böylece çakışmaları çözmüş olur. `HashMap`'in önce `hashcode`'a bakmasının sebebi, `integer` ile eşitlik kontrolünün bilgisayarlar tarafından daha hızlı yapılmasıdır.

`Hashcode`'u (hiç özel kütüphane kullanmadan) yazmak için genelde bu yöntem tercih edilir:

`IntelliJ IDEA` 2020 auto-generated `equals` ve `hashCode` örneği

```java
import java.util.Arrays;
import java.util.List;

public class TestDTO {

  private String valStr;
  private int valInt;
  private Integer valInteger;
  private TestDTO valTestDTO;
  private long valLong;
  private char valChar;
  private boolean valBoolean;
  private String[] valStringArray;
  private List<String> valStringList;

  @Override
  public boolean equals(Object o) {
    if (this == o) return true;
    if (o == null || getClass() != o.getClass()) return false;

    TestDTO testDTO = (TestDTO) o;

    if (valInt != testDTO.valInt) return false;
    if (valLong != testDTO.valLong) return false;
    if (valChar != testDTO.valChar) return false;
    if (valBoolean != testDTO.valBoolean) return false;
    if (valStr != null ? !valStr.equals(testDTO.valStr) : testDTO.valStr != null) return false;
    if (valInteger != null ? !valInteger.equals(testDTO.valInteger) : testDTO.valInteger != null) return false;
    if (valTestDTO != null ? !valTestDTO.equals(testDTO.valTestDTO) : testDTO.valTestDTO != null) return false;
    // Probably incorrect - comparing Object[] arrays with Arrays.equals
    if (!Arrays.equals(valStringArray, testDTO.valStringArray)) return false;
    return valStringList != null ? valStringList.equals(testDTO.valStringList) : testDTO.valStringList == null;
  }

  @Override
  public int hashCode() {
    int result = valStr != null ? valStr.hashCode() : 0;
    result = 31 * result + valInt;
    result = 31 * result + (valInteger != null ? valInteger.hashCode() : 0);
    result = 31 * result + (valTestDTO != null ? valTestDTO.hashCode() : 0);
    result = 31 * result + (int) (valLong ^ (valLong >>> 32));
    result = 31 * result + (int) valChar;
    result = 31 * result + (valBoolean ? 1 : 0);
    result = 31 * result + Arrays.hashCode(valStringArray);
    result = 31 * result + (valStringList != null ? valStringList.hashCode() : 0);
    return result;
  }
}
```

yukarıda görüldüğü gibi `Long`, `String` gibi class'ların `hashCode` metotları zaten `Java`'da implemente edilmiş durumda. `DTO`'larımızın `hashCode`'ları olmazsa onu kullanan sınıflarda `hashCode` alamayız.

`Integer` sınıfının `hashcode`'u (tahmin edildiği gibi) kendisidir.

`boolean`'ın `hashcode`'unu 1 veya 0 şeklinde çevirebiliriz. en basit çözüm olacaktır.

her field'ın `31` gibi sabit bir `asal sayı (⟷ prime number)` ile çarpılması önerilir. `Tek sayı (⟷ odd number)` olması değil, `asal sayı` olması önemlidir. Bunun sebebi matematikseldir. Burada önemli olan nokta şudur: `asal sayı` ile çarpmak unique sayı dönmesinin ihtimalini arttırmaz, onun yerine `Collection`'larda matematiksel avantaj sağlar.

Bunun uzun açıklaması şudur: `HashMap` ve `HashSet` gibi `collection`'lar, her `hash code` değerlerini `bucket` denilen yerde saklar. `Insert` yapıldığında, `insert` edilen objenin `hash code` değerini (`integer`) saklaması gerektiğinde, nokta atışı yerini çabuk bulabilmek/belirleyebilmek için `hashcode` değerini `bucket`'ın `index`'i olarak kabul eder. Fakat `bucket`, `Integer.MAX` büyüklüğünde olamaz. Öyle olursa çok yer kaplar. İşte bu sebeple `hash`'in modülünü (`%`) alır. Örneğin; `bucket`-size `20` ise, ve `hash code` değerimiz `21` ise, `bucket`-id `21%20=1` olur. Yani bu `hash` ilk `bucket`'a kaydedilir. Diğer gelecek `hash` değerleri eğer `index` `1`'e denk gelmezse `hashmap` daha hızlı çalışacaktır. İşte modül işlemine tabi tutulduğunda, sonucun dağıtık olması için en basit çözümü bir `asal sayı` ile çarpmaktır. Yani; `asal sayı` ile çarpmak `hashcode`'un tekil olmasını değil, modül işlemine tabi tutulduğunda dağıtık (farklı) sonuç vermesi ihtimalini arttırmaktadır.

`Ek not`:

aşağıdaki rakamlar kütüphaneye göre değişkenlik gösterir ama genelde ortalama rakamlar şu şekildedir:

- `hashmap` init edildiğinde 20 `bucket` alanı rezerv edilir.
- `bucket` sayısı %75 doluluk oranına ulaştığında yeni `bucket` alanları otomatik olarak rezerv edilir (`bucket` sayısı arttırılır).

`Hashcode`'un kalitesini 2 şey belirler:

- olabildiğince unique olması (eğer instance sayısı `Integer.MAX`'ı geçemez ise, unique olması, sıralama işlemleri için şarttır)
- `mod` alındığında dağıtık sonuçlar verebilmesi (`hash` ile işlem yapan collection'ların performance'ını arttır)

`Asal sayı` olarak özellikle `31` sıkça kullanılır. çünkü `31`, standart `CPU`'larda performans olarak basit bir işlem olarak yapılabilir. interpreter'lar bunu genellikle otomatik olarak aşağıdaki işleme çevirir:

```text
31 * i = (i << 5) - i
```

`Google Guava` kütüphanesi var ise aşağıdaki yöntem de kullanılabilir:

```java
import com.Google.common.base.Objects;

import java.util.List;

public class TestDTO {

  private String valStr;
  private int valInt;
  private Integer valInteger;
  private TestDTO valTestDTO;
  private long valLong;
  private char valChar;
  private boolean valBoolean;
  private String[] valStringArray;
  private List<String> valStringList;

  @Override
  public boolean equals(Object o) {
    if (this == o) return true;
    if (o == null || getClass() != o.getClass()) return false;
    TestDTO testDTO = (TestDTO) o;
    return valInt == testDTO.valInt && valLong == testDTO.valLong && valChar == testDTO.valChar && valBoolean == testDTO.valBoolean && Objects.equal(valStr, testDTO.valStr) && Objects.equal(valInteger, testDTO.valInteger) && Objects.equal(valTestDTO, testDTO.valTestDTO) && Objects.equal(valStringArray, testDTO.valStringArray) && Objects.equal(valStringList, testDTO.valStringList);
  }

  @Override
  public int hashCode() {
    return Objects.hashCode(valStr, valInt, valInteger, valTestDTO, valLong, valChar, valBoolean, valStringArray, valStringList);
  }
}
```

`Apache-commons` kullanırsak bu şekilde de yapabiliriz:

```java
// Apache-commons-lang 3 ve öncesi arasında hiçbir metot ve class ismi fark etmiyor. sadece package fark ediyor.

import org.Apache.commons.lang.builder.EqualsBuilder;
import org.Apache.commons.lang.builder.HashCodeBuilder;

import java.util.List;

public class TestDTO {

  private String valStr;
  private int valInt;
  private Integer valInteger;
  private TestDTO valTestDTO;
  private long valLong;
  private char valChar;
  private boolean valBoolean;
  private String[] valStringArray;
  private List<String> valStringList;

  @Override
  public boolean equals(Object o) {
    if (this == o) return true;

    if (o == null || getClass() != o.getClass()) return false;

    TestDTO testDTO = (TestDTO) o;

    return new EqualsBuilder().append(valInt, testDTO.valInt).append(valLong, testDTO.valLong).append(valChar, testDTO.valChar).append(valBoolean, testDTO.valBoolean).append(valStr, testDTO.valStr).append(valInteger, testDTO.valInteger).append(valTestDTO, testDTO.valTestDTO).append(valStringArray, testDTO.valStringArray).append(valStringList, testDTO.valStringList).isEquals();
  }

  @Override
  public int hashCode() {
    return new HashCodeBuilder(17, 37).append(valStr).append(valInt).append(valInteger).append(valTestDTO).append(valLong).append(valChar).append(valBoolean).append(valStringArray).append(valStringList).toHashCode();
  }
}
```

## 📌 java.lang.String.hashCode()

http://hg.openjdk.java.net/jdk8/jdk8/jdk/file/687fd7c7986d/src/share/classes/java/lang/String.java

bu şekilde tanımlanmıştır:

```java
/**
* Returns a hash code for this string. The hash code for a
* {@code String} object is computed as
* <blockquote><pre>
* s[0]*31^(n-1) + s[1]*31^(n-2) + ... + s[n-1]
* </pre></blockquote>
* using {@code int} arithmetic, where {@code s[i]} is the
* <i>i</i>th character of the string, {@code n} is the length of
* the string, and {@code ^} indicates exponentiation.
* (The hash value of the empty string is zero.)
*
* @return  a hash code value for this object.
*/
public int hashCode() {
    int h = hash;
    if (h == 0 && value.length > 0) {
        char val[] = value;

        for (int i = 0; i < value.length; i++) {
            h = 31 * h + val[i];
        }
        hash = h;
    }
    return h;
}
```

`jacvadoc` okunduğunda üstel işlem görünmektedir. Fakat metot implementasyonunda üstel işlem görünmüyor. Aslında `for loop` `debug` edip tamamlandığında `jacvadoc`'taki işleme denk gelmektedir:

- döngü 1:

```text
h = 31 * hash + s[0]
```

- döngü 2:

```text
h = 31 *(31* hash + s[0]) + s[1]
```

yada bu şekilde de yazabiliriz:

```text
h = 31^2 *hash + 31* s[0] + s[1]
```

- döngü 3:

```text
h = 31 *(31* (31 * hash + s[0]) + s[1]) + s[2]
```

yada bu şekilde de yazabiliriz:

```text
h = 31^3 *hash + 32^2* s[0] + 31 * s[1] + s[2]
```

Sonuç olarak dikkat edersek `jacvadoc` açıklama ile aynı kapıya çıktığını görebiliriz.

## 📌 hashmap of immutable DTO (performance advantage)

Eğer `DTO`'muz `immutable` ise, `hashcode`'u her defa hesaplatmak yerine;
- initialize olduğunda
- yada ilk defa `hashCode` çağrıldığında

sonucu bir field'da tutup, `hashcode`'u sürekli bu field'dan döndürebiliriz. bu bize performans avantajı sağlar.

## 📌 primitive'lerin obje seviyesinde karşılıklarındaki eşitlik kontrolleri (primitive wrapper'ların eşitlik kontrolleri)

### 📌📌 integer için

```java
new Integer(5) == new Integer(5); // false döndürür. referansları yeni obje olduğu için farklı.

Integer.valueOf(5) == Integer.valueOf(5) // valueOf metodu Integer objesi döndürmektedir.
```

ikinci satırın normalde `false` olması beklenmektedir. fakat `true` vermektedir. çünkü; `Integer` sınıfı kendi içinde initialize olurken static olarak 1'den 256'ya kadar olan sayılar için birer `Integer` objesi saklamaktadır. çünkü bunlar genelde cok kullanılan sayılardır. `valueOf` metodu bu sayılar haricinde `new Integer()` çalıştırıyor. fakat bu sayılardan ise hızlı olsun diye bunlardan donduruyor.

bazı `JVM`'ler 256'dan daha fazla saklamaktadır.

### 📌📌 string için

`Java`'da aynı `string`'ler RAM'de aynı yere referans eder. çünkü `JVM`, her `string`'i `string pool` denilen yerde saklamakta ve yeni oluşturulan `string`'ler aynı ise, referansını buradan dönmektedir. bu sebeple `==`, `equals` yerine kullanılmaktadır. fakat `new String("text")` her zaman yeni bir referansla obje yaratmaktadır. bu sebeple bunlarda `==` kullanılamaz. `new String("text").intern()` metodu var olan `JVM`'deki `string`'lere referans etmemizi sağlıyor. bu şekilde bellekten tasarruf etmiş oluyoruz.

`string pool`'un bir size'ı yok. direk `Metaspace`'te tutuluyorlar.

## 📌 hashcode and equals method for DB entity classes

lets define simply what is `equals`: `Equals` method should return `true`, if the identifiers of them are same.

- 1

if we don't implement the `hashcode` and `equals` methods, then if we read multiple times the same row from DB, the `equals` and the `hashcode` methods will return `false` for both instances because `Java`s will check the instance memory references.

- 2

if we will implement `hashcode` and `equals` method using all fields, that not's gonna work in this case:

```java
user1 = new User("Jack").
repository.save(user1);

user2 = repository.readByPrimaryKey("12345");
user2.equals(user1); // returns false because "ID" field is null on user1 yet.
```

- 3

if we will implement the `equals` and `hashcode` based on `natural key`, we will have inconsistent cases like this:

```java
user1 = new User("998877", "Jack").
repository.save(user1);

user1.setName(); // the user has a new identity number...

user2 = repository.readByNationalIdentityNumber("998877");
user2.equals(user1); // returns false.
```

The possible way is to do not use `JPA`'s `primary key`s. we can manage them manually. We can initialize `ID` field on constructor. And we have to compare only `ID` field which is unique. For consistent `ID` generation we can use `UUID`.

Example abstract class for all entities on the project:

```java
public interface PersistentObject {
    public String getId();
    public void setId(String id);

    public Integer getVersion();
    public void setVersion(Integer version);
}

public abstract class AbstractPersistentObject
        implements PersistentObject {

    private String id = IdGenerator.createId();
    // JPA, DB'den okuduğu zaman burası da her defasında çalışacak fakat JPA sonrasında setter'ı çağıracağı için eski data yazılacak. yani bir sorun yok

    private Integer version;

    public String getId() {
        return id;
    }
    public void setId(String id) {
        this.id = id;
    }

    public Integer getVersion() {
        return version;
    }
    public void setVersion(Integer version) {
        this.version = version;
    }

    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null ||
            !(o instanceof PersistentObject)) {

            return false;
        }

        PersistentObject other
            = (PersistentObject)o;

        // if the ID is missing, return false
        if (id == null) return false;

        // equivalence by ID
        return id.equals(other.getId());
    }

    public int hashCode() {
        if (id != null) {
            return id.hashCode();
        } else {
            return super.hashCode();
        }
    }

    public String toString() {
        return this.getClass().getName()
            + "[id=" + id + "]";
    }
}
```

Yukarıdaki örnekte neden version kullandık? Çünkü `JPA` normal koşullarda `ID` field'ı boş ise bu instance'ı yeni row olarak algılıyor. Bu durumu çözebilmek için version sütunu kullandık. version sütunu eğer set edilmiş ise `JPA` bunu zaten kaydedilmiş bir instance olarak algılayacak. Bunun için şu config'leri de yapmamız gerekli:

```xml
<?xml version="1.0"?>
<!DOCTYPE hibernate-mapping SYSTEM
"http://hibernate.sourceforge.net/
hibernate-mapping-3.0.dtd">

<hibernate-mapping package="my.package">

  <class name="Person" table="PERSON">

    <id name="id" column="ID">
      <generator class="assigned" />
    </id>

    <version name="version" column="VERSION"
        unsaved-value="null" />

    <!-- Map Person-specific properties here. -->

  </class>

</hibernate-mapping>
```

Source:

- `http://www.onjava.com/pub/a/onjava/2006/09/13/dont-let-hibernate-steal-your-identity.html?page=1`
- `http://www.onjava.com/pub/a/onjava/2006/09/13/dont-let-hibernate-steal-your-identity.html?page=2`
- `http://www.onjava.com/pub/a/onjava/2006/09/13/dont-let-hibernate-steal-your-identity.html?page=3`

## 📌 hashcode and equals for Java.util.ArrayList

Her class için olduğu gibi her `collection` içinde `hashcode` ve `equals` metodunun nasıl çalışacağı o class'ın kendisine (implementasyona) bırakılmıştır.

`ArrayList`'in `hashCode` metodu listenin her elemanı sırası ile dolaşarak `hashCode`'unu alıp matematiksel bir işleme sokmaktadır. Dolayısı ile listedeki elemanların sırası farklı ise ama elemanlar aynı ise, `hashCode` farklı çıkacaktır. Buradan dökümana bakılabilir: <https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/List.html#hashCode()> (Kod örneği ile gösterilmiş).

Benzer şekilde `equals` metodu da aynı şekilde çalışır. Tabi `equals` matematiksel işlem yerine sırası ile her elemanın eşit olup olmadığını kontrol eder.
