<!-- workshop-header -->
<img width="1347" height="127" alt="Coding Thailand 2026 header" src="https://github.com/user-attachments/assets/ba5cf267-f460-4fb0-b69b-c461ae061a3b" />

# 🎯 Track A — Motion Classification

> **Sensor:** Modulino Movement (IMU 6-axis)
> **Output:** Modulino Pixels + Buzzer
> **Difficulty:** ⭐ ง่ายที่สุด เหมาะกับมือใหม่

Track นี้เหมาะกับทีมที่อยากเริ่มไว ทดลองไว และเหลือเวลา iterate V2 ได้เยอะที่สุดในวันแรก

---

## 🎓 ทำไมเลือก Track A

- ✅ ของถึงมือทุกทีม (อยู่ใน Modulino kit)
- ✅ เก็บข้อมูลเร็ว (2 วินาที/sample)
- ✅ Train เร็ว เห็นผลใน 5 นาที
- ✅ ไม่ต้อง USB Hub / Webcam
- ✅ Real-world applications เยอะ

---

## 💡 โจทย์แนะนำตามระดับ

### 🟢 Basic: Tilt Detector

**Classes:** นิ่ง / เอียงซ้าย / เอียงขวา

**Use case:** ใส่ที่กล่อง — แจ้งเตือนถ้าถูกเอียงโดยไม่ได้รับอนุญาต

### 🟡 Intermediate: Activity Tracker

**Classes:** ยืน / เดิน / วิ่ง / นั่ง

**Use case:** ใส่ในกระเป๋า/เข็มขัด — track activity ของผู้ใช้

### 🔴 Advanced: Fall Detector

**Classes:** ยืน / เดิน / นั่ง / ล้ม

**Use case:** สำหรับผู้สูงอายุ — alert ครอบครัวเมื่อล้ม

---

## 🛠️ Lab Steps

## ✅ ก่อนเริ่ม track นี้ ทีมควรมี

- repo ทีมพร้อมใช้งานและมี commit แรกแล้ว
- Worksheet W1 ที่นิยาม class ชัดพอให้เริ่มเก็บข้อมูล
- UNO Q + Modulino Movement + Pixels + Buzzer ต่อครบและเปิดติด

### Step 1: Hardware Setup (5 นาที)

```
UNO Q ─Qwiic─→ Movement ─Qwiic─→ Pixels ─Qwiic─→ Buzzer
```

⚠️ Order สำคัญ — sensor ก่อน, output หลัง

ทดสอบ:
```bash
# ใน Arduino App Lab → Sample → Movement → Read Accelerometer
# ควรเห็น accel X, Y, Z update real-time
```

---

### Step 2: Edge Impulse Project (5 นาที)

1. ไปที่ https://studio.edgeimpulse.com
2. **Create new project** → ตั้งชื่อ `team-XX-motion`
3. Project info → Target = **Arduino UNO Q**
4. Connect device:
   - UNO Q terminal: `edge-impulse-linux`
   - Login + เลือก project

จบ step นี้แล้ว ทีมควรเห็น device ของตัวเองใน Edge Impulse Studio

---

### Step 3: Data Collection (45-60 นาที)

#### Recording Settings:

| Parameter | ค่า |
|---|---|
| Sample length | **2000 ms** |
| Sample frequency | **100 Hz** |
| Sensor | Accelerometer (X, Y, Z) |

#### เก็บตาม class ใน Worksheet W1

**Best practices:**
- เก็บจากหลายคนในทีม (กัน bias 1 person)
- เก็บใน 2-3 environment ต่างกัน
- เก็บใน 2 ความเร็ว (ช้า/เร็ว)
- ใส่ Modulino ที่จุดต่างๆ (มือ/เอว/ขา) — เลือก 1-2 จุด

#### Target จำนวน:

| ระดับ | Samples/class | Total time |
|---|---|---|
| Basic (3 classes) | 50 | ~30 นาที |
| Intermediate (4 classes) | 60 | ~50 นาที |
| Advanced (4 classes) | 80 | ~60 นาที |

ก่อนไป step 4 ให้เช็กสั้น ๆ ว่าแต่ละ class มีจำนวนใกล้กันและทีมไม่ได้เก็บจากคนเดียวหรือความเร็วเดียวทั้งหมด

---

### Step 4: Create Impulse (10 นาที)

ใน Edge Impulse:

1. **Impulse design**:
   - Input: Time series data (window 2000ms, increase 200ms)
   - Processing: **Spectral Analysis**
   - Learning: **Classification (NN)**
   - Output: ตามจำนวน class

2. **Spectral features** → Generate features
   - ดู scatter plot — classes ควรแยกออกจากกันชัดเจน
   - ถ้าซ้อนกันมาก → ต้องเก็บข้อมูลใหม่หรือแก้ class design

3. **NN Classifier**:
   ```
   Model size:      nano (2.4M)
   Training cycles: 30
   Learning rate:   0.0005
   Processor:       GPU
   ```
   กด Save & train (รอ 1-2 นาที)

เป้าหมายของ step นี้ไม่ใช่ accuracy สูงสุดทันที แต่คือการได้ V1 ที่พอทดสอบบนเคสจริงได้

---

### Step 5: Validate (5 นาที)

ดู Confusion Matrix:

```
ดี:                    ไม่ดี:
     A   B   C              A   B   C
A  [40] 1   2          A  [25] 10  8
B   2 [38] 4           B    8 [22] 12
C   1   3 [42]         C    7 11  [25]

→ Diagonal สูง       → ต้อง iterate
```

**ถ้า accuracy < 70% → กลับไปดู data**
**ถ้า accuracy > 95% → ระวัง overfitting**

อย่าเพิ่งดีใจกับตัวเลขใน Studio ถ้ายังไม่ได้ลองกับคนหรือท่าที่ไม่ได้อยู่ใน data train

---

### Step 6: Deploy to UNO Q (10 นาที)

1. **Deployment** → เลือก **Arduino UNO Q**
2. Model size: nano
3. **Build** → Download
4. ใน Arduino App Lab:
   - Import model
   - เลือก Bricks:
     - Input: Modulino Movement
     - Output: Modulino Pixels + Buzzer
   - Run

5. **Test inference** บนบอร์ดจริง

ถ้ารันได้แต่ตอบช้า หรือทำนายไม่ตรงกับสิ่งที่เพิ่งทำ ให้ถือว่ายังไม่จบ step นี้

---

### Step 7: 10-Case Testing (40 นาที)

ใช้ Worksheet W3

**Test cases ที่ต้องมี:**

| # | Type | ตัวอย่าง |
|---|---|---|
| 1-5 | Normal | ทำท่าตามที่ train |
| 6-7 | Boundary | ระหว่าง 2 classes |
| 8 | Speed variation | เร็วผิดปกติ |
| 9 | Person variation | คนที่ไม่ได้เก็บข้อมูล |
| 10 | Adversarial | ท่าใกล้เคียงแต่ไม่ใช่ |

---

### Step 8: Iterate to V2 (40 นาที)

จาก Prediction Log V1:
1. ระบุ class ที่ผิดบ่อย
2. เก็บข้อมูลเพิ่ม (โดยเฉพาะ variation ที่ขาด)
3. Re-train เป็น V2
4. Test เคสเดิม + เคสใหม่

📝 เขียนเปรียบเทียบใน `docs/v1-vs-v2.md`

V2 ที่ดีไม่จำเป็นต้อง accuracy สูงสุดเสมอ แต่ควรอธิบายได้ว่าปรับอะไรและเพราะอะไร

---

## 🎨 Output Configuration Examples

### Basic — Tilt Detector

```cpp
// Pixels
String predictedClass = topLabel;

if (predictedClass == "นิ่ง")        setColor(0, 255, 0);   // Green
if (predictedClass == "เอียงซ้าย")    setColor(255, 255, 0); // Yellow
if (predictedClass == "เอียงขวา")    setColor(255, 0, 0);   // Red
```

### Advanced — Fall Detector

```cpp
String predictedClass = topLabel;

if (predictedClass == "ล้ม") {
  // ทุก Pixel แดง + buzzer beep 3 ครั้ง
  setAllPixels(255, 0, 0);
  for (int i = 0; i < 3; i++) {
    buzzer.tone(1000, 200);
    delay(300);
  }
}
```

---

## 🚀 ไอเดียต่อยอด

ต่อยอดเป็น prototype จริงได้หลายแบบ:

1. **Fall Detector Wristband** — ติดที่ข้อมือ + LCD แสดงสถานะ
2. **Fitness Counter** — นับท่าซิทอัพ/วิดพื้น/สควอท
3. **Anti-theft Sensor** — ติดบนของมีค่า + alarm
4. **Smart Walking Stick** — ไม้เท้าผู้สูงอายุ + alert ครอบครัว
5. **Vibration Anomaly** — ติดบนเครื่องจักร detect ผิดปกติ

ถ้าอยากทำแนวโรงงานหรือ predictive maintenance ดูตัวอย่างแบบลงมือทำได้ที่ [example-case-vibration-monitor.md](example-case-vibration-monitor.md)

---

## ⚠️ Common Pitfalls

1. **เก็บข้อมูลจากคนเดียว** → V1 ใช้กับคนอื่นไม่ได้
   → **แก้:** สลับเก็บจากทุกคนในทีม
2. **ทำท่าเร็วมาก** → AI งง
   → **แก้:** เก็บทั้งช้าและเร็ว
3. **ไม่มี "noise class"** → AI จัดทุกอย่างเป็น 1 ใน 3
   → **แก้:** เพิ่ม class "unknown" หรือใช้ threshold confidence
4. **Window size 2000ms ใหญ่ไป** → ตอบสนองช้า
   → **แก้:** ลด window แต่เพิ่ม overlap

---

## 📚 References

- [Modulino Movement docs](https://store.arduino.cc/products/modulino-movement)
- [Edge Impulse Continuous Motion docs](https://docs.edgeimpulse.com/docs/tutorials/end-to-end-tutorials/continuous-motion-recognition)
- [Arduino UNO Q Edge Impulse setup](https://docs.edgeimpulse.com/hardware/boards/arduino-uno-q)
