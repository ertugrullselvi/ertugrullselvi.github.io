---
title: "Toygun: 17. Uluslararası MEB Robot Yarışması"
date: 2025-05-24
draft: false
tags: ["Robotik", "Elektronik", "Python", "OpenCV", "Görüntü İşleme", "UART", "Bus Servo", "MEB Robot", "Endüstriyel Robotik Kol"]
categories: ["Yarışma Projeleri"]
---


{{< figure src="/images/toygun-hunter/yarısma_alanı.jpg" alt="Toygun ve Hunter derece anı" >}}







---

## 1. Proje Özeti ve Görev Konsepti

**Toygun**, 17. Uluslararası MEB Robot Yarışması Robot Kol Kategorisi için tasarlayıp ürettiğimiz görüntü işleme destekli, otonom rota planlaması yapabilen 4 eksenli robotik kol platformudur. 

Robotun önünde **3x3'lük bir matris** bulunur ve bu matrise **4 mavi, 4 kırmızı küp** rastgele dizilmiştir (1 alan boş kalır). Robotun görevi; bu küplerin renklerini ve konumlarını otonom şekilde tespit edip, sağ tarafında bulunan kırmızı kutu ile sol tarafında bulunan mavi kutuya en kısa sürede ve sıfır hatayla tasnif etmektir.

---

## 2. Ekip Yapılanması ve Mentörlük Süreci

Geçtiğimiz dönemlerde elde ettiğimiz tecrübelerin üzerine, Robot Kol kategorisinin yarışmaya yeniden eklenmesiyle birlikte atölyede ciddi bir yapılanmaya gittik. Ekip mevcudumuzu 2 kişiden 6 kişiye çıkardık.

Buradaki amacımız sadece robot yapıp derece almak değil, sürdürülebilir bir robotik kültürü oluşturmaktı. Deneyimli iki yarışmacı olarak iki ayrı robot kol takımının (**Toygun** ve **Hunter**) başına geçtik ve yanımıza birer yeni yarışmacı alarak onları yetiştirmeye başladık. Eş zamanlı olarak ekibin kalan 2 üyesiyle de bir Tasarla-Çalıştır takımı kurarak süreç boyunca onlara mentörlük sağladık. 



{{< figure src="/images/toygun-hunter/mentorluk.jpg" alt="Hunter yarışma sahası" >}}

---

## 3. Donanım ve Mekanik Kararları 

Geliştirme sürecinin ilk ve en kritik kararı motor seçimiydi. Standart PWM ile çalışan RC servoların aksine projeye **25 kg/cm tork üreten Waveshare ST3020 Bus Servolar** alarak başladık.

- **Bus Servo Avantajı:** Bu motorlar klasik PWM yerine **UART haberleşme protokolü** kullanır. Tek bir hatta seri bağlanabilirler, anlık konum/tork geri bildirimi verirler ve kablo karmaşasını neredeyse sıfıra indirirler. 
- **Mekanik Tasarım:** Motor kararıyla birlikte SolidWorks üzerinde sıfırdan bir gövde mimarisi çizdik. Taşıyıcı ve hareketli parçaları 3D yazıcılarımızdan basarken, şasinin ana yükünü taşıyan gövde elemanlarını Dakota malzemeden hassasça kesip montajladık.
- **End-Effector (Tutucu):** Daha önceki şampiyon robotlarımız Dorlion ve Proteo'da rüştünü ispatlamış olan **8 bar pnömatik tutucu** sistemini bu robotlara da entegre ettik. Servo tutucuların açma-kapama sırasındaki zaman kayıplarını pnömatik tutucu ile sıfıra indirdik.

---

## 4. Algılama ve Kontrol Mimarisi: Sorular ve İterasyonlar

Tasarım ve üretim sürerken yazılım ve kontrol tarafında yanıtlamamız gereken kritik sorular vardı:

1. **Cisimlerin rengi ve konumu nasıl algılanacak?**
   Renk sensörü mü yoksa görüntü işleme mi sorusuna, esneklik sağlaması açısından görüntü işleme cevabını verdik.
2. **Kamera entegrasyonu nasıl yapılacak?**
   İlk olarak **ESP32-CAM** ile basit bir yapı kurmayı deenedik. Fakat esp 32 görüntü için yeterince güçlü bir seçim değildi. Bizde hiç vakitkaybetmeden **USB kamera modüllerine** geçtik.
3. **Sistem nerede işlenecek (PC vs. Raspberry Pi)?**
   USB kameraya geçince kontrolör kartı kararı gündeme geldi. Raspberry Pi'nin kapalı yapısı ve müdahale sınırları yerine, yarışma sahasında anlık kod müdahalesi ve yüksek işlem gücü rahatlığı sağladığı için sistemin kalbine **Bilgisayar (PC)** mimarisini oturttuk.

---

## 5. Yazılım, Görev Algoritması 

Donanım toplandıktan sonra yazılım aşamasına geçtik. Python ortamında ilk olarak UART üzerinden Bus Servoların kütüphanelerini kullanamyı öğrenip motorlara tam hakimiyet sağladık.

- **Hareket Planlama:** Görüntü işlemeye geçmeden önce robotun pozisyonlar arası hareket yazılımını tamamladık. Robota küpün koordinatlarını manuel verdiğimizde, hedefe gideceği en optimum rotayı kendisi hesaplayan bir otonom hareket algoritması oluşturduk.
- **Görüntü İşleme (OpenCV):** Kamera robota monte edildikten sonra OpenCV ile renk filtreleme, matris üzerindeki boş alanları atlama ve küp koordinatlarını matris indekslerine eşleme algoritmalarını yazdık.
- **Süre Optimize Çalışmaları:** Tüm sistem entegre bir şekilde ilk çalıştığında 8 küpü ayıklama süresi **35 saniye** civarındaydı. Yarışma standartları için bu süre oldukça uzundu. Yarışmaya son 1 hafta kala başlayan hızlandırma maratonumuz, yarışma alanına gittiğimizde de gece geç saatlere kadar kesintisiz devam etti. 
- Algoritmadaki bekleme sürelerini sıfırlayarak, motor hız profillerini ve pnömatik zamanlamaları optimize ederek bu süreyi **17 saniyeye** kadar indirmeyi başardık.

---

### 🛠️ Teknik Özellikler Tablosu

| Bileşen | Açıklama / Teknik Özellik |
|---|---|
| **Ana Kontrolcü** | Bilgisayar (PC) |
| **Eyleyiciler (4 Eksen)** | 4 x Waveshare ST3020 Bus Servo |
| **Sürücü** | Waveshare Bus Servo Driver |
| **Tutucu (End-Effector)** | 8 Bar Pnömatik Silindir Sistemi |
| **Tutucu Kontrolcüsü** | Arduino Nano (ATmega328P) |
| **Valf Sistemi** | 5 Yollu 2 Konumlu Selenoid Valf |
| **Güç Sistemi** | 3S LiPo Batarya |
| **Kullanılan Malzemeler** | Dakota ve PLA (3D Baskı) |

---

## 🏆 Başarılar

* **17. Uluslararası MEB Robot Yarışması (Kayseri / 21-24 Mayıs 2025):**
  * **Toygun** — Robot Kol Kategorisi **Dünya 2.liği** 🥈
  * **Hunter** — Robot Kol Kategorisi **Dünya 3.liği** 🥉


<div class="img-grid">

{{< figure src="/images/toygun-hunter/ekip_foto.jpg" alt="Robot Kol ve Tasarla Çalıştır Ekibi" >}}

{{< figure src="/images/toygun-hunter/robotlar.jpeg" alt="Robot Kol ve Tasarla Çalıştır Ekibi" >}}

</div>

---

