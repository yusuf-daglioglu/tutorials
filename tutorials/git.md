# GIT

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 GIT SCM (⟷ Git Source control management ⟷ Git Sürüm Kontrol Sistemi)

## 📌 Git vs SVN (⟷ Subversion)

temel fark `Git`'in dağıtık bir yapıya sahip olmasıdır. bunun sonucu olarak; herkes kendi local repo'suna sahiptir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 branching strategy (⟷ Branching workflow ⟷ branching model)

`merge strategy` farklı kavramlardır (başka bir başlıkta anlatılıyor).

development branch'lerimizi hangi branch'ten koparacağımız, branch ve merge strategy'lerimizin nasıl olacağı kurallarıdır. Geliştirilen yazılıma göre değişiklik gösterir.

Bu konuda güzel bir kaynak: https://trunkbaseddevelopment.com/game-changers/ Bu sayfayı Github'da community sürekli buradan güncelliyor: https://github.com/paul-hammant/tbd.

Not: `git-flow` hariç tüm diğer `Branching workflow`'lar baştan sonra bir framework değildir. Bu sebeple branch ismi gibi tam olarak ne yapılması gerektiğini belirten standartları yoktur. Sadece birer yaklaşım biçimleridir. `git-flow` ise, branch isimlerin bile standartlarda yer aldığı özel bir yaklaşımdır.

### 📌📌 1- git-flow

İlk olara bu makalede yayınlanmıştır: <https://nvie.com/posts/a-successful-git-branching-model/>

<https://github.com/nvie/gitflow> projesi komut satırından `git-flow`'u uygulamak için komutlar içermektedir. `git-flow` çok kullanıldığı için, 3üncü parti `GUI` tool'larının da direk native desteği vardır. `git-flow` desteği olan eklentiler şu tarz görevler yapar:

- yeni branch çıkıldığında tipini seçeriz ve isimlendirmesini otomatik yapar
- bir branch'te geliştirmemiz tamamlandığında, branch'imizin tipine göre nereye merge olması gerekiyorsa otomatik merge yapar

`git-flow` development'ta; bir `develop branch`'i olur. her developer buradan bir `feature branch` oluşturur ve ilgili geliştirmesi tamamlandığında `develop branch`'ine merge eder. `develop branch`'inin stabil olduğu bir noktadan __release branch__'ine merge oluruz. __release__'den de __master branch__'ine merge ederiz. deploy'lar __master__'dan alınır.

`master` ve `develop branch` dışındaki branch'ler:

- __Feature branches (⟷ topic branches)__

  develop'tan dallanıp, develop'a merge edilir.

- __Release branches__

  __develop__'tan dallanıp, __master__'a ve develop'a merge edilir.

  gerekirse acil hotfix'ler buraya çıkılabilir.

- __Hotfix branches__

  kritik canlı issue'ları için kullanılır.

  __master__'dan dallanır, __master__ ve __develop__'a her commit ayrı ayrı merge edilir.

### 📌📌 2- Github flow

burada default branch'ten feature branch'i çıkılır ve iş bitince `PR` açılır. `PR` tüm otomasyon testleri ve kod kalite süreçlerinden geçtikten sonra default branch'e merge edilir. Default branch direk yeni sürüm paket çıkar. `Github`'ın resmi sitesinde anlatılan basit bir yöntemdir.

### 📌📌 3- trunk-based development (⟷ TBD)

`trunk` kelime anlamı: gövde, ana hat, ana yol.

bu strategy'de `master` branch, `trunk` veya `main` veya `mainline` olarak da isimlendirilir.

`TBD`, `Github flow`'a aynı mantıktadır. Fakat değindikleri amaç farklıdır. `Github Flow`, nereden nereye merge yapılacağından ve `PR`'lar üzerinden gidilmesi gerektiğinden bahsederken, `TBD` feature branch'lerin hızlı olması gerektiğinin öneminden bahseder.

`TBD`'de master her zaman deploy'a hazır durumda olmalıdır. `TBD` küçük ekipler için uygundur. Ufak özellikler master'dan koparılıp master'a merge edilir.

### 📌📌 4- monorepo

`monorepo` bir şirketteki ilgili olan projelerin tek bir `Git` repo'sunda olma stratejisidir. sürekli lokale farklı repo indirme, repo için yetki isteme, depended projeleri indirip birbirine link etme (relative path ile aynı repo'da depended projeler gösterilir) problemlerinden kurtulması sağlanır.

Fakat eksileri de vardır:

- herkes her projeyi görebilir (güvenlik)
- repo boyutu çok büyüktür

### 📌📌 5- Selective Merge

`Feature branch`'lerini ayrı ayrı merge ederek oluşturulan branch'i master ortamına merge ederek deployment alma stratejisidir.

### 📌📌 6- Forking Workflow

Genelde açık kaynaklı projelerde kullanılan bir stratejidir. Tüm geliştiricilerin kendi repo'suna sahip olduğu yapıdır. Geliştirme yapacak developer asıl projeyi komple kendi sunucusuna kopyalar. Orada önce kendine push'lar, sonra asıl projenin sahibi oradan kendisine merge alır.

`Github` yapısı buna izin vermektedir.

### 📌📌 7- Centralized Workflow

### 📌📌 8- Feature Branch Workflow

`Feature branch`'lerin uzun süreli yaşayabildiği workflow'lardır.

Bu yapı `TBD`'in tersidir. `TBD`'de `feature branch`'lerin kısa sürede main branch'e atılması önerilir. Hatta gerekiyorsa toggle (switch) koyarak ortamda kodun çalışmaması sağlanmalıdır. Aksi durumda continuous delivery çok zor olur. Çünkü genelde eğer feature'ler uzun sürerse, test'te ona paralel uzun sürecektir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Git config file

`git` config ayalarını aşağıdakilerden toplayıp birleştirir:

- `SYSTEM`: `C:\Program Files (x86)\Git\etc\gitconfig`

- `GLOBAL`: `$HOME/.gitconfig`

- `GLOBAL`: `$XDG_CONFIG_HOME/git/config` `$HOME/.config/git/config`

- `LOCAL`: `my_java_project_repository/.git/config`

örnek `gitconfig` dosyası:

```ini
[http]
    sslBackend = openssl
    sslCAInfo = C:/Program Files/Git/mingw64/ssl/certs/ca-bundle.crt
[credential]
    helper = manager
```

system config dosyasından hangi bilgilerin okunduğunu bu komut ile görebiliriz:

```sh
git config --system --list
```

komutun çıktısı:

```
http.sslbackend=openssl
http.sslcainfo=C:/Program Files/Git/mingw64/ssl/certs/ca-bundle.crt
credential.helper=manager
```

Tüm config'lerin dosya bazlı listelenmesi için:

```sh
git config --list --show-origin
```

örnek çıktı:

```
file:"C:\\ProgramData/Git/config"                      core.symlinks=false

file:C:/Program Files/Git/mingw64/etc/gitconfig        http.sslbackend=openssl
       
file:C:/Program Files/Git/mingw64/etc/gitconfig        http.sslcainfo=C:/Program Files/Git/mingw64/ssl/certs/ca-bundle.crt

file:C:/Users/user-name/.gitconfig                     user.name=user-name
```

Benzer şekilde, dosya bazlı değilde, scope bazlı (global, sistem gibi) listelemek için bu komut gereklidir:

`--show-scope` parametresi `Git` `2.26` sonrasında gelmiştir

```sh
git config -l --show-scope
```

örnek çıktı:

```text
global    user.global=true
  
global    user.absolute=include
  
local     user.local=true
  
local     user.override=local
```

## 📌 Git Credentials

`git` `commit`'lerken, `push`'larken kullanacağı credential bilgilerini birçok farklı yerden okuyabilir. nereden okunacağı gitconfig dosyasında belirtilebilir. eğer config dosyasında belirtilmemiş ise, kullanılan `OS`'a göre default bir değeri vardır. biz bunu elle bu şekilde değiştirebiliriz:

```sh
git config --global credential.helper cache # cache moduna geçirdik
```

Bazı `credential-helper`'lar:

- `none` (no helper)

  does not store anywhere. on every commit, push action `Git` will ask for password and username.

- `cache`

  çok kısa bir süreliğine (default: 15 dk) `RAM`'de tutar. bunun için arkada `Git` daemon'u çalışır.

  terminal session'ı açık kaldıkça geçerlidir.

- `cache--daemon`

  cache ile aynı fakat daemon user session bazlıdır.

- `store`

  txt formatında, açık şekilde, `$HOME` dizini içinde bir dosyada saklar (default: `$HOME/.git-credentials`)

- `libsecret`

  `Gnome Keyring` ve `KDE Wallet`'ta (default key manager'larında) saklanmasını sağlıyor.

- `osxkeychain`

  `MacOS`'ta isek default olarak `MacOS`'un yüklü getirdiği `Osxkeychain` uygulamasında saklar.

- `MS Windows` için:

  - `winstore`

    3üncü parti. geliştirmesi durdu.

  - `Wincred`
  
    `MS Windows` için default mode'dur.
    
    `MS Windows`'un `Credential Manager`'nı kullanır.
  
  - `Git Credential Manager for Windows`
  
    `GCM Core` yayımlanınca geliştirilmesi durdu.

    `GCM Core`'a yayımlanana kadar kısa ismi `manager`'dı.

    `GCM Core` yayımlanınca kısa isimler böyle oldu:

    - `Git Credential Manager for Windows`: `manager`
    - `GCM Core`: `manager-core`

    `Git Credential Manager for Windows` tamamen kaldırılınca, `GCM Core`, `manager` olarak isimlendirildi.

  - `Git Credential Manager Core (⟷ GCM Core)`

    Bu 2 proje `GCM core` isimli projede birleştirildi:

    - `Git Credential Manager for Windows`
    - `Git Credential Manager for Mac and Linux`

    Eski dökümanlarda yukarıdaki iki proje `GCM` olarak adlandırıyordu kendini. `for Windows` gibi terimleri kullanmıyorlardı. Bu karışıklığa sebep olabilir.

    Tüm `GCM`'ler arka planda `OS`'u native key manager'larını kullanırlar.

- `credential-helper-selector`

  helper seçmek için `GUI` ekranını açar. 
  
  her credentials'ın kısa ismi yazmaktadır. fakat fare ile her seçeneğin üstünde durduğumuzda uzun ismi popup olarak gösterilmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Git workspace structure

`git` workspace'si içerisinde 3 tane alan vardır:

- __Git klasörü__

  "__.git__" isimli dizindir. burada `Git` kendisi için gerekli tüm meta bilgileri tutar.

- __Çalışma klasörü (⟷ working directory)__

  projemizin "__.git__" klasörü dışında kalan tüm dosyalarıdır.

- __Hazırlık alanı (staging area)__

  genellikle "__.git__" klasörümüzde bulunan bir dosyadır.
  
  commit işleminde hangi değişikliklerin dahil olacağını tutar.
  
  Bazı kaynaklarda "__index__" olarak isimlendirilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 State

dosyaların bulunabileceği 2 farklı state vardır:

- __Tracked__

  önceki commit'lerde DB'de olan yada hiçbir commit'te olmayıp şu anda "tracked" state'ine manuel olarak alınmış dosyadır.

  Kendi içinde 3 çeşit durumda olabilir:

  - __Kaydedilmiş (⟷ staged)__: verinin güvenli biçimde DB'de depolanmış olduğu anlamına gelir.

  - __Değiştirilmiş (⟷ modified)__: dosyayı değiştirmiş olduğunuz fakat henüz DB'ye kaydetmediğimiz anlamına gelir.

  - __Hazırlanmış (⟷ unmodified)__: değiştirilmiş bir dosyayı bir sonraki kayıt işleminde bellek kopyasına alınmak üzere işaretlediğiniz anlamına gelir.

- __Untracked__

  önceki commit'lerde bu dosya yoktur.

State'ler arası değişim aksiyonlara göre şu şekildedir:

```text
                                              TRACKED
                           ______________________|________________________
                          |                      |                        |
UNTRACKED              Unmodified              Modified                Staged


  state-1--------------------->>  add file  >>-------------------------state-2


                      state-1--->>  edit file  >>---state-2


                                              state-1-->>  stage file  >>--state-2


    state-2--<<  remove file  <<--state-1


                        state-2--------------<<  commit file  <<------------state-1
```

yukarıdaki şekildeki terimler, "Git status" çıktısında direk yazmıyor. Daha son kullanıcı dostu mesajlar yazıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Commands

## 📌 Amend

amend kelime anlamı: iyileştirmek

yeni değişiklikleri, son commit'e eklemek:

```sh
git commit -m 'last commit'

git add "forgotten_file"

git commit --amend
```

## 📌 reset

doysa dosyayı silmeden, sadece kayıt alanına eklenmiş ise sadece kayıt alanından kaldırmak için:

```sh
git reset HEAD myfile.txt
```

komutu kullanılır. bu şekilde dosya tracked olmaya devam eder fakat kayda atılmamış olur.

bir dosya değişmiş durumda fakat kayıt alanına eklenmemiş ise; dosyayı reset'lemek yani değiştirilmemiş hale getirebilmek için:

```sh
git checkout -- myfile.txt
```

komutu çalıştırılır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 remote

local repo ile çalışmaya benzer olarak, local repoyu uzak (remote) repo'lar ile işlemede tabi tutabilir. remote için pull yada push gibi işlemler yapılır.

__origin__, `Git`'in klonlamanın yapıldığı sunucuya verdiği öntanımlı addır.

bu komut ile origin adresi görüntülenir:

> git remote -v

bu komut birden fazla remote görüntüleyebilir. çünkü birden fazla remote olabilir. push ve pull için ayrı repo'larda olabilir. örneğin bir yerden çekip başka bir yere push yapıyor olabiliriz.

bu komut ile yeni remote'lar eklenebilir:

```sh
git remote add "[short_name]" "[URL]"

git fetch origin
```

komutu o anda olduğumuz branch'le ilişkili uzaktaki tüm değişiklikleri local `Git` DB'sine çeker fakat çalışma alanı üzerinde değişikleri yapmaz. çalışma alanı ile birleştirme işlemi için pull komutu kullanılır. çünkü "pull"; hem fetch, hem de rebase işlemini yapacaktır.

fetch komutu eğer remote verilmemişse (yukarıdaki örnekte "origin"), default olarak ilgili branch ile ilişkilendirilmiş remote branch'ten fetch yapar. o da yok ise default olarak 'origin'den çeker.

fetch komutunun "-all" parametresi tüm remote'lardan fetch işlemini yapar. fetch işlemi branch bazlı değildir.

> git push origin master

yukarıdaki komut ile local repoda commit'lenmiş değişiklikler remote'a yollanır. eğer biz yollamadan önce biri değişiklik yollamış ise, bizim değişiklik reddedilecektir. önce eski değişiklikleri alıp, local ile rebase olup, bu şekilde commit'lememiz ve daha sonra push etmemiz gerekmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 tag

etiket (tag) `Git` log'ları üzerinde görülebilirliği arttırmak amaçlı (sürüm tutma gibi) bilgisini saklamak için kullanılır.

"git push" işlemi tag'leri remote'a yollamaz. tag'leri ayrı komut ile yollamak gerekir. yada hepsini birden yollamak için özel parametre geçilmelidir:

> git push origin --tags

2 çeşit tag vardır:

- lightweight

  sadece commit'in referansına bir label atmak için kullanılır. örnek tag atma:

  > git tag "alpha"

- annotated

  birer `Git` objesidir ve gelişmiş özellikler içerir.

  örnek annotated tag atma:

  > git tag -a v1.4 -m "alpha"

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Git object

Aşağıdakiler için kaynak: <https://book.git-scm.com/book/en/v2/Git-Internals-Git-Objects>

Git bir commit'teki değişiklikleri, diff olarak değil, değişen dosyaların snapshot'ını alarak saklar. Oysa CVS, Subversion, Perforce diff olarak değişiklikleri saklar. Bu sebeple eğer dosyamız büyük ise, 1 karakter değiştirsek bile commit'imizin metadata'sı da büyük olacaktır.

1 commit şunları içerir:

- author
- committer
- commit message
- parent commit id
- 1 adet tree

1 tree birden fazla tree ve/veya birden fazla blob'a referans edebilir. tree'ye referans ediyorsa, bir klasör(dizin), blob ise dosyaya referans etmektedir. Yani bir klasör değiştirirsek tree oluştururuz, dosya değiştirirsek blob oluştururuz.

tree ve blob ayrı ayrı birer `Git` objesidir. hepsinin birer tekil ID'si vardır. Bu ID ile onları komut satırına direk bu komut ile yazdırabiliriz:

```sh
# rastgele "git" objelerini "git" klasöründe ararız:
find .git/objects -type f

# yukarıdaki komutun çıktısı bu olsun:
.git/objects/44/33333333333333333333333333333333333333
.git/objects/44/22222222222222222222222222222222222222
.git/objects/cb/11111111111111111111111111111111111111

# ikinci satırdaki objeyi ekranda gösterelim:
git cat-file -p 22222222222222222222222222222222222222

# komutun çıktısı:
# Hello this is my old file content
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 branch

Her branch, yukarıdaki gibi bir commit objesinin ID'sine referans etmektedir.

"master" isimli branch'in diğer branch'lerden hiçbir farkı yoktur.

yeni bir dal (branch) yaratmak için:

> git branch b1

var olan bir dala geçmek için:

> git checkout b1

master ve test dalımız olsun. test master'dan yeni dallanmış ve sadece ek 1 değişiklik içeriyor olsun. test dalımızdaki değişiklikleri master dalına almak için bu 2 branch'i birleştirmek gerekiyor.

önce master dalımıza geçiş yapalım:

```sh
git checkout master

git merge test
```

burada "fast forward (hızlı ileri alma)" yapılmıştır. çünkü test master'dan türemiştir.

`Git`'te branch birleştirme yada başka bir birleştirme işleminde aynı dosyalar düzenlenmiş ise conflict durumu söz konusu olur. böyle bir durumda işlem devam edemez. yarıda kalır. merge edilecek dosya "git status" komutu ile "unmerged" olarak görülür. git, bu dosyaların içine uyuşmazlıkları gösteren standart formatlarda işaretler koyar. örnek:

```text
<<<<<<< HEAD:index.html

<div> Hello </div>

======

<div> Hello World</div>

>>>>>>> my_branch:index.html
```

Yukarıda "Head" yazan bulunduğumuz branch'i temsil ediyor. ======= işaretinin üstü HEAD branch'inde olan metin iken, ======= aşağısı my_branch branch'indedir.

ilgili dosya (conflict olan dosya) üzerinde değişiklikleri manuel yaparız. daha sonra ilgili dosyayı "git add index.html" kayda hazır hale getiririz. daha sonrada kayıttakileri commit'leyerek birleştirme işlemini tamamlamış oluruz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 gitignore

`.gitignore` dosyaların commit'lenip commit'lenemeyeceğine karar veriyor.

bazen bazı dosyalar gitignore listesinde olmasına rağmen commit'lenebilir dosyalar listesinde yer alabilmektedir. bunun sebebi; o dosya/dosyaların "tracked" olmasıdır. o dosyaları önce "untracked" statüsüne geçirilmesi gerekmektedir.

bir dosyayı untracked yapmak için:

```sh
git rm --cache /dir/file.txt
```

gitignore %100 regex kurallarına uymuyor.

bazı örnekler:

```text
# ignore all .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in any directory named build
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory and any of its subdirectories
doc/**/*.pdf
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Git for MS Windows

`Git` resmi sitesinde download'larda görülen, ama resmi olmayan bir `Git` fork'udur.

official `Git` paketi; `MS Windows` için build edildiğinde `Git-bash` gibi programlarda yüklenmektedir. zaten `MS Windows` için çıkılan build arka planda shell bağımlılıkları vardır. çünkü `git` `MS Windows` için tasarlanmamıştır. `Git for MS Windows` ise, resmi `Git`'in bu kısımlarını ortadan kaldırmaya çalışır. çözüm olarak `MS Windows` için `Git`'in sadece `POSIX` uyumlu kısımlarını tekrar native olarak yazmaktadırlar.

`git-bash.exe` bu proje ile yüklü gelir. `Git` tümüyle `C`'de yazılmamıştır. Bir kısmı shell ve Perl içermektedir. Bu kısımlar tekrardan yazılması (ve yeni güncellemelerin desteklenmesi) büyük maliyet olduğundan bu kısmı kapatmak için "git-bash.exe" ile birlikte gelmektedir.

"git-bash.exe" eskiden Msys için özel üretilen bir fok'u (__MsysGit__) ile gelirken daha sonra Msys2'tye geçilmiştir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Git değişiklikleri nasıl tutar

`Git` değişiklikleri dosya olarak tutar. sadece değişiklik olan text'leri ve satır numaralarını değil.

komut satırından yapılan her aksiyonu DB'ye kaydeder. bu şekilde her şey geri alınabilir ve kaybolmaz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Git reset çeşitleri

A bir commit noktası olmak üzere;

```sh
git reset --soft A
```

A noktasına `HEAD`'ı çeker. Fakat `A`'dan sonra yapılmış tüm commit'lerdeki dosyaları staged olarak atar (ready to commit).

```sh
git reset --mixed A
# or same:
git reset A
```

A noktasına `HEAD`'ı çeker. Fakat `A`'dan sonra yapılmış tüm commit'lerdeki dosyaları değiştirilmiş hale getirir. yani son kullanıcı önce u dosyaları staged yapıp öyle commit'leyebilir.

```sh
git reset --hard A
```

A noktasına `HEAD`'ı çeker. A'dan sonra yapılmış tüm commit'teki dosyalar yok olur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 rebase vs merge

Ikisi de farklı branch'leri birleştirmek için kullanılan komutlardır.

- __rebase__

  X ve Y branch'lerini __rebase__ ile birleştirirsek; X'i Y'nin ucuna eklemiş oluruz. Yani Y'ni tabanını (parent zincirini) değiştirmiş oluruz.

- __merge__

  __merge__ işlemi nde ise birden fazla branch2i birleştirebiliriz. Merge'ün çok çeşit seçeneği var. Bazı seçenekler duruma göre rebase ile apaynı sonucu verebilir.

## 📌 pull

"__pull__" işlemi; sıra ile "__fetch from upstream__" ve "__rebase__" işlemlerinin ikisine birden denk gelir. burada "__rebase__" işlemi; local branch ile remote branch'i rebase etmektedir.

Ek not: remote branch'ler olmak zorunda değildir. kişi kendi içinde tek local-repo ile de geliştirme yapıp commit'leyebilir. commit'ler local repoya atılır. "push to upstream" yapıldığında (eğer remote anında onay vermiyorsa, örneğin arada Gerrit var ise), Eclipse'de remote kısmında hala remote-branch'in geride kaldığı(commit'i almadığı) görülür. yani aslında biz Eclipse'te çalışırken sadece local branch'lerimizde oynama yapabiliyoruz. remote tüm branch'ler sadece birer yansımadır.

## 📌 fast forward merge vs no-fast forward merge

- Ikisi de "__merge__" komutunun (işleminin) birer seçeneğidir. İkisinden birini seçmek zorundayız.
- `Git` default olarak __fast-forward merge__ yapmaya çalışır. "__--no-ff__" parametresi geçilerek bu durum engellenebilir.
- __Fast forward merge__'de __rebase__'deki gibi tüm commit'ler zincirleme olarak eklenir.
- "__no-fast forward__" da ise tüm commit'ler tek bir merge commit'i olarak görünür.
- "__squash__", "__no-fast forward__" tamamen aynıdır. tek fark: squash'te gir merge yapıldığını bilmezken, "no-fast forward" da, git; merge olduğunu bilir.

Örnekler:

```sh
## 📌 Aşağıdaki kodlar direk shell'de execute edilebilir.

createFileAndCommit(){

    OPERATION_ID="$1"
    FILE_NAME="file$OPERATION_ID.txt"
    touch "$FILE_NAME"
    echo "data$OPERATION_ID" > "$FILE_NAME"
    git add "$FILE_NAME"
    git commit -m "c$OPERATION_ID"
}

gitPrintLog(){
    git log --all --graph --pretty=format:'-%C(yellow)%d%Creset %s %Creset' --abbrev-commit
}

####################################
####################################
####################################
# EXAMPLE: MERGE WITH NO-FAST-FORWARD
####################################
####################################
####################################
rm -f -r project
mkdir project
cd project
git init

git checkout -b "b1"
createFileAndCommit "1"

git checkout -b "b2"
createFileAndCommit "2"
createFileAndCommit "3"

gitPrintLog
###################
# CURRENT GIT LOG
# * - (HEAD -> b2) c3
# * - c2
# * - (b1) c1
###################

git checkout b1
git merge --no-ff b2 -m "m1"

# Yukarıdaki satırda 'm' parametresi ile mesajı geçmeseydik 2 seçeneğimiz olurdu:
# 1- default olarak "git" mesajı kendi atardı.
# 2- merge işlemi tamamlandığında biz ayrı bir komut ile commit işlemi gerçekleştirip mesajı tanımlamak zorunda kaldık: git commit -m "m1"

gitPrintLog
###################
# CURRENT GIT LOG
# * - (HEAD -> b1) m1
# |\
# | * - (b2) c3
# | * - c2
# |/
# * - c1
###################
cd ..

####################################
####################################
####################################
# EXAMPLE: MERGE WITH NO-FAST-FORWARD
####################################
####################################
####################################
# Konunun daha kolay anlaşılması için bu örnek, yukarıdaki aynı örnek üzerinden anlatıldı.
rm -f -r project
mkdir project
cd project
git init

git checkout -b "b1"
createFileAndCommit "1"

git checkout -b "b2"
createFileAndCommit "2"
createFileAndCommit "3"

gitPrintLog
###################
# CURRENT GIT LOG
# * - (HEAD -> b2) c3
# * - c2
# * - (b1) c1
###################

git checkout b1
git merge b2 -m "m1" # bu komut için 2 bağımsız not:
# 1- no-ff olmadığı için, "git" default olarak "ff" olarak merge işlemine devam eder.
# 2- Bu komut'taki commit-message ignore ediliyor. "Git" warning olarak zaten sebebini yazıyor: "no commit created; -m option ignored".

gitPrintLog
###################
# CURRENT GIT LOG
# * - (HEAD -> b1, b2) c3
# * - c2
# * - c1
###################
```

## 📌 rebase

Rebase işlemi var olan zincirin tabanını değiştirmeye yarıyor.

### 📌📌 EXAMPLE: SIMPLE REBASE

```sh
createFileAndCommit(){

    OPERATION_ID="$1"
    FILE_NAME="file$OPERATION_ID.txt"
    touch "$FILE_NAME"
    echo "data$OPERATION_ID" > "$FILE_NAME"
    git add "$FILE_NAME"
    git commit -m "c$OPERATION_ID"
}

gitPrintLog(){
    git log --all --graph --pretty=format:'-%C(yellow)%d%Creset %s %Creset' --abbrev-commit
}

####################################
####################################
####################################
# REBASE
####################################
####################################
####################################
rm -f -r project
mkdir project
cd project
git init

git checkout -b "b1"
createFileAndCommit "10"

git checkout -b "b2"
createFileAndCommit "20"
createFileAndCommit "21"

git checkout "b1"
createFileAndCommit "11"

gitPrintLog
###################
# CURRENT GIT LOG
# * - (HEAD -> b1) c11
# | * - (b2) c21
# | * - c20
# |/
# * - c10
###################

git checkout b2
git rebase b1
# or same command without need to checkout to b2:
# git rebase b1 b2

# Bu komut ile b2'nin base'ini (tabanını veya başka bir değiş ile parent'ını) b1 olarak set ediyoruz. Yani; sanki b1'e checkout olup, oradan b2 isimli bir branch oluşturduk ve geliştirmelerimizi orada yapmış gibi oluyoruz.

# Commit ID'ler: b2 branch'inde Commit mesajları aynı kalıyor, fakat SHA bilgileri farklıdır.

gitPrintLog
###################
# CURRENT GIT LOG
# * - (HEAD -> b2) c21
# * - c20
# * - (b1) c11
# * - c10
###################
```

### 📌📌 EXAMPLE: REBASE WITH INTO PARAMETER

```sh
createFileAndCommit(){

    OPERATION_ID="$1"
    FILE_NAME="file$OPERATION_ID.txt"
    touch "$FILE_NAME"
    echo "data$OPERATION_ID" > "$FILE_NAME"
    git add "$FILE_NAME"
    git commit -m "c$OPERATION_ID"
}

gitPrintLog(){
    git log --all --graph --pretty=format:'-%C(yellow)%d%Creset %s %Creset' --abbrev-commit
}

####################################
####################################
####################################
# EXAMPLE: REBASE WITH INTO PARAMETER
####################################
####################################
####################################
rm -f -r project
mkdir project
cd project
git init

git checkout -b "b1"
createFileAndCommit "10"

git checkout -b "b2"
createFileAndCommit "20"
createFileAndCommit "21"

git checkout -b "b3"
createFileAndCommit "30"
createFileAndCommit "31"

git checkout "b2"
createFileAndCommit "22"
createFileAndCommit "23"

git checkout "b1"
createFileAndCommit "11"
createFileAndCommit "12"

gitPrintLog
###################
# CURRENT GIT LOG
# * - (HEAD -> b1) c12
# * - c11
# | * - (b2) c23
# | * - c22
# | | * - (b3) c31
# | | * - c30
# | |/
# | * - c21
# | * - c20
# |/
# * - c10
###################

git rebase --onto b1 b2 b3
# Yukarıdaki komut ve parametreleri hakkında:
# - her parametre commit id veya branch ismi alıyor.(Unutmamak lazım ki; Git'te zaten branch ismi, o branch'teki son commit'in referansı oluyor)
# b1 -> b1'i taban olarak kabul et.
# b2 -> 1inci parametredeki (bu örnekte b1'deki) commit'ten bu parametredeki commit'e (bu örnekte b2) kadar olan commit'leri ignore eder.
# b3 -> ikinci parametreden (bu örnekte b2'den) b3'e kadar olan kısmı referans aldığımızı gösterir. 
# Tüm parametrelerin anlamı farklı bir değiş ile şuna denk geliyor: b2'den b3'e kadar olan commit'lerin tümünün tabanını b1 yap.
# yukarıdaki komut HEAD'i b3'e çeker.
# Yukarıdaki komutta "b2" yerine "c21" yazarsak aynı sonucu alırız.

# Bu komut şu zamanlarda kullanılabilir: B3'ün b2'ye hiç depend olmadığı görülür. B1 master branch olsun. Master branch'ime sync olabilmek için bu komutu kullanabilirim.

gitPrintLog
################### 
# CURRENT GIT LOG
# * - (HEAD -> b3) c31
# * - c30
# * - (b1) c12
# * - c11
# | * - (b2) c23
# | * - c22
# | * - c21
# | * - c20
# |/
# * - c10
###################

cd ..
```

## 📌 merge strategies

merge işlemi fast forward değilse "-s" ile birçok strateji parametresi alabilir:

- recursive
- resolve
- octopus
- ours
- subtree

Her stratejinin ise kendine özgü parametreleri vardır. bunları komut satırında "-X" parametresinin yanında giriyoruz. örneğin recursive'in kabul ettiği bazı parametreler:

- ours
- theirs
- ignore-space-change

## 📌 ours

hem -X hem de -s ile yollanabilir ve ikisinin işlevi farklıdır.

> git merge -X ours b2

b2 branch'ini merge eder fakat eğer herhangi bir dosyada conflict olursa sadece current branch'i doğru olarak kabul eder.

> git merge -s ours b2

"-X ours" ile aynı işlevi görür. Ek olarak; b2'den current-branch'te olmayan bir dosya gelirse o zaman o dosyayı kabul etmez. sonuç olarak bakıldığında "-s ours" current-branch'te hiçbir değişiklik olmamasını sağlar. bu komut `Git`'e merge işlemi olduğunu bildirmek için yapılır.

## 📌 squash

kelime anlamı: ezmek.

merge işlemine geçilen özel bir parametredir.

```sh
createFileAndCommit(){

    OPERATION_ID="$1"
    FILE_NAME="file$OPERATION_ID.txt"
    touch "$FILE_NAME"
    echo "data$OPERATION_ID" > "$FILE_NAME"
    git add "$FILE_NAME"
    git commit -m "c$OPERATION_ID"
}

gitPrintLog(){
    git log --all --graph --pretty=format:'-%C(yellow)%d%Creset %s %Creset' --abbrev-commit
}

####################################
####################################
####################################
# MERGE WITH SQUASH
rm -f -r project
mkdir project
cd project
git init

git checkout -b "b1"
createFileAndCommit "1"

git checkout -b "b2"
createFileAndCommit "2"
createFileAndCommit "3"

gitPrintLog
###################
# CURRENT GIT LOG
# * - (HEAD -> b2) c3
# * - c2
# * - (b1) c1
###################

git checkout b1
git merge --squash "b2"
git commit -m "m1"

gitPrintLog
###################
# CURRENT GIT LOG
# * - (HEAD -> b1) m1
# | * - (b2) c3
# | * - c2
# |/
# * - c1
###################

cd ..
```

Dikkat edilirse `Git` merge yapıldığı bilgisini tutmuyor.

"--no-squash" parametresi ile squash yapılmamasını sağlayabiliriz.

## 📌 --continue

```sh
git merge --continue
```

şeklinde `merge`'e parametre olarak geçilir.

`merge` işlemi yapıldığında conflict olursa, git; conflict'leri gidermemizi bekler. o sıra merging moduna girer. artık user `merge`'u abort etmeli yada `merge`'ü conflict'leri tamamlayıp sonlandırmalıdır.

`--continue` parametresi `Git` komutuna pass edildiğinde, `Git` merging modundan sonlanma moduna girer. (merge işlemini tamamlamak için devam etmiş olur. "continue" terimi buradan geliyor.)

`Git` `--continue` işlemi sonrası genelde commit atılır. bu sebeple `Git` `2.12.0` ile `Git` commit artık `--continue` parametresi ile aynı işi görmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 hook

`./.git/hooks` içerisinde birçok `script` dosyası mevcut. bu `script`'ler her `Git` event'i ile birlikte çağrılmaktadır. eğer `script`'ler `commit`'te bir terslik olduğuna karar verirlerse, event'i yarıda kesebiliyorlar. örneğin `pre-commit script`'i `eslint` hatalarına bakabilir. eğer eslint hatası varsa `commit`'e izin vermez.

geçici bunu aşmak istiyorsak komut satırından;

`--no-verify`

parametresi her `Git` komutunun sonuna eklenebilir. yada  `pre-commit script`'inin içeriği elle silinebilir.

hook script'leri client side'da olduğu için yazılımcı tarafından silinebilir veya esgeçilebilir. bu sebeple sunucu tarafta da hook yapılabilir.

hook script'leri birer script'in language ile yazılmış dosyalardır. internetten rastgele örnek bir pre-commit hook'una dosyasının tepsine bakıldığında #!bin/bash veya PHP gibi tanımlar görünür. `Git` bu script'e parametre geçer ve exit code'una bakarak işlemin kesilip kesilmeyeceğine karar verir.

örneğin; projemizdeki .git/hooks/commit-msg.sample doyası örnek bir script içerir. bu devrede değildir. devreye almak için bu dosyanın sonundaki .sample'ı silmemiz gerekir. bu örnek shell script'i ile yazılmıştır. biz bu dosyanın içeriğini tümü ile silip NodeJS ile çalışmasını sağlayabiliriz. "commit-msg" dosyası `Git` tarafından, commit mesajı girildikten hemen sonra execute edilir. örnek bir commit-msg dosyası:

```js
#!/usr/bin/env node

var COMMIT_MSG_MIN_LENGTH = 50;

console.log("");
console.log("commit-msg hook started");
console.log("");

const fs = require('fs');
const commitMessage = fs.readFileSync(process.argv[2], 'utf8').trim();

const childProcessExec = require('child_process').exec;
const util = require('util');
const exec = util.promisify(childProcessExec);

checkBranchNameAndMessage();

function errorHook(erroMsg){
  console.log("");
  console.log("error detail: " + erroMsg);
  console.log("");
  process.exit(1);
}

async function checkBranchNameAndMessage(){
  const allBranches = await exec('git branch');
  currentBranch = allBranches.stdout.split('\n').find(b => b.charAt(0) === '*').trim().substring(2);

  console.log("");
  console.log("currentBranch:" + currentBranch);
  console.log("commit message: " + commitMessage);
  console.log("");

  if(commitMessage){
    if(commitMessage.length > COMMIT_MSG_MIN_LENGTH){

    }else {
      errorHook("commit message length is less then " + COMMIT_MSG_MIN_LENGTH);
    }
  } else {
    errorHook("commit message is not defined!");
  }

}
```

Yukarıdaki kod NodeJS üzerinde çalışacaktır.

- process.argv environment'ten değer çekebilmemizi sağlıyor.
- process.exit exit code'umuzun istersek 0 dan farklı olmasını sağlıyor ve commit işlemini yarıda kesiyor
- hook dizininde npm yada nodejs module olmamasına rağmen, "require('fs');" yapabiliyoruz. bunun sebebi NodeJS'in runtime'da bazı module'leri default olarak inject etmesidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 branch değiştirme

branch değiştirince; staged'de duran veya unstaged'de duran dosyalara hiçbir şey olmaz. oldukları yerde kalmaya devam ederler.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Tracking Branches

`Git` push ve pull komutlarının ek parametreye ihtiyaç duyulmadan çalışmasını sağlar. her branch'in takip ettiği branch'ler vardır. her branch'in "tracking branch"i olabilir. dolayısı ile push ve pull direk tracking branch üzerinden çalışır.

örnek:

```sh
git checkout --track "origin/fix1"

# output:
# Branch fix1 set up to track remote branch refs/remotes/origin/fix1.
# Switched to a new branch "fix1"
```

lokalde farklı isimde branch istiyorsak:

```sh
git checkout -b "fix1_local" "origin/fix1"

# output:
# Branch sf set up to track remote branch refs/remotes/origin/fix1.
# Switched to a new branch "fix1_local"
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 stash

Türkçe kelime anlamı: güvenli bir yere gizlemek, saklamak.

stage'de olan yada olmayan dosyaları (commit'lenmemiş dosyaları) yedek alabiliriz. stash'teki bu dosyaları istediğimiz zaman istediğimiz branch'e apply edebiliriz.

birden fazla stash'imiz olabilir. tümünü şu şekilde listeleyebiliriz:

```sh
git stash list

# output:
# stash@{0}: WIP on master: 049d078 Create index file
# stash@{1}: WIP on master: c264051 Revert "Add file_size"
# stash@{2}: WIP on master: 21d80a5 Add number to log
```

stage'de olan değişikliklerimizi şu şekilde stash'e alabiliriz:

```sh
git stash
```

başka branch'e geçip yada aynı branch'te istediğimiz zaman apply edebiliriz:

```sh
git stash apply stash@{2}
```

Eğer en son yarattığımız (en güncel) stash'i apply etmek istiyorsak hiçbir şey belirtmemize gerek yok:

```sh
git stash apply
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 shelve

- `Git`'te böyle bir özellik yoktur. Jetbrains'in IDE'lerinde (IntelliJ, WebStorm, PhpStorm, PyCharm...) kullandığı özel bir feature'dir.
- `Git`'teki "Stash" özeliği ile aynı mantıktadır.
- shelve, IDE'de açılmış olan projedeki SCM'den bağımsız çalışır ve kendi dosyalarını ".idea/shelf" dizinine saklar. Bu sebeple shelve kullanmak için projede SCM ihtiyacı dahi yoktur.
- Oysa Git, stash dosyalarını ".git" içinde tutar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 commit ID

SHA-1'dir ve unique'tir.

## 📌 refs

references'in kısaltmasıdır. ref'ler commit'lerin birer referansıdır. alias gibi düşünülebilir. ref'ler basit dosyalar şeklinde tutulur. ".git/refs" dizini içerisindedirler.

örneğin;

```text
.git/refs/remotes/origin/master
```

dosyasında sadece bir satır yazar ve bu satırda remote'un bulunduğu head (muhtemelen son commit'in) commit'in ID'si (hash'i - sha1) vardır.

```text
.git/refs/heads/master
```

dosyası da benzer şekilde local repo'daki son commit'imizin hash'ini tutar.

```text
.git/refs/remotes/origin
```

dosyasında:

```text
ref: refs/remotes/origin/master
```

yazmaktadır. bu satır farklı bir yere referans ettiğini gösterir.

```sh
git show master
```

komutu, master dosyamızdaki commit-id yi alır ve bize o commit hakkında detayları gösterir. bu komut yerine bu komutu da yazabilirdik:

```sh
git show refs/heads/master
```

## 📌 Refspecs

reference specifications'ın kısaltmasıdır.

git, push komutunda hangi referansın, hangi referans ile ilişkilendirileceğini tutmak zorundadır. formatı:

```text
source:destination
```

şeklindedir. örneğin;

```sh
git push origin localBranch1:refs/heads/remoteBranch1
```

opsiyonel "+" (artı) işareti non-fast-forward update ile push yapmasını istediğini belirtir:

```sh
git push origin +localBranch1:refs/heads/remoteBranch1
```

## 📌 delete remote branch

Aşağıdaki komutta source olmadığı için boştur ve uzaktaki makinada "remote-branch-name" isimli branch'i siler.

```sh
git push origin :remote-branch-name
```

`Git`'in güncel versiyonlarında remote branch'i silmek için ek komut gelmiştir:

```sh
git push origin --delete remote-branch-name
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Reflog

`Git`'te her şey burada log'lanmaktadır. silinen branch'ler dahi buradan geri getirilebilir.

Fakat git, aralıklarla erişilmeyecek objeleri silmektedir. bu sebeple %100 her şeyin sürekli geri getirilebilir olduğunu söylemek mümkün değil.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Github için "pull request (⟷ PR)" ve Gitlab için "merge request (⟷ MR)"

tamamen aynı mantıkta çalışırlar.

## 📌 "Git" altyapısında bulunan pull request

feature_1_branch push'larız. daha sonra:

```sh
git request-pull "master_branch" "https://git/project1" "feature_1_branch"
```

Artık upstream'de bulunan review/mail sistemi otomatik herkese email atacaktır. bu işlem sadece bildirim için kullanılır. yetkili kişilerin özel bir pull işlemi için imkan yoktur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 fork

sadece Github'a özgü bir terimdir.

bir repository'yi, yine Github sunucusunda kendi user account'umuzun içine klonluyoruz.

## 📌 upstream vs origin

upstream kelime anlamı: akıntı yönüne karşı.

Ikisi de birer "remote" alias'ıdır. bir projeyi klonlayıp, klonladığımız projeyi tekrar locale klonladığımızda; upstream ilk/orijinal projenin remote'udur. origin ise arada olan remote'tur.

upstream projeye göre, bizim projemiz downstream projedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Git patch

Bulunduğumuz branch ile "masterX" arasındaki diff'i alarak "PATCH1.patch" dosyasına yazılıyor:

```sh
git format-patch "masterX" --stdout > "PATCH1.patch"
```

Bu diff'i başka bir makinede apply etmemiz gerekir:

```sh
git apply "PATCH1.patch"
```

## 📌 author vs committer

author kodu yazan kişi, commiter o kodu `Git` DB'ye kaydeden kişidir. patch dosyası ile birine bir şey attığımızda; o patch'i hazırlayan kişi author, patch'i commit'leyen kişi commiter oluyor.
