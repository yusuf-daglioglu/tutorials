# TOGAF ADM

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 ADM (⟷ Architecture Development Method)

Her faz input olarak döküman (veya dökümanlar) alarak başlatılır. Her faz yine birden fazla döküman ile sonlanabilir. Bu her dökümanın `TOGAF` literatüründe özel bir ismi var ve hepsinin örnke dosyaları `TOGAF` tarafından internette paylaşılmaktadır.

Aşağıdaki sadece bazı input ve output'lar yazılmıştır.

Aşağıdaki fazlar mimari ekibe bir öneri geldiğinde `A`'dan `F`'ye doğru gider ve sonlanır. Daha sonra `PMO` ve 1 `lead architect` tüm implementasonu sürekli olarak takip eder, bu sebeple `G` fazı implementasyon süresi boyunca açık kalır.

## 📌 Preliminary Phase

### 📌📌 Yapılan

- Bütün projeler için geçerli bir fazdır. Her proje için bu faz koşulmaz. Şirkette 1 kere bu faz koşulur sadece. Sonra, güncelleme gerekirse bu fazda, tekrar koşulabilir. Bu faz dışındaki diğer tüm fazlar, sadece proje spesifik bilgiler ve süreçeri barındır (yani proje bazlıdır).

- Bu fazda `ADM`, şirkete (veya sadece ilgili birimlerine) uyarlanır.

- tüm projelerde uygulanacak ortak mimari ilkeleri oluşturulur.

- tüm projelerde kullanılabilecek ortak mimari veritabanı kurulur. bu veritabanında süreçlerin nasıl işleyeceği, hangi dökümanlarla her fazın çıktısı ve girdisi olacağı gibi bilgiler olmaldır.

### 📌📌 Output

- `Architecture Principles`: Mimarinin uyacağı temel kurallar.

- `Architecture Repository`: Mimari tüm belgelerin saklandığı yazılımsal veritabanıdır.

## 📌 Phase A – Architecture Vision

### 📌📌 Yapılan

Sadece ilgili proje için:

- Kapsam belirlenir.

- Paydaşlar belirlenir.

- Vizyon hazırlanır.

- high-level mevcut ve hedef teknoloji altyapı mimarisi modellenir.

- `Architecture governance board`'dan onay alınır.

### 📌📌 Input

- `Request for Architecture Work (⟷ RFAW)`: Başlatılan mimari çalışma talebi.

## 📌 Paralel koşulan fazlar

### 📌📌 Phase B – Business Architecture

#### 📌📌📌 Yapılan

- Mevcut ve hedef iş mimarisi modellenir.

- `Gap analizi` yapılır.

### 📌📌 Phase C – Information Systems Architectures

#### 📌📌📌 Yapılan

- Mevcut ve hedef veri mimarisi ve uygulama mimarisi modellenir.

- `Gap analizi` yapılır.

### 📌📌 Phase D – Technology Architecture

#### 📌📌📌 Yapılan

- Mevcut ve hedef teknoloji altyapı mimarisi modellenir.

- `Gap analizi` yapılır.

## 📌 Phase E – Opportunities & Solutions

### 📌📌 Yapılan

- burada mimarisel bir detay çıkarılmıyor. önceki fazlarda mimari hazırlandı.

- halihazırda yürüyen proje ekipleri ile toplantı yapılır. zaten benzer bir iş yapılıyorsa, burada ona göre birleşmeler önerilebilir.

- şirket içinde mi yazılım yazılcak, outsource firmaya mı verilecek bu görev, yoksa işi gören bir yazılım mı satın alınacak kararı verilir.

## 📌 Phase F – Migration Planning

### 📌📌 Yapılan

- `Phase E` ile arasında çok keskin çizgiler yok. internetteki yorumlara bakılırsa, bu 2 faz aynı süreç içinde ypılabiliyor.

- implementasyon takımlarından her task için takvim istenir. farklı çözüm alternatiflerimiz varsa ayrı ayrı proje takvim planı istenir.

- implementasyon taskların önceliklendirme yapılır.

- Geçiş sırası belirlenir.

- Yol haritası tamamlanır.

- `Architecture governance board`'dan Onay alınır.

## 📌 Phase G – Implementation Governance

### 📌📌 Yapılan

- Uygulama izlenir.

- Mimariye uygunluk denetlenir.

- Sapmalar kaydedilir.

## 📌 Phase H – Architecture Change Management

### 📌📌 Yapılan

- Değişiklik talepleri değerlendirilir.

- Yeni `ADM` döngüsü gerekip gerekmediği belirlenir.

- Mimari güncellenir.

## Requirements Management

bu özel başlatılan bir faz (süreç) değil. fazlar arası geçiş olabileceğini belirten sanal bir terim.

### 📌📌 Yapılan

- Gereksinimler toplanır.

- Takip edilir.

- Güncellenir.

- Fazlara dağıtılır.