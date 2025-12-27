# BLOCKCHAIN AND CRYPTOCURRENCY

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 blockchain

bir çeşit kayıt (DB) yönetim modelidir.

`Blockchain` içerisinde tutulacak verinin tipi (`blockchain` yapısı gereği), para transfer işlemleri kaydı, activity log gibi olmalıdır (`İngilizce`'de buna "`continuously growing list of records`" diyebiliriz). Geri silinmeyecek veya değiştirilmeyecek bilgiler olmalıdır. bir kaydın update edilmesi veya silinme bilgisi de yeni kayıt olarak gelmesi business tarafından kabul edilebilir ise, `blockchain` kullanılması uygundur.

`blockchain` dağıtık olmak zorunda değildir. fakat dağıtık olması, farklı otoritelerin oluşunu sağladığı için, tekil-otorite kaygısını bi nebze ortadan kaldırmaktadır.

`blockchain` için merkeziyetçilik diye bir nitelik olamaz. `blockchain` sadece DB'ninbilgileri nasıl tuttuğunun bir özelliğidir. merkeziyetçilik, ona bilgiyi kaydeden yazılımlar/process'ler için geçerli bir niteliktir.

`blockchain`'de her kayıt bir önceki kaydın hash'ini tutuyor demiştik. burada her kayıt 1 kayıt olmak zorunda değil. örneğin; Bitcoin'de her 100 kayıt bir blokta tutuluyor, ve her blok bir önceki blokun hash'ini tutuyor.

Bazı kulanım alanları:

- oylama sistemi. herkesin oyları digital imzalı şekilde merkezi olmayan bir sistemden görülebilir olacak. üst üste kayıtlar olduğundan dolayı, kimse eski kayıtları değiştiremeyecek. bu şekilde kimse oy çalamayacak.

- para transferi.

- `smart contract`

  2 şirket, yada 1 şirket bir insan, yada birçok şirket bir ortak anlaşma yapacak. bu ortak anlaşma karşılıklı olarak kendi `private key`'leri ile onaylanıp, `blockchain`'e programatik ifadeler içeren dosya olarak yazılabilir. bu dosyadaki programatik bilgiler zamanı geldiğinde, programatik olarak otomatik olarak işleme alınabilir.

- repository (like `git`)

  `Git`'te benzer bir yapı kullanıyor. fakat `Git`'te block'lar yok ve tüm kayıtlar direk olarak birbirinin ardına ekleniyor. ardı ardına eklenirken, eski kaydın sadece referansı (adresi) tutuluyor. eski kayıtta biri bir şey değiştirebilir. `Git`, `Linked List` mantığında çalışıyor.

- haber yayın kaynakları

  herkes kendi `private key`'i ile merkezi olmayan sisteme haber atabilecek. bu şekilde herkes haberin kimden geldiğini kesin olarak bilebilecek/güvenebilecek.

- noter işlemleri

  iki taraf karşılıklı onayladığı dökümanları `blockchain`'e yollar. artık ikisinin onayladığı gerçeği kimse tarafından değiştirilemez. eğer bu sistem resmi olarak devlet tarafından tanınırsa, notere gerek kalmaz.

- lojistik takip

  bir ilacın bir lokasyona yollanmak istediğini düşünelim. aracı olan her firma `private key`'lerle her aşamayı onayladığını varsayalım. erişimin hangi adımlarda nerede olduğunu adım adım takip edebiliriz. takibin özelliği her aşamanın garanti olması ve herkes tarafından onaylanarak yapılmasıdır. hiçbir aracı diğer network'te node'lardan onayı almadan bir sonraki aşamaya geçmeyecek. eğer onay alıp geçti ise geriye dönük kontrollerde ben bunu onaylatmamıştım diyemeyecek. bu işi `blockchain`'siz yaptığımızı düşünelim, yine bir takip sistemi yapabiliriz, fakat geriye dönük log'ların garantisi yoktur. sistemler hack'lenebilir/değiştirilebilir.

## 📌 genesis

kelime anlamı: yaratılış

`blockchain` DB'deki ilk veri kümesine verilen isimdir.

## 📌 sanal para (⟷ virtual currency) vs digital currency (⟷ dijital para) vs cryptocurrency (⟷ kripto-para)

3'ü farklıdır.

`sanal para` herhangi bir oyunda bile kullanılabilen paradır. Dijital olmak zorunda değildir. Fakat günümüzde genel herşey dijitalde olduğu için, `sanal para` da dijital tasarlanıyor.

`crypto (⟷ tr:kripto)` teriminin sözlük anlamı `gizli olan` ile eş anlamlıdır. `Cryptography (⟷ cryptology ⟷ tr:kriptografi)` bir veriyi gizlemek için inceleme yapan bilim dalıdır. `kripto-para` ise özel olarak şifreleme tabanlı koruma sistemi ile kullanıma açılmış para çeşididir.

örneğin; `Dolar` yada `TL` bir `kripto-para` değildir. çünkü korunmaları, para birimlerinin teknolojisinin altında yatan bir şifreleme ile sağlanmamaktadır. bankaya giriş yaptığımızda kullandığımız şifreler para sisteminin değil, bankanın paradan bağımsız kendi özel koruma sistemi ve devletlerin hukuksal korumalarıdır.

## 📌 Bitcoin cüzdan ve blockchain yapısı

### 📌📌 Bitcoin wallet (⟷ Bitcoin cüzdan)

`Wallet = address + (private key) + (public key)`

her bir `private key` adres ve `public key` sadece birer adet olacak şekilde birbiri ile ilişkilendirilmiştir.

`public key`'in `hash`'i (birden fazla `hash` işlemi var) alındığında `address` elde edilir.

### 📌📌 (Bitcoin için) transaction işlemi

`transaction` işlemi için cüzdan sahibi, diğer tüm node'lara bu bilgiyi yollar:

```text
adres + public_key + private_key_ile_imzala("yollayacağı ücret", "hangi adrese gideceği", ...)
```

network'teki node'lar eğer bilgiler uyuşuyorsa, işlemin kaydı `blockchain`'e kaydedilir ve sıradaki işlem kontrolüne geçilir.

### 📌📌 (Bitcoin için) input vs output

her `transaction` en az bir `input` ve bir `output` içermelidir. Client, 1 `transaction`'daki `input`'ları ve output'ları, `private key`'i ile imzalayarak `Bitcoin` network'üne yollar.

her 1 `input`, 1 adet source `address`'tir. yani `Bitcoin`'in hangi `address`'ten çekileceğidir.

her 1 `output` ise, `Bitcoin`'in transfer edileceği adrestir.

source `address`'lerdeki tüm para harcanmak zorundadır. eğer source'taki paranın bir kısmını harcayacak isek, o zaman output listesinde, kendi source adresimizi de bulundurmak zorundayız.

bir `output` eğer hiçbir farklı yere `input` olarak eklenmediyse, __unspent transaction outputs (⟷ UTXO)__ olarak kalır.

### 📌📌 UTXO-based vs Account-based

2'side `blockchain` tabanlı DB üzerinde tutulan mantıksal modellerdir.

`UTXO`-based sistemler, `BTC` ve bazı fork'ları tarafından kullanılan modeldir. Account-based ise `Ethereum` ve bazı fork'ları tarafından kullanılan modeldir.

`UTXO` DB'de, `address`'ler tamamen sanaldır. Herhangi bir `UTXO`'da olan `address`'e ait bir `private key`'imiz var ise, o `private key`'i, son kullanıcı, hesapmış gibi algılamaktadır.

Oysa hesapların listesi özel olarak ayrı bir yerde listelenmemektedir. Her `UTXO` içerisinde 1 adres vardır. Eğer `UTXO` harcanmamış ise, herhangi bir client, network'e harcama isteği yollayabilir.

`Bitcoin`'de, tüm transaction (`input` ve `output`'lar) blockchain'e kaydedilir. Böylece history, sürekli valide edilebilmiş kalır. Fakat son kullanıcı işlem yapmak istediğinde, son kullanıcının adresini tüm blockchain'deki `output`'larda aramak gerekir. Sonrada o `output`'un `UTXO` mi; yani harcanmış olup olmadığını da, tüm diğer `input`'ları kontrol ederek valide etmek gerekir. Bu işlem aşırı uzun süreceğinden, `Bitcoin` mine eden node'lar, ayrı bir indexed-DB'de sadece `UTXO`'leri tutar. Burada ikincil DB'lerinin nasıl çalışacağı ve hangi DB olacağı gibi konular, tamamen `Bitcoin` node'unun implementasyonuna bırakılmıştır. remsi bir yöntem yoktur.

### 📌📌 yeni Bitcoin'ler nasıl üretiliyor

mining yapanlara transaction fee'lere ek olarak, yeni ödül (`reward`) `Bitcoin` transfer edilir.

`reward` `BTC` transferleri için `input` yoktur. `reward` için yapılan transaction'a `coinbase` denir.

### 📌📌 (Bitcoin için) HD wallet (⟷ Hierarchical Deterministic wallet)

genelde kullanıcılar birden fazla cüzdan kullanır. bu sebeple `HD wallet` çözümüne gidilmiştir. kullanıcı bir passphrase yazar. bunu yedeklemesi yeterli olacaktır. passphrase ile `Bitcoin` uygulaması (`hd wallet` desteği olan uygulama) bir master key yaratır. son kullanıcı yeni bir adres istediğinde; uygulama bu key'in sonuna `+1` ekler. bu yeni key artık yeni oluşturulacak cüzdanın `private key`'i olarak kabul edilir ve yeni cüzdan açılır. `+2` eklenince başka bir cüzdan daha açılır. son kullanıcı sadece passphrase ve kaç adet cüzdan oluşturduğunu bilmesi yeterli olacaktır.

`HD wallet`, `Bitcoin` protocol'ünün verdiği bir özellik değildir. onun önündeki yazılım katmanında son kullanıcıya sunulan bir trick işlemdir.

### 📌📌 (Bitcoin için) adresin otomatik değişmesi

online cüzdanlar yada bazı client uygulamalar cüzdandan bir işlem gerçekleştirince otomatik olarak adresi değiştirir. bu şekilde gizliliği arttırmış olur. bu `Bitcoin` protocol'ünde olan bir durum değildir. tamamen yazılım katmanında kolaylık için geliştirilmiş bir yetenektir.

### 📌📌 passphrase

`Türkçe` karşılığı paroladır.

fakat `passphrase`; parola amaçlı kullanılan; kelimelerden (human-readable veya mnemonic phrase) oluşan uzun bir `string` anlamına gelmektedir.

### 📌📌 (Bitcoin için) seed

`Türkçe` kelime anlamı: tohum.

`Seed` kelimesi `HD wallet`'larda başlangıç noktasını temsil eder. yani; `passphrase`'e karşılık gelir.

### 📌📌 (Bitcoin için) watch-only address

adresler yedeklenirken `public key`'i de saklamak iyi olur. `public key` ve address bilgisi ile cüzdan takip edilebiliyor. dolayısı ile güvenmediğiniz bir bilgisayarda sadece `public key`'i bir programa import ederek takip edebiliyoruz. `HD wallet`'lar için bir 'master `public key`' vardır. bununla tüm alt cüzdanlar takip edilebilir. fakat sadece bir tane alt cüzdan takip edilmek isteniyor ise sadece o alt cüzdanın `public key`'ini export etmek gereklidir.

### 📌📌 (Bitcoin için) Multisignature wallet (⟷ multisig wallet)

bu özellik `Bitcoin` tarafından native olarak desteklenir. `M-of-N transaction`'ların yapılabilmesi sağlanır. birden fazla kişinin cüzdan üzerinden işlem yapılabilmesi için onaylaması gerekir. bu kişilerin her birinde birer `private key` bulunur. kullanım senaryosu örneği; bir şirket elemanının şirket `Bitcoin` özel hesabından işlem yapması için onay isteyebilmesi gerekebilir.

## 📌 Replace by Fee

bu bir transaction özelliğidir ve transaction yapılmadan önce set edilmiş olmalıdır. bu özellik var ise; transaction onaylanmamış ise, kullanıcı `fee (⟷ ücret)`'yi arttırabilir dinamik olarak. bunun için `Electrum`'da preferences'tan transaction'ı yapmadan önce bu özelliği açmak gerekiyor.

mining yapan insanlar `fee`'yi fazla veren transaction'lara öncelik verirler.

transaction başlatılmadan önce istediğiniz miktarda `fee` verebilirsiniz.

çok fazla uzun kuyrukta kalan ödemeler bir süre sonra iptal edilir ve hesaba geri yansıtılır (transaction geçersiz sayılır).

## 📌 CPFP (⟷ child pays for parent)

az `fee`'li bir transaction düşünelim ID'si `A` olsun. `A` hiçbir şekilde alıcıya ulaşamıyor. sürekli beklemede. alıcı bu parayı alabilmek için şu yolu izler: onaylanmamış bile olsa kendi cüzdanı üzerinden bu paranın bir kısmı ile bir harcama yapmaya kalkar. bu yeni transaction'ın komisyonunu yüksek tutar. miner'lar bu transaction'ı onaylamak isteyecek çünkü yüksek komisyonu var. fakat bu adamın cüzdanındaki parayı harcayabilmesi için öncelikle diğer (child) işlemin onaylanması gerekmektedir. CPFP desteği olan miner'lar bu tarz transaction'ları görebiliyor ve destek veriyor. dolayısı ile önce child işlemi (ID'si A olan işlemi) onaylamak zorunda kalıyor.

## 📌 Satoshi Nakamoto

Bitcoin'in yaratıcısı olarak bilinen kişi/grupların kullandığı isimdir.  daha öncesinde `blockchain` ile ilgili çalışmalar vardı. fakat Satoshi makalesinde `blockchain` mantığını ilk defa proof-of-work özelliği ile birleştirmiştir. bu makalesinde Bitcoin implementasyonunu da ortaya çıkarmıştır.

## 📌 Satoshi client (⟷ Bitcoin-Qt ⟷ Bitcoin core)

Referans Bitcoin client uygulamasıdır. Tüm `blockchain` zincirini indirebilmemizi sağlıyor. Piyasadaki diğer bazı client'lar tüm zinciri indirmeden de para transferi işlemleri yapabilmemiz sağlıyor. Bu zincirin lokalde olmasının tek avantajı: para transferi doğrulama işleminin sadece başkaları tarafından yapılmayacak olmasıdır. Yani lokaldeki zincirde de onaylanması gerekecek işlemin.

## 📌 Satoshi

Bitcoin'in ufak bir miktarına verilen özel isim.

`1 Satoshi = 0.00000001 ฿`

`0.001 ฿ = 1 mBTC`

`0.01  ฿ = 1 cBTC (Bitcent)`

## 📌 (Bitcoin için) Simplified Payment Verification (⟷ SPV)

Tüm zinciri lokale indirmeden yapılan transfer işlemidir. Daha güvensizdir. Bu transfer işleminin bir niteliğinin ismidir. her desktop uygulama spv desteklemeyebilir. bazıları zorunlu olarak full-node olacak şekilde tasarlanmıştır.

## 📌 (Bitcoin için) muhasebe defteri (⟷ ledger)

Her işlemin tutulduğu DB zinciri. `blockchain` tabanlı.

## 📌 (Bitcoin için) Mining (⟷ madencilik)

blockchain zinciri block'lardan oluşuyor. Bu bloklar kendi içinde belli adet transaction işlemi listesi barındırıyor. Yeni bir blok eklenmesi için birinin (bir client node'un) işlem yapması gerekmekte. İşte bu işlem karşılığında Bitcoin hediye edilmekte. Bitcoin hediyesi node'un tanımladığı cüzdana para transferi olarak geçmekte.

Yeni bir blok oluşturmak için eski bloğun hash'ini almak gerekiyor. Tabi bu kolay olduğundan ekstra olarak; random bir sayı ile bir rakam tutturulmaya çalışılıyor (bunun zorluk derecesi her geçen gün artıyor). İlk bulan node Bitcoin hediyelerini kendine transfer ediyor. Ve yeni bir blok oluşup zincire ekleniyor. Artık her para transfer işlemi bu bloğa kendini kaydediyor. Yani mining olmazsa hesaplama olmaz ve yeni blok zincirleri eklenmez. Hash alma işlemleri güçlü CPU istediği için mining; bir teşvik olarak sunulmaktadır.

hash alma işlemi zaman alıcı olduğundan, bu süre içerisinde yapılan transferler onaylanmadan bekletilmiş oluyor. para transferi tamamiyle onaylanmadan, gönderimi tamamlanmış para transferleri her zaman risklidir.

## 📌 Proof of Work (⟷ PoW ⟷ Proof-of-Work)

mining işlerinde kullanılan temel sistemin felsefesinin genel adıdır. Bitcoin'de şu şekilde işliyor:

```java
String calculateHash() {

    String newHash = sha256("zincirin son bloğunun hash'i", date, data, randomString() );

    return newHash;
}

if( newHash.startsWith("000") ){

    // newHash is valid !

} else {
    calculateHash();
}
```

Yukarıdaki kodda; hash'in ilk 3 karakteri sıfır mı diye kontrol ediliyor. zorluk derecesi arttığında belki ilk 4 karakteri kontrol edilecek. gibi...bunun gibi birçok kontrol algoritması var.

mining yapanlar ancak deneme yanılma yöntemleri ile mining yapabiliyor. yani; 'Proof of Work' felsefesine göre; çalıştıklarını kanıtlamış oluyorlar.

## 📌 nonce

kelime anlamı: şimdiki zaman

örnek kullanım: for the nonce --> 'şimdilik' anlamına geliyor

nonce bilgisayar bilimlerinde `number used once` kısaltmasıdır. yani bir kerelik kullanılacak sayı anlamına geliyor. örneğin; mining yaparken yukarıdaki örnekte randomString() yerine 'nonce' değeri kullanılır.

## 📌 mining pool

Madenciler güçlü bilgisayarlara ortak yatırım yapıyor. Kazanılan Bitcoin'ler ortak paylaştırılıyor. bu, diğer para birimleri içinde geçerli bir durum.

## 📌 Equihash

Proof-of-Work (⟷ proof of work ⟷ PoW) tabanlı bir mining algoritmasıdır.

piyasada sadece firmalar ASIC alabiliyor ve halk alamıyor. çünkü:

- hem satın alma maliyeti yüksek olabiliyor
- hem de bakımı için ancak şirketlerin üstelenebileceği durumlar olabiliyor (soğutma alma, fiziksel koşullar/ bakım için işçi çalıştırma)

bu gibi sebeplerden piyasa hakimi sadece firmalar oluyor. bu da merkeziyetçi sistem demek oluyor. işte bu durumda sanal paranın mantığına ters olduğu için pek istenmiyor.

1- mining yapan ASIC'ler maliyet olarak içlerinde memory bulundurmuyorlar. eğer memory bulundururlarsa gerçek bir ekran kartından farkları kalmıyor.
2- "CPU ve GPU (graphic process unit)" başlığında anlatıldığı gibi, GPU'lar paralel işlemler konusunda çok başarılı.

bu sebeple tüm piyasadaki ASIC'leri kullanılmaz hale getirecek yeni hash algoritmaları geliştirildi. bu algoritmalar:

1- bir nebze daha fazla hızlı erişimli memory'ye ihtiyaç duyuyor.
2- paralele çalışmaya daha az gereksinim duyacak şekilde tasarlanmış.

bu sebeple; ASIC'lerde memory olmadığından, artık işin içine halk ta ekran kartı satın alarak girebiliyor. işte bu tarz algoritmalara Proof-of-Work tabanlı algoritmalar deniliyor. Equihash bir algoritma ismidir.

Aslında bakıldığında ASIC'ler paralel işlemleri daha iyi yapacak diye bir kaideleri yok. sonuçta custom cihazlar. fakat eğer algoritma paralel işlemlere çok ihtiyaç duymuyor ise; ASIC almanın manası kalmıyor.

## 📌 proof of stake (⟷ PoS ⟷ proof-of-stake)

`stake` kelime anlamı: menfaat, çıkarcılık, hisse, ticari pay

`pow`'a alternatif bir metottur. `PoS`'ta elinizde bulunan coin'lerin miktarı ve bu coin'lerin yaşı (en son transfer edildiği tarih) önemlidir. bu değerler arttıkça kazandığınız ödül miktarı da artmaktadır.

`Pow`'a göre daha az makine gücü (elektrik gücü) istemektedir.

## 📌 Bizans General Problemi (⟷ Byzantine fault ⟷ interactive consistency ⟷ source congruency ⟷ error avalanche ⟷ Byzantine agreement problem ⟷ Byzantine generals problem ⟷ Byzantine failure)

yazılımlarda merkeziyetsiz sistemlerde haberleşmelerde kaynaklanan sıkıntılardır. bu sorunları çözen algoritmalara genel olarak "`Consensus algoritmaları`" denir.

Problemin ismi şuradan gelmektedir: 4 `Bizans` generali farklı noktalarda bir şehri kuşatmış durumda. şehri ancak sabah her biri dört bir yandan saldırıya geçerek kazanabilecek. peki birbirlerine nasıl haber verecekler. bir haberci yollanır. fakat haberci yolda ölebilir. bu sebeple haberci mesajı ilettiğine dair, karşı generalden getireceği şifreli haber ile ancak mesajın iletildiğinden emin olabiliriz. şifreli mesaj dönmesi şart, çünkü habercide yalan söyleyebilir. peki generaller farklı mesaj gönderirse veya haberci için zaman yetmezse? peki ya diğer generaller farklı bir mesaj iletirse? peki ya hain generallerin yapacağı diğer aksiyonlar? işte bu sorunlar merkeziyetsiz sistemlerin iletişim problemleridir.

problem için bazı çözümler:

- voting: hain sayısı için sistem önceden bir oran belirler: örnek `2/3`. yani en fazla 3 generalden biri yalancı olarak kabul edilir. buna göre her mesaj oylamaya alınır. oylama sonucunda fazla olan karar doğru olarak kabul edilir.
- sanal paralarda `PoW`, `PoS` gibi çözümler de mevcuttur.

`Bizans general problemi`nin birçok türevi ortaya atılmıştır. `kaynak: kaynak: https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.532.4485&rep=rep1&type=pdf "Edwin Soedarmadji", "The Chinese Generals Problem", "California Institute of Technology", title: "II. THE CHINESE GENERALS PROBLEM", 2inci paragraf.`

## 📌 Two Generals' Problem (⟷ Two Generals' Paradox ⟷ Two Armies Problem ⟷ the Coordinated Attack Problem)

`Two Generals' Problem`, aynı orduya mensup iki generalin güvenilmeyen bağlantılar üzerinden haberleşmesi problemidir.

bu sorunun tamamen bir çözümü yoktur. çünkü haberci, `alındı` mesajını da karşıya atması gerekir. karşı tarafın alındı mesajının bildirilmesi için de tekrar diğer general dönüş yapılması gerekli. bu döngü sonsuza dek gider. ve bunun bir çözümü yoktur.

`Bizans general problemi`; generallerin (node'ların) güvenilir olmamasından kaynaklı problemleri ele alırken, `Two Generals' Problem` ise iletişim kanallarının güvenilir olmaması problemlerini ele alır.

## 📌 Chinese Generals problem

birçok kaynakta `Bizans general problemi` ile eş anlamlı olduğunu okuyabiliriz. fakat bu yanlış. `kaynak: https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.532.4485&rep=rep1&type=pdf "Edwin Soedarmadji", "The Chinese Generals Problem", "California Institute of Technology", title: "Abstract", 2inci paragraf.`

Doğruluğundan emin olamadığım kaynaklarda, `Chinese Generals problem`'inin `Two Generals' Problem` ile aynı olduğu yazmaktadır.

## 📌 Karıştırma (⟷ Mixing ⟷ Çamaşırhane ⟷ Laundry) Servisleri

Bir servis bu işi yapabilir. Herkesten `Bitcoin`'leri aynı hesaba yollar, daha sonra herkese geri yollar. Sadece işlem karışıklığı yapılır ki takip zorlaşsın. Fakat bu servisin paraları geri iade etmemem durumu da söz konusu olabilir.

## 📌 %51 atağı vs double spend

%50'nin üzerinde onaylama gücüne sahip node'lar bir çok farklı kötü amaçlı işlem yapabilirler. buna bir örnek "double spend" olarak bilinen bir ataktır.

bu hack yönteminin maliyeti o parayı kazanmaktan daha fazladır. Ve bu maliyeti ortaya koyabilecek bir otoriter, tüm `blockchain` platformunun sisteminin güvenliğinin riske girmesi durumunu göze alamaz. Çünkü bu kadar node'u çalıştıracak sisteme büyük para yatırmıştır, bu sistemle para üretmesi daha mantıklı olacaktır.

## 📌 double spend

Tüm node'ların yüzde %50 sine sahip bir otoriter sunucu, `P` bloğundaki public olan ana `blockchain` üzerinden kendisinin `private key`'ine ait cüzdanlardan alışveriş yapar. üstüne V adet yeni blok yaratılmasını bekler. Bu arada P noktasından itibaren kendisi blok oluşturmaya devam eder fakat bunu kimse ile paylaşmaz. Yeni transaction'lar zaten public yayımlanıyor, bunları kendi zincirine eklemeye de devam eder. Piyasadaki işlemlere ekstradan kendisi de kendi ucuz cüzdanları ile gereksiz işlemler yapar, ve son durum şu olur:

Public `blockchain` uzunluğu : `P + (spend money as normally) + V`

Private `blockchain` uzunluğu: `P + V + (send-resend money only to create new blocks)`

Bu durumda artık private'ı public ile yer değiştirtir. tüm dünya %50'den daha az oylamaya sahip olacağı için bu hacker sunucunun dediği blockchain'i doğru olarak kabul edecektir. çünkü uzun olan ve valid olan `blockchain` kabul görmektedir. işlemler ortada yoktur. fakat örneğin; hacker kendine bu parayla buzdolabı çoktan almış olabilir. buzdolabı firması işlemin iptal olduğunu görecektir fakat buzdolabını teslim etmiş olacaktır.

double spend'i engellemenin en iyi yolu herkesin bir transaction'ın onaylandığını kabul etmesi için minimum blok sayısı arttırması önerilir.

## 📌 (Bitcoin için) transfer onayı

bir para transferi tüm node'lar tarafından onaylanır. işlemin herkese gitmesi çok çabuk olur, fakat herkesin bunu onaylaması yani; bloğa işlemesi gerekmektedir (yeni bloğun oluşturulması ve birinin hediye Bitcoin'i almış olması). bu işlem zaman alabiliyor. herkes onayladı, yeni blok oluştu. bu durumda, o transaction'a 1 confirmation geliyor. daha sonra dünyada birçok transfer oluyor. bunlarda yeni bloklar oluşturuyor. her blok sonrası bizim transfer +1 confirmation alıyor. yani daha garantilenmiş oluyor. çünkü blockchain'de her eklenen blok zincirin kırılmasını/hack'lenmesini daha da zorlaştırıyor. bu yüzden ne kadar çok onay alınırsa o kadar kesin işlem olmuş demek oluyor.

## 📌 (Bitcoin için) mempool (⟷ unconfirmed transactions pool)

`blockchain`'de bloğa işlenmemiş transaction'ların toplamının bulunduğu memory'dir.

## 📌 altcoin

alternative `Bitcoin`'in kısaltmasıdır. Bitcoin haricinde olan tüm `kripto-para`lar grubuna verilen ismidir. sadece halk dilinde kullanılan bir terim olduğundan; `blockchain` altyapısını kullanan ve içinde gömülü para modülü barındıran fakat çok farklı amaçla kullanılan sistemlerin (örneğin Ethereum) altcoin olup olmadığı tartışmaları ile karşılaşılabilir.

## 📌 (Bitcoin için) transaction fee

para transfer ücretleri. Bitcoin miner'larına hediye olarak verilmektedir.

## 📌 (Bitcoin için) hashrate

belli bir süre içinde bir node tarafından onaylanan blok sayısı, o node'un hashrate'idir.

## 📌 ASIC (⟷ Application Specific Integrated Circuit)

Sadece bir işi yapmak için tasarlanmış cihaz. örneğin; "ASIC mining"; son kullanıcıların kullandığı masaüstü bilgisayarların üzerine yazılım kurarak değilde, sadece para basma için kullanılan cihazlardır.

piyasada bir para birimi için ASIC yok ise; mining yapan kişiler normal son kullanıcıların kullandığı ekran kartlarını kullanmaktadır. bu sebeple; piyasada ekran kartı sıkıntısı yaşanabilmektedir.

## 📌 Ponzi Oyunu (⟷ Ponzi Düzeni ⟷ Ponzi Scheme)

yatırımcılara kendi paralarından geri dönen ile veya sonraki yatırımcılardan gelen paralarla ödemenin yapıldığı bir dolandırıcılık yöntemidir. toplam kar edilen pra üyelere bölüştürülür. ortada bir ürün satılıp alınmadığından dolandırıcılık yöntemi olarak adlandırılır fakat bazı ülkelerde illegal bir durum teşkil etmeyebilir.

## 📌 Saadet zinciri (piramit zinciri ⟷ pyramid scheme)

her yeni üye referans ile sisteme katılır. bu sistemde her üye olan kişi, referansına pra kazandırır. bu tarz kazanç sağlayan sistemlere verilen isimdir. dolandırıcılık olmak zorunda değildir.

## 📌 MMM

Bu özel isimde bir şirket ponzi şeması mekanizması ile para kazanmıştır. Daha sonra firma "MMM GLobal" olarak isim değiştirmiştir.

## 📌 Ethereum vs Ethereum Classic

`Ethereum`'daki 1 kontrat son kullanıcılardan para bulmak için dolandırıcılık yaptı. Ethereum'da olan bir açık ile değil, tamamen kontratta olan ufak bir detaya kimse dikkat etmedi ve birçok kişi bu kontrata para yatırdı. Miktar çok büyüktü. `Ethereum` projesinde consensus kararı ile bu kontratın yaptıkları işlemler geri alındı. Fakat buna karşı çıkanlar oldu. Karşı çıkanlar `Ethereum` projesini aynen çatalladı.

## 📌 (Ethereum için) Account

2'ye ayrılır:

- __Externally-owned account (EOA)__
- __Contract account__

## 📌 Ethereum Virtual Machine (⟷ EVM)

kontratlar özel bir programlama dili ile yazılır ve EVM'de execute edilir. tabi network'te kaç tane Ethereum node'u varsa, tümünde execute edilir.

__Solidity__, Ethereum'un support ettiği programlama dilidir. Bu kod derlenip, EVM'de execute edilir. Örnek contract kod:

(Aşağıdaki Solidity örnek kodu, JS syntax'ı ile renklendirilmiştir)

```js
pragma solidity >=0.4.0 <=0.6.0; // this is compiler version for this Solidity file.

contract ExampleDapp {

    string dapp_name; // state variable (⟷ storage variable): these are expensive. because they are sorted in blockchain.

    address owner; // address is a native special type of Ethereum.

    // Called only when the contract is created.
    constructor() public {
        dapp_name = "My Example dapp";
        owner = msg.sender; // "msg" is a global variable which holds info about transaction (such as the address of the sender and the ETH value included in the transaction).
    }

    // "view" means this function does not manipulate any storage data.
    // "constant" keyword wa using on older versions of solidity.
    function read_name() public view returns(string) {
        return dapp_name;
    }

    // functions which has "pure" keyword can not read storage variables like a "view".
    function multiply(int a, int b) public pure returns(int) {
        return a*b;
    }

    // "public" and "private" keyword are same as Java.
    function update_name(string value) public {
        string variable99 = "hello"; // memory variables --> they are cheaper then state variables. because they are not storing to blockchain.
        dapp_name = value;
    }

    function method99() public {

        // require is a function (similar to "assert" method on Java) which throws exception.
        // Only the contract owner can call this function.
        require(msg.sender == owner, "You are not the owner.");

        // Do something more here...
    }
}
```

Ethereum blockchain'ine contract'ların kendileri de store edilmektedir.

Ethereum network'üne deploy edilen her contract'ın bir instance'ı o anda oluşturulur. Instance oluştururken deploy eden client initial parametreleri (constructor parametrelerini) yollamak zorundadır. Artık contract'taki herhangi bir public metodu herhangi bir client çağırabilir. Eğer birinin çağırmasını engellemek istiyorsak, constructor'da account-id'mizi geçmeliyiz. Bu account-id'yi her public metodumuzun başında, assert-equal fonksiyonu ile, public metodu çağıran kişinin hesabının aynı olup olmadığına bakabiliriz.

Transaction'lar temelde 3'e ayrılır:

- bir hesaptan diğerine Ether transfer işlemi.
- Sözleşme deploy etme işlemi.
- sözleşmedeki bir fonksiyonu çağırma işlemi.

"view" metotlarını çağırmak ücretsizdir. piyasada, ücretsiz fonksiyon çağrıma işlemine "transaction" denmez. fakat resmi dökümantasyona göre transaction'dır.

Her transaction işleminde şu parametreler vardır:

- __from__: sender address (this sender also should sign the transaction data).
- __recipient__: receiving address.
  - eğer direk Ether transferi yapıyorsak, paranın aktarılacağı hesabın adresidir.
  - eğer sözleşmeyi deploy ettiğimiz transaction ise, bu kısım null olmalıdır.
  - eğer sözleşmedeki bir fonksiyonu çağırıyorsak, bu kısım sözleşmenin adresidir.
- vs...

## 📌 (Ethereum için) mining

Bitcoin'de olduğu gibi işlemler blockchain'lere kaydediliyor. contract'larda her variable değişimi (fonksiyonların içindeki değil) blockchain'e kaydediliyor. blokların birleştirilme işlemi mining yapanlar tarafından yapılıyor. fakat contract'lar herkes tarafından işletiliyor. zaten contract'lar tüm node'lar tarafından ayrı ayrı işletilmezse, node'lar blockchain'deki yeni bloğu hangi bilgiler ile dolduracağını veya onaylayacağını da bilemez. dolayısı ile ödülü kazanan, sadece bloğu doğru hash bilgileri ile bulan node'dur.

## 📌 Ether (⟷ ETH)

Ethereum'un kendi içinde kullandığı native para birimi.

## 📌 ERC-20

Ethereum üzerindeki contract'lar genelde kendi para birimlerini yaratırlar. bunlar tamamen sanaldır ve sadece ilgili contract'lar tarafından tanınırlar. Herkesin sıfırdan bunu implemente etmemesi için bazı standartlar belirlenmiştir. API'si, fonksiyonalitesinin ne şekilde implemente edileceği gibi standartlar içerir. ERC-20 bir spesifikasyon. Onu implemente eden ve ona alternatif birçok standart vardır.

## 📌 (Ethereum için) gas (⟷ gaz)

her contract içindeki işlem belirli bir işlem gücü gerektiriyor. bu işlem gücüne verilen teknik isim gas'tır. eğer kullanıcı contract işlemini yaptıracaksa, execute ettiği contract'ın içindeki fonksiyonların ihtiyaç duyduğu gaz miktarı kadar "Ether" para birimi olmalıdır. bu gas'ların parası hesaptan alınıyor ve miner'lara hediye olarak veriliyor.

## 📌 (Ethereum için) ENS (⟷ Ethereum Name Service)

İnternet DNS mantığında; her adresi bir domain'e bağlayabiliyorsunuz.

## 📌 (Ethereum için) fast vs full vs light

bu mod çeşitleri ile Ethereum client yazılımları run edilmektedir. full node tüm blockchain'i indiriyor ve tüm işlemleri baştan sona gerçekleştiriyor. fast mode; tüm blockchain'i indiriyor ama sadece artık bu zaman zarfından sonraki contract'ları yürütmeye başlıyor. light mode hiç DB'yi indirmiyor.

Light mode sadece metadata indiriyor.  metadata boyutu 2017 sonları itibariyle ortalama 200 MB.

## 📌 Mist (⟷ Mist Browser ⟷ Ethereum Wallet) vs Geth (⟷ go-Ethereum)

Mist Ethereum için electron bazlı GUI uygulaması. geth ise Ethereum için komut satırı uygulaması. Mist arka planda "geth" 'i kullanarak işlemleri yapıyor. eğer istenirse farklı bir implementasyona da referans edip çalıştırılabilir.

## 📌 Ethereum Classic

Ethereum'da bir DAO aracılığı ile fon toplanmıştı. Bu DAO içerisinde bir açıktan yararlanıldı ve bir hacker tüm fonları kendine aktardı. Hacker aslında Ethereum'u hack'lemedi. DAO programının içerisinde bir açık vardı. bundan yararlandılar. Ethereum projesi hacker'ların saldırdığı bu DAO oluşturuncaki kısımdan itibaren DB'yi klonlayarak hard fork yaptı. Bu hard fork'u kullanmayan ve hack'lenen DB ile devam eden proje ise "Ethereum Classic" olarak adlandırıldı.

## 📌 Consensus

`Türkçe` kelime anlamı: konsensüs, Mutabakat, uzlaşma, fikir birliği

blockchain herkeste senkron DB oluşturması için herkesin birbiri ile anlaşmış olması gerekmektedir. bu durum bir Mutabakat'ı zorunlu hale getiriyor. Mutabakat yapmayan gruplar kendileri birleşerek fork yaratabilirler.

## 📌 Merkle kök değeri

bir bloğun `hash`'i alınıyor. bu `hash`'e verilen isimdir. bu `hash` henüz içinde bir önceki bloğun `hash`'ini bulundurmuyor.

## 📌 block header vs block body

block body; DB'nindata kısmını barındırmaktadır. block header ise aşağıdakilerden oluşur:

- timestamp

- bir önceki bloğun `hash`'i

- `Merkle` kök değeri

## 📌 üst blok

bir önce yaratılmış bloğa verilen isimdir.

## 📌 en uzun zincir

bloklar üst üste eklenirken, bu bloğun tüm diğer node'lar tarafından onaylaması beklenir. ağdaki tıkanmalar, spam olarak onaylamayanlar, onaya geç dönüş yapanlar, gibi etkenlerden dolayı `blockchain` ağı bazen  zincirleme onaylandığı görülür. fakat protocol gereği her zaman en uzun olan zincir doğru olarak kabul edilir. "ikincil zincirler" ise daha kısa kalmış fakat bazı node'lar tarafından onaylanmış bloklardır. fakat ikincil zincirlerde artık geçersiz sayılacaktır. bir de orphan zincirler var. bunlar ise kimse tarafından onaylanmamış zincirlerdir.

## 📌 Bitcoin Cash

`Bitcoin` 1 Ağustos 2017'de hard çatallandı. `Bitcoin cash` isimli yeni fork olarak ortaya çıktı. para birimi kısaltması: `BCH`.

## 📌 SegWit vs SegWit2x

Ikisi de `Bitcoin`'e gelen ayrı protocol update'leridir.

`SegWit` güncellemesi `Bitcoin`'in büyük toplulukları topladı. `SegWit`'i beğenmeyenler `Bitcoin cash` topluluğunu oluşturdu.

`SegWit2x` güncellemesi `SegWit` sonrası Kasım 2017 gibi gelmesi planlanıyordu. Fakat daha sonra iptal edildi.

## 📌 Bitcoin gold

`Bitcoin` (`SegWit` dalından) 24 Ekim 2017'de hard çatallandı. yeni para biriminin ismi `Bitcoin gold`. para birimi kısaltması: `BTG`.

## 📌 Initial coin offering (⟷ ICO)

yeni bir coin protocol'ü/para birimi oluşturulmak istendiğinde bir maliyet söz konusudur + birilerinin elinde bu para biriminden olmalıdır ki para akışı meydana gelsin. bunun için (initial para için) bir fonlama başlatılır. eğer fonlama başarılı geçerse (beklenen minimum para toplanırsa) bu para birimi oluşturulur ve herkese ilk başlangıçta o para birimi fonladıkları oranla dağıtılır. fonlamaların çoğu Ethereum gibi kontratlardan yapılır. eğer para birimi canlıya geçerse kullanıcılara token verilmek zorunda kalınır. eğer proje canlıya geçmezse fonlamadan toplanan paralar geri iade edilir

para birimleri ilk çıktığında genel olarak ya tüm para birimlerini merkezi bir firma elinde tutar ve bunların satışından para kazanır. Dolayısı ile para biriminin artmasını onlarda desteklemektedir ve bunun için sürekli yazılım güncellerler. örneğin; Ripple. Bazı firmalar ise yeni para birimi oluşturulduğunda ise Genesis bloğunda belli miktarda para birimini kendilerinde tutarlar. Bu para birimlerini satarak kendileri de kar ederler. Yazılımı geliştirmek için buradaki paraları satarak para kazanırlar.

## 📌 (Bitcoin için) cold wallet vs hot wallet

istenirse `private key`'i hiç cüzdan uygulamasına vermeyebiliriz. transaction yapmak için output dosyasını oluşturulur. sadece oluşturma aşaması için bir süreliğine uygulamaya veririz. bu şekilde yüksek güvenlik sağlanır. bu yönteme cold wallet adı verilir. diğer tüm yöntemler hot wallet'tır.

adres takip işlemleri zaten `public key` ile yapılabiliyor.

## 📌 Litecoin

LTC para birimini kullanan sanal paradır. sadece Bitcoin'in bazı dezavantajlarını gidermek için ortaya çıkarılmıştır.

## 📌 Tether

Merkezi bir `blockchain` tabanlı para birimidir. Piyasada alış/satış ne olursa olsun para biriminin değerini sürekli 1 `Dolar` olarak sabitlemiş durumdalar.bu sebeple herkes `Dolar` kullanacağına Tether kullanıyor. çünkü para çevrimlerdeki komisyonları ve transfer ücretleri gerçek `Dolar`'a göre çok daha düşür. para birimi kısaltması: `USDT`.

## 📌 Stablecoin

`Tether` gibi sabit değerden satılacağı iddia edilen sanal-para birimlerine verilen isimdir.

## 📌 IOTA

proje ismi ile para birimi ismi aynı sanal paradır. asıl olarak IoT cihazlarda kullanılması hedeflenen bir para birimi mekanizmasıdır. `MIOTA`, `GIOTA` gibi para birimleri kullanılır. Bunlar farklı para birimi değildir. birer metric'tir. megabayt, gigabaytın ilk harflerini prefix olarak almıştır.

miner'lara gerek kalmadığı bir sistemdir. her node bir miner'dır. fakat 1 transaction yapabilmek için belirlenen sayıda transaction'ı onaylamanız beklenir. bu sebeple miner'ların işi parçalanmış olur ve ufak işlemler olduğu için cep telefonları dahi bu ufacık mining işini yapabilir hale gelmektedir. transaction sayısı arttıkça onaylama sayısı da arttığı için hiçbir zaman transaction süreleri yavaşlamayacaktır.

IOTA `blockchain` kullanmıyor. Onun yerine DAG (⟷ directed acyclic graph) veri yapısı kullanılır. DAG'ın boyutu sürekli büyüdüğünden, belli aralıklarla snapshot'ı alınarak sadece tüm işlemlerin özeti kalır ve artık herkes bu snapshot'ı kullanır. örneğin snapshot alındığında; eski transaction'lar silinir, geriye sadece son durumda parası olan cüzdanlar kalır.

## 📌 token vs coin

Ethereum gibi turing-complete sistemlerde akıllı sistemler yürütülmektedir. dolayısı ile EVM üzerinde akla her gelen program çalıştırılabilmektedir. bu programlardan bazıları para birimidir. bu tarz para birimi yazılımları kendi içlerinde "token" kullanırlar. Bu sistemler EVM üzerinde çalışan Bitcoin alternatifi yazılımlardır. Fakat bu para birimi, artık kendi altyapısını kurarak, EVM dışında, kendi bağımsızca çalışabilecek konuma geldiğinde o zaman "token" kullanmaktan çıkar ve "coin" kullanmaya başlar.

Örneğin; Ether coin'dır, ama ERC-20 token'dır.

## 📌 Directed acyclic graph (⟷ DAG)

bir çeşit veri yapısı modelidir. Bazı sanal para birimlerinde `blockchain` yerine tercih edilir.

## 📌 dağıtık (⟷ Distributed) vs merkeziyetsiz (⟷ Decentralized)

merkeziyetsiz sistemlerin tek farkı, merkez bir node'un karar verme yetkisinin olmamasıdır. ancak ve ancak tüm node'lar aynı kararı alırlarsa (yada quorum sağlanırsa), tüm network'te o güncelleme yapılabilir.

eğer bir grup kendi içinde güncelleme yapar ve diğer node'lar bunu kabul etmezse fork oluşur.

## 📌 Hard fork vs soft fork (⟷ yumuşak çatallanma)

Hard fork gerçekleştiğinde gerçekleşen andan itibaren, artık yeni yazılıma geçenler haricinde `blockchain` DB'ye yazabilen veya yazılanları valide edebilen olmuyor.

soft fork'ta ise, yeni yazılıma güncelleme yapanlar yeni blockchain'e yazabilirken, eski sürümde olanlar yeni blok ekleyemiyor.

Soft fork'ta yeni blokların sadece "validation" işlemi için yazılımı güncellemeye gerek yoktur. gelen yeni blokları eski sürüm yazılımlar onaylayabiliyor. ("validation" işlemini miner'lar değil, full-node'lar yapıyor).

Bazı soft fork'larda, bazı durumlarda eski node'larda blockchain'e yeni blok ekleyebilir. bu durumlar sadece şu durumda olabilir: eski node'un çıktığı yeni bloktaki bilgiler, yeni rule'ları violate (ihlal) etmiyordur. Fakat bu durum çok enderdir ve garantisi olmayacağı için miner'lar yazılımlarını güncellemek zorundadırlar.

hard fork sonrası iki farklı zincir oluşur. eskiler ve yeniler farklı zincirlere yazmaya devam eder. fakat soft-fork'ta herkes hala aynı zincire yazmaya devam eder.

## 📌 Oracle

`Türkçe` kelime anlamı: kahin.

`smart contract` tabanlı sistemlerde Oracle bir servistir/yazılımdır. Bu servis dışarıdan herhangi bir bilgi sorgulanmasını gerçekleştirir. örneğin hava durumu 30 dereceyi geçerse `Ethereum`'daki bir `smart contract` bir yere para gönderecek olsun. hava durumu servisi burada `Oracle` olarak adlandırılıyor.

bir çok farklı `Oracle` servisi mevcuttur. örneğin; `Oraclize`.

`smart contract`'lar direk olarak dış dünyaya `HTTP` isteği atamaz. `Oracle`'larla ancak iletişime geçebilir. bu iletişim 2 yönlü de olabilir. dizayn gereği bu şekildedir.

## 📌 testnet

Bitcoin'in test sürümünün ismidir. protocol daha kolay mining yapar ve DB test kayıtlarını içerir. daha sonra diğer para birimleri de bu isimle test sürümlerini yayınlamıştır. bu sebeple sadece Bitcoin'e özgü bir kelime değildir.

## 📌 Electrum ile testnet kullanımı

"electrum --testnet" komutu ile uygulama başlatılması yeterli.

## 📌 Electrum hesap yönetimi

Bitcoin protocol'ünde adreste olan tüm transaction-input'lar tek bir "output" olarak gitmek zorundadır. yani adresimden kısmen para gönderiyim diye bir şey yok. size kaç parça gönderilmişse,o parçaları aynı büyüklükte yollamak zorundasınız. fakat karşı tarafa para atılırken birden fazla adrese yollanabilir. ama tek işlemde yollanmak zorundadır. bu Bitcoin'in bir kuralıdır.

Electrum'da seed üzerinden cüzdan açıldığında "receiving" ve "change" grup isimleri altında birçok cüzdan adresi görülür. biri bize transfer yapmak istediğinde herhangi bir adrese transfer yapılabilir. "request payment" kısmında Electrum, receiving'deki ilk sıradaki adresi gösterir. biri bize para yolladığında ve Electrum'u açıp kapattığımızda artık "request payment" kısmında Electrum receiving'deki 2'inci adresi gösterir. Electrum'un bunu böyle yapıyor 2 sebepten: 1- gizliliği arttırıyor. 2- her "transaction input" 1 adreste tutulmuş oluyor. harcarken 1 adres 1 transaction output olarak hesaplanacağı için hesaplaması (görülebilirliği) daha kolay olmaktadır.

Şimdi ise yollama işlemine bakalım. para herhangi bir hesaptan yollandığında eğer yollanan para; herhangi bir adresimizin total amount'u ile eşit değilse; karşı tarafa nasıl bu paranın bir kısmını yollayacağız? işte burada Electrum bizim için devreye giriyor. bir adresteki paranın yollanacağı kısmı yolluyor, o adreste kalan diğer kısmı ise aynı "transaction output" ile bizim "change addresses" listesindeki hesaplardan birine yolluyor. bu şekilde tek işlemde kısmı para göndermiş oluyoruz.

## 📌 Lightning Network (⟷ LN ⟷ Yıldırım ağı)

Bitcoin'de transaction komisyonlarının (fee) artması ve transaction onaylanmalarının yavaşlaması üzerine LN geliştirilmiştir.

LN açık kaynaklı bir spesifikasyon. Birçok açık kaynaklı implementasyonu var. Bu sistem birçok para birimini destekleyebiliyor. bu sistem desteklediği para birimleri protocol'lerinden bağımsızca (off-chain) çalışıyor. bu sistem ile farklı bir `blockchain` üzerinde transaction yapılacağına dair onaylama gerçekleşiyor.

Bitcoin'i baz alan bir LN özetle şu şekilde çalışıyor: belli bir miktar Bitcoin'i X kişisi NL network'üne taşıyor. artık `NL` network'ündeki `smart contract`'lar ile bu Bitcoin yönetiliyor. Y kişisi Z kişisine Bitcoin atmak istediğinde, X kişisi aracılığı ile bu işlemi gerçekleştiriyor. İşte buradaki X-Y-Z arasındaki işlem yoluna "ödeme kanalı (⟷ payment channel)" adı veriliyor. X kişisinde hazır yüklü olan para birimi Z ye aktarılıyor. NL son Bitcoin durumunu Bitcoin'in blockchain'e göndermiyor. tabi kanalı biri kapatıyorum diyene kadar. Yani NL X-Y-Z kanalı 1 ay işlem yapıp kapatılırsa, sadece 1 ay sonraki özet bilgi (son durum bilgisi) Bitcoin blockchain'e aktarılıyor. böylece Bitcoin blockchain'i daha az şişiyor ve X-Y-Z arasındaki işlemler daha ucuza yapılmış oluyor. NL network'ünde de `blockchain` işleme ve B aracısına para verme (fee) durumu olacak fakat şu andaki Bitcoin ücretlerine ve hızına göre çok çok az olacak.

## 📌 Hyperledger

açık kaynaklı `blockchain` altyapısı kullanan ve `blockchain` ile network üzerinden anlaşma sağlayan framework altyapısı projesidir. bu proje tek başına kullanılamaz. bu projeye dayalı, birçok alt proje mevcuttur. bunlardan biri seçilmek zorundadır. örnek alt projeler: Hyperledger Fabric, Hyperledger Burrow...

Hyperledger bir sistemi tümüyle ele alıyor: `blockchain` defterin depolanması, konsensüs kuralları/yönetimi, güvenlik ve yetki/kimlik yönetimi, network haberleşme protocol'ü gibi...

Hyperledger projesi içinde araçlar da mevcuttur. örnek: Hyperledger Explorer (GUI ile kayıtları takip edebilmemizi sağlıyor)

Hyperledger alternatifi piyasada birçok proje mevcuttur.

## 📌 Blockchain 1.0 vs Blockchain 2.0 vs Blockchain 3.0

`blockchain` sadece terimi bir kayıt defteridir. fakat `smart contract`'lar ile defterden daha fazlasını sunan `blockchain` temelli yazılımlar ortaya çıkmıştır. bu yazılımların çıkışı ile 2.0 terimi de insanlar arasında yayılmıştır. oysa DB sisteminde bir değişiklik meydana gelmemiştir. aslında "`blockchain platformu 2.0`" ismi teorik olarak daha uygun olacaktı.

sıralama şu şekilde gitmektedir:

- 1.0 - `cryptocurrency`
- 2.0 - `smart contract`
- 3.0 - `DApp (⟷ decentralized application)`

## 📌 DApp temel yapısı

Native bir uygulama yada web browser'ımızda çalışan bir yazılım son kullanıcıya sunulur. Son kullanıcı bu kod aracılığı ile direk olarak kendi lokalinde yada 3üncü parti birinin sunduğu uzak makinadan, Ethereum node'una bağlanır ve Ethereum node'unda koşan kontratlara istek atar.

Son kullanıcı lokalinde yürüttüğü native uygulamadan Ethereum node'a bağlantı kolay olacaktır. Fakat son kullanıcıyı web browser'ından bağlandırtmak daha dolambaçlı yapılmaktadır: Kullanıcı web tarayıcısına bir eklenti kurmaktadır. Eklenti hem kendi native kodu var, hem de web sayfasının kodları ile temasa geçmektedir. Örnek bir web tarayıcısı eklentisi: Metamask.

Herhangi bir web sayfası aşağıdaki kod ile son kullanıcının Metamask eklentisini kurmuş olup olmadığına bakar:

```js
if (provider !== window.ethereum) {
    console.error('extension is not installed?');
}

// interact with Ethereum nodes (servers) via "window.ethereum" object's API.

// Current user may have defined it's wallet inside Metamask extension on the current web browser.

// when we call "request" function from JS, the MetaMask extension will show a popup to end-user and it will ask for permission to show the account to the current web page.
const accounts = await window.ethereum.request({ method: 'eth_requestAccounts' })
  .catch((err) => {
    // user rejected or another error happened
  });

const account = accounts[0];
```

Yukarıdaki örnekteki gibi birçok API bize sunulur. Örneğin; JS aracılığı "window.ethereum" objesinden son kullanıcın bağlanmış olduğu network2ü de okuyabiliriz.

Ethereum gibi yapılarda contract'lara istek attığımızda anında cevap alamayız. İşlemlerin ne zaman tamamlanacağı kesin değildir. çünkü işlemleri dünyadan bir çok node gerçekleştirecek ve blockchain'e yazıp işlemi onaylaması gerekecek. dolayısı ile istekler yapılır, event'lere subscribe olunur.

Web3.js JS kütüphanesi eklentisiz şekilde Ethereum sunucusuna istek atabilmemizi sağlıyor:

```js
const web3 = new Web3('https://mainnet.infura.io/v3/YOUR_API_KEY');

const txHash = await web3.eth.sendTransaction({
  from: '0xYOUR_ACCOUNT_ADDRESS',
  to: '0xANOTHER_ACCOUNT_ADDRESS',
  value: web3.utils.toWei('0.1', 'ether'),
});
```

Fakat MetaMask cüzdanlarında daha kolay yönetilip, istenilen web sayfası sekmesinden kullanılmasını sağlıyor.

`Ethereum` üzerindeki `smart contract`'lar, __Solidity__ isimli dil ile yazılmış ve native olarak `Ethereum` node'lar üzerinde koşmaktadır.

## 📌 Decentralized Autonomous Organization (⟷ DAO ⟷ decentralized autonomous corporation ⟷ DAC)

`decentralized`, `Amerikan` `İngilizce`'sinde "`Decentralised`" olarak yazılır.

bu terim; bir makalede ortaya atılmıştır. bu terim; kendi başına çalışan otonom bir sistemi temsil eder.

`DAO` bir açıdan bakıldığında; `Bitcoin`, `Ethereum` gibi birçok sistemi içine almaktadır. çünkü bu sistemler de tek başlarına işleyebiliyor. fakat `Bitcoin`, `Ethereum` gibi sistemler ne kadar da merkeziyetsiz olsalar da, birileri tarafından güncelleniyorlar. `DAO` kavramında insan elinin sadece başlangıçta olması beklenir.

`DAO` pratikte en yakın; `Ethereum` tarzı sistemlerde çalışan `smart contract`'lara benzetilebilir. Çünkü pratikte gerçek bir işleyişin olması için, bir `runtime`'ın olması gerekli. `runtime` ortamı içinde `Ethereum` tarzı sistemlerdeki kontratlar uygun bu sağlayan uygun yapılardır.

## 📌 DAO vs Dapp

Her 2 terimde resmi olarak makalelerde geçmektedir.

`DAO` tamamiyle autonomous olması beklenmektedir. yani; sisteme manuel müdahale gerektirmemelidir. %100 bunu yapan bir yazılım yapılamadı fakat insan elinin daha az değdiği birçok `Ethereum` `smart contract`'ı geliştirildi ve işletildi.

`Dapp` ise insan elinin müdahalesi sonucunda işlem yapar.

`DAO` kavramı biraz felsefik/teoriktir.

`The Dao` özel bir isimdir. simgesi: `Đ`. Bir grup yazılımcının `Ethereum` üzerinde çalıştırdıkları `DAO`'ya verdikleri özel isimdir.

## 📌 merkezsiz borsa (⟷ decentralized exchange ⟷ DEX)

peer-to-peer tabanlı protocol'lerle insanların paralarını birbirine transfer edebileceği yazılımlar (borsa yazılımları) geliştirildi. bu borsa yazılımları direk para sahibinin cep telefonunda yada bilgisayarında çalıştırılıyor. para sahibi uygulaması açık olduğu sürece parasını topluluğa açabiliyor veya başkasının parasını alabiliyor. herkes kendi teklifini sunabiliyor yada emir bırakabiliyor. sanal paraları bu sistemlerden nakit paraya çevirmek mümkün değil. nakit paraya çevirebilmek için nakit paranın servis sağlayıcısına erişim olması gerekir.

## 📌 decentralized finance (⟷ DeFi)

DEX'lerin çalışabilmesi için Ethereum gibi sistemlerde contract çalıştırmak gerekir. Bu contract'lara (protocol'lere), DeFI denir ve birçok alternatifleri vardır. piyasadaki örnek DEFI'ler: Uniswap, Dai, Chainlink...

Defi contract'ında da işlem yapmanın bir bedeli olduğundan onunda kendi para birimleri vardır.

## 📌 non-fungible token (⟷ NFT)

fungible kelime anlamı: geri ödenebilir.

varlıkları digital ortamda satmak için geliştirilmiş token'lardır.

Eşi olmayan bir token üretilir ve bu token'ın sahibi ilgili asset'e sahip olarak kabul edilir. İlgili asset'e erişim public'tir. fakat sahibi eşi olmayan o token'a sahip olan kişidir.
