# JAVA SERIALIZERS

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 GSON

Java için Java objelerini JSON'a çevirip tekrar Java objesine çevirme kütüphanesidir.

## 📌 JAXB (Java Architecture for XML Binding)

- Java içinde gelen XML kütüphanesidir.
- `javax.xml.bind.*` paketleri kümesini kapsar.
- Java 9 ile deprecated olarak işaretlendi, 10 ile tamamen kaldırıldı.
- `marshalling` ve `unmarshalling` yapar ve verilen XML şemasına göre Java objeleri oluşturur.
- `marshalling` işlemi sonrası bir XML oluşur. bu XML hem kolay okunabilirliği de sağlar, aynı zamanda üzerinde `un-marshalling` yapmadan önce değişiklikler de yapılmasını kolaylaştırır. oysa serializable'da bir nesne dosyaya dönüştürülmüşse artık değişiklik mümkün değildir.

## 📌 Jackson

asıl öncelikleri JSON olan, fakat ekstradan XML gibi birçok formatı ayrıştıran, üreten, işleyen bir Java Kütüphanesidir.

- `2.x` sürümünden eski sürümler `org.codehaus.#` paketi altında dağıtılıyordu. 
- `2.x` sürümü ise `com.fasterxml.*` altında dağıtılıyor.

```java
@JsonPropertyOrder({"age", "id", "name"})
public class Person {

    @JsonProperty("_id")
    private String id;

    private String name;

    private int age;

    @JsonIgnore
    private String note;
}
```

Jackson 3 farklı alt projeden oluşuyor:

- `Streaming (jackson-core)`

  `XML` ve `JSON` parse etme gibi işlemleri yapan core kodlar mevcut. alttaki tüm alt projeler bu projeye depend eder.

- `Annotations (jackson-annotations)`

  sadece annotation'lar ayrı olarak bu projede barındırılıyor. annotation kullanmak istemeyenler bu projeyi classpath'te bulundurmak zorunda değil.

- `Databind (jackson-databind)`

  `ObjectMapper` ve `XmlMapper` gibi sınıfların bulunduğu paket.

Yukarıdaki tüm dependency'leri tek bir `Maven` paketinden bulabiliriz:

```xml
<dependency>
    <groupId>com.fasterxml.jackson.dataformat</groupId>
    <artifactId>jackson-dataformat-xml</artifactId>
    <version>2.10.0</version>
</dependency>
```

Yukarıdaki `Maven` dependency'si sadece `XML` formatı için `Objectmapper` türevini içerir. `jackson-dataformat-xml` yerine buradaki formatlardan birini tercih edebiliriz: https://github.com/FasterXML/jackson-dataformats-binary

### 📌📌 ObjectMapper vs XmlMapper

`ObjectMapper`, `JSON` işleme metotlarının bulunduğu API. `XmlMapper` ise projeye sonradan eklendi. `XmlMapper`, `ObjectMapper`'dan türemiş bir sınıf. tüm metotları override edilmiş ve sadece `XML` işleme için kullanılmak için tasarlanmıştır.

### 📌📌 Modules

`Jackson` 3üncü parti module altyapısını destekler. modüller araya girerek birçok ek özellik katabilir. modülleri `Jackson`'a register etmemiz gereklidir:

```java
ModuleX moduleX = new ModuleX();
objectMapper.registerModule(moduleX);
```

Eğer manuel registration yapmaz isek bu metot ile classpath'te bulunan tüm metotları oto register yapabiliriz:

```java
ObjectMapper.findAndRegisterModules();
```

`Jackson` wiki'lerinde `Core modules` kısmında, yukarıda belirttiğimiz 3 proje gösteriliyor: `Streaming`, `Annotations`, `Databind`. fakat bunlar birer modül gibi register etmek gerekmiyor.

Bazı modüller aşağıdaki repo'larda geliştiriliyor. her repo'nun içinde birden fazla modüle mevcut.

- https://github.com/FasterXML/jackson-modules-base
- https://github.com/FasterXML/jackson-modules-java8

Bazı modüller:

#### 📌📌📌 Jackson Module JAXB Annotations

`JAXB`, `Jackson`'a alternatiftir. önceden `JAXB` kullanıyorsak, `JAXB`'nin annotation'larını bazı Java `Bean`'lerimize atmış olabiliriz. bunları `Jackson`'ın da algılamasını istiyorsan, bu projeden yararlanmamız gerekir.

```xml
<dependency>
<groupId>com.fasterxml.jackson.module</groupId>
<artifactId>jackson-module-jaxb-annotations</artifactId>
<version>2.4.0</version>
</dependency>
```

#### 📌📌📌 jackson-datatype-jdk8

JDK 8 ile gelen optional değerler için support sağlıyor. örnek: serialize edeceğimiz sınıfımızda bu değere destek verebiliriz:

```java
class Person {
    private final String name;
    private final Optional<String> email;
```

#### 📌📌📌 jackson-datatype-jsr310

Java 8 ile gelen `java.time.*` kütüphanesinin serileştirmesine yardımcı olur.

```xml
<dependency>
    <groupId>com.fasterxml.jackson.datatype</groupId>
    <artifactId>jackson-datatype-jsr310</artifactId>
</dependency>
```

## 📌 Jackson annotations

### 📌📌 @JsonAnyGetter

```java
public class Person {
    public String name;
    private Map<String, String> myMap;

    @JsonAnyGetter
    public Map<String, String> getMyMap() {
        return myMap;
    }
}
```

output:

```json
{
    "name": "Jack",
    "prop1": "val1",
    "prop2": "val2"
}
```

### 📌📌 JsonAnySetter

`JsonAnyGetter`'ın deserialize sırasında kullanılan versiyonu olarak düşünebiliriz.

```java
public class User {

    public String name;
    private Map<String, String> properties;

    @JsonAnySetter
    public void add(String key, String value) {
        properties.put(key, value);
    }
}
```

```json
{
    "name": "Jack",
    "prop1": "val1",
    "prop2": "val2"
}
```

### 📌📌 @JsonGetter

```java
public class Person {
    public int id;
    private String name;

    // defines that this method is getter for the "name" field.
    @JsonGetter("name")
    public String getTheName() {
        return name;
    }
}
```

`@JsonSetter("name")` deserializer sırasında kullanılan, `setter` için geliştirilmiş bir versiyonudur.

### 📌📌 JsonPropertyOrder

```java
// the output of JSON is in that order.
@JsonPropertyOrder({ "name", "id" })
public class Person {
    public int id;
    public String name;
}
```

### 📌📌 JsonRawValue

```java
public class Person {
     public String name;

    @JsonRawValue
    public String json;
}
```

output:

```json
{
    "name": "Jack",
    "json":{
        "hello": "world"
    }
}
```

### 📌📌 JsonValue

```java
public class Person {
    public String name;

    @JsonValue
    public String description;
}
```

Only "description" will be serialize.

output:

```json
{
    "description": "this is admin"
}
```

### 📌📌 JsonRootName

```java
@JsonRootName(value = "user")
public class Person {
    public String name;
    public String description;
}
```

Only "description" will be serialize.

output:

```json
{
  "user": {
      "name": "Jack",
      "description": "this is admin"
  }
}
```

### 📌📌 JsonSerialize

```java
public class User {

    public String name;

    @JsonSerialize(using = CustomDateSerializer.class)
    public Date eventDate;
}
```

`@JsonDeserialize(using = CustomDateDeserializer.class)` yukarıdakinin deserializer'ında kullanılan versiyonudur.

### 📌📌 JacksonInject

```java
public class User {

    @JacksonInject
    public int id;

    public String name;
}
```

Now we can inject value to `id` from Java code:

```java
String json = "{\"name\":\"Jack\"}";

InjectableValues inject = new InjectableValues.Std().addValue(int.class, 1);
BeanWithInject bean = new ObjectMapper().reader(inject)
                            .forType(BeanWithInject.class)
                            .readValue(json);
```

### 📌📌 JsonAlias

Farklı JSON dosyalarını aynı Java objesine map edebilmek için aşağıdaki gibi `JsonAlias` kullanabiliriz.

```java
public class User {

    public int id;

    @JsonAlias({ "firstName", "f_name" })
    public String name;
}
```

### 📌📌 JsonTypeInfo

```java
@JsonTypeInfo(
      use = JsonTypeInfo.Id.NAME,
      include = As.PROPERTY,
      property = "my_class_type")
    @JsonSubTypes({
        @JsonSubTypes.Type(value = Dog.class, name = "dog"),
        @JsonSubTypes.Type(value = Cat.class, name = "cat")
    })
public class Animal {

    // some code here...
}
```

`Animal` class'ımızdan, `Dog` ve `Cat` ve başka sınıflar türemiş olsun. Türeyen sınıflarda farklı field'lar olabilir. Bu sebeple o field'ların ignore edilmemesini istiyorsak, yukarıdaki tanımlamayı yapmak zorundayız. Çünkü eğer bu tanımlamayı yapmazsak, JSON'dan hangi türemiş instance'ı yaratacağını `Jackson` bilemez.

Yukarıdaki tanımlama örneğinde sadece 2 sınıf tanımladığımız için, sadece `Cat` veya `Dog` deserialize edilebilir.

JSON okunurken `my_class_type` tipinden `Jackson` hangi class'a çevrilmesi gerektiğini anlıyor.

`my_class_type` seçenekleri:

- `JsonTypeInfo.Id.NAME` kullanmışsak; `@JsonSubTypes.Type`'daki `name` parametresinde yazan kısım.
- `JsonTypeInfo.Id.CLASS` kullanmışsak; `com.mypackage.Dog`.
- `JsonTypeInfo.Id.CUSTOM` kullanmışsak; `Jackson`'da olmayan bir custom eklentiye göre karar veriliyor.

`include` seçenekleri:

- `As.PROPERTY`: `my_class_type`'ın JSON objesinde direk olması gerektiği anlamına geliyor.
- `As.EXTERNAL_PROPERTY`: `my_class_type`'ın JSON objesinde bir üst seviyedeki JSON objesinde olması gerektiğini belirtiyor.
- `As.EXISTING_PROPERTY`: `As.PROPERTY` ile aynı. Tek farkı `my_class_type` `Animal` class'ımızda da bir `Java` `String`'i olması durumunda bu annotation kullanılır.
