# JAVA THREAD

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Java thread

thread kelime anlamı: 1. iplik, 2. konu

## 📌 thread oluşturma

iki şekilde thread yaratılabilir:

1- __Runnable arayüzü implemente edilerek__

2- __thread sınıfı extend edilerek__

örnek:

```java
class ImplementsRunnable implements Runnable {

   // runnable sadece run metodunu içerir.

   private int counter = 0;

   public void run() {

         counter++;

         System.out.println("ImplementsRunnable : Counter : " + counter);
   }
 }

 class ExtendsThread extends Thread {

   private int counter = 0;

   public void run() {

         counter++;

         System.out.println("ExtendsThread : Counter : " + counter);
   }
 }
```

hangisini kullanmak gerekli:

- runnable kullanılırsa; ImplementsRunnable sınıfımız isterse başka sınıflardan extend edebilir. fakat ExtendsThread zaten extend hakkını kullandığı için tekrar extend edemez (Java kuralı)

- runnable'den implemente etmiş sınıfta başlatılan her thread "private int counter" değerini ortak kullanır.

ImplementsRunnable direk çağrılamaz. Thread içine atılmalıdır.

```java
Thread t1 = new Thread(new ImplementsRunnable());
t1.start();

Thread t2 = new Thread(new ImplementsRunnable());
t2.start();
```

yukarıda t1 ve t2 aynı "counter" değeri kullanır.

## 📌 Callable

Runnable ile tamamen aynı mantıkta çalışır. Tek farkı: Callable değer dönebilir ve hata fırlatabilirdir. Oysa Runnable'de bunları yapamayız.

## 📌 void run vs void start

run metodu çağrıldığında yeni bir paralel thread yaratılmaz. fakat start metodu run metodunu thread yaratarak çağırır.

start metodu aynı sınıf için 2 kere çağrılırsa IllegalThreadStateException fırlatır.

## 📌 void sleep(long)

Millisecond cinsinden parametre alır ve ilgili thread'i o kadar bekletir.

## 📌 void join()

örnek:

```java
t1.join();
a++;
```

t1 thread'i sonlanmadan a++ çalışmayacaktır.

## 📌 void join(long)

örnek:

```java
t1.join(3000);
a++;
```

t1 ölürse a++ çalışır, yada 3 saniye içinde ölmezse, a++ çalışır.

## 📌 static Thread currentThread()

bu metot o anda çalışmakta olan thread'in instance'ını dönüyor. static bir metot olmasına rağmen, o anda bu metodu çağıran thread'in bilgisini alabiliyor.

## 📌 getName setName

thread sınıfına isteğe bağlı isim atanabiliyor.

## 📌 setDaemon getDaemon

boolean bir parametre alır. normalde Java tüm thread'ler sonlanmadan `JVM`'i kapatmaz. daemon thread'lar bu kurala dahil değildir. daemon thread'ler sonlanmadan `JVM` kapanabilir.

## 📌 Executors

Birçok thread'i yönetebilmemizi sağlayan bir yapıdır.

```java
 // önceden 5 thread açık tutuluyor. hızlı başlangıç için
ExecutorService executor = Executors.newFixedThreadPool(5);

for (int i = 0; i < 10; i++) {

   MyRunnableImpl runnable = new MyRunnableImpl(i);
   executor.execute(runnable);
}

executor.shutdown();

while (!executor.isTerminated()) {

     //wait
}

System.out.println("Finished all threads");
```

## 📌 ThreadGroup

```java
ThreadGroup tg1 = new ThreadGroup("Group A");
Thread t1 = new Thread(tg1,new MyRunnable(),"one");
Thread t2 = new Thread(tg1,new MyRunnable(),"two");
```

artık tümüne aynı işlemi uygulamayabiliriz:

```java
Thread.currentThread().getThreadGroup().interrupt();
```

| Özellik            | ExecutorService                        | ThreadGroup                      |
|--------------------|----------------------------------------|----------------------------------|
| Amaç               | Görevleri yönetmek ve çalıştırmak      | İş parçacıklarını gruplamak      |
| Görev Yönetimi     | Var (`Runnable`, `Callable`, `Future`) | Yok                              |
| Thread Pooling     | Var                                    | Yok                              |
| Kontrol            | Gelişmiş (başlatma, durdurma, takip)   | Sınırlı (interrupt, activeCount) |
| Kapanış (Shutdown) | Var (`shutdown`, `shutdownNow`)        | Yok                              |
| Modern Kullanım    | Evet                                   | Hayır (eskimiş)                  |
| Pake               | `java.util.concurrent`                 | `java.lang`                      |


## 📌 synchronized

bu Java keyword'ü metodun imzasına koyulduğunda o metot aynı anda en fazla 1 thread tarafından çalıştırılabilir olur. diğer thread'ler beklemeye alınır:

```java
public synchronized void anyMethod() {
    // any code here
}
```

İstersek sadece belli bir kod bloğunu da kilitleyebiliriz:

```java
public void anyMethod() {

    // some code here

    synchronized (monitorObj) {
        // some code here
    }

    // some code here
}
```

yukarıdaki synchronized bir metot değil. özel bir syntax. bu örnekte; monitorObj, bir variable'dır. monitorObj, bu kod bloğunun o andaki thread için bekletilip bekletilmeyeceğine karar veriyor. O anda monitorObj instance'ı aynı olan her thread bu metoda girmesi engelleniyor. monitorObj yerine istediğimiz şeyi kullanabiliriz. Örnek:

```java
synchronized (this) {
// Bu satırda, this'leri eşit olan her thread beklemeye alınacak ve sadece 1 thread bu kod bloğuna girebilecek.
```

synchronized'ın kullanımının birçok detayı var: Örneğin:

```java
synchronized (MyClass.class) {
// Bu özel bir durumdur.
```

Ek olarak synchronized, static sınıfların static metotlarında farklı davranış biçimleri sergiliyor.

## 📌 volatile

kelime anlamı: uçucu.

Kullanımı:

```java
public class SharedObject {
    public volatile int counter = 0;
}
```

JVM bazı değişkenleri hızlı erişmek için CPU-cache'te kopyasını tutar. 1 thread, bu değişkeni CPU-cache'te değiştirirse, `JVM` anında RAM'de de update etmeyebilir. Her thread 1 CPU'da çalışır. Dolayısı ile diğer thread'ler bu değişkeni okumaya kalkarsa, RAM'den okuyacakları için, güncel olmayan veriyi okuma ihtimalleri olur. Sadece güncelleyen thread, kesinlikle güncel veriye erişir.

Bu durumu engellemek için o değişkeni __volatile__ yapmak şarttır. Fakat bu performans olarak olumsuzdur. Çünkü artık `JVM` her variable güncellendiğinde, CPU-cache ile RAM arasında mutlaka sync yapıyor.

Ama unutmamak lazım ki "volatile" atomic'lik sağlamaz. Sadece güncel data'nın okunabildiğini garanti eder.

Eğer bir metod aracılığı ile o değişkeni okuyorsak, metodu __synchronized__ yaparsak, __volatile__'e ihtiyacımız kalmıyor. çünkü __synchronized__ metodu çağrıldığı anda, CPU-cache ile RAM mutlaka sync yapılıyor.

## happens-before relation

Bu terim `JVM`'e özgü değildir. Genel programlama dünyasında kullanılır.

```java
int commonValue = 0;

// Thread 1:
commonValue = 10;

// Thread 2:
System.out.println(commonValue);
```

Yukarıda, eğer Thread-1, Thread-2 ekrana basmadan önce işlemi yapmasını gerekiyorsa, bu 2 thread arasında "__happens-before ilişkisi var__" denir.

## 📌 Collections.synchronizedMap() vs ConcurrentHashMap

`Collections.synchronizedMap()` ve `ConcurrentHashMap` iki farklı `thread-safe` sunan alternatiflerdir. Çünkü standart `Java` `HashMap` `thread-safe` değildir.

Bazı farklar:

- `ConcurrentHashMap` kullandığımızda `Iterator` ile `for loop` içerisinde `map.put()` metodu `ConcurrentModificationException` fırlatmaz. Oysa `synchronizedMap` bu hatayı fırlatır.

- `ConcurrentHashMap` `null` key veya `null` value kabul etmez. Fakat `synchronizedMap` implementasyonuna göre kabul edebilir.

- `synchronizedMap`'te tüm metotlar `synchronized`'dır. Yani tüm işlemler blocking'dir. Bu sebeple yavaştır. Fakat ConcurrentHashMap'ta okuma işlemleri non-blocking'dir. ConcurrentHashMap'te aynı anda aynı bucket'a kayıt işlemleri blocking iken, farklı bucket'lara kayıt işlemi non-blocking'dir.

## 📌 Is "Iterator" thread-safe

Collections sınıfı synchronizedList, synchronizedMap gibi birçok metot sunar. Bunlardan dahi dönen çoğu  sınıfı thread-safe değildir (bazıları hariç). Bu sebeple Iterator'ları manuel synchronized yapmak gerekmektedir:

```java
Collection c = Collections.synchronizedCollection(myCollection);

synchronized (c) {
    Iterator i = c.iterator(); // Must be in the synchronized block
    while (i.hasNext())
        foo(i.next());
}
```

Fakat bazı Iterator'lar yapısı gereği `thread-safe` olabilir. Buna en bilinen örnek CopyOnWriteArrayList'tir. CopyOnWriteArrayList Iterator'a data'nın klonunu döndürüyor. Dolayısı ile asıl CopyOnWriteArrayList listesinde bir değişiklik olduğunda, Iterator'da sorunsuzca döngüye devam edebiliyoruz. Fakat CopyOnWriteArrayList'ın Iterator'daki remove işlemi direk olarak UnsupportedOperationException fırlatmaktadır.

## 📌 thread-safe

bu terim tüm programlama dilleri için geçerlidir. bir metot aynı anda istenildiği kadar thread tarafından çağrılıyor ve sorunsuz çalışıyor ise o metot `thread-safe`'tir. bir sınıf `thread-safe` ise o sınıfın içindeki tüm public metotlar `thread-safe` anlamına gelir.

örnek:

StringBuilder `thread-safe` değildir. Bu durumda;

```java
private StringBuilder sb = new StringBuilder("abc");

public void addProperty(String name, String value) {

    sb.append(name).append('=').append(value);
}
```

Aşağıdaki gibi çağırırsak, kodun yanlış yapma olasılığı vardır:

```text
Thread1: addProperty("a", "b");
Thread2: addProperty("c", "d");
Thread3: addProperty("e", "f");
```

## 📌 Threadlocal

Java'da bir sınıftır. aşağıdaki örnekte basit şekilde kullanımı mevcut. ThreadLocal her thread için yeni bir nesne yaratmaktadır. nesneyi yaratmak için initialValue() metodunu çağırır.

```java
public class Foo
{
    private final ThreadLocal<SimpleDateFormat> formatter = new ThreadLocal<SimpleDateFormat>(){

        @Override
        protected SimpleDateFormat initialValue()
        {
            return new SimpleDateFormat("yyyyMMdd HHmm");
        }
    };

    public String formatIt(Date date)
    {
        return formatter.get().format(date);
    }
}
```

Real world examples:

- `RequestContextHolder` (request ve response'u komple içinde barındırır)
- `LocaleContextHolder` (Dil gibi bilgileri thread için tutar)
- `DateTimeContextHolder` (ülke gibi bilgileri thread için tutar)
- `SecurityContextHolder`: her thread kendi user ve role bilgilerini içeren instance'a (DTO'ya) erişir. bu instance tüm thread için aynıdır. `SecurityContextHolder`'a, spring-security-filter, user ve role ekler. sonra ilgili thread hep buna erişir. her eriştiğinde yeni nesne oluşmaz.
- `MDC` (log sistemlerinde kullanılır)

## 📌 wait notify

`Java`'nın obje sınıfına ait metotlarıdır. `wait` ile o thread artık bekleme durumuna girer. taki başka biri o thread üzerinde `notify` metodunu çağırana kadar.
