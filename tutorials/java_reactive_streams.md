# JAVA REACTIVE STREAMS

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 reactive (⟷ tr:reaktif) programming

`reactive` `Türkçe` kelime anlamı: duyarlı, tepkili.

`reactive` bir programlama paradigmasıdır.

çoğu yerde fonksiyonel programlama ile karıştırılır fakat farklı kavramlardır. fakat `reactive` programlamak için fonksiyonel programlama çok idealidir.

`a = b + c` dediğimizde `b` ve `c` toplanır sonucu `a`'ya atanır. peki `b` değişirse, `a` değişir mi? normalde hayır. fakat `reactive` programlamada değişmesi bekleniyor. `reactive` programlama; data'ların değişimi ve bu değişimin etkileri/yayılımına dayanır.

`Reactive` programlama ile yapılan her şey diğer programlama paradigmaları kullanılarak da yapılır. burada amaç; development sürecine bir yön vermektir. şunlar ağırlıklı kullanılır:

- `reactive`'de `event`'lere `subscribe` işlemi çok sıkça kullanılır.
- data değişimleri de birer `event` olarak algılanır
- `subscribe` olurken `Listener`/`handler` sınıfı atmak yerine sadece ilgili metot atılır. `listener` sınıfı komple oluşturulmaz. kod fazlalığı olmaz. `listener` sınıfı yarattığımızda override etmek zorunda kaldığımız fakat kullanmadığımız metotlarda olur. onları da yazmak durumunda kalmayız.
- `event`'lerimiz `non-blocking` thread mekanizmasını destekliyor olur.
- fonksiyonel dil; `lambda` ve `stream` gibi teknolojilerden çok sıkça yararlanılır.
- bazı `listener`'larda hatayı yakalamak için try ve catch mekanizması gereklidir. fakat `reactive`'de hatalar onError metotları ile yakalanır.
- diğer paradigmalarda, DB'den veri çekerken tüm veriyi çekeriz. oysa reative'de 10 adet veri çekilir event tetiklenir. daha sonra diğer 10 adet daha çekilir ve event tetiklenir. amaç data geldikçe harekete geçmektir. eskisi gibi aldım yolladım kaydettim bitti mantığı yok. data aktıkça, sistem buna hazırdır (tepkilidir).

Sonuç olarak `reactive`'e uygun yazarken, event başladığında yaptığınız işlemde `side effect`'leri düşünmezsiniz. çünkü zaten her `side effect`'in subscriber'ı mevcuttur. onlar işlerini yapacaklardır. bir programda binlerce/milyarlarca `side effect` olduğunu düşünürseniz reactive çok ideal olacaktır. yani kod akışını takip etmek yerine, olayları takip etmeye çalışırız.

## 📌 reactive vs event-driven

`reactive`, `event-driven`'ın bir çeşididir. klasik `event-driven` development bir event gerçekleştiğinde devreye giren kodları barındırır, fakat React'ta event data'nın da değişmesini kapsar. yani data değiştiğinde event tetiklenir. klasik `event-driven` sistemlerde data'nın değişmesi diye bir kavram yoktur.

## 📌 reactive system

büyük resme bakıldığında (mimarisel olarak) `reactive` bir sistem için aşağıdakiler şarttır ve bunlar <www.reactivemanifesto.org'da> belirtilmiştir:

- `Responsive` (Duyarlı): sistem her `event`'e cevap veriyor olmalı ve mümkün olduğunca hızlı cevap vermelidir. timeout gibi hata durumlarında da hata `event`'leri devreye girmelidir.

- `Resilient` (dirençli): sistem hatalar karşısında da `responsive` davranır

- `Elastic` (esnek): ölçeklendirilebilir durumda olmalıdır. daha fazla kaynak üzerinde çalıştığında daha hızlı cevap verebilmelidir.

- `Message Driven` (mesaj güdümlü): her şey asenkron mesajlaşma tekniği ile gerçekleşmelidir.

`reactive` programming, `reactive` sistem'in uygulanabilmesi için uygun bir programlama paradigmasıdır.

not: `reactive` manifesto'da `reactive` programlama'daki data değişimine tepki gösterme zorunluluğu maddesini içermemektedir.

## 📌 Reactive Streams

`Reactive Streams`, `reactive` programlamaya uygun data çekme ve işleme politikası için olan sınıflardır.

örnek: data çekerken tüm data yerine 10'ar 10'ar çekmemizi sağlayan `API`'ler `React` yapısına uygundur. dikkat edilirse 10'ar 10'ar çekme işlemi bir `stream` yapısıdır. bu sebeple `reactive` prefix'i alarak, `reactive stream` olarak adlandırılır.

`Java` 9 ile gelen `reactive` `API`'sinin ismi `Reactive Streams` olarak geçer. `java.util.concurrent.Flow` paketi altındaki sınıfları içeriyor.

`reactive stream` paketleri `1.0.2` öncesi `org.reactivestream` altındaydı. `1.0.2` ile birlikte `JDK`'ya dahil edildi ve `java.util.concurrent.Flow` altına taşındı. `kaynak: https://www.reactive-streams.org/announce-1.0.2`

Aşağıdaki `dependency` her iki paketinde birbirine referans edebilmesini sağlıyor.

```xml
<dependency>
   <groupId>org.reactivestreams</groupId>
   <artifactId>reactive-streams-flow-adapters</artifactId>
  <version>1.0.2</version>
</dependency>
```

## 📌 ReactiveX (⟷ Reactive Extensions)

tüm programlama dilleri için `reactive` `API` kütüphanesi oluşturmayı amaçlayan proje.

## 📌 RxJava

`ReactiveX` `Java` implementasyonu. Aynı zamanda `Java` 9 ile gelen `Reactive Streams` implementasyonudur.

Diğer diller için implementasyonlar `Rx` prefix'i iledir: `RxJS`, `RX.Net`, `RxGo` gibi.

## 📌 Reactor

`Java` 9 ile gelen `Reactive Streams` implementasyonudur.

`Spring` `5.0` ile birlikte, `spring-webflux` paketi ile `spring-mvc`'ye `Reactor` ile `reactive` programlama desteği getirmiştir.

`Reactor`; `Mono` ve `Flux` objelerini sunar. Bu objeler, `Java` 9 ile gelen `reactive stream`'in `Publisher`'ını implemente eder.

## 📌 reactor.core.publisher.Flux

is a `publisher` that produces 0 to `N` values. It could be unbounded. Operations that return multiple elements use this type.

## 📌 reactor.core.publisher.Mono

is a `publisher` that produces 0 to 1 value. Operations that return a single element use this type.

örnek basit bir kod:

```java
@RestController
public class GreetReactiveController {

    @GetMapping("/greetings")
    public Publisher<Greeting> greetingPublisher() {

        Flux<Greeting> greetingFlux = Flux.<Greeting>generate(sink -> sink.next(new Greeting("Hello"))).take(50);

        return greetingFlux;
    }
}
```

Temel olarak `non-blocking` olması için akış bu şekilde ilerliyor:

`greetingFlux` objesi özel bir `Webflux` sınıfı. `greetingPublisher` metodu neredeyse başladığı gibi bitiyor. fakat `greetingFlux` objesi `onsuccess` yada `onerror` olunca `Spring Webflux` altyapısı `servlet`'ten cevabı önyüze dönüyor. `onerror` yada `onsuccess` olması uzun zaman da alabilirdi. fakat biz, `greetingPublisher` metodunun `thread`'ini kapatmış oluyoruz. dolayısı ile `onerror` veya `onsuccess` ne kadar geç tamamlanırsa tamamlansın, `thread`'imiz açık kalmamış olacaktır.

## 📌 Reactive Streams vs legacy stream

`reactive stream` özel bir kütüphane ismidir. `reactive stream` kendince belli özellikleri barındıran `stream`'lerdir. örnek: `backpressure`, `consumer-producer` modeli, data'ları parça parça çekebilme... Bu tarz nitelikler `Java`'daki standart `stream`'lerde yoktur.

## 📌 cold source (⟷ cold Observables) vs hot source (⟷ Hot Observables)

`cold source`, `consumer` `pull` etmedikçe data yollamayan kaynaktır. oysa `hot source` `pull` etmesek de bize data yollayan kaynaktır. örnek dosya okuduğumuz bir `stream`, `cold store`'dur. fakat `TCP` soketten bize bilgi yollayan bir `stream`, `hot source`'dir.

`backpressure`'ı her iki kaynak çeşidinde de uygulayabiliriz, fakat `cold source`'a uyguladığımız `backpressure` çok çok daha verimli olmaktadır.

```java
// generates values on demand.
// it is cold source.
Flowable.range(1, 100);
```

## 📌 Observable vs Flowable

`RxJava` 1'de, `backpressure` uygulayabilen veya uygulamadan çalışabilmemizin için tek bir sınıf vardı. oda `Observable`'dı. oysa `RxJava` 2 ile birlikte `Observable` `backpressure` yapamazken, `Flowable` `backpressure` yeteneklerine sahiptir.

Convert `Observable` to `Flowable`:

```java
Observable<Integer> observable = Observable.just(1, 2, 3);
Flowable<Integer> flow = observable.toFlowable(BackpressureStrategy.BUFFER);
```

## 📌 örnek RxJava kodlar

```java
Observable<String> o = Observable.from("a", "b", "c");

int[] list = [5, 6, 7, 8]
Observable<Integer> o2 = Observable.from(list);

// 1 elemanlık observable oluşturuyor.
Observable<String> o3 = Observable.just("only one object");

Flowable<Integer> integerFlowable = Flowable.just(1, 2, 3, 4);
```

```java
// aşağıdaki örnekte map ve filter'a geçilen obje terminolojik olarak şu terimlerle ifade edilir: emission, emits, item, event, signal, data and message.

// aşağıdaki örnekte, map filter'ın upstream'idir, tersi ise downstream'dir.

Flowable<Integer> flow = Flowable.range(1, 5)
  .map(v -> v * v)
  .filter(v -> v % 3 == 0)
  .sorted();

// Burada henüz yukarıdaki kod execute edilmez. Sadece tanımlama yapılmıştır. Bu sebeple yukarıya "Assembly time" fazı denir.

// aşağıdaki satırda işlem başlatılmış olur:
flow.subscribe(System.out::println)
```

```java
Observable<Integer> observable = Observable
  .range(1, 100) // 1 den 100 e kadar integer oluşturur.
  .take(30); // sadece ilk 30 elemanı döndür. backpressure örneği.
  // take'e alternatif farklı metotlarda var. örnek: takeLatest(10) --> son 10 tanesini alır.

observable.subscribe(number -> { print(number); });
```

```java
// 1 elemanlık observable oluşturuyor.
Observable<String> observable = Observable.just("Hello World");

helloWorldObservable.subscribe(new DefaultObserver<String>() {

    @Override
    public void onNext(String s) {
    System.out.println(s);
    // do anything
    }

    @Override
    public void onError(Throwable e) {
    // do anything
    }

    @Override
    public void onComplete() {
    // do anything
    }
});

// Yukarıdaki örnek, eski Java sürümlerine uyumlu. yeni anonim sınıf oluşturarak subscribe'a parametre geçiyoruz. Oysa aşağıda ise aynı örnek lambda ile yapılmış.

helloWorldObservable.subscribe(
  s -> System.out.println(s),
  throwable -> throwable.printStackTrace(),
  () -> System.out.println("everything completed!"));
```

## 📌 backpressure

`BackpressureStrategy` `enum`'ında belirttiğimiz config'e göre `stream` çalışmaktadır. config'i set etmek için:

```java
TestSubscriber<Integer> testSubscriber = observable.toFlowable(BackpressureStrategy.BUFFER)
```

`BackpressureStrategy` şu zaman devreye girer: `subscriber` ve `consumer` arasında bir `buffer(array)` var. Bu `buffer`, 50 boyuta ulaşırsa, `consumer`, `producer`'a göre yavaş kalıyor demektir (bu durum resmi dökümanlarda `downstream can't keep up` olarak yazılmış). `CPU` veya farklı bir bilgiye bakılarak karar verilmiyor.

`BackpressureStrategy`'lerinin bazıları:

- `BackpressureStrategy.DROP`

`buffer` aşıldığında, yeni gelen data'lar `buffer`'a da alınmadan direk olarak bellekten silinir.

```java
observable.toFlowable(BackpressureStrategy.DROP);
// veya aşağıdaki aynı şeyi yapmaktadır:
observable.toFlowable(BackpressureStrategy.MISSING).onBackpressureDrop();
```

- `BackpressureStrategy.BUFFER`

Bütün okunan data'lar direk bir `buffer`'da saklanır. `buffer` sonsuz büyüklüktedir. bu şekilde hiç data kaybı meydana gelmez.

```java
observable.toFlowable(BackpressureStrategy.BUFFER);
// veya aşağıdaki aynı şeyi yapmaktadır:
observable.toFlowable(BackpressureStrategy.MISSING).onBackpressureBuffer();
```

- `BackpressureStrategy.LATEST`

`buffer` sadece 1 eleman vardır. yani 1 eleman o anda işlenir. başka data gelirse o data'lar ignore edilir.

```java
observable.toFlowable(BackpressureStrategy.LATEST);
// veya aşağıdaki aynı şeyi yapmaktadır
observable.toFlowable(BackpressureStrategy.MISSING).onBackpressureLatest();
```

- `BackpressureStrategy.ERROR`

`buffer` dolduğu zaman `MissingBackpressureException` fırlatır. Bu error `subscriber` tarafından da yakalanabilen bir error'dur.

- `BackpressureStrategy MISSING`

`buffer` mekanizması hiç yoktur. gelen data ile direk olarak `subscriber`'ın metodu çağrılır.
