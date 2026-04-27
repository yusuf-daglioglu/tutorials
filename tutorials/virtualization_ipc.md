# VIRTUALIZATION IPC

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

### 📌📌 IPC çeşitleri

#### 📌📌📌 Message passing

Mesaj karşıya sync olarak atılır ve direk atılır.

Bu teknolojiler arkaplanda bir protocol kullanmak zorunda. Bazıları birden fazla protocol'ü destekliyor olabilir.

Örnekler:
- `Java` `RMI`
- `CORBA`
- `DCOM`

#### 📌📌📌 Message queue (kernel level services)

They are `OS` kernel level services.

Supported by many `POSIX`-like. Also may supported via `Cygwin` or `WSL`.

Examples:

- `POSIX.1-2001`
- `SYS V` (Used by `Unix`)

#### 📌📌📌 DBus (⟷ D-Bus) (user level services)

Tüm `OS`'ler tarafından support edilebiliyor.

`DBus` üst seviyeli kalıyor. Kernel değilde, user yetkileriyle çalışabiliyor. `DBus` daemon'a istek atan client'lar `TCP` socket veya `Unix domain socket` kullanabilir.

sync veya `publish-subscribe` mantığında işlem yapabilmemizi sağlar.

`DBus` daemon'a istek atabilen birçok client var:
- `GDBus` (`GNOME`)
- `QtDBus` (`Qt` or `KDE`)
- `dbus-java`
- `libdbus` (reference implementation)

her yazılım, `DBus` aracılığı ile dışarıya birçok servis açabilir. her servis ID'si tüm `OS`'ta tekil olmalıdır. bu servislerin her birine `bus` denir. örnek bir `bus`:

```text
/com/appname/account
```

yukarıda `account` servisi var. bu servis içinde birçok metot olabilir.

#### 📌📌📌 shared memory
  
Share part of the `RAM` directly. Fastest.

example:

- `POSIX shared memory`
  
  `POSIX` standartında olan özel bir `API` ismidir. Bu `API`'ye uyan herhangi bir kütüphane aracılığı ile 2 process `OS`'a ortak `RAM` kullanmak için haber verebilir.

  `nmap`, `Unix`'in sunduğu bir `system call`. `POSIX shared memory` yapabilmek için `nmap` `system call`'undan yararlanmak zorundayız.

#### 📌📌📌 Memory-Mapped File

`shared memory` ile aynıdır. ama dosya aynı zamanda disk'te de tutulur. böylece kalıcılık sağlanır.

#### 📌📌📌 network socket

fiziksel veya sanal network cihazı yok ise bile, sanal olan `loopback` gibi `network interface` üzerinden yine haberleşilebilir.

#### 📌📌📌 Named Pipe

`Unix domain socket`'e alternatiftir ama daha basit bir yapıdır.

#### 📌📌📌 Unix domain socket

network socket'i ile alakası yoktur.

file tekniği ile aynıdır. sadece dosyanın tipi socket'tir.

gerçek dosya gibi veriyi `HDD`'de tutmazlar, `RAM`'de tutar. Bu sebeple okunamayan bilgiler olursa, `OS` restartında bu bilgiler kaybolur.

Çift yönlü haberleşme sağlar.

`Java` example:

```java
File socketFile = new File("/tmp/mysocket.sock");
AFUNIXServerSocket server = AFUNIXServerSocket.bindOn(socketFile)
server.accept().getOutputStream().write("hello".getBytes());
```

Arka planda `TCP/IP` stack'i kullanılmaz.

network socket yerine tercih edilir. çünkü:

- network ağı olmayan gömülü sistemlerde, `TCP/IP` modülleri yüklü olmayabilir.

- herkes network portuna herkes istek yapabilir. bu güvensizdir. dışarıya kapatmak için ekstra config gerekebilir.

- `loopback` işlemleri yavaştır.

#### 📌📌📌 file

bir app bir dosyaya yazar diğeri okur.

#### 📌📌📌 process signal

bir process diğer process'e bilgi atamaz ama basit Signal'ler yollayabilirler. Signal yollama sürecinde karşı tarafın process ID'sini bilmemiz şarttır.

`PID namepaces`'e dayanır.

### 📌📌 container teknolojilerinde IPC için çözümler

Paylaşım için 2 çözüm var:

- `pod` (mümkünse tercih edilmeli)
- `system container`
