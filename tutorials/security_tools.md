# SECURITY TOOLS

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Identity Management

Aşağıdaki bilgiler `Gartner`'a göre yazılmıştır.

### 📌📌 IAM (⟷ Identity and Access Management ⟷ Kimlik ve Erişim Yönetimi)

Kim, hangi sisteme erişebilir?

bu yazılımda vault olmak zorunda değildir. ama buna rağmen, son kullanıcı; `web` `UI` üzerinden `RDP`, `SSH` başlatabilir.

- Workforce `IAM` (Çalışanlar, İç Kullanıcılar)
- `Customer IAM (⟷ CIAM)` (Müşteriler, Dış Kullanıcılar)

bazı ürünler:
- `keycloak`
- `IBM Security Access Manager (⟷ ISAM)`

### 📌📌 PAM (⟷ Privileged Access Management ⟷ Ayrıcalıklı Erişim Yönetimi)

zorunlu özellikler:

- vault for passwords
- Session Recording
- JIT (Just-In-Time) Access (1 defalık session izni verme)
- aşağıdkailerden en az birine hizmet etmek zorunda:
  - Makineler (Machine / Non-interactive)
    
    sadece bunu destekleyen yazılımlara `Secrets Manager` denir.
    
    bazı yazılımlar:
    - `HashiCorp` `Vault`
    - `conjur secrets manager enterprise`

  - İnsanlar (Human / Interactive)
  
    örnek yazılımlar:
    - ürün adı: `CyberArk PAM`. firmanın adı `CyberArk`. ürünün eski adı: `PAS (⟷ Privileged Access Security)`. Bulut hizmetinin ismi: `CyberArk Privilege Cloud`

### 📌📌 IGA (⟷ Identity Governance & Administration)

- Kullanıcı Yaşam Döngüsü (Lifecycle / Provisioning) (işten çıkarılan kişinin yetkilerinin 3üncü parti bulut ve on-prem sistemlerden silinmesi için aksiyon alabilir)
- Yetki Gözden Geçirme (belirli periyotlarda yetkileri yöneticilere raporlama)
- Görevler Ayrılığı (Segregation of Duties - SoD) Kontrolü (Bir kullanıcı hem "Fatura Oluşturan" hem de "Ödeme Onaylayan" rolünde olursa suistimal yapabilir. bunları engeller.)

bazı yazılımlar:
- `Okta Identity Governance (⟷ OIG)`
- `Microsoft Entra ID Governance`

### 📌📌 SIEM (⟷ Security Information & Event Management)

Farklı yazılımlardan:
- standart formattaki log'ları dosyalardan okur,
- `OTEL` veya `Syslog` gibi protocol'lerden bilgi toplar

bunların hepsini analiz eder, saldırıları tespit eder, alarm'lar üretir.

Bazı ürünler:
- `Microsoft Sentinel`
- `Splunk Enterprise Security`

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Hashicorp vault

## 📌 config file

`HCL` (`HashiCorp Configuration Language` kısaltmasıdır) formatındadır. `HCL`, `JSON` ve `YAML`'ye alternatif olarak geliştirilmiştir. örnek syntax:

```hcl
service {
    key = "value"
}

service2 {
    key2 = "value2"
}

// comment-1

## comment-2

/* multi-line
   comment */

root1 "child1" {
    prop1 = "value1"
}
// equivalent to YAML:
// root1.child1.prop1: value1
```

## 📌 initialization

`vault` sunucusu başladığında sadece 1 kere init etmek gerekli. init output'u olarak şunlar basılıyor:

- 5 adet unseal-key'lerı

  unseal-key'lerden en az 3 tanesi ile vault ancak unseal edilebiliyor. Unsealing süreci her sunucu restart'ında gerektiği için bu key'lere ihtiyacımız olacak.

- 1 adet root token

  bu token ile client her işlemi yapabiliyor.

## 📌 secret engine

vault'ta her şeyi gruplamak amaçlı her "secret" belli 1 adet "secret engine" altında olmalıdır. bu şekilde bir secret engine'i komple disable edebiliriz yada enable edebiliriz.

## 📌 authentication

client'ın vault'tan data çekmesi veya manipülasyon yapabilmesi için her uygulamada olduğu gibi önce login olmalıdır. "token" ile authentication'da buna bir örnektir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •