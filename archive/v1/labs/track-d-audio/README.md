<!-- workshop-header -->
<img width="1347" height="127" alt="Coding Thailand 2026 header" src="https://github.com/user-attachments/assets/ba5cf267-f460-4fb0-b69b-c461ae061a3b" />

# 🎯 Track D — Audio Classification

> **Sensor:** USB Microphone (Linux side)
> **Output:** Modulino Pixels + Buzzer + LED Matrix
> **Difficulty:** ⭐⭐⭐ Advanced (sensitive to noise)

Track นี้เหมาะกับทีมที่อยากทำงานกับเสียงและพร้อมจัดการเรื่อง noise อย่างจริงจัง

---

## 🎓 ทำไมเลือก Track D

- ✅ Audio Edge AI กำลังมาแรง
- ✅ Edge Impulse มี keyword spotting template ให้
- ✅ Real-world applications เยอะ (medical, industrial)

⚠️ **ต้องมี:** USB Microphone — เช็คก่อนเลือก track นี้

ถ้าทีมยังไม่มี mic หรือหามุมเงียบไม่ได้จริง Track นี้จะเสี่ยงกว่าที่เห็นในคำอธิบายเบื้องต้น

---

## 💡 โจทย์แนะนำตามระดับ

### 🟢 Basic: Sound Type Classifier

**Classes:** ปรบมือ / ดีดนิ้ว / เงียบ (3 classes)

### 🟡 Intermediate: Cough Detector

**Classes:** ไอ / จาม / พูด / เงียบ (4 classes)

### 🔴 Advanced: Custom Keyword Spotting

**Classes:** "Hey Arduino" / "Stop" / "Help" / Background noise

---

## 🛠️ Lab Steps

## ✅ ก่อนเริ่ม track นี้ ทีมควรมี

- repo ทีมพร้อมใช้งานและมี commit แรกแล้ว
- W1 ที่ class แยกกันด้วย pattern เสียงจริง ไม่ใช่คำอธิบายกว้างเกินไป
- แผนคร่าว ๆ ว่าจะเก็บ background/noise class ยังไง

### Step 1: Hardware Setup (10 นาที)

```
            ┌─→ USB Microphone
UNO Q ─Hub─┤
            └─→ USB Power

UNO Q ─Qwiic─→ Pixels ─Qwiic─→ Buzzer
```

ทดสอบ mic:
```bash
# SSH เข้า UNO Q
arecord -l    # list audio devices
arecord -d 5 test.wav   # record 5 sec
aplay test.wav          # ฟัง
```

---

### Step 2: Edge Impulse Project (5 นาที)

1. https://studio.edgeimpulse.com → New Project
2. ตั้งชื่อ `team-XX-audio`
3. Target = **Arduino UNO Q**
4. Project info → Input = **Audio**

จบ step นี้แล้ว ทีมควรพร้อมเริ่มเก็บเสียงจาก device ที่เลือกโดยไม่ต้องกลับมาแก้ project setting ใหม่

---

### Step 3: Data Collection (60 นาที)

#### Recording Settings:

| Parameter | ค่า |
|---|---|
| Sample length | **1000 ms (1 วินาที)** |
| Sample frequency | **16,000 Hz (16kHz)** |
| Channels | Mono |

⚠️ Sample rate สูงกว่านี้จะใหญ่เกิน UNO Q

#### Connect Mic to Edge Impulse:

**Option A:** UNO Q เป็น device
```bash
# บน UNO Q
edge-impulse-linux
# เลือก mic input
```

**Option B:** มือถือเป็น device (ง่ายกว่า)
- Studio → Connect new device → Use your mobile phone

#### Target จำนวน:

| Class | Samples (ของจริง) | Background/Noise |
|---|---|---|
| Class 1 | 50 | - |
| Class 2 | 50 | - |
| Class 3 | 50 | - |
| Background (ห้ามขาด!) | 100 | เงียบ + เสียงพื้นหลัง |

💡 **Class "Background" สำคัญมาก** — ป้องกัน false positive

ถ้าไม่มี background class หรือมีน้อยเกินไป model จะพยายามยัดทุกเสียงให้เป็นหนึ่งใน class หลักของทีม

---

### Step 4: Create Impulse (10 นาที)

1. **Impulse design**:
   - Input: Audio (1000ms window)
   - Processing: **MFE** (Mel-Filterbank Energy) — แนะนำสำหรับเสียงทั่วไป
     - หรือ **MFCC** สำหรับ keyword spotting
   - Learning: **Classification (NN)**

2. **MFE features** → Generate features
   - ดู spectrogram — เสียงต่าง class ควรดูต่างกัน
   - ถ้าเหมือนกันหมด → recording quality มีปัญหา

3. **NN Classifier**:
   ```
   Model size:      nano (2.4M)
   Training cycles: 100
   Learning rate:   0.005
   Processor:       GPU
   ```

⚠️ Audio model ต้องการ epochs เยอะกว่า (100 vs 30 ของ motion)

เป้าหมายของ V1 คือดูว่าเสียงแต่ละ class แยกกันได้ในสภาพห้องจริงหรือไม่ ไม่ใช่ดู accuracy ตอน train อย่างเดียว

---

### Step 5: Validate (5 นาที)

ดู Confusion Matrix + **F1 score**
- F1 สำคัญกว่า accuracy สำหรับ audio (เพราะ background เยอะ)
- ถ้า background class ขโมย sample อื่น → เพิ่ม sample class อื่น

ถ้าค่า F1 ต่ำแต่ accuracy ดูดี ให้เชื่อ F1 ก่อน โดยเฉพาะเวลามี background class เยอะ

---

### Step 6: Deploy to UNO Q (15 นาที)

#### Method 1: Arduino App Lab

1. Deployment → Arduino UNO Q
2. Model size: nano
3. Build → Download
4. App Lab:
   - Input: USB Microphone
   - Output: Pixels + Buzzer
   - Run

#### Method 2: Linux runner

```bash
# บน UNO Q
edge-impulse-linux-runner
```

ถ้าต้องการไฟล์ `.eim` แยกสำหรับ offline run ให้ export/download จาก Edge Impulse ก่อน แล้วค่อยรันตามเอกสารของ runner รุ่นที่ใช้อยู่

### Step 6.5: Prototype Code Starter

หลัง quick test ผ่าน ให้ map class ไป output โดยระวัง false positive เป็นพิเศษ เช่น `ไอ → alert`, `background → idle`, `confidence ต่ำ → unknown`

เปิดตัวอย่าง logic ได้ที่ [../common/prototype-code-starters.md](../common/prototype-code-starters.md#5-track-d--audio-mapping)

---

### Step 7: 10-Case Testing (40 นาที)

**Test cases ที่ต้องมี:**

| # | Type | ตัวอย่าง (กรณี Cough Detector) |
|---|---|---|
| 1-3 | Normal | ไอชัดเจน 3 ครั้ง |
| 4-5 | Variation | ไอเบา / ไอแรง |
| 6 | Different person | คนที่ไม่ได้เก็บข้อมูล |
| 7 | Boundary | พูด แล้วไอแทรก |
| 8 | Noisy environment | มี TV/พัดลม background |
| 9 | Far distance | ไอจากระยะ 2 เมตร |
| 10 | Negative | เสียงคล้ายไอ (กระแอม) |

---

### Step 8: Iterate to V2 (40 นาที)

จาก V1 analysis:
1. ถ้า "ไอ" สับสนกับ "พูด" → เก็บ sample ทั้ง 2 เพิ่มในห้องเดียวกัน
2. ถ้า false positive จาก noise → เพิ่ม background class
3. ลอง MFCC แทน MFE (สำหรับ keyword spotting)
4. Re-train V2

การ iterate ของ audio มักได้ผลจากการเก็บเสียงเพิ่มในสภาพแวดล้อมจริง มากกว่าการปรับ parameter อย่างเดียว

---

## 🎨 Output Configuration Examples

### Cough Detector

```cpp
String predictedClass = topLabel;

if (predictedClass == "ไอ") {
  setPixels(255, 0, 0);   // Red
  buzzer.tone(800, 200);
  // Log timestamp เพิ่มภายหลังได้
} else if (predictedClass == "จาม") {
  setPixels(255, 165, 0); // Orange
} else if (predictedClass == "พูด") {
  setPixels(0, 255, 0);   // Green
} else {
  setPixels(0, 0, 0);     // Off
}
```

---

## 🚀 ไอเดียต่อยอด

1. **Cough Tracker** — นับครั้งไอใน 24 ชม. — สำหรับผู้ป่วยทางเดินหายใจ
2. **Smart Speaker DIY** — voice command เปิด/ปิดไฟในห้อง
3. **Industrial Listener** — ตรวจเสียงเครื่องจักรผิดปกติ
4. **Baby Cry Detector** — แจ้งเตือนแม่
5. **Bird ID** — แยกชนิดนกจากเสียงร้อง

---

## ⚠️ Common Pitfalls

### Problem 1: ห้อง workshop เสียงดัง → recording เสีย
**แก้:** หามุมเงียบ / record ใกล้ๆ mic / เก็บ background noise ของห้อง workshop เป็น class

### Problem 2: False positive เยอะ
**สาเหตุ:** Background class น้อยเกินไป
**แก้:** เพิ่ม background ให้ ≥2x class อื่น

### Problem 3: ใช้บนคนอื่นไม่ได้
**สาเหตุ:** เก็บเสียงคนเดียว → AI จำเสียงคนนั้น
**แก้:** เก็บจากหลายคน หลายเพศ หลายอายุ

### Problem 4: Sample rate ผิด
**ถ้าสูงเกิน:** ไม่ลง UNO Q
**ถ้าต่ำเกิน:** เสียงสูงจับไม่ได้
**แก้:** 16kHz ดีที่สุดสำหรับ UNO Q

### Problem 5: Reverb/Echo ในห้อง
**สาเหตุ:** ห้องว่างมี echo มาก
**แก้:** Record ในห้องที่มีของเยอะ / ใช้ mic แบบ directional

---

## 📚 References

- [Edge Impulse Keyword Spotting](https://docs.edgeimpulse.com/docs/tutorials/end-to-end-tutorials/keyword-spotting)
- [MFCC vs MFE](https://docs.edgeimpulse.com/docs/edge-impulse-studio/processing-blocks/audio-mfcc)
- [UNO Q with "Hey Arduino"](https://www.edgeimpulse.com/blog/announcing-support-for-the-arduino-uno-q/)
