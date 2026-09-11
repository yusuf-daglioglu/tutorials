# IT MANAGEMENT - AGILE SCRUM

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 ITIL (⟷ Information Technology Infrastructure Library)

`İngiltere Ticaret Bakanlığı` tarafından geliştirilmiştir. `ITIL`, kitaplar şeklinde, bilişim altyapısı için yaklaşımlar anlatılmaktadır. Bu yaklaşımlar dünyanın birçok firması tarafından özellikle uyulmakta ve takip edilmektedir.

`ITIL` katı kuralları olan bir framework değildir. Öneriler ve açıklamalar içermektedir.

versiyonları vardır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Kanban

- It's an implementation of `Agile`.
- The main focus of `Kanban` is the visualization of tasks. So everyone can see an predict the failures.
- Below are the differences between `Kanban` comparing to `Scrum`:
  - There are no specific meeting and activity definitions on `Kanban`. But on any bottlenecks the team decides to any new action. On each project the progress improves itself.
  - On `Kanban`, to giving estimations is optional.
  - On `Kanban`, there is no definitions of roles (like `Scrum Master` etc).
  - On `Kanban`, the iteration is optional. On `Agile`, the `iteration` is mandatory and it's equals to `sprint`.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Extreme programming (⟷ XP)

- It's an implementation of `Agile`. It designed only for software projects.
- Below factors have high priority on `XP`:
  - quality of code is important. Therefore in some cases refactoring tasks has high priority.
  - `Test-driven development`
  - `YAGNI`
  - Small releases
  - Simple design

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 WaterFall Model (⟷ Şelale modeli)

- It's not a implementation of `Agile`.
- Code of entire project is designing after the analysis. The analysis should be ready completely.
- In most cases it is not eligible in current era. Because to prepare all analysis takes really long time and in that preparing period, customer needs may change. Also sometimes the customer does not know what we wants. At `Scrum`, the customer follows/joins the development progress and demonstrations of each `sprint`. So he can see whats going on, and he can make decisions on the development period.
- Whole project can be separated in steps/periods. In this case, end of each these periods called `milestone` (kilometre taşı, dönüm noktası)

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Lean software development (⟷ yalın yazılım geliştirme)

- Very first usage of `Lean product development` is the name of the culture of production environment of `Toyota` fabric. After that, a book has been written in 2003 called `lean software development` which adapt this culture to software project.
- `Lean software development` generally based on `YAGNI` and optimized development systems.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Spotify model

terim olarak çok farklı anlaşılabilmektedir. Piyasada farklı isimleri var:

- `Spotify Organizasyonel Modeli`
- `Spotify Agile Organizasyon Modeli`
- `Squad Framework`

2012'de "`Scaling Agile with Tribes, Squads, Chapters & Guilds @Spotify`" makalesinde public olarak anlatılmıştır.

`Spotify` şirketinin kendi içinde kullandığı SDLC'dir.

### 📌📌 Squad

türkçe kelime anlamı: ekip.

`Scrum`'daki team'e çok benzeyen bir yapı. kendi içinde `Kanban`, `Agile` veya farklı bir framework kullanabilir.

### 📌📌 Tribe

`türkçe` kelime anlamı: takım. meslek grubu.

Birbiri ile ilişkili `Squad`'ları birbiri ile uyumunu sağlamakla görevli takım.

### 📌📌 örnek

e-ticaret sitesinde:

`squad-1`: arama motoru ve frontend'deki önyüz component'i.

`squad-2`: ürün detayı sayfaları.

`tribe`: squad 1 ve 2'nin uyumu için çalışan ekip. bu ekipte bu kişiler var:

- `Tribe Lead` (1 kişi)
- `squad lead` (her `Squad`'dan 1 kişi)
- duruma göre `project manager` tarzı kişiler olabilir.

### 📌📌 takımların bağımsızlığı

`Squad`'lar diğer `Squad`'lardan bağımsız olmalı. fakat bu pratikte pek olmuyor. ama `tribe`'lar da diğer `tribe`'lardan bağımsız olmalı. `Tribe`'ların bağımsızlığı daha üstüne düşülebilen(düzeltilebilen) bir konu. çünkü 1 `tribe`'daki 1 `squad` diğer `tribe`'dan bir `squad`'a bağımlıysa, ilgili `squad` ilgili `tribe`'a taşınabilir.

### 📌📌 operasyon ekibi yok

`spotify`'da operasyon ekibi var, ama bu ekip, `squad`'lar kendi başına deploy alıp, kendi başlarına operasyonları alabilmeleri için onlara yardımcı olmaktadır. operasyon ekibi, deployment yapmamaktadır.

### 📌📌 chapter

aynı `tribe` içindeki tüm testçiler, veya FE'lerin kendi içinde oluşturduğu takımdır. bu takım birbirine bilgi aktarır. eğer bu olmazsa, birbirlerinin yaptığı iyi ve kötü şeyleri birbirlerine öneremezler.

### 📌📌 guild

türkçe kelime anlamı: sendika.

`chapter` ile aynı amaçtadır. farkları:

- farklı `tribe`'lerden katılım olabilir.
- farklı teknolojilerden katılım olabilir. mesela 3 FE, 1 BE birleşip `DTO` standartlarına karar vermesi gibi bir ekip olabilir.

Her `guild`'in bir `Guild` Koordinatörü vardır ve bu kişi sadece bu işi yapar.

### 📌📌 sistem sahibi (⟷ System Owner)

`Spotify` modelinin riski, hiç kimse sistemin bir bütün olarak entegre olmasına odaklanmaz ise, sistemin altyapısının bozulmasıdır.

`spotify`'da ortalama 200 kişi varken 2 kişi bu roldeydi.

bu sebeple; 2 `sistem sahibi` var. biri daha çok operasyonel, diğer development konularına odaklıdır. ortalama zamanının yüzde 10'larını sistemi inceleyip, kod kalitesi, dökümantasyon, teknik borç, gibi mimarisel konulara odaklanır. kala %90 zamanında `squad`'ın bir parçası veya `chapter lead` olmak zorundadır.

### 📌📌 chief architect

1 kişidir.

`spotify`'da ortalama 200 kişi varken 1 kişi bu roldeydi.

`sistem sahibi` ile aynı amaçtadır. fakat bu role daha high level mimari dizayn konularını inceler.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 klasik organizasyon yapısı vs matrix organizasyon yapısı

`SDLC` değildir. fakat sıkça bu terim geçtiği için not olarak buraya ekledim.

klasik yapı:

```text
CEO
│
├── Mühendislik Müdürü
│     ├── Ahmet   → Proje A
│     └── Mehmet  → Proje B
│
├── Tasarım Müdürü
│     ├── Ayşe    → Proje A
│     └── Derya   → Proje C
│
└── Pazarlama Müdürü
      ├── Zeynep  → Proje B
      └── Ali     → Proje C
```

matris yapı:

```text
                       Fonksiyonlar
               Mühendislik   Tasarım   Pazarlama
             ─────────────────────────────────────
Proje A  │     Ahmet        Ayşe        -
Proje B  │     Mehmet       -           Zeynep
Proje C  │     -            Derya       Ali
```

`Matris model`inde, Ahmet 2 kişiden emir alıyor:

- proje a yöneticisi
- mühendislik yöneticisi

Ama klasik modelde Ahmet sadece mühendislik yöneticisinden emir alıyor. Proje A yöneticisi bu modelde sadece koordine eden oluyor.

Klasik Modelin Avantajları:

- Yönetim ve komuta net, basit.
- Rol ve sorumluluklar açık.
- iletişim kolay.

dezavantajlar:

- Projeler arası iş birliği zayıf.
- Yenilik ve hız kısıtlı.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Software craftsmanship

craft kelime anlamı: zanaat (el sanatı, beceri).

İngilizce: http://manifesto.softwarecraftsmanship.org/#/en

```text
As aspiring Software Craftsmen we are raising the bar of professional software development by practicing it and helping others learn the craft. Through this work we have come to value:

Not only working software, but also well-crafted software
Not only responding to change, but also steadily adding value
Not only individuals and interactions, but also a community of professionals
Not only customer collaboration, but also productive partnerships

That is, in pursuit of the items on the left we have found the items on the right to be indispensable. 
```

Türkçe: http://manifesto.softwarecraftsmanship.org/#/tr

```text
Yüksek emeller peşinde koşan Yazılım Ustaları olarak bizler, profesyonel yazılım geliştirme çıtasını, bizzat uygulayarak ve başkalarının bu mesleği öğrenmelerine yardım ederek yükseltiyoruz. Bu çalışmaların sonucunda: 

Sadece çalışan yazılıma değil, ustaca üretilmiş yazılıma da
Sadece değişikliğe cevap vermeye değil, sürekli değer katmaya da
Sadece bireylere ve etkileşimlere değil, profesyoneller topluluğuna da
Sadece müşteri ile işbirliğine değil, üretken ortaklığa da

Değer vermeye kanaat getirdik. Sol taraftaki maddeleri takip etmekle birlikte, sağ taraftaki maddeleri vazgeçilmez bulmaktayız. 
```

`Agile`'ın çıkışının üzerine bazı yazılım kalitesini arttırmaya yönelik çıkarılan manifestodur. Bu manifestoda `agile` reddedilmiyor, fakat bu maddelerin de çok önemli olduğu bastırılıyor/hatırlatılıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
