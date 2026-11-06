# STATISTICS & BENCHMARKS & SPEED

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Real operation performance

a benchmark from year 2010.

- L1 cache reference 0.5 ns
- L2 cache reference 7 ns
- Mutex lock/unlock 100 ns
- Main memory reference 100 ns
- Compress 1K bytes with Zippy 10,000 ns
- Send 2K bytes over 1 Gbps network 20,000 ns
- Read 1 MB sequentially from memory 250,000 ns
- Round trip within same data center 500,000 ns
- Disk seek 10,000,000 ns
- Read 1 MB sequentially from network 10,000,000 ns
- Read 1 MB sequentially from disk 30,000,000 ns
- Send packet CA->Netherlands->CA 150,000,000 ns

Writes are 40 times more expensive than reads.

1 ms = 10^-3 seconds = 1,000 μs = 1,000,000 ns

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Availability numbers

real uptime numbers

| availability | Downtime per day (average) | downtime per year(average) |
|--------------|----------------------------|----------------------------|
| 99%          | 15 minutes                 | 4 days                     |
| 99.9%        | 2 minutes                  | 9 hours                    |
| 99.99%       | 9 seconds                  | 50 minutes                 |
| 99.999%      | 900 millisecond            | 5 minutes                  |
| 99.9999%     | 90 millisecond             | 30 seconds                 |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 GSM SPEEDS

| Generation | Icon | Technology         | Maximum Download Speed | Typical Download Speed |
|------------|------|--------------------|------------------------|------------------------|
| 0G         |      |                    | internet yok           | internet yok           |
| 1G         |      |                    | internet yok           | internet yok           |
| 2G         | G    | GPRS               | 0.1Mbit/s              | <0.1Mbit/s             |
| 2G         | E    | EDGE               | 0.3Mbit/s              | 0.1Mbit/s              |
| 3G         | 3G   | 3G (Basic)         | 0.3Mbit/s              | 0.1Mbit/s              |
| 3G         | H    | HSPA               | 7.2Mbit/s              | 1.5Mbit/s              |
| 3G         | H+   | HSPA+              | 21Mbit/s               | 4Mbit/s                |
| 3G         | H+   | DC-HSPA+           | 42Mbit/s               | 8Mbit/s                |
| 4G         | 4G   | LTE Category 4     | 150Mbit/s              | 15Mbit/s               |
| 4G         | 4G+  | LTE-Advanced Cat6  | 300Mbit/s              | 30Mbit/s               |
| 4G         | 4G+  | LTE-Advanced Cat9  | 450Mbit/s              | 45Mbit/s               |
| 4G         | 4G+  | LTE-Advanced Cat12 | 600Mbit/s              | 60Mbit/s               |
| 4G         | 4G+  | LTE-Advanced Cat16 | 979Mbit/s              | 90Mbit/s               |
| 5G         | 5G   | 5G                 | 1,000-10,000Mbit/s     | 150-200Mbit/s          |

- 2.5G ve üzerinde hız çok yüksek olduğundan kota tüketimi hızlı olmaktadır. bu sebeple güncel mobil uygulamalar bundan korunacak şekilde tasarlanmaktadır. örneğin; video player'lar hiçbir koşulda videonun tamamını indirmiyor. sadece kullanıcının bulunduğunu noktanın ve birkaç saniye ilerisi yükleniyor.

- 4.5G'nin hızları da kullanılan cihazlara göre değişmektedir. Cihazlar kategori ("CAT") olarak ayrıştırılmaktadır:

| Kategori Seviyesi    | Max İndirme (Download) Hızı | Max Yükleme (Upload) Hızı |
|----------------------|-----------------------------|---------------------------|
| Kategori 3 (Cat 3)   | 100Mbit/Sn                  | 51Mbit/Sn                 |
| Kategori 4 (Cat 4)   | 150Mbit/Sn                  | 51Mbit/Sn                 |
| Kategori 6 (Cat 6)   | 300Mbit/Sn                  | 51Mbit/Sn                 |
| Kategori 9 (Cat 9)   | 450Mbit/Sn                  | 51Mbit/Sn                 |
| Kategori 10 (Cat 10) | 450Mbit/Sn                  | 102Mbit/Sn                |
| Kategori 12 (Cat 12) | 600Mbit/Sn                  | 102Mbit/Sn                |
| Kategori 15 (Cat 15) | 3.9Gbit/sn                  | 1.5Gbit/sn                |

## 📌 Latency

| Generation | Typical Latency     |
|------------|---------------------|
| 2G         | 500ms (0.5 seconds) |
| 3G         | 100ms (0.1 seconds) |
| 4G         | 50ms (0.05 seconds) |
| 5G         | 1ms (0.001 seconds) |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 netflix

source: <https://help.netflix.com/en/node/306>

| Call type                  | Recommended megabits per second |
|----------------------------|---------------------------------|
| High definition (HD)       | 720p: 3 Mbps                    |
| Full high definition (FHD) | 1080p: 5 Mbps                   |
| 4K/Ultra HD (UHD)          | 4K: 15 Mbps                     |

## 📌 Skype

Source: <https://support.skype.com/en/faq/FA1417/how-much-bandwidth-does-skype-need>

| Call type                      | Minimum download/ upload speed | Recommended download/ upload speed |
|--------------------------------|--------------------------------|------------------------------------|
| Calling                        | 30kbps / 30kbps                | 100kbps / 100kbps                  |
| Video calling / Screen sharing | 128kbps / 128kbps              | 300kbps / 300kbps                  |
| Video calling (high-quality)   | 400kbps / 400kbps              | 500kbps / 500kbps                  |
| Video calling (HD)             | 1.2Mbps / 1.2Mbps              | 1.5Mbps / 1.5Mbps                  |
| Group video (3 people)         | 512kbps / 128kbps              | 2Mbps / 512kbps                    |
| Group video (5 people)         | 2Mbps / 128kbps                | 4Mbps / 512kbps                    |
| Group video (7+ people)        | 4Mbps / 128kbps                | 8Mbps / 512kbps                    |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Speed comparison of technologies

| name         | max distance (about)           | Bandwidth                   |
|--------------|--------------------------------|-----------------------------|
| `NFC`          | 5 cm                           | 2 kbps                      |
| Bluetooth 5  | 200 metre (max ve açık alanda) | 2 Mbps (max ve açık alanda) |

## 📌 Wi-Fi

Source: <https://www.intel.com/content/www/us/en/support/articles/000005725/wireless/legacy-intel-wireless-products.html>

| Protocol       | Marketing name | Frequency    | Channel Width      | MIMO                  | Maximum data rate (theoretical) | Year Released | Range (Indoor) | Range (Outdoor) |
|----------------|----------------|--------------|--------------------|-----------------------|---------------------------------|---------------|----------------|-----------------|
| 802.11ax       | `Wi-Fi` 6        | 2.4 or 5GHz  | 20, 40, 80, 160MHz | Multi User (MU-MIMO)  | 2.4 Gbps1                       | 2019          | 30m            | 120m            |
| 802.11ac wave2 | `Wi-Fi` 5        | 5 GHz        | 20, 40, 80, 160MHz | Multi User (MU-MIMO)  | 1.73 Gbps2                      |               |                |                 |
| 802.11ac wave1 | `Wi-Fi` 5        | 5 GHz        | 20, 40, 80MHz      | Single User (SU-MIMO) | 866.7 Mbps2                     | 2013          | 35m            |                 |
| 802.11n        | `Wi-Fi` 4        | 2.4 or 5 GHz | 20, 40MHz          | Single User (SU-MIMO) | 450 Mbps3                       | 2009          | 70m            | 250m            |
| 802.11g        | `Wi-Fi` 3        | 2.4 GHz      | 20 MHz             | N/A                   | 54 Mbps                         | 2003          | 38m            | 140m            |
| 802.11a        | `Wi-Fi` 2        | 5 GHz        | 20 MHz             | N/A                   | 54 Mbps                         | 1999          | 35m            | 120m            |
| 802.11b        | `Wi-Fi` 1        | 2.4 GHz      | 20 MHz             | N/A                   | 11 Mbps                         | 1999          | 35m            | 120m            |
| Legacy 802.11  | `Wi-Fi` 0        | 2.4 GHz      | 20 MHz             | N/A                   | 2 Mbps                          | 1997          | 20m            | 100m            |

Notlar:

- Farklı çalışma moduna, her sürüm farklı hızlarda çalışmaktadır. Örneğin; "802.11ax", "1x1 20 MHz" modunda çalışırken maximum "143 Mbps" hıza çıkabilirken, "2x2 160 MHz" modunda çalışırsa, 2.4 Gbps'ye çıkıyor. Bu detayların tümü için kaynaktaki web sayfasına bakılabilir.
- `Wi-Fi` 0 - 4 arası olan tüm sürümler, bu sürüm numaraları ile pazarlanmadı. 3'ten sonra yazılan makalelerde, eski sürümlere bu şekilde referans edildi.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 USB file transfer Speed

source:

- <https://www.sony.com/electronics/support/articles/00024571>
- <https://support.hp.com/us-en/document/c01208644>

| VERSION                    | MAX SPEED |
|----------------------------|-----------|
| 1.0                        | 1.5 Mbps  |
| 1.1                        | 12 Mbps   |
| 2.0 (USB)                  | 12 Mbps   |
| 2.0 (Hi-Speed USB)         | 480 Mbps  |
| 3.1 Gen 1 (⟷ SuperSpeed)   | 5 Gbps    |
| 3.1 Gen 2 (⟷ SuperSpeed+)  | 10 Gbps   |
| 3.2 Gen 1x2                | 10 Gbps   |
| 3.2 Gen 2x2                | 20 Gbps   |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
