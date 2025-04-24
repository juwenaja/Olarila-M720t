![Hackintosh Banner](https://olarila.com/uploads/monthly_2024_08/Newlogo44.png.84e2ad7fbe4222bd820c1c52bdb39b2f.png)

# Hackintosh - Version Coffee Lake (Lenovo M720t)

---

## 📎 Shortcut Navigasi

- 💾 [Spesifikasi Komputer](#-spesifikasi-perangkat)
- 🔧 [Edit config.plist (Boot-arg, SMBIOS, dsb)](#-penting-untuk-diperhatikan)
🧰 [Setelah Instalasi & Tools Tambahan](#-tambahan-configplist-setup)
- 🛠️ [Panduan Install Pertama & Flash USB dengan EFI](#-struktur-file-efi-seharusnya)
- 💻 [Cara Mengganti config.plist](#-cara-mengganti-configplist-via-ubuntu-atau-vm-ubuntu)

---
Proyek ini menyediakan konfigurasi EFI khusus untuk proses instalasi macOS pada perangkat **Lenovo ThinkCentre M720t** dengan prosesor Intel Coffee Lake, berdasarkan konfigurasi dari Olarila yang telah disempurnakan ulang.

> ⚠️ Catatan: Proyek ini hanya menyediakan EFI dan file konfigurasi untuk proses booting. Kamu tetap harus menyediakan sendiri file image macOS secara terpisah.

---

## 🧩 Spesifikasi Perangkat

- **Model**: Lenovo ThinkCentre M720t  
- **Prosesor**: Intel Core i3-8100  
- **iGPU**: Intel UHD Graphics 630  
- **Monitor**: ThinkVision S22e-19 (22" LCD)  
- **RAM**: DELTA RGB DDR4 8x2GB (Dual Channel)  

---

## 🚀 Fitur

- Konfigurasi EFI khusus untuk Coffee Lake  
- Kompatibel dengan perangkat keras M720t  
- Dukungan grafis dasar dan boot-arg khusus untuk instalasi pertama kali  

---

## ⚙️ Instalasi

### Clone Repo Ini

```bash
git clone https://github.com/juwenaja/Olarila-M720t.git
cd Olarila-M720t
```

Salin folder `EFI` ke partisi `EFI` pada USB installer macOS kamu.

---

## 📌 Penting untuk Diperhatikan

- Untuk proses instalasi **pertama kali**, kamu **wajib menambahkan boot-arg** berikut:

```
-igfxvesa
```

Ini bisa dilakukan dengan mengedit file `config.split` terlebih dahulu.  
### Edit config.split(https://github.com/juwenaja/Olarila-M720t/blob/main/EFI/OC/config.split)

- Setelah proses instalasi selesai, kamu bisa menghapus boot-arg tersebut dan menggunakan konfigurasi default.
- Untuk file image macOS, silakan unduh dari situs resmi Apple atau referensi dari Olarila.

---

## 🧰 Tambahan config.plist setup

Untuk penyesuaian lebih lanjut pada konfigurasi EFI setelah proses instalasi, kamu bisa memperhatikan hal-hal berikut:

📌 **Boot-args**:  
Lihat referensi lengkap tentang boot-args di dokumentasi OpenCore:  
🔗 https://dortania.github.io/OpenCore-Install-Guide/config.plist/coffee-lake.html#nvram

📌 **Device Properties**:  
Untuk mengatur GPU, Audio, atau USB Mapping:  
🔗 https://dortania.github.io/OpenCore-Install-Guide/config.plist/coffee-lake.html#deviceproperties

---

### 🔧 Tools Tambahan

🛠️ **ProperTree** (untuk mengedit `config.plist`):  
🔗 https://github.com/corpnewt/ProperTree

🛠️ **GenSMBIOS** (untuk generate SMBIOS):  
🔗 https://github.com/corpnewt/GenSMBIOS

| SMBIOS  | Hardware Support |
| --------| ------------- |
| iMac19,1  | For Mojave and newer  |
| iMac18,3  | For High Sierra and older  |


---

## 📁 Struktur File EFI (Seharusnya)

THIS_REPO/  
 ├── EFI/  
 │    ├── BOOT/  
 │    └── OC/  
 │         ├── ACPI/  
 │         ├── Drivers/  
 │         ├── Kexts/  
 │         ├── config.plist 

Perhatian ⚠️  
Ingat, ini hanya repo EFI, yang hanya menyediakan kext-kext yang disempurnakan dan `config.plist` yang dapat digunakan.

Untuk install macOS Olarila pertama kali, klik di sini:  
🔗 https://olarila.com/topic/6278-olarila-vanilla-images-macos-installer/  
Saya pribadi menggunakan **macOS Sonoma v14.7.5** sebagai installernya.

> Semua versi macOS bisa digunakan, pastikan hanya kamu mencocokkan versinya dengan konfigurasi EFI ini.

🧠 Kamu akan membutuhkan:
- Flashdisk minimal **32GB**
- Tool flashing bernama **Elena Batcher** (untuk pengguna Windows)

Setelah kamu flash menggunakan Elena Batcher, USB tersebut:
- Akan terbaca di `diskpart`
- Tapi tidak muncul di File Explorer Windows  
Hal ini **normal**, karena partisi macOS tidak dikenali oleh Windows, tapi bisa dibaca di Ubuntu/macOS.

---

## 💻 Cara Mengganti `config.plist` via Ubuntu (atau VM Ubuntu)

### 1. Buka Terminal, lalu cek lokasi USB installer:
> pastikan flashdrive yang sudah di install macOS itu terhubung.

```bash
sudo lsblk
```

Contoh output:

```
sdb      8:16   1  29.3G  0 disk
├─sdb1   8:17   1   200M  0 part
└─sdb2   8:18   1   16GB  0 part
```

> `sdb1` adalah partisi EFI (200MB), tempat kita akan salin `config.plist`.

> `sdb2` adalah partisi Installer MacOS (16GB), ini biarkan saja.

---

### 2. Buat folder mount point

```bash
sudo mkdir -p /mnt/usb-efi
```

📁 *Perintah ini membuat folder tujuan untuk me-mount partisi EFI di USB installer.*

---

### 3. Mount partisi EFI dari USB

```bash
sudo mount /dev/sdb1 /mnt/usb-efi
```

📦 *Perintah ini akan menempelkan partisi `sdb1` (EFI) ke folder `/mnt/usb-efi` agar bisa diakses.*

---

### 4. Salin file config.plist dari Downloads ke EFI/OC

```bash
sudo cp ~/Downloads/config.plist /mnt/usb-efi/EFI/OC/config.plist
```

📄 *Perintah ini akan mengganti file `config.plist` lama dengan yang baru dari folder Downloads.*

---

### 5. Ubah permission agar bisa dibaca oleh sistem bootloader

```bash
sudo chmod 644 /mnt/usb-efi/EFI/OC/config.plist
```

🔐 *Mengatur permission file agar dapat dibaca oleh OpenCore saat booting.*

---

### 6. Lepas (unmount) USB EFI

```bash
sudo umount /mnt/usb-efi
```

✅ *Selesai! USB sudah siap digunakan untuk proses instalasi dengan konfigurasi baru.*

---  


## 🤝 Kredit

### **`OLARILA`**
<a
href="https://github.com/OlarilaHackintosh"><img src="https://github.com/OlarilaHackintosh.png" width="110" height="110" alt="OlarilaHackintosh"/></a>

### **`DORTANIA`**
<a
href="https://github.com/dortania"><img src="https://github.com/dortania.png" width="110" height="110" alt="Dortania"/></a>
![OpenCore Banner](https://raw.githubusercontent.com/acidanthera/OpenCorePkg/master/Docs/Logos/OpenCore_with_text_Small.png)


---

## 📬 Kontak & Sosial Media

[![Gmail](https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:justw3nn@gmail.com)
[![Instagram](https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white)](https://instagram.com/juwennnn_)
[![Website](https://img.shields.io/badge/Website-0078D4?style=for-the-badge&logo=google-chrome&logoColor=white)](https://w3nn.ksp-natamaromandirijaya.or.id)