# 🏭 Factory IO & TIA Portal: Pick and Place Automation (State Machine & Ladder Logic)

Bu proje, **Siemens TIA Portal** ve **PLCSim** kullanılarak geliştirilmiş, **Factory IO** simülasyon ortamındaki *Pick and Place* (Al ve Yerleştir) istasyonunu kontrol eden, mühürleme yerine **Set/Reset ve ara hafıza bitleri (%M)** ile kurgulanmış ardışık bir otomasyon projesidir.

---

## ⚙️ Sistem Mantığı ve Çalışma Senaryosu
1. **Sistem Başlatılması:** Start butonuna basıldığında ana hafıza biti Set edilir.
2. **Konveyör 1 ve Parça Algılama:** Parça sensöre ulaştığında konveyör durur ve çakışma önleyici mantıkla Z ekseni aşağı inerek ürünü kavrar.
3. **X Ekseni Transferi:** Tutucu aktif olduktan sonra mekanik gecikmeyle birlikte ürün 2. konveyöre doğru taşınır.
4. **Bırakma ve Geri Dönüş:** İkinci istasyonda parça bırakılır, eksenler başlangıç konumuna döner.
5. **Sayım ve Çıkış:** Anlık sensör düşmelerini engelleyen ara hafıza bitleri yardımıyla parçalar sayılır ve döngü tamamlanır.

---

## 🖥️ Network Adımları ve Görsel Simülasyon

### Network 1 & 2: Sistem Start / Stop ve Ana Hafıza
* **TIA Portal Kodu:**
 
  ![Network 1 - Start Stop](https://github.com/FurkanCsk/Pick-and-Place-Otomasyonu/blob/main/Sistem%20Start%20Tia%20Portal.png)
* **Factory IO Sahne Görünümü:**
  ![Factory IO - Genel Bakış](https://github.com/FurkanCsk/Pick-and-Place-Otomasyonu/blob/main/Factory%20IO%20Sahne%20G%C3%B6r%C3%BCn%C3%BCm%C3%BC.png)
> **Açıklama:** Start butonuna (`%I0.0`) basıldığında sistem Set/Reset (SR) bloğu üzerinden ana hafıza bitini (`%M10.0`) aktifler.

---

### Network 3 & 4: Konveyör 1 Hareketi ve Z Ekseni Koordinasyonu
* **TIA Portal Kodu:**

  ![Network 3 - Konveyör ve Z Ekseni](https://github.com/FurkanCsk/Pick-and-Place-Otomasyonu/blob/main/Konveyor1%20Hareket%20TIA%20PORTAL.png)
* **Factory IO Sahne Görünümü:**
  ![Factory IO - Konveyör 1 ve Piston](https://github.com/FurkanCsk/Pick-and-Place-Otomasyonu/blob/main/Factory%20IO%20Konveyor1%20ve%20Piston.png)
> **Açıklama:** Ürün tespit sensörü (`%I0.3`) ürünü gördüğünde Konveyör 1 durur ve Z ekseni çıkışının aynı anda set/reset olmasını engelleyen kapalı kontak mantığıyla Z ekseni aşağı inmeye başlar (`%Q0.4`).

---

### Network 5 & 6: Ürün Tutma ve X Ekseni Transferi
* **TIA Portal Kodu:**
  
  ![Network 5 - Tutucu ve X Ekseni](https://github.com/FurkanCsk/Pick-and-Place-Otomasyonu/blob/main/%C3%9Cr%C3%BCn%20Tutma%20ve%20Transfer%20TIA%20PORTAL.png)
* **Factory IO Sahne Görünümü:**
  ![Factory IO - Pick and Place Kolu](https://github.com/FurkanCsk/Pick-and-Place-Otomasyonu/blob/main/Factory%20IO%20Transfer.png)
> **Açıklama:** Tutucu (`%Q0.2`) Set edilip parçayı kavradıktan sonra, zaman rölesi gecikmesiyle X ekseni taşıma hareketi (`%Q0.3`) tetiklenir ve transfer hafızası (`%M10.1`) devreye girer.

---

### Network 7 & 8: Bırakma, Geri Dönüş ve Konveyör 2
* **TIA Portal Kodu:**
  
  ![Network 7 - Bırakma ve Konveyör 2](https://github.com/FurkanCsk/Pick-and-Place-Otomasyonu/blob/main/B%C4%B1rakma%20ve%20Konveyor2%20TIA%20PORTAL.png)
* **Factory IO Sahne Görünümü:**
  ![Factory IO - Konveyör 2 ve Çıkış](https://github.com/FurkanCsk/Pick-and-Place-Otomasyonu/blob/main/Factory%20IO%20B%C4%B1rakma%20ve%20Konveyor2%20.png)
> **Açıklama:** İkinci istasyona varan ürün bırakılır, eksenler evine döner. `Sensor2` ile Konveyör 2 çalıştırılır; anlık sensör kesilmeleri ara hafıza biti (`%M10.2`) ile kararlı hale getirilerek parça sayımı yapılır.

---

### Network 9, 10 & 11: Ürün Sayımı ve Otomatik Durdurma
* **TIA Portal Kodu:**
  
  ![Network 9 - Ürün Sayımı ve Otomatik Durdurma](https://github.com/FurkanCsk/Pick-and-Place-Otomasyonu/blob/main/%C3%9Cr%C3%BCn%20Say%C4%B1m%C4%B1%20ve%20Otomatik%20Durdurma%20TIA%20PORTAL.png)
* **Factory IO Sahne Görünümü:**
  ![Factory IO - Sayım ve Çıkış Bandı](https://github.com/FurkanCsk/Pick-and-Place-Otomasyonu/blob/main/Factory%20IO%20%C3%9Cr%C3%BCn%20Say%C4%B1m%C4%B1.png)
> **Açıklama:** `Sensor3` (`%I0.5`) üzerinden anlık düşmeleri engelleyen ara hafıza bitleriyle (`%M10.2`) sayım (`Counter1`) gerçekleştirilir. Ürün sayısı istenen değere (`== 3`) ulaştığında `Sistemi Durdur` biti (`%M10.3`) tetiklenerek Konveyör 2 güvenli bir şekilde durdurulur.

---

## 🛠️ Kullanılan Teknolojiler ve Araçlar
* **PLC Programlama:** Siemens TIA Portal (Ladder Logic - LAD, Set/Reset Mimarisi)
* **Simülasyon Ortamı:** Factory IO & Siemens S7-PLCSim
* **Sürüm Kontrol:** Git & GitHub
