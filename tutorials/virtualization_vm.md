# VIRTUALIZATION VM

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Hypervisor (⟷ virtual machine monitor ⟷ VMM)

`VM` yönetimini çalıştıran yazılımlara verilen genel isimdir.

`Hypervisor`, run edildiği katmana göre 2'ye ayrılır:

### 📌📌 Type-1 hypervisor (⟷ native hypervisor ⟷ bare-metal hypervisor)

`VM`'de çalışan `OS`, donanım ile direk temastadır.

Örnekler:
- `Microsoft` `Hyper-V`
- `Xen`
- `VMware ESXi (⟷ old-name:VMware ESX)`

### 📌📌 Type-2 hypervisor (⟷ hosted hypervisor)

`VM`'de çalışan `OS`, donanım ile direk temasta değildir. Arada `host` `OS` vardır.

örnekler:
- `VirtualBox`
- `QEmu (⟷ Quick Emulator)`

## 📌 KVM (⟷ Kernel-based Virtual Machine)

- özel bir isimdir.
- `KVM`, `Linux` modülüdür.
- `Hypervisor`'dür.
- `type-1`'dir çünkü donanıma direk erişir, ama `Linux` üzerinde bir yaızlım olarak kurulduğu için `type-2` de diyebiliriz.

## 📌 Parallels Desktop (⟷ Parallels Desktop for Mac)

`MacOS` için `Hypervisor`.

## 📌 yazılım sanallaştırma

`Docker` gibi uygulamalara verilen genel isimdir.

## 📌 Intel VT-x vs AMD-V

Sanallaştırma teknolojileri için `CPU`'ların yapmış olduğu özel teknoloji isimleri.

## 📌 Nested virtualization

`VM` içinde `VM` çalıştırmaktır.

## 📌 ThinApp

- `VMware` firmasının uygulaması.
- yazılım sanallaştırması yapar.

## 📌 Emülatör (⟷ öykünücü ⟷ emulator) vs simulator (⟷ simülatör)

`emulator`; çalıştırılacak gerçek executable'ı run eder. bunu yaparken, her step'i (en ufak çalıştırma birimini) o anda gerçek çalışan native `CPU`'nun anlayacağı dile çevirir.

`simülatör` ise davranışı taklit ettiririr. Örneğin; `Android` emulator'ünü `TV` olarak açtığımızda, `TV` ünitesi vamrmış gibi, `TV`'nin kumandası varmış gibi, `API`'lerin fake olarak kullanılabilmesini sağlar.

## 📌 VirtualBox OSE (⟷ VirtualBox open source edition)

`Virtualbox` 4üncü sürümden önce kapalı kaynak ve açık kaynaklı sürüm olarak dağıtılıyordu. 4 üncü sürüm ile birlikte ikisi birleştirildi. ve lisans gerektiren ek özellikler; kapalı kaynak eklentiler olarak indirilebiliyor.

## 📌 VMware

`VMware`'in birçok farklı ürünü mevcut. Tüm ürünler kapalı kaynaktır.

## 📌 VMware Workstation (⟷ VMware Workstation Pro)

`VM` çalıştırıcısıdır. lisans gerektiren versiyon.

## 📌 VMware Workstation Player (⟷ VMware Player)

Kişisel kullanım için ücretiz sunuluyor. `VMware Workstation`'dan farklı bir executable olup, `PRO`'ya göre daha az özellik içeriyor.

## 📌 VMware Fusion

`MacOS`'in altyapısı farklı olduğundan, `MacOS` versiyonu farklı build üretilmiştir. Build o kadar farklı ki, farklı bir ürün ismi ile dağıtılmaktadır.

## 📌 Virtual Machine Manager (⟷ virt-manager) vs Gnome-Boxes vs Cockpit

tümü birbirine alternatif `GUI` uygulamalarıdır.

`virt-manager` özel bir uygulama ismidir.

`gnome-boxes` aşırı az özellik içerir.

`virt-manager` native desktop `GUI`'si var ve artık geliştirilmiyor. onun yerine `Web UI` ile hizmet veren `Cockpit` geliştirilmeye başlandı.

## 📌 Vagrant

`Virtualbox`, `VMWare` gibi uygulamaların içerisinde olan makinaları, `gitops` tarzı yönetimini sağlayan açık kaynaklı bir komut satırı uygulamasıdır.

örnek işlemler:

- multi `hypervisor`'ü tek bir `API`'den yönetebilmemizi sağlar.
- `OS`'u hazır bulut image'lerden indirip açar.
- `OS` run, stop eder.
- `OS` içinde bir user tanımlar.
- bu user'ın içine `SSH` komutu ile girip, istediğimizi çalıştırabilmemizi sağlar.

## 📌 Vagrant nasıl guest OS'un içine girip komut çalıştırabiliyor

## 📌📌 SSH

`OS` imaj dosyaları özeldir ve içinde default `vagrant` isminde user ve `SSH` server yüklü gelir ve `TCP` porttan default olarak dinlemeye başlar. `Vagrant` buraya istek atar.

## 📌📌 WinRM (⟷ Windows Remote Management)

sadece `MS Windows` guest için çalışır.

`MS Windows` açıldığında bir `TCP` portundan komut çalıştırılmasına izin verir.

## 📌📌 Diğerleri

Bu alt başlıktakileri `Vagrant`'ın destekleyip desteklemediğini bilmiyorum.

Ama bu alt başlıktakilerde host ile guest arasında bir veri paylaşımı yapılabilmesini sağlıyor.

### 📌📌📌 guest tools

#### 📌📌📌📌 PCI device

guest'e bir `PCI` kablosu takılır. `VM` manager buradan zaten guest'te yürklü olan guest tools yazılımına bilgi yollar ve alır.

#### 📌📌📌📌 Backdoor I/O port

`CPU`'ya bilgi atarken ve alırken özel komutlar üzerinden data alışverişi sağlanır.

### 📌📌📌 Cloud/Remote provisioning (cloud-init, metadata, vs.)

basit bir text config dosyası devoloper tarafında doldurulur. bu config dosyası okunarak sistem kurulur veya boot olurken okunur.

`Cloud-init` veya `user-data` veya `metadata`: `OS` boot olurken çalışır.

`Preseed` veya `Kickstart`: `OS` install olurken çalışır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 VM vs app container

container, `OS` çekirdeği içermez. ortak kullanır. `VM` içerir.

- `VM` fully portable'dır. fakat container değildir. container, `OS`'u sanallaştırmıyor. var olan `OS` çekirdeğini kullanıyor. bu sebeple başka makinaya atılan container, aşağıdaki durumlarda farklı tepkiler verebilir:

  - `host` `OS` çekirdeği farklı sürüm ise
  - `host` `OS` çekirdeği farklı ise
  - `host` `OS` çekirdeğin bazı özellikleri aktif ise, başka makinada aynı özellikler aktif olmayabilir (örnek `Linux` kernel module yüklü olmayabilir)
  - donanım farklı ise (özellikle donanıma bağlı kodlarda) (Bu durum `VM` içinde geçerli)

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 VM disk formatları

| extension      | native-support | support                     | açıklama                                                                                                                                             |
|----------------|----------------|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|
| `VMDK`         | `VMWare`       | `Virtualbox`, `QEMU`, `KVM` |                                                                                                                                                      |
| `VDI`          | `VirtualBox`   | `QEMU`, `KVM`               | Genel olarak `VDI` ve `WDMK` arasında önemli farklar mevcut değil.                                                                                   |
| `raw` or `img` | `QEMU`, `KVM`  |                             | hiçbir özellik barındırmıyor. sanal diskte ne varsa direk burada binary olarak tutuluyor. format header dahi barındırmıyor. en iyi performans bunda. |
| `cow`          | `QEMU`, `KVM`  |                             |                                                                                                                                                      |
| `qcow`         | `QEMU/KVM`     | `VirtualBox`                | `cow`'un yeni sürümü.                                                                                                                                |
| `qcow2`        | `QEMU/KVM`     |                             | `qcow`'un yeni sürümü.                                                                                                                               |
| `vhdx`         | `Hyper-V`      | `VirtualBox`                | `vhd`'nin yeni sürümü.                                                                                                                               |
| `vhd`          | `Hyper-V`      | `VirtualBox`                |                                                                                                                                                      |
| `cloop`        | `QEMU`, `KVM`  |                             | özel amaçlı kullanılan bir format.                                                                                                                   |
| `qed`          | `QEMU`, `KVM`  | `VirtualBox`                | `qcow2`'nin özellikleri silinerek performans kazandırılmaya çalışılıyor. fakat proje durduruldu.                                                     |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
