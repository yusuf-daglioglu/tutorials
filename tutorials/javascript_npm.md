# JAVASCRIPT NPM

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 npm package versions

```text
> version
```

Must match version exactly

```text
> version
```

Must be greater than version

```text
>=version
```

version'dan küçük sürümler

```text
~version
```

"Approximately equivalent to version". See: "semver". example: `~1.2.3`, will accept `1.2.5` but will miss `1.3.0`.

```text
^version
```

"Compatible with version". See "semver". example: `^1.2.3`, will accept `1.3.1` but will miss `2.0.0`.

```text
1.2.x
```

`1.2.0`, `1.2.1` olabilir. `1.3.0` olamaz.

```text
http://myDomain.com/repoName
```

will use the remote folder as dependency

```text
#
```

Matches any version

```text
""
```

(just an empty string) Same as `*`

```text
git://myDomain.com/repoName
```

Git Repository URLs as Dependencies

```text
- path/path/path
```

use local folder for dependency.

## 📌 npm update

npm sürümleri bazen `#` gibi dinamik sürümler kullanabiliyor. bu sebeple bunların update kontrolü için bu komut çalıştırılmaktadır.

## 📌 "main" inside package.json

the file which is exported when the library is importing anywhere.

example: 

```json
"main": "app.js"
```

## 📌 npm install

bağımlılıkları `/node_modules` dizininin altına indirir.

## 📌 npm install -g module1

`-g` parametresi module1'i global paket olarak sisteme ekliyor. yani artık sadece projenin bulunduğu dizinden değil tüm dizinlerden çağrılabilir durumda oluyor.

## 📌 npm list

çalıştığı dizinde node_modules altındaki modüllerin listesini ağaç şeklinde listeler. `-g` parametresi ile çalıştırılırsa bulunduğu dizine bakmaz ve global kurulan node paketlerinin listesini ağaç şeklinde gösterir.

## 📌 directory structure

- NODE_INSTALLATION_PATH/bin -> path'e eklenmiş olması gereklidir. burada global kullanılan node paketlerinin executable dosyalarının kısa yolları mevcut. "npm" node ile çalışan bir modül olduğundan burada (path'te) her zaman npm kısa yolu bulunur.

- NODE_INSTALLATION_PATH/lib/node_modules/module1 --> global kurulan npm paketleri buradadır. module'in kendi dependency'leri "NODE_INSTALLATION_PATH/lib/node_modules/module1/node_modules" altındadır. npm projesi de buranın içindedir.

- NODE_INSTALLATION_PATH/include/node --> node'un kendi sistem dosyaları buradadır. node'un executable dosyasının kısa yolu "bin" dizinindedir.

- NODE_INSTALLATION_PATH/share --> manual/help gibi dosyaları barındırır.

- $HOME/.npm --> internetten indirilen tüm repository yedekleri buradadır. Maven'ın ".m2" dosyası ile aynı mantıkta çalışmaktadır.

## 📌 npm link module1

bir uygulamamız ve bir de ondan bağımsız module1 isminde paket geliştiriliyor olalım. uygulamamız module1'e depend olsun. module1'de değişiklik olduğunda bu paketi repoya atıp, daha sonra uygulamamızdan npm install çalıştırmamız gerekecek. bunu sürekli yapmamak için link komutu kullanılabilir. module1 dizini içerisinde "npm link" komutunu çalıştırırız. daha sonra uygulamamızın root dizinine gidip "npm link module1" komutunu çalıştırırız. artık bizim proje module1 dizinini direk görüyor olur. her değişiklik anında görülmüş olur. npm link çok basit bir taktikte kısa yol atarak bu işi çözüyor.

npm link işleminin geri alınması için, modüle olan paketin dizine gidip "npm unlink" ve daha sonra uygulamamızın dizinine gidip "npm unlink module" komutunu çalıştırmamı gerekmektedir.

## 📌 scripts

komut listesi barındırıyor. isteğe bağlı komutlar ve default komutlar mevcut. örneğin:

```json
"scripts": {

  "start": "node node_modules/react-native/local-cli/cli.js start",

  "xxx": "node node_modules/react-native/local-cli/cli.js start"

 },
```

yazarsak;

- xxx isimli komut için, şu komutu "npm run-script xxx" çalıştırabiliriz.

- start'a yazdığımız komutu "npm start" ile çağırabiliriz. "start" script'i, npm'in birçok standart komutlarından sadece bir tanesidir. standart olduğu için "run-script" ile çağrılmamaktadır. custom script ismi (standart olmayan isim) kullandığımızda run-script ile çağırmamız şart.

"start" komutu; "prestart", "start" ve "poststart" komutlarını (eğer package.json'da yazılmış ise) sırası ile çalıştırmaktadır.

## 📌 package-lock.json

bu dosya package.json ile aynı dizinde otomatik npm tarafından oluşturulur. burada node_modules altındaki dizinlerin bir ağaç yapısı şeklinde metadata'ları ile bulundurur. bunun birkaç sebebi var:

- npm tekrar install yaptığı zaman daha hızlı şekilde kontrol yapması sağlanır (tekrar tüm dizinler okunup metadata'larına bakmaya gerek kalmaz.).

- bu dosya source code repository'de history'si durur ve eski paketler history'den kontrol edilebilir.

- bir geliştirici yanlış paketleri kurmuş ve commit etmiş ise buradan farklı olan kısımlar incelenip neden farklı paket kurduğu incelenmeye başlanabilir.

## 📌 npm-shrinkwrap.json

manuel oluşturulan bir dosyadır. package-json ile aynı yerde olmalıdır. package.json içindeki dependency'lerin bazıları dinamik (1.0.* gibi) sürümlere sahiptir. dolayısı ile herkesin makinesinde  npm paketleri olabilir. işte bunun için npm-shrinkwrap.json dosyası çözüm olarak sunulmuştur. bu dosyada yazan paketler spesifik sürümlerde tutulmaktadır. böylece birileri bu dosyayı update etmediği sürece npm install komutu herkeste aynı sürümü kuracaktır. bu dosya yerine direk package.json içerisindeki sürümlerle de oynanabilir fakat bu sadece bir alternatiftir. örneğin MomentJS paketini sürekli yükselmesinde sakınca görülmez fakat geliştirme yapılan süreçte bunu fix tutmak isteyebilirsiniz. bunu yorum satırı olarak yazacağımıza npm-shrinkwrap.json dosyasından yararlanırız ve package.json'da MomentJS "latest" tag'i ile kalmaya devam edecektir.

"npm shrinkwrap" komutu ile hızlıca package-lock.json dosyasından  npm-shrinkwrap.json dosyası oluşturmaktadır.

## 📌 --no-shrinkwrap

npm install işleminin yanına bu parametre eklendiğinde install işlemini npm-shrinkwrap.json ve package-lock.json dosyalarını ignore ederek yapıyor.

## 📌 --save (⟷ -S)

npm install --save jquery

bu parametre npm 5 öncesi versiyonlarda package.json'a dependency'yi eklemek için yapılıyordu. npm 5 ile birlikte sadece install komutu zaten package.json'a ekleme yapıyor.

## 📌 scoped packages

"@sample-scope/sample-package" şeklinde dağıtılan paketler scope'u açan kullanıcılar tarafından yönetiliyor. tamamen gruplama amaçlı yapılmış bir şeydir. herkes dilediği gibi istediği scope'a paket atamıyor. authorization söz konusu.
