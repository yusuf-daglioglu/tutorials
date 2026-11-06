# PRINCIPLES AND LAWS SOLID

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 SOLID Principles

object-oriented için çıkmış 5 tane principle bir arada bulunduran bir terimdir (baş harflerini birleştirilmiştir).

### 📌📌 Single Responsibility Principle (⟷ SRP ⟷ Tek Sorumluluk Prensibi)

Bir sınıfın veya metodun tek bir sorumluluğu olmalıdır. eğer metodun/sınıfın değişmesi gerekiyorsa; muhtemelen ona yeni sorumluluklar mı eklenecek şüphesi yaratmalıdır.

### 📌📌 Open/closed principle (⟷ OCP)

code/software should be open for extension, but closed for modification. yani; yeni eklemeler yapıldığında programda minimum değişiklik yeterli olabilmelidir. örneğin; online ödemeler. yeni bir ödeme geldiğinde yeni ödemeyi bir sınıftan implemente edip bir listeye eklememiz yeterli olabilmelidir.

Farklı bir değişle; yeni geliştirme geldiğinde; var olan kodumuzu az düzenlemeliyiz fakat yeni sınıflar yaratmalıyız. Bunu ne kadar iyi yapabiliyorsak, kodumuz `OCP`'ye o kadar yakındır.

2 gerçek örnek:

- `Observer` pattern

  Yeni observer class'ları yarattığımızda, subject'i değiştirmeye gerek kalmıyordu.

- `ödeme sitemleri`

  e-ticaret web sitemizde yeni bir ödeme çeşidi gelince yeni sınıf (implementasyon) eklenir. diğer ödemeler veya kodlar değiştirilmez.

- `JDBC-driver`

  Yeni driver eklediğimizde, sistemde daha önceden kullanılan driver'ları hiç değiştirme gereği duymuyoruz.

### 📌📌 Liskov'un Yerine Geçme Prensibi (⟷ Liskov Substitution Principle ⟷ LSP)

Alt sınıflardan oluşan nesnelerin, üst sınıfın nesneleri ile yer değiştirdikleri zaman, aynı davranışı sergilemesi gerekmektedir.

Bu kuralı şu gibi durumlarda ihlal ediyoruz demektir:

- implementasyonlarımızda not-implemented hatası fırlatan metodumuz varsa,
- online-ödemelerde `öde()` fonksiyonumuz kredi-kartı için her daim ödeme yapıyor, ama `kripto-para` implementasyonundaki `öde()` fonksiyonumuz 500 `TL` üzeri durumda `yüksek fiyatlı siparişleri, kripto ile ödeyemezsiniz` uyarısı fırlatıyorsa (bir diğer değiş ile, metodun davranışı bazı durumlarda değişiyor ise).

Yukarıdaki online ödemeler örneğindeki gibi bir durumda 2 farklı süper class olması daha verimli olacaktır:

- `RiskliOnlineÖdemeler`
- `RisksizOnlineÖdemeler`

Not aslında bu şekilde 2 interface yapılması `ISP` başlığının konusudur. Bu durum `ISP vs LSP` başlığında da anlatılmaktadır.

### 📌📌 Interface Segregation Principle (⟷ ISP ⟷ Arayüz Ayrımı Prensibi)

Direk örnek üzerinden gidelim: Mesaj arayüzümüz olsun. bunu implemente eden bir MesajImpl sınıfımız olsun. MesajImpl kendi içinde `attachmentVideo`, `attachmentPhoto` gibi özellikler içerecek. fakat her mesajda video olmayabilir. sadece foto olabilir. bu sebeple burada önerilen; birden fazla `MesajImpl` olsun ve her biri video için yada photo için spesifik geliştirilsin. (bir mesajda hem foto hem video olamayacağını varsayarak örnek verilmiştir). bu şekilde her implementasyon sadece tek bir işe odaklı yani; arayüzün metotlarını implemente etmeye odaklı olmalıdır. böylece kullanılmayan metotlar çıkarılmış olacaktır.

### 📌📌 ISP vs LSP

Bu 2'si aynı kapıya çıkmaktadır. `ISP` arayüzlerin, gruplanıp tek bir arayüz haline getirilmemesini anlatırken, `LSP` arayüz, yerine subclass'lardan birini çağırınca aynı davranışların sergilenmesini beklemeliyiz der. Bu iki principle'dan birini tamamen düzgün şekilde yaptığımızda, diğer principle'ı da düzgün uygulamış oluruz.

### 📌📌 Bağımlılığın Ters Çevrilmesi Prensibi (⟷ Dependency Inversion Principle ⟷ DIP)

iki kuralı vardır:

#### 📌📌📌 High-level modules should not depend on low-level modules. Both should depend on other high levels

__Örnek-1:__

`Good design`: shopping servisi (high level module), ödeme servisini (high level module) çağırarak işlem yapıyor.

`Bad design`: shopping servisi (high level module), `CreditCard` (low-level module) servisini direk çağırıyor.

__Örnek-2:__

`Good design`: shopping servisi (high level module), DB servisini (high level module) çağırarak işlem yapıyor.

`Bad design`: shopping servisi (high level module), `mysql` servisini (low level module) çağırarak işlem yapıyor.

```java
public abstract Shopping {

    // Bad design example:
    private MySqlDb mySqlDb = new MySqlDb();

    // Good design example:
    private DB db = new MySqlDb();

    public pay(){

        // payment operations...

        // log the result to DB at the end of the payment

        // bad design:
        mySqlDb.insert("payment finished");

        // good design:
        db.insert("payment finished");
    }
}
```

#### 📌📌📌 Abstractions should not depend on details. Details should depend on abstractions

```java
// ExternalMessageProcessService class is abstraction.
public abstract ExternalMessageProcessService {

    boolean httpParserLogException;

    public processMessage(Message message){

        // Example-1 for bad design
        if(httpParserLogException == true){
            processMessageWithoutLog(message);
        } else {
            processMessage(message);
        }

        // Example-2 for bad design
        if(Type.KAFKA == getType()){
            log.warn("prefer Http Service which because it is faster (or any warning here about implementation!)")
        }
    }

    public Type getType();
}

// KafkaMessageProcessService class is detail.
public abstract KafkaMessageProcessService implements ExternalMessageProcessService {

    public Type getType(){
        return Type.KAFKA;
    }

    public processMessage(KafkaMessage message){
        // code here...
    }
}

// HttpMessageProcessService class is detail.
public abstract HttpMessageProcessService implements ExternalMessageProcessService {

    public Type getType(){
        return Type.HTTP;
    }

    public processMessage(HttpMessage message){
        throw new Exception("Unimplemented!");
    }

    public processMessageWithoutLog(HttpMessage message){

        // code here...
    }
}
```
