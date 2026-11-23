---
title: "Esata2024: 16. Uluslararası MEB Robot Yarışması"
date: 2024-10-19
draft: false
tags: ["Robotik", "Elektronik", "Arduino", "Yazılım", "MEB Robot", "Tasarla-Çalıştır"]
categories: ["Yarışma Projeleri"]
---

<div class="img-grid">

{{< figure src="/images/Esata2024/robot-yatay.jpg" alt="atölye çalışmaları" >}}

{{< figure src="/images/Esata2024/otel.jpg" alt="otel çalışmaları" >}}

{{< figure src="/images/Esata2024/tasarla-calistir.jpg" alt="tasarla-çalıştır anı" >}}

{{< figure src="/images/Esata2024/zafer.webp" alt="zafer pozu" >}}

</div>

---

## 1. Proje Özeti ve Amaç

Esata2024, 16. Uluslararası MEB Robot Yarışması için geliştirilmiş bir Tasarla-Çalıştır robotudur. Bu kategorinin amacı, yarışmacıların belirli bir süre içerisinde ilk defa gördükleri sürpriz bir pisti tamamlayacak bir robot tasarlayıp kodlamalarıdır.

---

## 2. Kategori Bilgisi

Kategorinin ilk aşamasında katılımcılarla kuralları ve yarışma anında kullanılacak malzeme listesini içeren bir doküman paylaşılır. Yarışmacılar edindikleri bu bilgiler ışığında malzemeleri kullanmayı öğrenir ve olası senaryolar ile pratik yapar. 

Yarışmanın ilk aşamasında yarışmacılar bir temel yeterlilik testine sokulur. Bu testten en yüksek puanı alan ilk 40 takım bir sonraki Tasarla-Çalıştır aşamasına geçmeye hak kazanır. 

Tasarla-Çalıştır aşamasında yarışmacılar, kendilerine verilen malzemeler ile ilk defa gördükleri bir pisti bitirecek robotu dışarıdan izole bir ortamda yapmaya çalışır. Bu aşamada robotlarını zamanında bitirip teslim eden ekipler, bir sonraki gün yarışma pistinde robotlarını yarıştırmaya hak kazanır.

---

## 3. Çalışma ve Yarışma Süreci

Tasarla-Çalıştır kategorisine katılmamızın ana nedeni, o yıl Robot Kol kategorisinin kapatılmasıydı. 2024 yılını boş geçmemek adına yarışmaya 1 ay kala Tasarla-Çalıştır kategorisi için çalışmalara başladık. 

Yoğun geçen bir ayın ardından, çalışmalarımıza son gece de dahil olmak üzere otel odamızda devam ettik. Temel yeterlilik testinde 140 takım arasından **11. olduk** ve Tasarla-Çalıştır aşamasına geçmeye hak kazandık. İkinci aşamada dışarıdan izole bir 8 saatlik karantina sürecinin ardından robotumuzu eksiksiz teslim etmeyi başardık. Üçüncü aşamada ise robotumuzu pistte yarıştırarak **Dünya Birinciliğimizi** elde ettik.

---

## 4. Donanım ve Sistem Mimarisi

Esata2024; üzerinde bulunan çizgi izleme sensörü ve ultrasonik mesafe sensörü ile pist üzerindeki çizgileri ve duvarları izleyerek görevini yerine getirir. 

* **Çizgi İzleme:** Çizgi izleme sensörü zemindeki kontrast farkını okuyarak PID algoritması ile robotu daima çizgi merkezinde tutmayı hedefler.
* **Duvar & Mesafe Algılama:** Ultrasonik mesafe sensörü, üzerine monte edildiği servo motor vasıtasıyla 5 farklı açıda (0°, 45°, 90°, 135°, 180°) mesafe taraması yapar. Böylece robotun duvarlar arasındaki konumunu ve açısal sapmalarını tespit ederek hassas navigasyon sağlar.

---

### 🛠️ Teknik Özellikler Tablosu

| Bileşen | Açıklama / Teknik Özellik |
|---|---|
| **Mikrodenetleyici** | Arduino Mega (ATmega2560) |
| **Sürüş Motorları** | 2x 250 RPM Redüktörlü DC Motor |
| **Motor Sürücü** | L298P Çift Kanallı Motor Sürücü Kartı |
| **Eyleyiciler (Mekanizma)** | SG90 Mikro Servo Motor |
| **Sensörler** | QTR-8A Çizgi Sensör Dizilimi & HC-SR04 Ultrasonik Mesafe Sensörü |
| **Güç Sistemi** | 2S LiPo Batarya Bloğu |
| **Mekanik Gövde & Şasi** | Lazer Kesim Pleksi (Akrilik) Gövde |
| **Denge & Destek** | 2x Sarhoş Teker |

---

## 💻 Örnek Kod (Arduino C++)

```cpp
#include <Servo.h> 
#include <QTRSensors.h>  

QTRSensors cizgiSensoru;
Servo myservo; 
unsigned int sensorDegerleri[6];

#define MotorLi 13
#define MotorLg 2

#define MotorRi 4
#define MotorRg 12

int MotorRP = 3;
int MotorLP = 5;

#define trigPin 8 
#define echoPin 7 

// Değişkenler
int mesafeF = 0;
int mesafeR = 0;
int mesafeL = 0;
int mesafeL45 = 0;
int mesafeR45 = 0;
unsigned int PwmHiz = 200;
long gecenSure;

float Kp = 0.07, Kd = 0.58;
int oncekiHata = 0;
unsigned int Hiz = 150;

int level = 0;

void setup() {
  cizgiSensoru.setTypeAnalog(); 
  cizgiSensoru.setSensorPins((const byte[]){A5,A4,A3,A2,A1,A0}, 6);    

  pinMode(MotorRi, OUTPUT);
  pinMode(MotorRg, OUTPUT);
  pinMode(MotorLi, OUTPUT);
  pinMode(MotorLg, OUTPUT);
  pinMode(MotorRP, OUTPUT);
  pinMode(MotorLP, OUTPUT);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  
  myservo.attach(10);  
  delay(2000);

  for(int i=0; i<300; i++) {
    cizgiSensoru.calibrate();
  }
}

void loop() {
  cizgiSensoru.read(sensorDegerleri);

  if((sensorDegerleri[0] < 100) && (sensorDegerleri[1] < 100) && (sensorDegerleri[2] < 100) && (sensorDegerleri[3] < 100) && (sensorDegerleri[4] >= 300) && (sensorDegerleri[5] >= 300)) {
    kosesol();
  }
  else if((sensorDegerleri[0] >= 300) && (sensorDegerleri[1] >= 300) && (sensorDegerleri[2] < 100) && (sensorDegerleri[3] < 100) && (sensorDegerleri[4] < 100) && (sensorDegerleri[5] < 100)) {
    kosesag();
  }
  else if((sensorDegerleri[0] >= 300) && (sensorDegerleri[1] >= 300) && (sensorDegerleri[2] >= 300) && (sensorDegerleri[3] >= 300) && (sensorDegerleri[4] >= 300) && (sensorDegerleri[5] >= 300)) {
    dur();
    duvar_izle();
  }
  else {
    cizgi_izle();
  }
}

void cizgi_izle() {
  unsigned int cizgiPozisyonu = cizgiSensoru.readLineWhite(sensorDegerleri);
  int hata = 2500 - cizgiPozisyonu;

  float P = Kp * hata;  
  float D = Kd * (hata - oncekiHata);
  oncekiHata = hata;
 
  int PID_cikis = P + D;

  int solMotPwmDegeri = Hiz - PID_cikis;
  int sagMotPwmDegeri = Hiz + PID_cikis;

  if(solMotPwmDegeri > 255) solMotPwmDegeri = 255;
  else if(solMotPwmDegeri < 0) solMotPwmDegeri = 0;

  if(sagMotPwmDegeri > 255) sagMotPwmDegeri = 255;
  else if(sagMotPwmDegeri < 0) sagMotPwmDegeri = 0;

  digitalWrite(MotorRi, 1);
  analogWrite(MotorRP, sagMotPwmDegeri);
  
  digitalWrite(MotorLi, 1);
  analogWrite(MotorLP, solMotPwmDegeri);
}

void duvar_izle() {
  solaBak();
  sol45Bak();
  delay(150);
  oneBak();
  sag45Bak();
  delay(150);
  sagaBak();

  if(mesafeF < 15) {
    if(mesafeL > 30) {
      hafifSol();
      delay(1100);
    }
    else if(mesafeR > 30) {
      hafifSag();
      delay(1100);
    }
    else {
      dur();
      delay(1000);
    }
  }
  else if(mesafeL < 12) {
    hafifSag();
    delay(300);
  }
  else if(mesafeR < 12) {
    hafifSol();
    delay(300);
  }
  else if(mesafeL45 < 20) {
    hafifSag();
    delay(200);
  }
  else if(mesafeR45 < 20) {
    hafifSol();
    delay(200);
  }
  else {
    ilerle();
    delay(200);
  }
  dur();
}

int sagaBak() {
  myservo.write(0);
  delay(100);
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  gecenSure = pulseIn(echoPin, HIGH);
  mesafeR = (gecenSure / 2) / 29.1;
}

int oneBak() {
  myservo.write(90);
  delay(100);
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  gecenSure = pulseIn(echoPin, HIGH);
  mesafeF = (gecenSure / 2) / 29.1;
}

int oneBak2() {
  myservo.write(90);
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  gecenSure = pulseIn(echoPin, HIGH);
  mesafeF = (gecenSure / 2) / 29.1;
}

int solaBak() {
  myservo.write(180);
  delay(300);
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  gecenSure = pulseIn(echoPin, HIGH);
  mesafeL = (gecenSure / 2) / 29.1;
}

int sag45Bak() {
  myservo.write(45);
  delay(100);
  digitalWrite(trigPin, LOW);  
  delayMicroseconds(2);  
  digitalWrite(trigPin, HIGH);  
  delayMicroseconds(10);  
  digitalWrite(trigPin, LOW);  
  gecenSure = pulseIn(echoPin, HIGH);  
  mesafeR45 = (gecenSure / 2) / 29.1;  
}

int sol45Bak() {
  myservo.write(135);
  delay(100);
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  gecenSure = pulseIn(echoPin, HIGH);
  mesafeL45 = (gecenSure / 2) / 29.1;
}

void dur() {
  digitalWrite(MotorRi, 0);
  digitalWrite(MotorRg, 0);
  digitalWrite(MotorLi, 0);
  digitalWrite(MotorLg, 0);
}
  
void ilerle() {
  digitalWrite(MotorRi, 1);
  digitalWrite(MotorRg, 0);
  analogWrite(MotorRP, PwmHiz);
  analogWrite(MotorLP, PwmHiz);
  digitalWrite(MotorLi, 1);
  digitalWrite(MotorLg, 0); 
}

void hafifSag() {
  digitalWrite(MotorRi, 0);
  digitalWrite(MotorRg, 0);
  analogWrite(MotorRP, 0);
  analogWrite(MotorLP, PwmHiz);
  digitalWrite(MotorLi, 1);
  digitalWrite(MotorLg, 0);
}

void hafifSol() {
  digitalWrite(MotorRi, 1);
  digitalWrite(MotorRg, 0);
  analogWrite(MotorRP, PwmHiz);
  analogWrite(MotorLP, 0);
  digitalWrite(MotorLi, 0);
  digitalWrite(MotorLg, 0);
}

void kosesag() {
  dur();
  delay(200);
  ilerle();
  delay(160);
  hafifSag();
  delay(1190);
  dur();
  delay(200);
  cizgi_izle();
}

void kosesol() {
  dur();
  delay(200);
  ilerle();
  delay(160);
  hafifSol();
  delay(1075);
  dur();
  delay(200);
  cizgi_izle();
}
```

---

## 🏆 Başarılar

* **16. Uluslararası MEB Robot Yarışması (Erzurum / 16-19 Ekim 2024):**
  * **Esata2024** — Tasarla-Çalıştır Kategorisi **Dünya 1.liği** 🥇
