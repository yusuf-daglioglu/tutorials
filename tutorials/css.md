# CSS

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Cascading Style Sheets (⟷ CSS)

## 📌 sürümleri

2'inci sürümde önemli hatalar bulunduğundan 2.1 spec'i ile bu hatalar düzeltilmiştir.

3'üncü sürüm ise modüler yapıdadır. spec'ler modüllere bölünmüştür ve hepsi 2.1 üzerine kurulu yeni sürümler çıkarır. her spec (modül) farklı hızda ilerlemektedir. örnek modüller: Selector, borders, images...

Bazı modüller 2010'ların başlarında 4'üncü sürümlere atlamıştır. Oysa diğer modüller hala 3üncü sürümdedir. Yani resmi olarak tek bir CSS4 standartı yoktur. Her alt modülü ayrı ayrı spec numarası ile belirtmek gereklidir.

Bu linkte çok basit bir görsel ile açıklanmıştır: https://developer.mozilla.org/en-US/docs/Archive/CSS3#modules_and_the_standardization_process

CSS spec'leri belirtilirken 'sürüm' terimi yerine 'level' kullanılır. örnek:

- 'Selectors Level 3' -> selector modülünün CSS 3 versiyonu
- CSS level 1

## 📌 Bir HTML Objenin kenarların property'leri (bölümleri)

Bu modele tasarımcılar kendi aralarında "Box Model" olarak adlandırıyor.

Content -> Padding -> Border -> Margin

## 📌 outline

bu property box model'deki `margin (⟷ tr:marj)` bölümüne denk geliyor. fakat kullandığı alanı diğer elementlerin üstüne yazıyor. yani kendi alanı olmuyor. örneğin 10px'lik kırmızı bir outline yaptık. bu kırmızı background sayfanın arka plan renginin üzerine yazılıyor. fakat sağ ve sol taraftaki elementler ile 10 px'lik boş yer kaplamadığı için, etrafındaki elementlerin gözükmesin engelliyor.

## 📌 display property

- none: hides the element

- block: this elements start from new line

- inline: bir önceki eleman ile aynı satırda devam etmeye çalışıyor.

## 📌 div vs span

ikisi de temelde bir bölgeyi (HTML obje grubunu) tanımlamak için kullanılmaktadır. Fakat span varsayılan olarak display:inline iken; div display:block'tur. CSS ile bu özellikleri override edilebilir. aslında her HTML elementinin varsayılan olarak block yada inline olup olmadığı bilgisi vardır. fakat genelde diğer elementler (p, a...) div veya span'ın içine alındığından, display özelliği en çok bilinmesi gereken div ve span'dır.

## 📌 position property

position şu değerleri alabilir:

- __static__: varsayılan değerdir. top, right, left, bottom property'lerindeki değerlerini hiçe sayar.

aşağıdaki örnekte; 'left: 30px' un hiçbir önemi yoktur. myClass içeren div elementi normal olması gerektiği pozisyondadır. sadece top, right, left, bottom property'lerindeki değerlerini hiçe sayar.

```html
<style>
div.myClassStaticExample {
  position: static;
  left: 30px;
  border: 3px solid #73AD21;
}
</style>

<h2>ABC</h2>

<div class="myClassStaticExample">XYZ</div>
```

- __relative__: HTML'de olması gereken normal pozisyonunu sıfır noktası olarak kabul eder.

relative'in, static ile tek farkı: top, right, left, bottom property'lerindeki değerlerini hiçe saymamasıdır.

static'teki aynı örneği; static değerini relative yaparak örneklendirirsek:

```html
<style>
div.myClassRelativeExample {
  position: relative;
  left: 30px;
  border: 3px solid green;
}
</style>

<h2>ABC</h2>

<div class="myClassRelativeExample">XYZ</div>
```

- __fixed__: top, right, left, bottom property'lerindeki değerleri viewport'unun görünen kısmını baz alarak kullanır. sayfa scroll ettikçe yeri değişmez. pencerede sürekli aynı pozisyonda durur.

- __absolute__: fixed ile benzerdir. tek farkı; viewport'un görünen kısmına değil, sayfanın başlangıcını referans (sıfır noktası) olarak kabul etmesidir. böylece scroll ettikçe sayfada yeri değişir.

- __sticky__: sayfa ilk yüklendiğinde "relative" özelliğindedir. fakat sayfa scroll edildiğinde ve görünmeyecek duruma geldiğinde, otomatik olarak "fixed" niteliğine geçer.

## 📌 z-index

bu property top, bottom, right, left gibidir. X ve Y koordinatlarına ek olarak Z koordinatını verir. elementler üst üste denk geldiğinde hangi elementin son kullanıcıya gözükeceğini belirlemek için kullanılır.

## 📌 overflow

bir objenin kendi bloğuna sığmaması durumunda scrollbar'ın davranışına karar verir. sadece overflow-x, overflow-y sadece bir kısımda scroll'un çıkıp çıkmayacağına karar vermek için kullanılır.

## 📌 transform

bu property birçok farklı değer alabiliyor. elementi 2d ve 3d olarak çevirebiliyor. anlık olarak çevirme işlemi yapılabildiği gibi, direk sayfa yüklendiğinde çevrilmiş hali ile de objeyi oluşturmuş olabiliyor.

örnekler:

```html
<div style="width: 150px;
            height: 80px;
            background-color: red;
            transform: rotate(20deg)">Hello World 1!</div>

<div style="width: 150px;
            height: 80px;
            background-color: blue;
            transform: skewY(20deg)">Hello World 2!</div>

<div style="width: 150px;
            height: 80px;
            background-color: yellow;
            transform: scaleY(1.5)">Hello World 3!</div>
```

## 📌 media query

aşağıdaki örnekte eğer ekran min-width 480 ise; içerideki CSS etki ediyor. yoksa bu CSS'ler disable oluyor.

```css
@media screen and (min-width: 480px) {

 body {
      background-color: lightgreen;
     }
}
```

format bu şekildedir:

```css
@media not|only mediatype and (expressions) {

 CSS-Code;
}
```

mediatype şunlar olabilir:

all, print (print edilirken bu CSS uygulanır), screen (PC, mobil, tablet), speech (when screen reader reads the page)

diğer media tipleri yeni sürüm CSS'lerde deprecated olmuştur (2017 yılı).

web tarayıcılarda olan `reader view` özelliği web tarayıcıların kendince kullandıkları bir yöntemdir. özel olarak CSS/JS'ten yakalanamamaktadırlar. örneğin web tarayıcıları, `reader view` tarafından basılınca sadece en az 10 karakter içeren `<p>` içindeki text'leri görüntülemektedirler.

## 📌 !important

'CSS Precedence' konusu altında incelenmektedir.

kullanımı:

```css
body {
    background-color: white !important;
    }
```

aynı HTML elementini etkileyen birçok CSS değeri olabilir. örneğin; bir \<p> elementine inline CSS ile text rengini değiştirelim. birde CSS dosyamızda rengini değiştirelim, bir de başka bir CSS dosyasında rengini değiştirelim. hangisi geçerli olacak? CSS standartlarında bunların kuralları var. öncelik inline CSS'tedir. daha sonraki öncelik (eğer inline CSS yok ise önemsenecek değer); __spesifik__ olan CSS değerlerindedir. spesifik; o elementi daha iyi tanımlayan değer olarak düşünülebilir. örneğin 'ID' en spesifik CSS selector'üdür. dolayısı le selector'de ne yazdığımız çok önemli. öncelik sırası şu şekildedir:

| CSS selector type | priority - comment                                                                                                            |
|-------------------|-------------------------------------------------------------------------------------------------------------------------------|
| Inherited styles  | Lowest specificity of all selectors - since an inherited style targets the element's parent, and not the HTML element itself. |
| *                 | Lowest specificity of all directly targeted selectors                                                                         |
| element           | Higher specificity than universal selector and inherited styles.                                                              |
| attribute         | Higher specificity than element selector                                                                                      |
| class             | Higher specificity than attribute, element and universal selectors.                                                           |
| ID                | Higher specificity then class selector.                                                                                       |

'!important' ile o özelliği en yüksek önceliğe taşımız oluyoruz.

Eğer 2 important var ise bu sefer CSS selector değerine göre karar veriliyor. fakat eğer 2 CSS selector aynı ise, bu sefer CSS dosyasındaki satır sayısı önemli oluyor.

## 📌 farklı yazım şekilleri

aşağıdaki iki örnekte aynı anlama gelmektedir:

```css
div {
  padding-top: 10px;
  padding-right: 20px;
  padding-bottom: 30px;
  padding-left: 40px;
}
```

```css
div {
  padding: 10px 20px 30px 40px;
}
```

## 📌 pseudo element

web tarayıcısının otomatik tespit ettiği bölgelerdir. bu bölgelere CSS uygulayabilmemizi sağlar. HTML DOM'undan çekilemezler.

```css
selector::pseudo-element {
  property:value;
}
```

örnek 1:

```css
p::first-line {
  color: #ff0000;
  font-variant: small-caps;
}
```

örnek 2:

h1 başlığının sağına resim ekler.

```css
h1::after {
  content: url(smiley.gif);
}
```

web tarayıcısında developer menu açılıp, sayfanın HTML satırları incelendiğinde aşağıdaki bölge'de şunu görürüz:

> ::after

## 📌 pseudo class

pseudo element; belli bir bölge için CSS yazabilmemizi sağlarken, pseudo class ise bir özelliği temsil eder ve o özelliğe sahip element için CSS yazabilmemizi sağlar.

pseudo elementler çift iki nokta ile gösterilirken,  pseudo class'larda tek iki nokta karakteri vardır:

```css
selector:pseudo-class {
  property:value;
}
```

örnek 1:

```css
/* visited link */
a:visited {
  color: #00FF00;
}

/* mouse over link */
a:hover {
  color: #FF00FF;
}
```

örnek 2:

```css
p:first-child i {
  color: blue;
}
```
