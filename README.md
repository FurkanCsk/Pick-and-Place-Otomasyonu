# 🏭 Factory IO & TIA Portal: Pick and Place Automation (State Machine & Ladder Logic)

Bu proje, **Siemens TIA Portal** ve **PLCSim** kullanılarak geliştirilmiş, **Factory IO** simülasyon ortamındaki *Pick and Place* (Al ve Yerleştir) istasyonunu kontrol eden ardışık bir endüstriyel otomasyon projesidir.

---

## ⚙️ Sistem Mantığı ve Çalışma Senaryosu
1. **Sistem Başlatılması:** Start butonuna basıldığında sistem mühürleme devresi üzerinden aktifleşir.
2. **Konveyör 1 ve Parça Algılama:** Parça sensöre ulaştığında konveyör durur ve Z ekseni aşağı inerek ürünü kavrar.
3. **X Ekseni Transferi:** Tutucu aktif olduktan sonra mekanik gecikmeyle birlikte ürün 2. konveyöre doğru taşınır.
4. **Bırakma ve Geri Dönüş:** İkinci istasyonda parça bırakılır, eksenler başlangıç konumuna döner.
5. **Sayım ve Çıkış:** Parçalar sayılır ve sensör kararlılığı hafıza bitleriyle sağlanarak döngü tamamlanır.

---

## 🖥️ Network Adımları ve Görsel Simülasyon

### Network 1 & 2: Sistem Start / Stop ve Ana Hafıza
* **TIA Portal Kodu:**
  ![Network 1 - Start Stop](network1_start.png)
* **Factory IO Sahne Görünümü:**
  ![Factory IO - Genel Bakış](factory_overview.png)
> **Açıklama:** Start butonuna (`%I0.0`) basıldığında sistem ana hafıza biti (`%M10.0`) üzerinden mühürlenerek çalışmaya başlar.

---

### Network 3 & 4: Konveyör 1 Hareketi ve Z Ekseni Koordinasyonu
* **TIA Portal Kodu:**
  ![Network 3 - Konveyör ve Z Ekseni](network3_konveyor.png)
* **Factory IO Sahne Görünümü:**
  ![Factory IO - Konveyör 1 ve Piston](factory_conveyor1.png)
> **Açıklama:** Ürün tespit sensörü (`%I0.3`) ürünü gördüğünde Konveyör 1 durur ve çakışmaları önleyen mantıksal kontaklarla Z ekseni aşağı inmeye başlar (`%Q0.4`).

---

### Network 5 & 6: Ürün Tutma ve X Ekseni Transferi
* **TIA Portal Kodu:**
  ![Network 5 - Tutucu ve X Ekseni](network5_tutucu.png)
* **Factory IO Sahne Görünümü:**
  ![Factory IO - Pick and Place Kolu](factory_pickplace.png)
> **Açıklama:** Tutucu (`%Q0.2`) aktifleşip parçayı kavradıktan sonra, 2 saniyelik mekanik gecikmeyle X ekseni taşıma hareketi (`%Q0.3`) tetiklenir.

---

### Network 7 & 8: Bırakma, Geri Dönüş ve Konveyör 2
* **TIA Portal Kodu:**
  ![Network 7 - Bırakma ve Konveyör 2](network7_birakma.png)
* **Factory IO Sahne Görünümü:**
  ![Factory IO - Konveyör 2 ve Çıkış](factory_conveyor2.png)
> **Açıklama:** İkinci istasyona varan ürün bırakılır, kollar evine döner. `Sensor2` ve `Sensor3` yardımıyla Konveyör 2 çalıştırılır ve parça sayımı yapılır.

---

## 🛠️ Kullanılan Teknolojiler ve Araçlar
* **PLC Programlama:** Siemens TIA Portal (Ladder Logic - LAD)
* **Simülasyon Ortamı:** Factory IO & Siemens S7-PLCSim
* **Sürüm Kontrol:** Git & GitHub
