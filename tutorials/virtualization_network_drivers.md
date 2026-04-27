# VIRTUALIZATION NETWORK DRIVERS

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 VirtualBox network drivers

`network interface` konusu başka başlıkta anlatılıyor.

1 `guest`'e 1'den fazla network bağdaştırıcısı eklenebilir.

`VirtualBox` network bağdaştırıcısı opsiyonları:

### 📌📌 Not attached

network cihazı takılıdır. ama kablosu takılı değildir.

### 📌📌 NAT

`subnet` oluşturmaz.

`quest` dış dünyaya çıkmak isterse, `VM` driver tarafından `NAT` işlemine tabi tutulur.

`guest`'in aldığı `IP`, private bir `IP`'dir.

`Host` dışından bu `VM`'e erişmek için `port forwarding` yapmak şarttır.

### 📌📌 NAT Network (⟷ NAT Service)

`subnet` kurar. bu ağa bağlı her `quest` birbirini görebilir. `quest` bu `subnet`'ten `IP` alır.

`quest` dış dünyaya çıkmak isterse, `VM` driver tarafından `NAT` işlemine tabi tutulur.

### 📌📌 bridge networking

`host`'un bağlı olduğu `DHCP`'den yeni bir gerçek `IP` alır.

Bunu yapabilmek için random `MAC` adresi üretiyor. Bazı host `OS`'ler veya donanımlar bunu desteklemeyebilir.

### 📌📌 Host-only networking

sanal bir ağ kartı yaratır. bu sanal ağ kartı içerisinde istediğimiz kadar `VM`'i birbiri ile iletişime geçirebilir. dış dünya ile iletişimde olmazlar.

opsiyonel olarak, bu `subnet`'te `DHCP` sunucuda yaratılır. (`VirtualBox` ayarlarından).

### 📌📌 Internal networking

`Host-only networking` yöntemi ile apaynıdır. sadece sanal bir kart yaratılmaz. pnun yerine fiziksel network kartı kullanılır. fakat internete çıkmazlar. yazılım tarafında 1 kart fakat 2 network interface'i oluşur.

sanal bir kart yaratılmaması; var olan bir fiziksel kartın kullanılmasına sebep olur. Böylece `Wireshark` gibi bir uygulama ile `Internal networking`'e bağlı `VM`'lerin network trafiğini izleyebiliriz.

### 📌📌 Cloud networking

`guest`'in uzaktaki bir subnet'teymiş gibi davranabilmesi sağlanır.

### 📌📌 Generic networking

`Virtualbox` driver eklentilerinin çalışmasını sağlar.

## 📌 ("host-only networking" and "internal networking") vs ("NAT" and "NAT Network")

`host-only networking` and `internal networking` private network'ten dışarıya `VirtualBox`'un engine'inde olan sanal bir `Switch` cihazı simülasyonu ile çıkarken, `NAT` and `NAT Network` yine `VirtualBox`'un yarattığı sanal bir `router` ile dışarıya çıkar.

## 📌 Docker network drivers

### 📌📌 none

network cihazı hiç yok gibidir container içinde.

### 📌📌 host

container kendi IP'sini almaz. host ile aynı network'ü kulanır.

### 📌📌 bridge

default.

sanal interface ve `subnet` yaratılır.

container'a bu `subnet`'ten bir `IP` atanır.

her `subnet`'in bir adı var. bunu parametre olarak veririz. default `subnet` adı: `bridge`'dir.

sadece aynı `subnet`'te olan container'lar birbirleri ile haberleşebilir.

### 📌📌 overlay

kelime anlamı: kaplama (örtü).

Bunun için `docker` engine'in diğer uzak makinadaki `docker` engine ile `Docker Swarm` gibi bir araçla bağlanmış olması şarttır.

Uzak network'teki container'lar birbirleri ile aynı `subnet`'teymiş gibi davranılar.

### 📌📌 macvlan

`host`'ın network'ünden kendisine yeni `IP` alır. Yani `VM`'deki `bridge` gibi çalışır.

`host`'un bulunduğu network'ün bilgilerini vermek zorundayız.

```sh
docker network create -d macvlan \
      --subnet="172.16.86.0/24" \
      --gateway="172.16.86.1" \
      -o parent="eth0" \
      "pub_net"
```

parent olarak ne vereceğimiz çalışma mode'u belirleyecektir:

#### 📌📌📌 bridge mode

direk olarak gerçek fiziksel kartımız üzerinden çalışır. bu sebeple gerçek fiziksel arayüzümüzü parent'a yazmak zorundayız.

#### 📌📌📌 802.1q trunk bridge mode

sanal yaratılan bir interface üzerinden fiziksel karta bağlanır. böylece host'un arayüzünde olan routing veya filter'lara takılmamış oluruz.

parent'a `eth0.50` gibi `Docker` engine'in belirttiği şekilde isim vermeliyiz.

### 📌📌 IPvlan

container'lar `host`'un network'üne bağlanır. ama:

- `host` ile aynı `MAC` adresi kullanır.
- sanal bir `subnet` oluşturulur.
- her container bu `subnet`'te `IP` alır (ama host `DHCP` vermez bunu. `Docker` engine verir.)

Artık dış dünyaya istek atarsak ve alırsak, `OS` kime yönlendireceğini bilir.

### 📌📌 Docker plugin

3üncü parti network driver'lar.
