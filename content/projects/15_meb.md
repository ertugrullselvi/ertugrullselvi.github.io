---
title: "Dorlion & Proteo: 15. Uluslararası Meb Robot Yarışması"
date: 2023-09-09
draft: false
tags: ["Robotik", "Elektronik", "Arduino", "Pnömatik", "Gölge Robot", "MEB Robot", "Endüstriyel Robotik Kol"]
categories: ["Yarışma Projeleri"]
---

<div class="img-grid">

{{< figure src="/images/dorlion-proteo/dorlion_plaket.jpg" alt="Dorlion plaketli fuar" >}}

{{< figure src="/images/dorlion-proteo/dorlion_fuar_arka.jpg" alt="Dorlion fuar" >}}

{{< figure src="/images/dorlion-proteo/dorlion_fuar.jpg" alt="Dorlion fuar arka" >}}

{{< figure src="/images/dorlion-proteo/proteo_yarisma.jpg" alt="Proteo yarışma" >}}

</div>

---

## 1. Proje Özeti ve Amaç

Dorlion & Proteo, 15. Uluslararası MEB Robot Yarışması için geliştirilmiş **gölge robot (master-slave)** tekniğiyle çalışan 4 eksenli robotik kol platformlarıdır. 

Sistemin temel amacı; kullanıcı tarafından birebir eş zamanlı olarak yönlendirilen gölge robot aracılığıyla 10 adet farklı geometrideki cismi, en kısa sürede ve hatasız bir şekilde hedef yuvalarına yerleştirmektir.



---

## 2. Projenin Geliştirilme Süreci

Dorlion ve Proteo robotları, daha önceden okulumuzun eski yarışmacı ekipleri tarafından geliştirilmiş bir altyapıya sahipti. Ancak robotlarda mekanik/donanımsal eksikler bulunmaktaydı. 

Takımımızla birlikte sistem üzerindeki eksikleri tespit edip yeniden yapılandırdık. Hazırlık ve yarışma sürecinde defalarca yanan motorlara, donanımsal aksaklıklara ve saha baskısına rağmen tüm bu sorunları aşarak robotları sapa sağlam sahaya çıkardık. Sonuç olarak **Dünya 1.liği (Dorlion)** ve **Dünya 2.liği (Proteo)** derecelerini elde etmeyi başardık.

---

## 3. Donanım ve Sistem Mimarisi

Sistem, görsellerde yer alan şeffaf **gölge robot** vasıtasıyla kontrol edilir:

* **Gölge Kontrolör:** Ana robot kolların boyut olarak küçültülmüş bir ölçeğidir. Üzerinde konum verisi okumak için modifiyeli potansiyometreli/servolar kullanılır.
* **Sinyal İşleme & Filtreleme:** Gölge robottan okunan analitik konum verileri **Arduino Nano** tarafından alınır. Kullanıcı el titreşimlerini ve anlık parazitleri engellemek amacıyla özel yazılımsal filtrelerden geçirilerek ana eyleyicilere yumuşak hareket verisi olarak aktarılır.
* **Pnömatik Tutucu Sistem (End-Effector):** Standart robot kollardaki yavaş servo tutucuların aksine, Dorlion & Proteo **8 bar yüksek basınç ile çalışan pnömatik tutucu** mimarisine sahiptir. Bu sayede tutma/bırakma işlemleri servo mekanizmalara kıyasla **10 kata kadar daha hızlı ve yüksek verimle** gerçekleşir.

---

### 🛠️ Teknik Özellikler Tablosu

| Bileşen | Açıklama / Teknik Özellik |
|---|---|
| **Kontrolcü** | Arduino Nano (ATmega328P) |
| **Eyleyiciler (4 Eksen)** | 3 x 25kg Yüksek Torklu Servo Motor |
| **Geri Bildirim / Sensör** | Modifiyeli Servolar (Gölge Kontrolör) |
| **Tutucu (End-Effector)** | 8 Bar Pnömatik Silindir & Valf Sistemi |
| **Güç Sistemi** | 3S LiPo + Yüksek Akımlı Voltaj Regülatörleri |
| **Mekanik Gövde** | SolidWorks CAD Tasarımı, Dakota, Pleksi, Delrin (POM / Asetal) |

---

## 🏆 Başarılar

* **15. Uluslararası MEB Robot Yarışması (Bursa / 6-9 Eylül 2023):**
  * **Dorlion** — Robot Kol Kategorisi **Dünya 1.liği** 🥇
  * **Proteo** — Robot Kol Kategorisi **Dünya 2.liği** 🥈