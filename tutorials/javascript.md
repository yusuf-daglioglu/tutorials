# JAVASCRIPT

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 JS promise

Diğer programlama dilleri farklı isimlerde kullanmaktadır: Future, delay, deferred...

Ecmascript 6 ile Promise nesnesi native olarak gelmiştir. daha önceki 3üncü parti kütüphanelerle yapılmaktaydı.

güncel Ecmascript promise örneği:

```js
let myPromise = new Promise(function(myResolve, myReject) {
  
  let x = 0;

  if (x == 0) {
    myResolve("success1");
  } else {
    myReject("Error");
  }
});

myPromise.then(
  function(value) {console.log(value);},
  function(error) {console.log(error);}
);
```

Eğer aşağıdaki gibi yazarsak;

- handleResolvedA çalışır,
- handleResolvedA'nin dönüş değerine göre handleResolvedB çalışır...

```js
myPromise
  .then(handleResolvedA)
  .then(handleResolvedB)
  .then(handleResolvedC)
  .catch(handleRejectedAny)
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 passing values to functions

JS always use:

- "pass by value" for primitive types

- "pass by reference" for object types

Java ile aynı.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 "use strict"

bu satır ile JS dilinin derlenmesi daha detaylı ve daha temiz olmasını sağlar. bazı örnekler:

- "var" olamadan global değerler yaratılmasını engeller.

- throws exception on Assignment to a non-writable global:

```js
var undefined = 5; // throws a TypeError
var infinity = 5; // throws a TypeError
```

"use strict" olmasaydı yukarıdaki satırlar hata fırlatmayacak fakat variable simleri özel oldukları için bu isimde variable da oluşmayacaktı.

- throws exception on Assignment to a non-writable property

```js
var obj1 = {};
Object.defineProperty(obj1, 'x', { value: 42, writable: false });
obj1.x = 9; // throws a TypeError
```

- throws exception on Assignment to a `getter` only property.

```js
var obj2 = { get x() { return 17; } };
obj2.x = 5; // throws a TypeError
```

"use strict" satırı bir metodun içine yazılırsa sadece metot için geçerli olmaktadır. eğer globale yazılırsa tüm sistemi etkiler.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Web Hypertext Application Technology Working Group (⟷ WHATWG)

<https://github.com/whatwg> repo'sunda geliştirilen web standartlarını dökümante edilen bir web projesi geliştiren topluluktur. __World Wide Web Consortium (⟷ W3C)__'ten farklı bir topluluktur fakat birlikte çalışmaları vardır. Yayınladıkları eski dökümanlarda ufak farklılıklara rastlayabiliriz. Fakat daha sonrasında bu farklılıklar kapatıldı.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 fetch API

Promise destekliyor.

```js
fetch('www.google.com');

    .then(data => console.log(data))

    .catch(error => console.log('Error:' + error));
```

ES7 async/await example:

```js
async function doAjax() {
    try {
        const res = await fetch('www.google.com');
        const data = await res.text();
        // dikkat edersek hem "text" hem de "fetch" metotları promise dönüyor.
        // çünkü "fetch", header'ları çekiyor promise'i sonlandırırken,
        // "text" metodu da body'yi load edip promise'i sonlandırıyor.
        // yani body stream ediliyor. bu durum aşağıda daha detaylı anlatılıyor.
        console.log(data);
    } catch (error) {
        console.log('Error:' + error);
    }
}

doAjax();
```

Eski tarayıcılar için core JS polyfill kütüphanesi: <https://github.com/github/fetch> (whatwg-fetch isimli npm paketi). Fetch polyfill kendi içinde var XMLHttpRequest'i wrap eder.

Fetch, JQuery.Ajax()'den bazı farklılıklar var.

example codes for fetch:

```js
var metaData = {
    method: 'post',
    headers: {
      "Content-type": "application/x-www-form-urlencoded; charset=UTF-8"
    },
    body: 'foo=bar&lorem=ipsum'
  };

fetch('./API/some.json', metaData)
  .then(
    function(response) {
      if (response.status !== 200) {
        console.log('Looks like there was a problem. Status Code: ' + response.status);
        return;
      }

      // read some headers:
      console.log(response.headers.get('Content-Type'));
      console.log(response.headers.get('Date'));

      // read some metadata:
      console.log(response.status);
      console.log(response.statusText);
      console.log(response.type);
      console.log(response.URL);

      // json() is a stream.
      // response.status, headers are coming first from the `TCP` connection. they are ready inside this function, otherwise fetch will call "catch".
      // but the payload is coming after header and status. payload can be very long. so we call it with stream.
      // kaynak: https://deanhume.com/experimenting-with-the-streams-api/ sayfanın sonundaki Android emulator'lu animasyonda 8 saniyelik download'un nasıl stream edildiği gösteriliyor.
      response.json().then(function(data) {
        console.log(data);
      });
    }
  )
  .catch(function(err) {
    console.log('Fetch Error ' + err);
  });
```

another example:

```js
// instead of response.json() we can call the reader (which is a stream-reader)
const reader = response.body.getReader();

// Not: Bu kısmı daha iyi anlamak için "JSON network stream" başlığına bakılmalı.

// infinity loop while the body is downloading
while(true) {
  // done is true for the last chunk
  // value is Uint8Array of the chunk bytes
  const {done, value} = await reader.read();

  if (done) {
    break;
  }

  console.log(`Received ${value.length} bytes`)
}
```

another example:

```js
function status(response) {
  if (response.status >= 200 && response.status < 300) {
    return Promise.resolve(response)
  } else {
    return Promise.reject(new Error(response.statusText))
  }
}

function json(response) {
  return response.json()
}

fetch('users.json')
  .then(status) // yukarıdaki status metodu oto çağrılıyor ve response parametresi oto geçiliyor
  .then(json) // yukarıdaki status metodu oto çağrılıyor ve response parametresi oto geçiliyor

  // burada istediğimiz kadar then kullanabiliriz. her then metodunda return ettiğimiz her obje, diğer then'e parametre olarak geçecektir.

  .then(function(data) {
    console.log('Request succeeded with JSON response', data);
  }).catch(function(error) {
    console.log('Request failed', error);
  });
```

## 📌 Axios

- browser'da XMLHttpRequest'i wrap eden, NodeJS'te "http" objesi ile çalışan açık kaynaklı JS HTTP client.
- fetch-API ile yapılacak birçok şeyi daha kolay şekilde yapabilmemizi sağlıyor.
- fetch-API'ye göre yapılamayan bazı özellikleri de var (örnek upload'da progress'ler adım adım takip edilemiyor).

## 📌 JSON network stream

JSON formatını stream edebilmek için (örneğin network üzerinden HTTP ile) her parçasının anlamlı olması gerekli. Yoksa client taraf neye göre stream edecek? parçaları nasıl anlamlandıracak? Normalde JSON için böyle bir durum mümkün değil. Fakat buna çözüm olması amaçlı; JSON içindeki field'ların sonuna satır sonu karakteri ekleyerek network'ten atarsak, client taraf ise, satır sonunu görünce var olan field'ı valid bir JSON olarak kabul ederse bu işi çözmüş oluruz. Satır sonu karakterini JSON'a eklediğimizde yeni bir standart yaratmış oluyoruz. Bunun için şu anda (yıl 2023) 2 farklı specification var:

- JSON Lines (web site: <https://jsonlines.org/>)
- Newline Delimited JSON (web site: <https://ndjson.org/>)

Bu iki standart neredeyse aynı. Burada da bu durum tartışılıyor: <https://github.com/ndjson/ndjson-spec/issues/35>

Örneğin Python için geliştirilen "jsonlines" kütüphanesi yukarıda yazan 2 format ile de çalışabiliyor. kaynak: <https://jsonlines.readthedocs.io/en/latest/> 1st sentence.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 vanilla-js

vanilla JS bir framework'tür. fakat içi boş(kod olmayan) bir JS projesidir: http://vanilla-js.com/.

Bu proje, herkesin çok fazla gereksiz JS kütüphanesi kullanmasına tepki olarak oluşturulmuştur.

VanillaJS, saf JS kodu ile yazılmış projelere takma isim olarak kullanılmaktadır.

"vanilla" teriminin sık kullanılması ile birlikte artık herkes her framework/uygulamaya "vanilla" takma ismini prefix olarak koymaktadır. o uygulamayı yada framework'ü dependency olmadan yazıldığını belirtmek için kullanılır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 cross-env

<https://www.npmjs.com/package/cross-env> sitesinde dağıtılan nodejs modülüdür. bu modül ile npm script'lerinde çağrılan komutlara environment variable'ları geçebilmemizi sağlamaktadır. bu özellik normalde de npm tarafından desteklenir, fakat cross-env, bunu OS'tan bağımsız bir syntax ile yapabilmemizi sağlar.

örnek package.json file:

```json
{
  "scripts": {
    "build": "cross-env NODE_ENV=production webpack --config build/webpack.config.js"
  }
}
```

cross-env isimli nodejs modül 2 adet komut export eder: cross-env ve cross-env-shell. cross-env komuta parametre geçilen komut variable'ı okuyabilir. fakat ikinci komut bu variable'ı okuyamaz. iki veya daha fazla komut üst üste çağrılacak ise cross-env-shell kullanılması şarttır. cross-env-shell için örnek:

```json
{
  "scripts": {
    "greet": "cross-env-shell XXX=111 YYY=222 \"echo $XXX && echo $YYY\""
  }
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 flow

flow.org sitesinde dağıtılan JS için type desteği getiren bir typechecker'dır.

örnek JS kodu:

```js
let num: number = 1 + 2;

//fonksiyon örneği:
function square(n: number): number {
  return n * n;
}

square("2"); // hem derleyici hata verir, hem de IDE olan flow eklentimiz (veya lint içindeki flow eklentimiz) hata verir.

// sol tarafta bir nesneye referansı atanmadan yazılan örnekler:
(1234: number);
("hi": string);
(true: boolean);
([1, 2]: Array<number>);
({ prop: "value" }: Object);
(function method() {}: Function);

```

Flow tipleri JS syntax'ında yoktur. bu sebeple derleyicimizde de ayar yapmamız gerekecektir.

- configs:
  - JS dosyanın flow'a göre algılanması için "// @flow" satırının, JS dosyanın bir yerinde olması gerekmektedir.
  - projenin root'unda ".flowconfig" dosyası olmalıdır. bu dosya için ignore/include edilecek dizinler gibi ayarlar bulunuyor

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 JS thread

- JS 1 thread çalışır.
- asenkron fonksiyonlar (setTimeout callback'i gibi) bitse bile, main thread bitmeden çalışmaz.
- Ecmascript'te, 1 thread zorunluluğu yazmamaktadır.
- thread kavramı, OS thread'i olarak algılanmamalıdır. JS motoru multi-thread'dir. JS'in çağırdığı Ajax işlemi multi-thread olabilir. fakat JS'in kod seviyesinde tek thread olarak işletilir. Yani sıra ile işletilir.

## 📌 non-blocking IO

JS tek thread olmasına rağmen birçok yazılım case'inde daha hızlı çalışır.

en çok bilinen basit case'i ele alalım:

server'ımıza HTTP isteği gelir. servlet'e gelen istek DB'ye gider. DB'den işlemin dönüş yapması 5 saniye olsun. bu 5 saniye boyunca 1 thread beklemede durmaktadır (blocking-IO).

günümüzde 1 CPU max 8 CPU'lu olabiliyor. bu durumda; thread'in DB işlemini beklemesi demek, CPU'yu da boşuna bekletmesi demek. aynı zamanda thread memory'si demektir.

İşte JS, DB işlemini callback ile yaptığı için bloklanmıyor. Yani non-blocking çalışıyor.

Java'da bunu tam olarak yapabilmek için "reactive" kütüphanelerini kullanmak gerekli.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 NodeJS

## 📌 express

NodeJS için sunucu yazılmasını kolaylaştıran framework'tür, npm modülüdür. özellikle REST controller işini yapması ile ön plana çıkar.

## 📌 REPL (⟷ Read-Eval-Print Loop ⟷ interactive toplevel ⟷ language shell)

işlevini gören NodeJS içinde gelen komut satırı uygulamasıdır.

sadece "node" komutu komut satırına yazıldığında komut satırı yeni bir satıra geçer ve son kullanıcıdan JS girişi bekler. buraya yazılan JS'ler o anda işletilir.

REPL genel bir kavramdır. Örneğin Java 9 ile de REPL gelmiştir.

## 📌 Yarn

npm'e alternatif NodeJS için paket yöneticisidir.

## 📌 npx

eskiden bağımsız geliştiriliyordu fakat daha sonra npm içine alındı.

npm paket kurar ve "package.json" daki "script" komutlarını koşmamızı sağlar.

fakat npx direk olarak indirilmiş node dependency paketlerini execute edebilmemizi sağlar.

## 📌 Gulp vs Grunt

- NodeJS modülleridir.
- bunlar sayesinde projemizde minify, auto-watch and auto-deploy gibi işlemlerimizi task'lar tanımlayarak yapabiliyoruz.
- task-runner diye isimlendirilirler.
- normalde bu işi npm ile "script" yazarak da yapabilirdik. fakat bu script'ler çok uzun ve parametrik. bu script'ler Gulp ve Grunt'ın içinde var. Biz sadece Gunt'ın config dosyasına parametre verip çalıştırmamız yeterli oluyor.
- Gulp programatik kod yazarak akışları değiştirebiliriz. Oysa Grunt sadece config dosyasını baz alarak koşar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 how to use common static string variables

MomentJS'in kodunda bu satırı görebiliriz:

```js
moment().add(1, 'month');
```

normalde buraya gönderilen string'lerin common bir yerde tutulması tavsiye edilir.

Fakat MomentJS geliştirme ortamında bu kodlar hardcode değildir. prod'a paketlenirken bilinçli hardcode girilir. Çünkü böyle daha hızlı çalışır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 JS core ile obje klonlama

Ecmascript güncel versiyonları ile gelen aşağıdaki ile yapılabilir:

```js
var newObject = { ...oldObject, {} };
```

Syntax yazılımcılar arasında "object spread" deniliyor. (spread kelime anlamı: yayılmış). 3 noktaya (...) "spread operatörü" denir.

Yukarıda oldObject array veya obje olabilir. her türlü JS otomatik iterate ediyor.

Aşağıdaki işlem spread operatörü ile yapılan işlem ile aynı işi yapmaktadır:

```js
var newObject = Object.assign(oldObject);
```

assign metodu sonsuz parametre alıyor. tüm parametreleri tek objede topluyor.

Tek farkı şudur: `Object.assign`, `setter(newValue)` kullanarak yeni objeye değerleri atıyor. yani `setter` metodu tetikleniyor. fakat spread direk nesneyi property atayarak oluşturuyor.

## 📌 JQuery ile obje klonlama

- Shallow copy

Shallow Türkçe kelime anlamı: yüzeysel

```js
var newObject = jQuery.extend({}, oldObject);
```

shallow copy yeni obje yaratıyor. eski objenin en üstteki property'lerini geziyor. bu property'leri literal tipte ise; variable'larını kopyalıyor. eğer property'ler obje ise referansları ile kopyalıyor. alt elementlere girmiyor.

- Deep copy

```js
var newObject = jQuery.extend(true, {}, oldObject);
```

Deep copy'de tüm alt elementler dahi geziliyor ve tüm değerler variable olarak yeni objeye kopyalanıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Watchman

bir dosya dizini veriliyor, bu dosya dizinlerini sürekli takip ediyor eğer bir değişiklik olursa, önceden belirlenen komutları tetikliyor.

Executable dosyası var. opsiyonel olarak FB-Watchman (Facebook geliştirdiği için) ismi ile NodeJS paketi olarak dağıtılıyor. Böylece JS API'si ile de kullanılabiliyor.

Genelde web uygulamalarında development yaparken bu uygulamadan çok yararlanılıyor. live-reloading işlemi yapılması sağlanıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Modernizr

bu JS kütüphanesi ile hangi teknolojilerin tarayıcıda var olup olmadığı kontrol edilebiliyor. aşağıdaki örnekteki gibi tüm teknolojiler tek bir variable'da kontrol edilebiliyor. örnek:

```js
if(modernizr.cookies){

  //cookies enabled
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 JS scope

The "global" scope is the outermost scope. It is accessible from any inner/local scope. global scope on web browser is accessible via "window" object. on other JS engines can be different.

```js
this.window === window.window // true
this === window // true
```

fonksiyonların kendi scope'u vardır. "new" olmazsa yukarıdakinin "this" variable'ı ile, fonksiyon içindeki "this" eşit olur. fakat aynı this'i kullandıkları, aynı scope'ta oldukları anlamına gelmiyor. aşağıdaki örnek bu durumu açıklıyor:

```js
// function tanımlama. burada execute edilmiyor.
function run() {
  // "run" function scope
  var message = 'hello';
  console.log(message);
  console.log(this === window);
}

run(); // burada execute ediliyor.
// prints:
// "hello"
// true

console.log(message); // throws ReferenceError

new run(); // burada execute ediliyor.
// prints:
// "hello"
// false
```

__lexical scoping (⟷ static scoping)__ closure konusunda anlatılmıştır.

## 📌 closure (⟷ lexical closure ⟷ function closure)

closure Türkçe kelime anlamı: kapatma

```js
var add = function () { // function-1

    var counterVAR = 5;
    counter = 5;
    this.counterTHIS = 5;
    let counterLET = 5;
    const counterCONST = 5;

    return function () { // function-2
        counterVAR += 1;
        counter += 1;
        counterTHIS += 1;
        counterLET += 1;
        // const variables can not be changed! they are read-only.
        // (that rule is not about "scopes".)
        // counterCONST += 1;

        console.log("INNER- counterVAR: " + counterVAR);
        console.log("INNER- counter: " + counter);
        console.log("INNER- counterTHIS: " + counterTHIS);
        console.log("INNER- counterLET: " + counterLET);
        console.log("INNER- counterCONST: " + counterCONST);
    }
};

console.log(typeof add); // function (which is "function-1")

var addFunc = add();

console.log(typeof addFunc); // function (which is "function-2")

addFunc();
addFunc();
// new addFunc(); --> ile çalıştırılacak fonksiyonun scope'unu değiştiriyoruz, ama function-2 scope'unda kullanılan bir variable yok. o yüzden aşağıdaki sonuçlar değişmeyecekti.

console.log(typeof addFunc()); // undefined. because function-2 does not have a return value.

// counterVAR is undefined on this scope
// var addFunc = new add(); --> yapsaydık yine de aynı şekilde olacaktı.
try {
console.log("counterVAR:" + counterVAR); // throws undefined exception
} catch(e) { console.log("counterVAR is undefined"); }

// counter, en üst seviye "this" olan "window" objesinin scope'undadır.
// var addFunc = new add(); --> yapsaydık yine de aynı şekilde olacaktı.
console.log("counter:" + counter); // prints 8

// counterTHIS add fonksiyonumuzu "window" scope'unda run ettiğimiz için "counter" ile aynı seviyede tanımlanmıştır.
// var addFunc = new add(); --> yapsaydık, zaten addFunc() hata verecekti.
// Bu kısım özellikle önemli!
// new operatörü kullanmazsak this=window oluyor.
// new operatörü kullanırsak this=MS Windows olmuyor. yeni bir "this" oluyor. fakat bu durumda inner scope'lar buraya erişemiyor.
console.log("counterTHIS:" + counterTHIS); // prints 8

// counterLET is undefined on this scope
// var addFunc = new add(); --> yapsaydık yine de aynı şekilde olacaktı.
try {
console.log("counterLET:" + counterLET); // throws undefined exception
} catch(e) { console.log("counterLET is undefined"); }
```

closure ile API'lerin private metotlar ve değerler içerebilmesi sağlanmaktadır.

resmi olarak closure'un ne olduğu net değil. bazıları: return edilen metot'un önceki metottaki variable'lara hala erişebilme tekniğine "Closure" denirken, bazıları ise return edilen fonksiyona closure adını veriyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 CommonJS module API vs AMD (Asynchronous Module Definition) vs ECMAScript module vs SystemJS

JS dosyaları yazıldığında ve daha sonra kullanılabilir hale getirilmesi için belli bir formatta yazılması gerekmektedir. bu formatların standartlarıdır.

balıkta yazanlar 3 farklı en çok tercih edilen standartlardır.

"ECMAScript module" ECMAScript 6 ile gelmiştir.

CommonJS; sadece sync module load'a izin veriyor. AMD ise buna çözüm olarak sadece asenkron load edilebilen modül formatı olarak ortaya çıkmıştır. Daha sonrasında ECMAScript modüle tanımı getirilmiştir. Fakat geriye uyumluluk ihtiyacı olduğundan pek piyasada tercih edilemiyor.

Yukarıdaki alternatifleri kullanmadan da kendimiz ayrı ayrı dosyalarda ayrı JS objeleri tutup, kendimizde module benzeri bir altyapı kurabiliriz. Fakat bu, static code analyzer'ler tarafından algılanmayacağı için sıkıntı yaşayabiliriz.

Yukarıdaki tüm alternatifler (SystemJS'in nasıl çalıştığını bilmiyorum. bu sebeple onu ayrı değerlendirmek gerekebilir) aslında birer syntax'tır. Bunların çalışabilmeleri için runtime veya build sırasında destek şarttır. Dolayısı ile implementasyona ihtiyacımız var. Bu implementasyonlar şunlar olabilir: web browser JS motoru, NodeJS, RequireJS, Dojo, Webpack and Browserify...

## 📌 require.js

JS kütüphanesi. sadece ilgili JS modüllerinin yüklenmesini sağlamaktadır. yüklenen modüllerin bağımlılıkları varsa onları da otomatik yüklemektedir.

requirejs(['app/main']); ile istediğimiz modülün yüklenmesini sağlıyoruz.

modül tanımlamasını şu şekilde yapıyoruz:

```js
define("alpha", ["credits","products"], function(credits,products) {

        //my module code is here

});
```

bağımlılık olmadığı durumlarda define ikinci aldığı dizi parametresini hiç almayabilir. alpha ise bu tanımlayacağımız modülün ID'si. biri dependency ile bu ID ile çağırıyor bizim modülü.

require("myLib") ile bize "myLib" ID'li kütüphaneyi dönüyor. bu objeyi kullanarak; içinden bir metot çağırabiliriz:

require("myLib").myMethod();

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 library oluştururken genelde yapılan tanımlamalar

```js
if (typeof define === 'function' && define['amd']) {

     define(['jquery'], factory);

} else {

// other codes here
```

Burada "factory" takip edilirse; API'nin (kütüphanenin) olduğu kodu olduğu görülür.

"define" bir fonksiyon mu diye kontrol edilmiş. "define" RequireJS'in modül tanıtmak için public sunduğu bir metot'dur. eğer bu varsa require.js vardır demek oluyor. require.js var ise; define['amd'] ile "define" içerisinde AMD desteğinin olup olmadığına bakılır.

ek not:

```js
define['amd'] = define.amd // true
```

eğer AMD support var ise if bloğuna giriliyor. require.js ile yeni bir modül yaratıyor. 'JQuery' dependency'sini alacak şekilde yeni modülümüz tanımlanıyor. else durumlarda require.js yokken yapılacaklar yazılıyor.

- örnek

```js
(function (global) {

    'use strict';

    var MyModule = function () {

      /* Your awesome module logic here... */

    };

    // ...and here

    MyModule.foo = 'bar';

    // AMD support

    if (typeof define === 'function' && define.amd) {

        define(function () { return MyModule; });

    // CommonJS and Node.js module support.

    } else if (typeof exports !== 'undefined') {

        // Support Node.js specific `module.exports` (which can be a function)

        if (typeof module !== 'undefined' && module.exports) {

            exports = module.exports = MyModule;

        }

        // But always support CommonJS module 1.1.1 spec ('exports' cannot be a function)

        exports.MyModule = MyModule;

    } else {

        // Eğer AMD, NodeJS, CommonJS yoksa window.MyModule ile erişilebilmesi için bu işlem yapılıyor.

        global.MyModule = MyModule;
    }
})(this);
```

sondaki this ile MS Windows objesi yakalanmış oluyor. kendini execute eden metot "global" parametresi ile this'i (window'u) alıyor.

Kütüphaneler kendini parantez içine alıp execute ediyor. Eğer bunu yapmazlarsa "var" oluşturdukları her variable global scope'ta tanımlanır. Bu da diğer lib'lerdeki variable'lerle çakışmasına sebep olabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 web page lifecycle

### 📌📌 events

- > $(document).ready( func );

    readystate interactive olduğunda bu metodu JQuery çalıştırmaktadır. (readystate aşağıda anlatılıyor)

- > document.addEventListener("DOMContentLoaded", func);

    Executes when HTML-Document is loaded and DOM is ready to run any JS safely.

    It waits to execute all internal inline JS's (script tags) and also external src JS documents. only exception is "async" and "defer" attributes of external scripts.

    browser'ların autofill-form (otomatik form tamamlama) DOMContentLoaded event'ini beklemektedir.

    DOMContentLoaded eğer sayfa yüklendikten sonra handler'a set edilirse hiçbir zaman execute edilmez.

- > $(window).load( func ); yada window.onload = func;

    iframe'lerin içi yüklendiğinde, resimler tamamen indirildiğinde bu metot çağrılır

- > window.onunload = func;

    kullanıcı sekmeyi kapattığı zaman

- > window.onbeforeunload = func;

    kullanıcı sekmeyi kapatmak istediği zaman. burada tek seçeneğimiz string döndürmek:

    ```js
    window.onbeforeunload = function() {

        return "There are unsaved changes. Leave now?";
    };
    ```

    bu string'i tarayıcı bir dialog'da gösteriyor ve evet hayır seçeneğini sunuyor. evet seçeneğini seçerse sekme kapatılıyor. hayır'ı seçerse kapatılmıyor.

### 📌📌 readystate

DOMContentLoaded sadece readystate loading durumundayken handler'a attach edilebilir.

```js
if (document.readyState == 'loading') {

  document.addEventListener('DOMContentLoaded', func);
}
```

document readyState's:

```text
"loading" – the document is loading.

"interactive" – the document was fully read.

"complete" – the document was fully read and all resources (like images) are loaded too.
```

readystate'in değiştiği event'i de yakalayabiliriz:

```js
document.addEventListener('readystatechange', func );
```

### 📌📌 event'ler ile ilgili notlar

- aşağıdaki iki metot aynı şeyi yapar:

  - $(window).load( func );

  - $(window).on('load', func )

    Yukarıdaki load metodu document'te alabilir, button da alabilir.

- aşağıdaki iki code ayni anlama geliyor. JQuery sık kullanıldığı için bu şekilde ayarlanmış.

  - $( document ).ready( func );

  - $( func );

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 $ prefix on variables

JS kodlarında çoğu zaman JQuery'nin namespace'i olan $ işareti görülür. bunun sebebi oluşturulan variable'ın JQuery'den dönen bir değeri tuttuğunu belli etmektir. örneğin;

```js
var a = 3;
var $myObject = $("#email"); // myObject JS'in native tipi değil. JQuery'nin özel bir objesi.
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 namespace

JS'te en global değerler namespace olarak kullanılır. örneğin bir kütüphane eklediğimizde kendini buraya bir tanımlama yaparak ekler. bu şekilde herkes buradan onu çağırır. bu sebeple JS'te buradaki değerler namespace'indendir.

genelde bu tanımlama ile init yapılır:

var MAYAPPLICATION = MYAPPLICATION || {};

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 yield keyword

yield Türkçe kelime anlamı: esneme.

fonksiyona kalındığı yerden devam edilebilmesini sağlar. yield return eder, fakat bir "Generator" objesi döndürür. "function \*" ibaresindeki yıldız işareti fonksiyonun bir generator objesi döndürdüğünü temsil eder. örnek:

```js
function* foo(x) {

    while (true) {

        x = x * 2;
        yield x;
    }
}

var g = foo(2);

g.next(); // -> 4

g.next(); // -> 8

g.next(); // -> 16
```

## 📌 yield*

yield içindekileri bir üstteki metodun generator'ı ile birleştirmek istersek yield# kullanırız. örnek:

```js
function* g1() {

  yield 2;

  yield 3;

  yield 4;
}

function* g2() {

  yield 1;

  yield* g1();

  yield 5;
}

var iterator = g2();

console.log(iterator.next()); // {value: 1, done: false}

console.log(iterator.next()); // {value: 2, done: false}

console.log(iterator.next()); // {value: 3, done: false}

console.log(iterator.next()); // {value: 4, done: false}

console.log(iterator.next()); // {value: 5, done: false}

console.log(iterator.next()); // {value: undefined, done: true}
```

## 📌 yield ile hiçbir şey döndürülmediğinde ne olur?

yield keyword'unun yanında hiçbir şey olmadığında, örnek:

```js
function* g1() {

  yield;
}
```

iterator şunu döner: {value: undefined, done: true};

## 📌 generator nasıl done=true olduğunu anlıyor?

eğer yield'dan sonra herhangi bir kod satırı var ise; done:false oluyor.

## 📌 var iterator = g2()

"g2" bir generator dönen obje ise; "var iterator" artık generator objesidir ve "g2()" kodunda parantez açılmasına rağmen; hiçbir kod execute edilmez. g2'nin ilk satırları dahi ancak iterator.next() dediğimiz zaman çağrılır.

## 📌 yield kullanılan fonksiyonlarda return keyword'ü

return de iterator next metodunda yield gibi değer döndürür. yield ile apaynı davranışı sergiler. fakat her zaman done:true'dur. bundan sonra iterator devam etmez. tek fark budur.

return değeri aşağıdaki gibi değer atama görevi görmektedir.

```js
function# g1() {

  yield 2;
  return 3;
}

function# g2() {

  yield 1;
  var a = yield# g1();
  console.log(a);
  yield 4;
}

var iterator = g2();

console.log(iterator.next()); // {value: 1, done: false}

console.log(iterator.next()); // {value: 2, done: false}

console.log(iterator.next()); // {value: 3, done: false}

3 //console.log(a) çıktısı

console.log(iterator.next()); // {value: 4, done: false}

console.log(iterator.next()); // {value: undefined, done: true}
```

Yukarıda "var a = yield# g1();" daki satırda # karakteri hiç kullanılmasaydı:

var a = yield g1();

a değerine hiçbir değer atanmayacaktı. generator'ımız;

{value: 2, done: false}

yerine;

{value: Generator, done: false}

dönecekti. ve a değeri undefined olacağından console.log undefined basacaktı. fakat; next(); 'e yollayacağımız ilk parametre a'ya atanıyor olacaktı. aşağıda direk örnekle gidelim:

var a = yield g1(); satırını işleten next() metodundan sonraki next() metoduna parametre geçseydik: next(99); o zaman "var a" değerimiz 99 olacaktı.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 noConflict metotları

JS'te namespace'ler diğer dillerdeki gibi düzenli yönetilemeyebiliyor. karışıklık olmaması için her kütüphane en tepede noConflict isimli bir metot tanımlıyor. örneğin JQuery'de bu metot şunu yapıyor.

```js
var j = jQuery.noConflict();
j( "div p" ).hide();
$( "content" ).style.display = "none";
```

Yukarıda noConflict metoduna true parametresi geçilseydi artık 3 üncü satırda $ işareti de kullanılamaz hale gelecekti. noConflict(true)'yu, $ işaretini başka bir kütüphane kullanıyor ise yapabiliriz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 arrays

### 📌📌 obje'den türemiştir

```js
var cars = ["Porsche", "Volvo", "BMW"];

var cars = new Array("Porsche", "Volvo", "BMW"); // array fonksiyonu.
```

"new Array" tercih edilmemeli, çünkü yanlış anlaşılma olabilir. zira constructor tek parametre ve sayı alınca boş array oluşturuyor:

```js
new Array(20);
// 20 elemanlı bir dizi oluştu. bazıları bunu 1 elemanlı bir dizi olduğunu düşünebilir.
```

### 📌📌 erişmek için

```js
cars[1];
```

JS'te diziler birer objedir. aslıda her dizi elemanı bir property'dir. sadece access'i farklıdır. bunun sebebi:

herhangi bir objede 0 veya 1 isimli property'ye şu şekilde erişebiliyoruz:

```js
myArr["0"]
```

yada

```js
myArr[0]
```

Sayı vererek eriştiğimizde aslında; JS interpreter'ı onu string'e çeviriyor. Bu sebeple `myArr["0"]` değeri `myArr[0]` ile aynı şeyi veriyor.

Yani bu erişim dizilere özgü bir durum değildir. dizilerin içindeki metotlar otomatik property'leri sayısal cinsten tutmaktadır. örneğin "JQuery objesi" (konu başka başlıkta anlatılıyor) dizi dönmüyor fakat liste içinden bir elemana erişmek istediğimizde; yine index ile erişebiliyoruz.

### 📌📌 array'in hazır metotları

- length

```js
cars.length;
```

- sort

```js
cars.sort();
```

default sorts alphabetically.

- pop

```js
fruits.pop();
```

removes last element

- push

```js
fruits.push("Banana");
```

adds element to last index and return the last new index

- shift

```js
fruits.shift();
```

removes first element

- unshift

```js
fruits.unshift("Lemon");
```

adds element to first index

- splice

```js
fruits.splice(2, 4, "Lemon", "Banana");
```

2'inci elemandan itibaren, 4 tane eleman siliyor (silinen elemanların index'leri: 3,4,5,6). ardından, 2'inci index'ten itibaren sağ tarafta olan bütün argümanları; "Lemon" ve "Banana" elemanlarını diziye ekliyor. örnek:

```js
var fruits = ["1", "2", "3", "4"];
fruits.splice(2, 0, "x", "y");
prints(fruits); // prints 1,2,x,y,3,4
```

- concat

```js
arr1.concat(arr2, arr3);
```

sırası ile birleştiriyor. sonsuz parametre kabul ediyor.

- slice

```js
fruits.slice(1,3);
```

returns new array from 1-3 index.

## 📌 For loop comparison table

| how to call                                                      | change scope/context | iterate over objects | how to break                              | access all array list | change orijinal variable | iterate over empty slots |
|------------------------------------------------------------------|----------------------|----------------------|-------------------------------------------|-----------------------|--------------------------|--------------------------|
| lodash.forEach(arr,   function( index, val )); (or)  lodash.each | false                | true                 | return  false/null/undefined  on callback | false                 | true (read NOT-A)        | true                     |
| JQuery.each( arr,  function( index, val ));                      | false                | true                 | return false/null/undefined on callback   | false                 | true (read NOT-A)        | true                     |
| for(val in arr) { ... }                                          | true                 | true                 | break keyword                             | true                  | true                     | false                    |
| for(i=0; i<arr.length; i++) { ... }                              | true                 | false                | break keyword                             | true                  | true                     | true                     |
| underscore.each(arr,function(val, index, arr),context)           | true                 | true                 | not possible                              | true                  | true (read NOT-A)        | true                     |
| underscore.every(arr,function(val, index, arr),context)          | true                 | true                 | return false/null/undefined on callback   | true                  | true (read NOT-A)        | true                     |
| arr.forEach(function(val, index, arr), context);                 | true                 | false                | not possible                              | true                  | true (read NOT-A)        | false                    |

NOT-A:

orijinal elemanı callback'e geçiyor. dolayısı ile değişkenler burada değiştirilebilirler. fakat genel JS kuralları gereği bilinmesi gerekenler var. callback metodu yeni bir fonksiyon olacağı için geçilen değerler 2 durumda değiştirilemezler:

1- eğer yeni bir obje ataması yapılırsa (çünkü yeni objenin referansı farklı. orijinal  obje'nin içeriği değiştirilmemiş olacak. ve orijinal dizi; referansla elemanları tutuyor.

2- değiştirilen eleman bir primitive (string, int...) ise. çünkü primitive'ler metotlara atanırken sadece değerleri geçer (referansları değil).

Bu iki durum için geçici olarak "access all array list" kuralı true ise; orijinal objenin içindeki referans manuel değiştirilir.

### 📌📌 Tablo hakkında

- lodash.forEachRight ve lodash.eachRight aynı metotlardır. lodash.each ile aynı özelliklere sahipler. tek farkı liste elemanlarının sırasını tersten dönmesidir.

- "iterate over empty slots" kuralı:

   boş slotlar (hiç atanmamış değerler) için callback çağrılıyor dönmüyor.

   örnek:

   ```js
   var a = ['a0'];

   a[3] = 'a3';

   a[4] = undefined;

   a[5] = null;
   ```

   sadece `a[1]` ve `a[2]` boş. bunlar için `for loop` callback çağrılması.

   eğer boş olup olmadığı kütüphane içinde kontrol ediliyorsa bu yavaşlığa sebep olmaktadır.

- "`iterate over object`" kuralı:

   eğer list bir `JS` objesi ise, yine döngü çalışıyor ve her property'si dönmeye başlıyor.

   metodun aldığı parametreler'in anlamı değişiyor:

   list array ise; (value, index)

   list obje ise; (value, key)

- elle for döngüsünün yazılması ( for(i++) şeklinde ) diğer tüm döngülerden çok daha performanslı çalışmaktadır.

- $.each() metodu ile $(selector).each() aynı değildir. $(selector).each() bir JQuery objesinin içerisinde dönmeye yarıyor.

- çoğu kütüphanenin "map" metotları da vardır. bu metotlar for metotları ile aynı parametreleri alır. map metotları callback metotlarından return edilen her elemanı yeni bir diziye atar. bu yeni diziyi de map metodu return eder. map metodunun temel felsefesi budur. for yerine map yada map yerine for kullanmak anti-pattern'dir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Ecmascript vs JS vs Jscript vs ActionScript

JS ve Jscript vs ActionScript; birer Ecmascript implementasyonudur. buna rağmen, bazı implementasyonlar kendilerince spesifikasyon dışı ek özellikler içerebilirler.

## 📌 CoffeeScript vs typescript

JS'e çevrilen bir programlama dilleridir.

## 📌 source-to-source compiler (⟷ transcompiler ⟷ transpiler)

Bir dil, derlenmeden önce başka bir dile çevrilebilir. Bu işlemi yapan çeviriciye transpiler denir.

## 📌 ECMAScript

ECMA-262 ve ISO/IEC 16262 standartları ile oluşturulan betik dilidir.

## 📌 ECMAScript (⟷ ES) version history

| Name                                              | Date Published |
|---------------------------------------------------|----------------|
| ES5                                               | 2009           |
| ES5.1                                             | 2011           |
| ECMAScript 2015 (⟷ ES2015 ⟷ ECMAScript 6 ⟷ ES6)   | 2015           |
| ECMAScript 2016 (⟷ ES2016 ⟷ ECMAScript 7 ⟷ ES7)   | 2016           |
| ECMAScript 2017 (⟷ ES2017 ⟷ ECMAScript 8 ⟷ ES8)   | 2017           |
| ECMAScript 2018 (⟷ ES2018 ⟷ ECMAScript 9 ⟷ ES9)   | 2018           |
| ECMAScript 2019 (⟷ ES2019 ⟷ ECMAScript 10 ⟷ ES10) | 2019           |
| ECMAScript 2020 (⟷ ES2020 ⟷ ECMAScript 11 ⟷ ES11) | 2020           |
| ECMAScript 2021 (⟷ ES2021 ⟷ ECMAScript 12 ⟷ ES12) | 2021           |
| ECMAScript 2022 (⟷ ES2022 ⟷ ECMAScript 13 ⟷ ES13) | 2022           |
| ECMAScript 2023 (⟷ ES2023 ⟷ ECMAScript 14 ⟷ ES14) | 2023           |
| ECMAScript 2024 (⟷ ES2024 ⟷ ECMAScript 15 ⟷ ES15) | 2024           |
| ECMAScript 2025 (⟷ ES2025 ⟷ ECMAScript 16 ⟷ ES16) | 2025           |

"ES.Next" ECMAScript'in en son versiyonunu belirten özel bir takma addır.

Ecmascript ile JS'in versiyonları aynıdır. Çünkü Ecmascript'in referans implementasyonu JS'tir.

Typescript'in herhangi bir sürümü, JS'in herhangi bir sürümüne çevrilebilir. ancak istisna olarak bazı özellikler JS'te hiç desteklenmiyor olabilir. böyle durumlarda, JS tarafında sürüm yükseltmek gerekebilir.

## 📌 Ecmascript implementasyonları standardize edilirken, her sürüm kendi içinde belli sürümlere ayrılır

| sürüm kodu | sürüm adı | açıklama                                          |
|------------|-----------|---------------------------------------------------|
| stage-0    | Strawman  | just an idea, possible Babel plugin               |
| stage-1    | Proposal  | this is worth working on                          |
| stage-2    | Draft     | initial spec                                      |
| stage-3    | Candidate | complete spec and initial browser implementations |
| stage-4    | Finished  | will be added to the next yearly release          |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Browser Object Model (⟷ BOM)

HTML tarayıcılarının JS'e ekledikleri JS objeleridir. bu objeler NodeJS gibi ortamlarda olmuyorlar.

### 📌📌 window

genel bir objedir. JS'te global tanımlanan her değer buraya bağlanmaktadır. örnek metot:

> window.close()

### 📌📌 window.screen

fiziksel ekranla ilgili bilgiler içeren JS objesidir. örnek:

- screen.width

- screen.height

### 📌📌 window.location

URL ile ilgili metotlar buradadır.

> window.location.href

### 📌📌 window.history

> history.back();

### 📌📌 window.navigator

tarayıcının user agent bilgilerini barındırır.

### 📌📌 document.cookie

cookie metotları buradadır.

### 📌📌 document

MS Windows.document ile de aynı objeye erişilebiliyor. DOM ile ilgili metotlar buradadır. örnek:

- document.appendChild(element);

- document.write(text);

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 JQuery object (⟷ JQuery collection object)

$( 'li' ); metodundan dönen obje, JQuery objesi olarak adlandırılır. bu adlandırma JQuery'nin kendi içinde belirlediği bir standarttır. yazılımcıların birbiri arasında anlaşabilmesi için konulan özel bir isimdir. JS'te her şey ( $, JQuery dahi ) birer obje olduğundan "JS objesi" ile isim karışıklığı yaşanabilmektedir.

JQuery object, array objesi ile benzer property'ler içeriyor fakat Array yada Array'den türemiş bir obje değildir.

Örnek kullanımlar:

```js
var listItems = $( 'li' );
var rawListItem = listItems[0]; // same with: listItems.get( 0 );
$( '<p>Hello!</p>' ); //creates new element

//search inside a specific block
var paragraph = $( 'p' );
$( 'a', paragraph );
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 JS executes code in 3 phases

1- First, the engine walks through the code, looks for function declarations and hoists them (moves them to the top)

2- it hoists variable declarations

3- finally it runs the "normalized" code.

example:

```js
function bar() {

        return foo; // foo değerinin tanımı yok.

        foo = 10; // foo değerinin hala tanımı yok.

        function foo() {} // foo değerini tanımlaması burada. yukarıda eksikti. bu sebeple kodun en başına alınacak.

        var foo = 11; // foo değerinin artık tanımlanması var. "var" keyword'üne artık gerek yok.
}
```

"normalized" code:

```js
function bar() {

    function foo() {}

    return foo; // kod execute edildiğinde kod bu kısımda sonlanacaktır. aşağıdaki kodlar unused'dır.

    foo = 10;

    foo = 11;
}
```
