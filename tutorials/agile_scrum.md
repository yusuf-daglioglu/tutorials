# AGILE SCRUM

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Systems development life cycle (⟷ SDLC ⟷ application development life-cycle)

It's a general term for all below titles.

## 📌 Çevik proje yönetimi (⟷ Agile project management)

It's a type of project (not necessarily software project) management. It's suitable for projects which:

- not have well defined requirements,
- are open for change,
- are complex.

`Agile` manifesto is as following:

`Turkish`:

```text
Bizler daha iyi yazılım geliştirme yollarını

uygulayarak ve başkalarının da uygulamasına yardım ederek ortaya çıkartıyoruz.

Bu çalışmaların sonucunda:

Süreçler ve araçlardan ziyade bireyler ve etkileşimlere

Kapsamlı dökümantasyondan ziyade çalışan yazılıma

Sözleşme pazarlıklarından ziyade müşteri ile işbirliğine

Bir plana bağlı kalmaktan ziyade değişime karşılık vermeye

değer vermeye kanaat getirdik.

Özetle, sol taraftaki maddelerin değerini kabul etmekle birlikte,

sağ taraftaki maddeleri daha değerli bulmaktayız.
```

`English`:

```text
We are uncovering better ways of developing

software by doing it and helping others do it.

Through this work we have come to value:

Individuals and interactions over processes and tools

Working software over comprehensive documentation

Customer collaboration over contract negotiation

Responding to change over following a plan

That is, while there is value in the items on

the right, we value the items on the left more.
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Scrum

kelime anlamı: itişip kakışma, saldırı

There is no any special word in `Turkish` for `Agile`.

It's an implementation of the `Agile`. It defines the terms like `sprint` (kelime anlamı: sürat koşusu), `backlog` (kelime anlamı: rezerv, birikim), `daily meetings` etc. It is designed only for software projects.

Generally `Scrum` defines more details about the management process. But `Agile` is set of abstract principles comparing to `Scrum`. Some examples to understand that difference:

- `Agile` defines that face to face communication is required. But `Scrum` defines that the customers should work with `developers`.
- `Agile` defines the iterations, but `Scrum` defines the `sprint`.
- `Agile` points out that visualizing is important, but `Scrum` defines `scrum board`.

Analysis had to be sent to `developers` before `sprint` starts. The developers should read the analysis after the retrospective meeting and before giving them story points.

`Scrum`'s guide is being updated. Changes are made in the flow and phrases. The 2020 version is here:

https://www.scrumguides.org/docs/scrumguide/v2020/2020-Scrum-Guide-US.pdf

https://www.scrumguides.org/docs/scrumguide/v2020/2020-Scrum-Guide-Turkish.pdf

Other versions are here: https://scrumguides.org/revisions.html

## 📌 Scrum terms

## 📌 Scrum people

- __Scrum master__
  - He is responsible and manages the `Scrum` progress.
  - He also solves the non-technical issues which blocks the `team`.

- __project manager (⟷ proje yöneticisi)__
  - In some `Agile` systems this person has been defined.
  - But there is no any term as `project manager`, if we talk specific to `Scrum`.

- __team leader (⟷ takım lideri)__
  There is no role like this at `Scrum`. Because the team is `self-organized`.

- __Scrum team__
  Consist of:
  - `PO`
  - `Scrum master`
  - `development team`

- __stakeholder__
  - A person except the `scrum team`.

  may consist of:

  - customers/ends users

  - company members of customers and `scrum team`'s company (like CEO, CTO)

  - Sales/Marketing teams of developed product

  - support teams

- `development team`
  - In 2020, the term update as `developers`.
  - Those people are only the people who develop the software. They should not be separated as subgroups. If they should separate, these subgroups should be independent `Scrum` teams. Analysts and testers also they should be separated groups.
  - It is not recommended to be less then 3 or more then 9 people.

- __product owner (⟷ PO ⟷ ürün sahibi)__
  - After all decisions and researches, he has the responsibility to decide which tasks should be develop in the next `sprint`.
  - `PO` is constantly in contact with customer.
  - He understands the requirements of customers and he sets the business points on each task on backlog.
  - He also writes the details on each task.
  - He don't have to be a technical person. He focuses only on business.
  - `PO` can let the `development team` to do his jobs. But in this case, `PO` is still the responsible of those things.
  - `PO` is the only person who can decide the cancellation of `Sprint`. But the `stakeholders`, `Scrum master` and the `developer team` are effective to making this decision. If the `Sprint` will cancel, all the remaining tasks in that `Sprint` will be reviewed and the required meetings should be planned to start the new `Sprint`.
  - At the end of the `Sprint` the DEMO should be present to `PO`.

## 📌 Scrum events

Each meeting/event should be done on predefined time (time-boxed).

- `sprint`
  - It should not be longer then 1 month.

- `sprint planning (⟷ Sprint Planlama)`: in this meeting:
  - The team decides which tasks should be develop in the next `Sprint`.
  - If a task is unknown or it's not clear, it should be superficially technically clarified.
  - `Definition of done` should be wrote if it does not exist.
  - Below questions should be answered in planning meeting:
    - Why this `Sprint` is important? (This question come in `Scrum` which released 2020 November)
    - What will be done in this `sprint`?
    - How the selected items will complete?
  - For 1 month `sprint`, max 8 hours is enough for this meeting. For shorter `sprint`, `planning meeting` should be less than 8 hours.

- __günlük toplantılar (⟷ daily Scrum meeting)__
  - This meeting should be scheduled every day.
  - In this meeting everyone should answer these questions:
    - what I did until the last meeting?
    - what I'm going to do from now on?
    - do I have something which blocks me?
  - This meeting should be 15 minutes.

- __sprint review (⟷ sprint değerlendirme)__
  - Anybody can join on this meeting.
  - This meeting should be scheduled at the end of the `sprint`.
  - Each completed task should be demonstrated and questions should be answered about the tasks.
  - Blocked issues and the solutions solved should be discussed.
  - Everybody can comment and suggest about the `sprint`.
  - For a 1 month `sprint` period, this meeting takes maximum 4 hours. For shorter `sprint`, this meeting should be shorter then 4 hour.

- __retrospective (⟷ retrospektif)__
  - retrospektif kelime anlamı: 'dünden bugüne', 'geriye dönük'.
  - At the end of the `sprint`, `developer team` and the `Scrum master` joins in this meeting and they are talking about success and the failures.
  - Both technical and non-technical topics (like `Scrum` process) can be discuss. But most ideally it is recommended to discuss only about non-technical topics (like `Scrum` process).
  - No matter how successful the last `sprint` was, this meeting should be scheduled. Otherwise there will be no improvement.
  - It should take maximum 3 hours for 1 month `Sprint`. For shorter `sprint`, this meeting should be shorter.
  - In this topic everybody should answer these questions:
    - how those problems were and were not solved
    - what went well during the `Sprint`?
    - what problems we encountered?
  - This meeting should be scheduled before `Sprint Planning` and after `Sprint Review`.

- __Product Backlog refinement__
  - refinement kelime anlamı: iyileştirme.
  - `grooming` means: prepare or train (someone) for a particular purpose or activity.
  - Term __Grooming__ replaced as `refinement` with official `scrum guide` 2013.
  - This meeting is defined in official `Scrum Guide`, but it's not under `scrum event` topic.
  - In this meeting `development team` set story points and details for each task item.
  - This meeting should not be more then %10 of the `sprint` time.

## 📌 Scrum artifacts (⟷ Scrum eserleri)

- __increment__
  - `Increment` is the each contribution for the target of the project.
  - `Increment` should be something which can be demonstrated.
  - Every task should be `Done`. Otherwise they are accepted as a `increment`.
  - `increment` is an uncountable value.

- __Product Backlog (⟷ ürün iş listesi)__
  - Tasks which are managed by `PO` and will not developing yet.

- __sprint backlog (⟷ sprint iş listesi)__
  - The tasks which will or started to develop in current `sprint`.

## 📌 Other terms for Scrum

- __Planning poker (⟷ Scrum poker)__
  - It's a technique which is using when `developer` team set the story points for each task. Every `developer` shows their story points using cards (some kind of poker card).
  - It's not an official term defined in `Scrum Guide`.

- __çapraz fonksiyonlu takım (⟷ cross-functional team)__
  - `cross functional` means that in the `developer` team each person may have it's own specific knowledge. But as a team they should be able/enough to finish. Because `scrum` team should not depend on non-team members/people.
  - `cross functional` does not mean that each person should be able to do the work of all others on the team.
  - Is a term that officially defined in `Scrum Guide`.

- __business Value__
  - It is a number which values the priority of the task.
  - The value is setting by `PO`.
  - Is not a term that officially defined in `Scrum Guide`.

- __Story points__
  - Is the number is the combination of both complexity and estimation (time) of a task.
  - The value is like "__Normalized Value__" on statistics. It does not have a unit. It is a value given by team via old experiences. The team compares the old tasks with the current (new) one.
  - The values can be Fibonacci-like numbers: 0, 0.5, 1, 2, 3, 5, 8, 13, 20, 40, 100.
  - High values should not be given to tasks. If it should have high value, then the tasks should be divided.
  - Bug and technical tasks also should have story points. Because they are validating by `PO` and they are developing in the `sprint`.
  - Is not a term that officially defined in `Scrum Guide`.

- __story point vs hour__
  - The team not always estimate the tasks correctly. For that reason, a technique called `story point` designed as an alternative way to calculation of tasks.
  - `story point` based on comparing technique. On each new `sprint`, the team gives the new point on the new tasks by comparing the old tasks.
  - Experienced project managers observed that the story points are reflect more accurate comparing to estimated hours.

- __Estimation__
  - The time value for 1 developer to complete the task.
  - It's not a term which defined in `Scrum Guide`.

- __Burn-down Chart__
  - The graphic chart which shows the remaining tasks of the current `sprint`.
  - It's not a term which defined in `Scrum Guide`.

- __Burn-up Chart__
  - The graphic chart which shows the completed tasks of the current `sprint`.
  - It's not a term which defined in `Scrum Guide`.

- __Definition of Done (⟷ DoD)__
  - It's a term which defined in `Scrum Guide`.
  - The all required definitions to complete a task. To complete a task, it requires to be demonstrated on the production environment. Otherwise the task should not be completed.

- __spike__
  - `spike` kelime anlamı: sivri uç.
  - Some of tasks may have a research phase. Maybe the team will decide which framework/library they will use to develop a feature. In that cases a new tasks should be created for the research phase. This task is named "spike".

## 📌 The Chicken and the Pig

It is a business fable. It was also mentioned in the official `Scrum Guide` but it has been removed on the new versions.

The chicken and pig decided to open a restaurant. They both should serve a food to customer as partners. The chicken had to serve his eggs and the pig had to serve meat which will be part of itself. In the bottom line; The Chicken is involved, but the Pig commits. This situation is simulated on `Scrum` framework as: the `Development Team`, `PO`s & `Scrum Master`s are considered as people who are committed to the project while `stakeholders`, customers and executive management are considered as involved but not committed to the project.

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

`squad-1`: arama motoru ve front-end'deki önyüz component'i.

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
