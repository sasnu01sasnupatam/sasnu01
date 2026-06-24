<!-- workshop-header -->
<img width="1347" height="127" alt="Coding Thailand 2026 header" src="https://github.com/user-attachments/assets/ba5cf267-f460-4fb0-b69b-c461ae061a3b" />

# 🎬 Anchor Demo — Gesture Wand

> ลองครบ flow สั้น ๆ ภายใน **15 นาที** จาก Setup → Demo

demo นี้ช่วยให้เห็นปลายทางของทั้งวันแบบเร็ว ๆ ว่า Data → Train → Deploy → Test ทำงานต่อกันยังไง

---

## 🎯 โจทย์ของ Demo

**Gesture Wand** — ทำให้ Modulino Movement กลายเป็นไม้กายสิทธิ์ที่รู้จักท่าทาง

**3 Classes:**
1. 🌀 **Circle** — หมุนเป็นวงกลม
2. ⚡ **Z-shape** — วาดเป็นรูป Z
3. 😴 **Still** — นิ่งๆ

**Output:**
- Modulino Pixels เปลี่ยนสีตามคลาส
- Circle → 🔵 น้ำเงิน
- Z-shape → 🟢 เขียว
- Still → ⚪ ขาว

---

## 📦 อุปกรณ์ที่ต้องเตรียม

- [ ] Arduino UNO Q
- [ ] Modulino Movement
- [ ] Modulino Pixels
- [ ] Qwiic cable ×2
- [ ] USB-C cable + Power
- [ ] Laptop + Arduino App Lab
- [ ] บัญชี Edge Impulse (ใช้ project ตั้งไว้แล้ว)

---

## ⏱️ ลองทำตามนี้ (15 นาที)

### นาที 0:00-1:30 — ต่อบอร์ดและดูภาพรวม

ถ้าเวลาไม่มาก ใช้ project ที่เตรียม dataset ไว้แล้วได้ และเก็บสดเพิ่มแค่ 5-10 samples เพื่อให้เห็น flow จริง

เป้าหมายของช่วงนี้คือให้เห็นว่าอุปกรณ์ทั้งหมดต่อไม่ยาก และ input/output ของงานนี้คืออะไร

ต่อ UNO Q → Modulino Movement → Modulino Pixels (daisy chain)
ใส่ในมือ ทำท่าเหมือนเตรียมร่ายเวทมนต์

---

### นาที 1:30-4:00 — Data Collection (Live)

เปิด Edge Impulse Studio → project "Gesture-Wand-Demo"

1. Go to **Data acquisition** tab
2. Click **Start sampling**
3. Label: `circle`, Length: 2000ms
4. Press Start → ทำท่าวงกลม → รอ data upload
5. Repeat 5 ครั้ง (ทำเร็วๆ ให้ดูง่าย)

สิ่งที่ควรสังเกตระหว่างเก็บ:
- วงกลมแต่ละครั้งไม่เหมือนกัน 100% นี่คือ diversity ของ data
- ถ้าเก็บแต่วงกลมขนาดเดียวหรือคนเดียว model จะจำ pattern แคบเกินไป

ทำซ้ำกับ `z-shape` (5 ครั้ง) และ `still` (5 ครั้ง)

ถ้าจะเดินต่อให้ครบใน 15 นาที ให้สลับไปใช้ project ที่เตรียม data ไว้แล้วประมาณ 50 samples ต่อ class

---

### นาที 4:00-7:00 — Training

1. Go to **Create Impulse**
2. Add processing block: **Spectral Analysis** (สำหรับ IMU)
3. Add learning block: **Classification**
4. Save Impulse
5. Spectral features → Generate features
6. NN Classifier:
   - Model size: **nano (2.4M)** ★ ย้ำให้นักเรียนเห็น
   - Training processor: **GPU**
   - Epochs: 30
7. Save & Train (รอ ~1-2 นาที)

ระหว่างรอ train ลองคาดเดาก่อนว่า class ไหนน่าจะสับสนกันมากที่สุด ส่วนใหญ่ `circle` กับ `z-shape` จะใกล้กันกว่ากลุ่ม `still`

---

### นาที 7:00-9:00 — อ่านผล Training

แสดง training output:

```
Confusion Matrix:
              circle  z-shape  still
circle         [45]      4      1     ← 90% accuracy
z-shape         3      [42]     5     ← 84% accuracy
still           0       2     [48]    ← 96% accuracy

Validation accuracy: 90.0%
```

สิ่งที่ควรอ่านให้ออก:
- `still` มักง่ายที่สุด เพราะ pattern ค่อนข้างนิ่ง
- `circle` กับ `z-shape` สับสนกันได้ เป็นเรื่องปกติของ V1
- ถ้ายังสับสนกันอยู่ แปลว่ารอบถัดไปต้องเก็บ data เพิ่มหรือทำ class definition ให้ชัดขึ้น

---

### นาที 9:00-11:00 — Deploy to UNO Q

1. Go to **Deployment**
2. เลือก **Arduino UNO Q**
3. Model size: nano
4. Build → รอ ~30 วินาที
5. Download

ใน Arduino App Lab:
1. Import model
2. Configure bricks:
   - Input: Modulino Movement
   - Output: Modulino Pixels (RGB)
3. Run on board

---

### นาที 11:00-14:00 — Live Demo 🎉

**ทำท่าทีละท่า:**
1. นิ่งๆ → Pixels เป็นสีขาว ✓
2. วาดวงกลม → Pixels เป็นสีน้ำเงิน ✓
3. วาด Z → Pixels เป็นสีเขียว ✓

**สาธิต Edge cases (สำคัญ!):**
4. ทำท่าครึ่งๆ กลางๆ → ดูว่า model confused
5. ทำท่าเร็วมาก → ดูว่า model handle ได้ไหม
6. ส่งให้ผู้เข้าร่วม 1 คนลอง → "อันนี้ใช้ได้ไหม?"

ตรงนี้คือจุดสำคัญของ demo: อย่าดูแค่เคสที่ตั้งใจให้ชนะ ให้ลองเคสหลุด ๆ ด้วย จะเห็นเลยว่าทำไม data จากหลายคนและหลายสภาพถึงสำคัญ

---

### นาที 14:00-15:00 — สรุปสิ่งที่ได้จาก Demo

pipeline ที่ควรจำให้ได้คือ:

```text
Data → Train → Deploy → Test → Iterate
```

หลังจบ demo นี้ แต่ละทีมจะเอา flow เดียวกันไปใช้กับโจทย์ของตัวเอง ไม่จำเป็นต้องทำ gesture wand เหมือนกันทุกทีม

→ Transition ไป Slide 11 (Pipeline Summary)

---

## 🛠️ ถ้าอยากลองแบบไวขึ้น

ถ้าอยากเห็นภาพรวมก่อนโดยไม่รอ train นาน:

1. ใช้ project ที่มี dataset 50/class และ train ไว้แล้ว
2. ดูผล Feature Explorer + Confusion Matrix ก่อน
3. ข้ามไปขั้น Deploy และ Live test บน UNO Q
4. ค่อยย้อนกลับมาเก็บ data เองอีกหนึ่งรอบเพื่อเข้าใจ process เต็ม

---

## 🎨 Sample Code Reference

### Arduino sketch สำหรับ deploy (ใน App Lab จะ generate ให้)

```cpp
#include <Modulino.h>
#include <gesture-wand_inferencing.h>  // จาก Edge Impulse export

ModulinoMovement movement;
ModulinoPixels pixels;

void setup() {
  Modulino.begin();
  movement.begin();
  pixels.begin();
}

void loop() {
  float features[NUM_FEATURES];
  // อ่าน accel จาก Modulino Movement
  collectFeatures(features);

  // Run inference
  ei_impulse_result_t result;
  run_classifier(features, &result);

  // ตอบสนอง output ตาม class
  String topClass = result.classification[0].label;
  if (topClass == "circle") {
    setPixelsColor(0, 0, 255);     // Blue
  } else if (topClass == "z-shape") {
    setPixelsColor(0, 255, 0);     // Green
  } else {
    setPixelsColor(255, 255, 255); // White
  }

  delay(100);
}
```

> ⚠️ Code นี้เป็น pseudocode สำหรับ reference — code จริงให้ใช้ที่ Arduino App Lab generate

---

## 📊 Expected Outcome

หลังจบ demo นี้ ควรเข้าใจ:

- ✅ Pipeline 4 ขั้นของ Edge AI
- ✅ ทำไม Edge Impulse ช่วยให้เร็วขึ้น
- ✅ UNO Q + Modulino เสียบใช้ง่าย
- ✅ Training มี config ที่ต้องระวัง (model size!)
- ✅ Test cases เผยจุดอ่อนของ model
- ✅ "ฉันก็ทำได้!"

---

## 🚨 ถ้าลองแล้วติดตรงไหน

| ปัญหา | วิธีแก้ตอน demo |
|---|---|
| UNO Q ไม่ boot | เช็กสายไฟ, เช็ก USB-C, ถอดเสียบใหม่ แล้วค่อย restart App Lab |
| Wi-Fi ห้องช้า | ใช้ hotspot มือถือ |
| Edge Impulse load ไม่ขึ้น | refresh หน้า Studio หรือสลับไปใช้ project ที่เตรียมไว้ |
| Inference accuracy ต่ำตอน live | มองว่าเป็นสัญญาณว่า data ยังไม่พอ แล้วจดว่าจะเพิ่ม variation อะไร |
| Modulino Pixels ไม่ติด | เช็ก Qwiic cable, brick order และลองใช้ Serial log ช่วยดูผลทำนายก่อน |
