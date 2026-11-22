---
title: "Pars: 3-DOF Masaüstü Robot Kol"

date: 2026-11-08
summary: "Bipolar moda dönüştürülmüş 28BYJ-48 step motorlar, A4988 sürücüler ve özel 3D baskı mekanik şasi ile geliştirilen açık kaynak masaüstü manipülatör."
weight: 1
pinned: true
hiddenInHomeList: false
tags: ["Robotik", "C++", "SolidWorks", "Step-Motor", "Arduino"]
---

{{< project-devlogs tag="Toygun" >}}


## 1. Sistem Özeti ve Amaç  

Toygun projesi, düşük maliyetli step motorlarla yüksek tekrarlanabilirliğe sahip bir masaüstü robot kol geliştirme ve ardından ROS 2 / MoveIt üzerinde ters kinematik (Inverse Kinematics) testleri yapma amacıyla başlatıldı.

## 2. Donanım ve Sistem Mimarisi (BOM)

| Bileşen | Teknik Özellik |
|---|---|
| Mikrodenetleyici | Arduino Nano (ATmega328P), ileride ROS 2 seri köprüsü |
| Eyleyiciler (3 Eksen) | 3x 28BYJ-48 Step Motor (Bipolar modifikasyonlu) |
| Motor Sürücüler | 3x A4988 Stepper Driver (Vref = 0.08V kalibrasyonlu) |
| Güç Sistemi | 3S 18650 Li-ion Batarya Paketi + LM2596 Regülatör |
| Mekanik Gövde | SolidWorks tasarımı, PLA+ (Ender 3 V3 SE baskı) |

## 3. Kontrol Yazılımı Mimarisi

`src/main.cpp` (PlatformIO / AccelStepper)

```cpp
#include <Arduino.h>
#include <AccelStepper.h>

// 3 Eksen Tanımlamaları (STEP, DIR)
AccelStepper baseStepper(AccelStepper::DRIVER, 2, 5);
AccelStepper shoulderStepper(AccelStepper::DRIVER, 3, 6);
AccelStepper elbowStepper(AccelStepper::DRIVER, 4, 7);

void setup() {
  Serial.begin(115200);
  baseStepper.setMaxSpeed(800);
  baseStepper.setAcceleration(400);

  shoulderStepper.setMaxSpeed(600);
  shoulderStepper.setAcceleration(300);
  Serial.println("Toygun 3-DOF Kontrolcu Hazir.");
}

void loop() {
  baseStepper.run();
  shoulderStepper.run();
}
```

## Kaynak Kodlar






{{< github-card url="https://github.com/ertugrulselvi/toygun-robot-arm" >}}