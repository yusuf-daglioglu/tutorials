# LANGUAGE PARADIGMS

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Programming paradigm

`paradigm` `Türkçesi`: paradigma

`paradigma` kelime anlamı: 1-örnek, 2-bir şeyin nasıl üretileceği konusunda örnek/model

programlama dilinin özellikleridir: örneğin cephe yönelimli olması, object-oriented olması birer paradigmadır.

## 📌 multi-paradigm programming language

bazı programlama dillerinin bazı özellikleri birden fazla paradigmayı desteklemektedir.

## 📌 programlama dilleri jenerasyonları

- 1inci jenerasyon (1GL): 1 ve 0'lardan oluşan programlama dilleridir. 2inci dünya savaşında Nazi almanyasının şifreli mesajlaşma makinesi olan Enigma'yı çözebilmek için, Alen Turing, Turing makinesi ile 1940'larda ilk programlama mantığı ortaya atılmıştır. Fakat programlama dilleri olmadığı için manyetik teypler panel aracılığı ile kapatılıp açılıyordu (switch/toggle). bu şekilde sadece 1 ve 0'lar ile işlemler yapılıyordu. Bu sebeple bu jenerasyonun dillerinde kodu çevirmeye ihtiyaç yoktur. direk kod makinede işletilebilir durumdadır.

- 2inci jenerasyon (2GL): düşük seviyeli programlama dilleri. assembly. İlk 1950'lerde ortaya çıkmıştır. Bu jenerasyonda temel yenilik, 1 ve 0'ların insanlar tarafından okunmasının aşırı gereksiz ve zor olmasıdır.

- 3inci jenerasyon (3GL): yüksek seviyeli programlama dilleri. C, C++, C#, Java. Bu jenerasyonda temel yenilikler:
  - 2GL'nin daha çok cihazlara bağlı syntax'lar içermesinden kurtulmak
  - daha human-readable bir syntax yapmak
  - byte üzerinden yönetilebilen yeni bütünleşik veri tipleri oluşturabilmek. örnekler:
    - array
    - Class/obje
    - OS'un register'larının desteklemediği büyüklükte sayılar

- 4inci jenerasyon (4GL): diğer jenerasyondakiler gibi sadece bit, byte veya bunların üzerine kurulmuş temel veri tiplerini yönetmekle değil, direk olarak logical anlamda "veriler" kullanabilmektedirler. örneğin tablo üzerinde direk işlem sağlanabilmekte, ve ekrana GUI raporu alınabilmektedir. Örnekler: SPSS, Unix shell, SQL. Bazı araştırmacılar, bu jenerasyonun sadece DSL olduğunu düşünmektedir. Araştırmacılar bu jenerasyonun domain'e özel geliştirilen bir dil olduğunu düşünmektedir. örneğin; web development, DB manipulating/reporting, mathematical optimization, GUI development...

- 5inci jenerasyon (5GL): programcının algoritma geliştirerek çözüm geliştirmesinin ötesinde, koşulları ve kısıtları bilgisayara verdiğinizde, bilgisayarın çözümü kendisinin bulmasına yönelik olarak tasarlanmaktadır. Bazı araştırmacılar, bu tarz dillerin jenerasyon grubu altında olamayacağını belirtmekte.

- akademik kaynaklar 4 ve 5inci jenerasyonları çok net bir çizgi ile ayırmış durumda değil. 5inci jenerasyonun, bir jenerasyon olmaması gerektiğini düşünenlerde var.

- Bir dil, birkaç jenerasyona birden uyabilir.

- jenerasyon yükseldikçe, daha kolay şekilde donanımdan daha bağımsız uygulamalar yazılması sağlanır. "high level programming language" terimi üst seviyeli diller için kullanılır.

- en üst seviyede dahi donanımdan bağımsız veya donanıma bağımlı uygulama yazılabilir. çağırdığımız API (kütüphaneler) ve/veya dilin binary/bit bazında işlem destekleyip desteklememesi gibi özellikler sayesinde donanıma bağımlı uygulamalar en üst seviyede de yazılabilir.

## 📌 bazı diller

popüler yüksek seviyeli diller (çıkış tarihleri ile - yıllar draft veya stable olabilir):

- Fortran (1954)
- Lips (1956)
- COBOL (1959)

  Bu yıllarda varolan programlama dillerine göre şu avantajları vardı:
  - üst seviyeli portable dildi. diğer diller gibi donanıma bağlılıkları aşırı azdı.
  - okunabilir API'ler sunuyordu.
  - dosya işleme gibi işlemler için hazır API'ler gömülüydü.

  Bu sebeple banka gibi sadece business odaklı logic'lerde ve pazarı kaptı.

- `PASCAL` (1970)
- `Prolog`, `SQL`, `C` (1972)
- `Ada` (1980)
- `C++`, `Objective-C`, `ABAP` (1983)
- `MATLAB` (1984)
- `Perl` (1987)
- `Visual Basic`, `Python` (1991)
- `Lua` (1993)
- `R` (1993)
- `Java` (1995)
- `PHP` 1995
- `Ruby` (1995)
- `JavaScript` (1995)
- `VBScript` (1996)
- `ECMAScript` (1997)
- `ActionScript` (1998)
- `C#` (2000)
- `Visual Basic .NET` (2001)

  `Visual Basic` ile bu dil farklıdır. successor olarak yayımlandı.

- `Scala` (2003)
- `Groovy` (2004)
- `Go` (2009)
- `CoffeeScript` (2009)
- `Rust` (2010)
- `Kotlin` (2011)
- `TypeScript` (2012)
- `Swift` (2014)

## 📌 Yordamsal programlama dili (⟷ Procedural programming language ⟷ prosedürel programlama dili)

kod tekrarı engellemek amaçlı (goto/jump gibi terimler yazmaya gerek kalmadan) programın yordamlara bölünmesi ile yazılmayı destekleyen programlama dilleridir.

yordamsal programlama'da, programın dallandığı yerlere `subroutine (⟷ subprogram ⟷ callable unit)` adı verilir. dolayısı ile kullandığımız dilin diğer paradigmalarına göre "prosedure" şunlardan biri olacaktır: "function", "method".

fonksiyonel programlamadan temel farkı; fonksiyonel programlamada daha çok "pure" fonksiyonlar ile çalışırken, prosedürel'de genelde pure fonksiyonlar yoktur.

## 📌 object-oriented (⟷ nesneye yönelimli) vs object based (⟷ nesne tabanlı)

`JS` object-oriented değildir. nesne kullanır bu sebeple nesne tabanlıdır. fakat object-oriented dillerin desteklemesi gerektiği özellikleri destekleyecek şekilde özenle geliştirilmez. örneğin `JS`'te inheritance ve polymorphism yoktur. son çıkan `JS` sürümlerinde sınıflar gelmiştir. fakat bu seferde encapsulation yoktur.

bu iki kavram piyasada sürekli birbiri ile karıştırılmaktadır.

object-oriented'lar ikiye ayrılır:

### 📌📌 class based

sınıfları kullanarak implementasyon yapan dillerdir. (Java buna bir örnektir)

### 📌📌 prototype based

bazı kaynaklarda bu şekilde de isimlendirilir: prototypal, prototype-oriented, classless, instance-based programming

obje klonlayarak implementasyon yapan dillerdir. her obje türetilen objenin prototype'ını referans olarak tutar. bu yapının en güzel özelliği runtime sırasında prototype'a metot veya field eklenebilir. ve hatta prototype referansını tutan instance'lara da bu değişiklikler yansıtılabilir. (JS buna bir örnektir)

## 📌 4 major principles of object-oriented programming

Aşağıdaki 4 madde sadece en temel principle'lerdir. Birbirlerine çok yakın principle'lerdir.

`interface` veya `abstract` sınıflar ile `OOP`'nin aşağıdaki maddelerini gerçekleyebilmemizi sağlar.

not: her dilde; `Java`'daki gibi hem `interface` hem de `abstract` sınıf kavramları birden olmayabilir.

### 📌📌 encapsulation (⟷ kapsülleme)

2 temel kavramdan oluşur:

- Information hiding

  modifier'lar (public, private...) gibi keyword'ler aracılığı ile bilgiyi erişime kısıtlayabilmemizdir.

- Packaging

  paket(ler) veya sınıf(lar) içerisinde tüm bilgilerin/metotların, bir unit olarak dışarıya sunulmasıdır..

### 📌📌 Abstraction (⟷ soyutlama)

sınıf içerisindeki detayların (property'lerin/metotların), dışarıdan sınıfı çağıranlar tarafından bilinmesine gerek kalmamasıdır.

### 📌📌 Inheritance (⟷ kalıtım)

sınıfların bir yada birden fazla yerden türeyebilme özelliğidir.

### 📌📌 Polymorphism (⟷ polimorfizm ⟷ çok biçimlilik)

türeyen sınıflar, süper sınıfın metotlarını isterse override edebilmesidir.

## 📌 fonksiyonel programlama (⟷ functional programming)

fonksiyonel programlama genelde;

- (Immutable) Veri Yapıları vardır. yani; veriyi değiştirmek yerine kopyalayarak yeni versiyonlar üretildiği,

  Not-1: Object-oriented'ta ise, bir nesnenin durumu sürekli değişir.

  Not-2: Bunun sonucu olarak aynı nesneni state'i değişmeyeceği için, `for loop` gibi döngüler yerine rekürsif fonksiyon çağırmaları daha sıkça tercih edilir.

- pure fonksiyonlar ile çalışıldığı,

- fonksiyonlar fonksiyonlara parametre olarak geçilebildiği, return edilebildiği, bir variable'da tutulabildiği, (bu madde fonksiyonel olmanın bir zorunluğu değil, ama %90 tercih edilen bir durumdur)

dillerdir.

Burada "multi-paradigm programming language" başlığında belirtildiği gibi her dilin birden fazla özelliği desteklediğini hatırlatmak gerekli. Aynı zamanda yordamsal, fonksiyonel, object-oriented terimleri programlama dilinin bir özelliği değildir. aslında bir yazım tarzıdır.

tabi ki de eğer bir dilde nesne yapısı hiç yoksa object-oriented tarzda uygulama yazmak mümkün olmayacağı için bu terimler sanki dilin bir özelliğiymiş gibi anlatılıyor. oysa değil.

## 📌 Olaya dayalı programlama (⟷ olay güdümlü programlama ⟷ event-driven programming)

GUI, donanım sinyalleri gibi olaylar sonrası aksiyon alan programlama dilleridir. aslında tüm programlama dilleri kütüphaneleri desteklediği sürece event bazlı olabilir. fakat genelde ağırlıklı olarak event'lere dayalı programlar için bu terim kullanılır. örneğin JS. sadece son kullanıcı sayfada bir yere tıkladığında yada sayfa yüklediğinde bir aksiyon alınır. onun dışındaki aksiyonlar nadirdir. zaman tetiklemeleri mevcuttur. belirli zamanda event tetiklenir. timeout metotları gibi...

örneğin komut satırı ile işlem yapan script'ler event bazlı değildir.

## 📌 declarative programming

declaration Türkçe kelime anlamı: beyan etmek, bildirmek

declarative diller işin nasıl yapıldığına karışmaz. sadece ne yapılması istendiğini belirtir. örnek: SQL, CSS, HTML, Java'daki annotation'lar...

## 📌 imperative programming

imperative Türkçe kelime anlamı: zorunlu, mecbur

declarative'in tersidir. nasıl olacağına tam olarak karışır ve sonuçta ne istediği dilin kendisini ilgilendirmez.
