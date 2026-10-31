
# PATTERN MVC

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 MVC vs MVP vs MVVM vs Redux vs Flux vs Presentation Model (⟷ Application Model)

Redux ve Flux pattern'leri detaylı şekilde farklı başlıkta anlatılıyor.

bu konuya girmeden önce birkaç terimi incelemek gerekli:

### 📌📌 concern

concern Türkçe kelime anlamı: ilgi, endişe, ait olmak

yazılım projelerinde concern her bir fonksiyonalitenin yazılım tarafındaki katmanıdır. örneğin logging layer'ı bir concern'dir.

### 📌📌 Cross-Cutting Concern

yazılımımızın bir katmanında diğer concern'lere ihtiyaç duyabiliyoruz. "logging", "security" gibi concern'ler diğer tüm katmanlar tarafından ihtiyaç duyulmaktadır. başka katmanlardan çağrılan concern'lere `Cross-Cutting Concern` denir. bazı örnekler:

- security
- configuration management
- logging
- communication between services (protocols, `DTO` standards...)
- `monitoring` (`health check`, `metrics`, `distributed tracing`)
- persistence management
- caching
- data validation
- business rules (common rules which are using by independent services)
- exception handling
- performance/memory management -> performans cross-cutting concern'dür. fakat her yere kod yazmayı gerektiren bir durum olmayabilir. yada bunun altına sub-category olarak caching'i ekleyebiliriz.

### 📌📌 Separation Of Concern (⟷ SoC)

`SoC` concern'lerin birbirinden ayrılması için tasarlanmış pattern'lerdir/çözümlerdir.

aşağıdakilerin her biri `SoC` çözümü için geliştirilmiş birer pattern'dir:

- `CQRS`
- `MVC`
- `MVP`
- `MVVM`
- `Presentation Model (⟷ Application Model)`

`MV` ile başlayan pattern'ler ailesine `Model-View-* (⟷ MV*)` denir.

Bu pattern'ler hem server hem client tarafta kullanılabilir.

`React`, `Angular`, `Android` gibi framework'lerin sadece 1 tanesi ile yukarıdaki istediğimiz pattern ile yazabiliriz. Önemli olan yazım tarzımızdır. Fakat framework'ün best practice'leri bizi bir pattern'e yaklaştırabilir.

### 📌📌 aspect oriented programming (⟷ cephe yönelimli programlama ⟷ AOP)

bu konu başka yerde de anlatılıyor.

AOP, concern'leri birbirinden ayırmayı temel alan programlama felsefesidir.

## 📌 Flow Synchronization pattern vs Observer Synchronization pattern

Model herhangi birileri tarafından güncellendiğinde bu framework tarafından view'a otomatik yansıtılıyor ise bu durum Observer Synchronization pattern'dir. Bunun tersi durumlarda manuel işlem yapmak gerekir. Buna da Flow Synchronization denir. kaynak: <https://martinfowler.com/eaaDev/uiArchs.html> title:"Model View Controller", paragraph:12.

## 📌 Widget-based User Interfaces (⟷ Forms and Controls)

- Developer pattern kullanmadığında, tüm kaynaklara isteği herhangi bir kod satırından erişebildiği yapılarda bu terim kullanılır.
- Bu modelde hiçbir `SoC` kullanılmaz.
- Flow Synchronization kullanılır.

## 📌 Web MVC

Web MVC, MVC'nin bck-end'e uyarlanmış halidir. Bu sebeple %100 olarak MVC pattern'ine uyması her zaman beklenmeyebilir. http://www.washi.cs.waseda.ac.jp/wp-content/uploads/2017/03/Sami-Lappalainen.pdf "A Journey Through the Land of Model-View-* Design Patterns", authors: "Artem Syromiatnikov", publish date: 2014-08-16, page:25, title: "Collaborations".

Web MVC'lere örnek:

- `Microsoft` `ASP.NET` `MVC`
- Spring Framework Web MVC
- Grails
- JSP model 2 Architecture

## 📌 JSP model 1 Architecture vs JSP model 2 Architecture

model 1:

```text
Client <----> JSP <----> Java-Bean(s) <----> Database
```

On above architecture, both JSP and Java-Bean(s) may have the responsibility of "Controller" (C of MVC). For that reason model 1 does not fit to MVC architecture.

model 2:

```text
Client <----> Controller(Servlet) <----> JSP <----> Java-Bean(s) <----> Database
```

Model 2 fits in MVC architecture. On this model:

- Java-Bean(s) are representation of model (M of MVC)
- JSP is the View of MVC
- Controller(Servlet) is the C of MVC

In model-2 architecture, JSP's and Java Beans may contain business logic but it's not mostly recommended.

## 📌 MVC (⟷ Model-View-Controller)

`Flux vs MVC` başka başlıkta anlatılıyor.

__Smalltalk__ firması 1988'de `MVC` denilen pattern'i bu makalede "A Description of the Model-View-Controller User Interface Paradigm in the Smalltalk-80 System" ortaya atmıştır. Yazarları: "Glenn E. Krasner" ve "Stephen T. Pope".

Bu makalede bu pattern sadece masaüstü için tasarlanmıştır. Fakat (başka başlıkta anlatıldığı gibi) daha sonradan farklı kişiler/firmalar backend'e uyarlanmıştır.

MVC'de __Observer Synchronization__ kullanılır.

MVC'de controller, dolaylı yoldan view'a bağımlıdır. controller'ın bir metodunu çağırırken, view yoksa muhtemelen exception alırız. çünkü örneğin; muhasebe sayfamızda kullanıcının parası bittiyse "0 TL" kırmızı ile gösterilecektir. bu sebeple unit test controller'ların metotlarını çağırarak yapamıyoruz. (Bu sebeple MVP, MVC'den türetilmiştir.)

bir Android projesinde ilk düşünüldüğünde MVC şunlara karşılık gelebilir:

- M: herhangi bir sınıftaki data sınıfı yada objesi
- V: layout dizinindeki XML dosyamız
- C: activity sınıfımız

oysa bu bakış açısı her zaman doğru değildir. programı yazarken mimariyi biz tasarlamaktayız. örneğin; "layout.xml" oluşturmayıp, tüm GUI elementlerini activity'mizde (Java kodumuzda) dinamik oluşturabiliriz. araştırma yapılırsa "MV*" ailesinden birçok pattern'in de Android projelerinde kullandığı görülebilir.

"MV*" ailesindeki pattern'ler sadece önyüz için tasarlanmamıştır. Bir projenin bütünü iyice incelenirse birçok kısmında "MV*" ailesinden pattern'ler görebiliriz. Örneğin sunucu tarafta REST controller'ımızda da MVC görebiliriz.

Model sınıfı, sadece bilgilerin tutulduğu sınıf değildir. business logic'inde burada olduğu unutulmamalıdır.

Aşağıda MVC'nin temel şeması çizilmiştir. View, Model'dan data'ları gözlemlemek (observe) etmekle yükümlü. Controller ise Model'de update'ler gerçekleştiriyor.

Aşağıdaki şekilde yıldız (*) prefix'ine sahip olan text, ok işaretinin yaptığı örnek bir aksiyondur.

```text
*show
 > > > > > > > >  User > > > > > > > > *click
^                                          v
^                                          v
^                                          v
^                                          v
^                                          v
^                                          v
View                                   Controller
v                                          v
v                                          v
v                                          v
v                                          v
v                                          v
v                                          v
v                                          v
 > > > > > > > >  Model < < < < < < < < *write
*observe
```

Şekil için kaynak: http://www.washi.cs.waseda.ac.jp/wp-content/uploads/2017/03/Sami-Lappalainen.pdf "A Pattern Language for MVC Derivatives", authors: Takashi Kobayashi and Sami Lappalainen.

Yukarıdaki şekilde "View" ile "model" arasındaki ok işaretinin yönünü diğer türlü çizenler de var. Bu biraz çizimin nasıl algılanacağı ile alakalı bir durum. arka planda herkesin bahsetmek istediği şey aynı oysa.

Yukarıdaki şekilde tam gösterilmese de "view", controller'daki event'leri tetikliyor. Aslında user view'a tıklamış oluyor. (Şekilden farklı anlaşılabilir.)

Yine yukarıdaki şekilde controller ile view arasına şu görevi gören çizgi çizilebilir: User event'lerine göre Controller, farklı view'ı initialize edebilir.

MVC'yi basit bir pseudo-code üzerinde gösterirsek:

```java
class MyController {

    MyController(MyView view, MyModel model) {
        this.view = view;
        this.model = model;
    }

    void onEventX(int buttonNumber) {
        model.setPoints(buttonNumber);
        // view kendi içinde model.calculateTotalPoints() metodunu çağırıyor.
        view.showAlertBox("Your points changed as");
        view.setAlertBoxVisibility(true);
    }
}
```

Piyasada genelde; MVC'de, observer yapısının olmadığı algısı vardır. Oysa bu yanlış. MVC'de view, observe etmekle yükümlüdür. Bu cümle için kaynaklar:

- http://www.washi.cs.waseda.ac.jp/wp-content/uploads/2017/03/Sami-Lappalainen.pdf "A Journey Through the Land of Model-View-* Design Patterns", authors: "Artem Syromiatnikov", publish date: 2014-08-16, page:24, paragraph:2.
- <https://gist.github.com/addyosmani/1594678> title:"Smalltalk-80 MVC", paragraph:3. Dökümanın raw hali: <https://gist.githubusercontent.com/addyosmani/1594678/raw/d534c683684b630af7bc28c2e2a6ff8fde7fae1e/javascriptmvc.md>

## 📌 MVP (⟷ Model-View-Presenter)

MVC'deki "controller" yerine "Presenter" gelir, ve MVC'deki "View" artık daha çok View değişikliklerinden de sorumludur. pseudo-code bir örnekle ilerleyelim:

```java
class MyPresenter {

    MyPresenter(MyView view, MyModel model) {
        this.view = view;
        this.model = model;
    }

    void onEventX(int buttonNumber) {
        model.setPoints(buttonNumber);
        view.showPoints(model.getPoints()); // "Presenter" does not care what "view" does. "view" can show alert-box or can show small text inside screen.
    }
}
```

Burada MyView bir interface. MyView'ın View tarafında implemente edilmesi gerekiyor. Yani view, her olayda kendinde nelerin güncelleneceğini belirlemesi gerekiyor. Dolayısı ile view'ı mock'layıp, presenter'ın metotlarını çağıran Unit testler yazabiliriz.

Yani; MVC'de view'a daha spesifik ne yapması gerektiğini söylüyorduk. Oysa MVP'de View'ın abstract/arayüzünü yönetiyoruz, yani daha genel/yüzeysel bir View-API'si kullanabiliyoruz.

Bu konu burada sade kod örnekleriyle açıklanmış: https://academy.realm.io/posts/eric-maxwell-mvc-mvp-and-mvvm-on-android/ Linkteki projelerin gerçek kod örnekleri burada: https://github.com/ericmaxwell2003/ticTacToe

MVP 3'e ayrılıyor:

- 1- __Dolphin Smalltalk's MVP__
- 2- __Passive View__
- 3- __Supervising Controller__

### 📌📌 1- Dolphin Smalltalk's MVP

IBM'in alt kuruluşu olan Taligent firması tarafından yayımlanan makalede ilk olarak ortaya atılmıştır. Makale adı: "MVP: Model-View-Presenter The Taligent Programming Model for C++ and Java". Makale Mike Potel tarafından yazılmıştır.

Bu pattern ilk olarak Dolphin Smalltalk'ta sunulan bir framework ile yaygınlaşmıştır.

### 📌📌 2- Passive View

Object Mentor firmasında çalışan Michael Feathers, "The Humble Dialog Box" makalesinde bu pattern'i ortaya atmıştır. Fakat direk olarak "passive view" terimi makalede geçmemektedir. Fakat Passive View terimi, piyasada bu pattern'e referans etmek için kullanılmaktadır.

Presenter, modeli güncelledikten sonra view'a yeni model'i yollar. örnek:

```java
// this code should be on the Presenter class
view.showPoints(model.getPoints());
```

view'ın model değişikliğinden haberdar olabilmesi için, Presenter'ın view'a bunu bildirmesi gerekir.

Aşağıdaki şekilde yıldız (*) prefix'ine sahip olan text, ok işaretinin yaptığı örnek bir aksiyondur.

```text
        USER
        ^  v
        ^  v
        ^  v
  *show ^  v *click
        ^  v
        ^  v
        ^  v
        ^  v
        ^  v
        ^  v               *onEventX
        ^  v  > > > > > > > > > > > > > > > > > > > > >
        VIEW                                            PRESENTER
              < < < < < < < < < < < < < < < < < < < < <     v
                         *showAlert                         v
                     (with specific data)                   v
                                                            v
                                                            v
                                                            v
                                                            v
                                                            v
                                   MODEL < < < < < < < < *write
```

### 📌📌 3- Supervising Controller

Martin Fowler tarafından ortaya atılmıştır.

passive'dekinin tersine, modelde yapılan bir değişiklik otomatik olarak view tarafından bilinir. view kendini update eder. bunun için binding yapılması şarttır. Data-binding olduğundan dolayı bu pattern'in MVC ve MVVM'e çok benzediği söylenir. kaynak: - <https://gist.github.com/addyosmani/1594678> title:"MVP", subtitle:"Models, Views & Presenters", paragraph:6. Dökümanın raw hali: <https://gist.githubusercontent.com/addyosmani/1594678/raw/d534c683684b630af7bc28c2e2a6ff8fde7fae1e/javascriptmvc.md>

```java
// this code should be on the Presenter class
model.setPoints(buttonNumber);
view.showPoints();
```

Aşağıdaki şekilde yıldız (*) prefix'ine sahip olan text, ok işaretinin yaptığı örnek bir aksiyondur.

```text
    USER
    ^  v
    ^  v
    ^  v
*show ^  v *click
    ^  v
    ^  v
    ^  v
    ^  v
    ^  v
    ^  v
    ^  v         *onEventX
    ^  v  > > > > > > > > > > > > > > >
    VIEW                               PRESENTER
    v     < < < < < < < < < < < < < < <     v
    v            *showAlert                 v
    v                                       v
    v                                       v
    v                                       v
    v                                       v
    v                                       v
    v*read                                  v
    v                                       v
    v                                       v
    v                                       v
    v                                       v
    MODEL < < < < < < < < < < < < < < < < *write
```

Şekil için kaynak: http://www.washi.cs.waseda.ac.jp/wp-content/uploads/2017/03/Sami-Lappalainen.pdf "A Pattern Language for MVC Derivatives", authors: Takashi Kobayashi and Sami Lappalainen.

## 📌 MVVM (⟷ Model View ViewModel)

Bazı kaynaklarda __Model–View–Binder__ olarak da isimlendirilir.

MVVM iki farklı biçimde kullanılabilir:

- 1- __Microsoft's MVVM__
- 2- __Application Model__

### 📌📌 1- Microsoft's MVVM

- Microsoft yazılımcısı John Gossman tarafından, 2005 yılında ortaya atılmıştır. Böyle bir özel terim yoktur. Bu dökümanda gruplandırmak amaçlı (Microsoft'un olduğunu belirtmek için) bu şekilde isimlendirilmiştir.
- Framework'ün en çok uygun olarak kullanıldığı framework'ler:
  - __KnockoutJS__, MVVM yapısı üzerine kuruludur.
  - __Windows Presentation Foundation (⟷ WPF)__ (which is a UI framework that creates desktop client applications)
  - `Microsoft` __Silverlight__
- `MVVM`'de __Presenter__ yerine __ViewModel__ kullanılıyor.
- `MVVM`'de __data binding__ kullanmak şart. Çünkü;
  - View, ViewModel'a erişiyor fakat tersi yapılamıyor.
  - ViewModel, modele erişebiliyor fakat tersi yapılamıyor.

  Bu sebeple `MVVM` projelerinde çoğunlukla `MVVM` altyapısı sunan framework'lerden yararlanılır. örnek olarak;
  - data binding özelliği Android tarafından destekleniyor.
  - `Microsoft`'ta bunu desteklemek için sunduğu framework'lerde data binding'i destekliyor.
- View, model'deki verilere data binding yapar. Ve event'leri `ViewModel`'a bildirir. `ViewModel`, model'i günceller. modelde olan değişiklik, view'a, `viewModel` aracılığı ile binding olduğu için otomatik yansır.

View code:

```html
<Button
  style="red"
  onClick="@{viewModel.onEventX(buttonNumber)}"
  text='current points: @{viewModel.points} . Click to update your points'
/>
```

ViewModel code:

```java
class MyViewModel {

    // "ObservableField" is an example from Android "Data binding API".
    // But every framework has different API.
    // "points" is the model object here. It can be more complex object
    // or we can have more fields like "points" inside MyViewModel class.
    public final ObservableField<String> points = new ObservableField<>();

    void onEventX(int buttonNumber) {
        points.set(buttonNumber);
    }
}
```

```text
      USER
      ^  v
      ^  v
      ^  v
      ^  v
*show ^  v *click
      ^  v
      ^  v
      ^  v
      ^  v
      ^  v
      ^  v          *onClick
      VIEW  > > > > > > > > > > > > > > VIEWMODEL
                                            v  ^
                                            v  ^
                                            v  ^
                                            v  ^
                                            v  ^
                                    *write  v  ^ *on-model-change-event
                                            v  ^
                                            v  ^
                                            v  ^
                                            v  ^
                                            v  ^
                                            MODEL
```

Şekil için kaynak: http://www.washi.cs.waseda.ac.jp/wp-content/uploads/2017/03/Sami-Lappalainen.pdf "A Pattern Language for MVC Derivatives", authors: Takashi Kobayashi and Sami Lappalainen.

### 📌📌 2- Presentation Model (⟷ Application Model)

- __Smalltalk__ firması tarafından __VisualWorks__ framework'ünde kullanılmak üzere "__Application Model__" isimli pattern ortaya atıldı.
- Bu pattern'de View, Controller, Application Model ve Model 4'lüsü vardır.

## 📌 Comparison Table

| Pattern  | Project size            | `SoC` | Code complexity | Modifiability | Testability | Requires view sync framework |
|----------|-------------------------|-------|-----------------|---------------|-------------|------------------------------|
| MVC      | Small/Medium            | Fair  | Low             | Low           | Fair        | Yes                          |
| MVC2     | Medium/Enterprise       | Good  | Medium/High     | Low           | Good        | No                           |
| MVP (SC) | Medium/Enterprise       | Fair  | High            | High          | Fair        | Yes                          |
| MVP (PV) | Small/Medium/Enterprise | Good  | High            | High          | Good        | No                           |
| MVVM     | Medium/Enterprise       | Good  | Medium/High     | Low           | Very good   | Yes                          |

kaynak: http://www.washi.cs.waseda.ac.jp/wp-content/uploads/2017/03/Sami-Lappalainen.pdf "A Pattern Language for MVC Derivatives", authors: Takashi Kobayashi and Sami Lappalainen.
