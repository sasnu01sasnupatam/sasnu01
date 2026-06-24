<!-- workshop-header -->
<img width="1347" height="127" alt="Coding Thailand 2026 header" src="https://github.com/user-attachments/assets/ba5cf267-f460-4fb0-b69b-c461ae061a3b" />

# 🎯 Track B — Vision Classification

> **Sensor:** USB Webcam (Linux side)
> **Output:** Modulino Pixels + LED Matrix + Buzzer
> **Difficulty:** ⭐⭐ ปานกลาง

Track นี้เหมาะกับทีมที่อยากทำ computer vision และพร้อมเก็บภาพให้หลากหลายพอสำหรับใช้จริง

---

## 🎓 ทำไมเลือก Track B

- ✅ ผลลัพธ์เห็นชัด ตื่นเต้น
- ✅ Real-world applications มากที่สุด
- ✅ เชื่อม Computer Vision concept

⚠️ **ต้องมี:** USB Webcam + USB Hub (powered) — เช็คก่อนเลือก track นี้

ถ้าทีมยังไม่มีกล้องหรือ powered hub ครบ ให้เปลี่ยน track ตั้งแต่ต้นจะคุ้มกว่าฝืนแก้ hardware กลางวัน

---

## 💡 โจทย์แนะนำตามระดับ

### 🟢 Basic: Object Sort

**Classes:** ขวด / กระป๋อง / กระดาษ

**Use case:** Smart recycle bin

### 🟡 Intermediate: Mask Detector

**Classes:** ใส่หน้ากากถูก / ไม่ถูก / ไม่ใส่

**Use case:** ระบบเข้าออกพื้นที่ที่ต้องสวมหน้ากาก

### 🔴 Advanced: Fruit Ripeness

**Classes:** ดิบ / สุกพอดี / สุกเกิน

**Use case:** Quality control ในตลาด/โรงงาน

---

## 🛠️ Lab Steps

## ✅ ก่อนเริ่ม track นี้ ทีมควรมี

- repo ทีมพร้อมใช้งานและมี commit แรกแล้ว
- worksheet W1 ที่กำหนด class ชัดเจนและไม่ซ้อนกันเกินไป
- USB Webcam หรือวิธีเก็บภาพทางเลือกที่ทีมตกลงกันแล้ว

### Step 1: Hardware Setup (10 นาที)

```
                ┌─→ USB Webcam
UNO Q ─USB Hub─┤
                └─→ USB Power
                
UNO Q ─Qwiic─→ Pixels ─Qwiic─→ Buzzer
```

**สำคัญ:** USB Hub ต้องเป็น powered hub ไม่งั้น webcam ไม่ทำงาน

ทดสอบ webcam:
```bash
# SSH เข้า UNO Q
ls /dev/video*
# ควรเห็น /dev/video0
```

---

### Step 2: Edge Impulse Project (5 นาที)

1. https://studio.edgeimpulse.com → New Project
2. ตั้งชื่อ `team-XX-vision`
3. Project info → Target = **Arduino UNO Q**
4. Project info → Input = **Images**

จบ step นี้แล้ว ทีมควรเห็น project ถูกตั้งเป็นภาพและพร้อมรับ data เข้า train set/test set

---

### Step 3: Data Collection (60-75 นาที)

#### Image Settings:

| Parameter | ค่า |
|---|---|
| Resolution | **96×96** (ขั้นต่ำ) ถึง **160×160** |
| Color depth | RGB (3 channels) |
| Format | JPEG |

#### Connect Webcam to Edge Impulse:

**Option A:** ใช้มือถือเป็น camera (ง่ายที่สุด)
- Studio → Devices → Connect new device → **Use your mobile phone**
- สแกน QR code

**Option B:** ใช้ webcam จาก UNO Q
- ใน UNO Q: `edge-impulse-linux`
- เลือก camera input

**Option C:** Upload จาก laptop
- Studio → Data acquisition → Upload data
- เลือกรูปจากคอม

#### เก็บข้อมูล:

| Class | จำนวน Train | จำนวน Test |
|---|---|---|
| Class 1 | 80 รูป | 20 รูป |
| Class 2 | 80 รูป | 20 รูป |
| Class 3 | 80 รูป | 20 รูป |

**ตัวอย่าง variation ที่ต้องมี:**
- 📐 3+ มุมกล้อง
- 💡 2+ ระดับแสง
- 🎨 2+ background
- 📏 2+ ระยะห่าง

ถ้าทีมมีภาพสวยแต่ถ่ายมุมเดิม แสงเดิม พื้นหลังเดิมหมด ให้ถือว่ายังเก็บข้อมูลไม่พอสำหรับใช้จริง

---

### Step 4: Create Impulse (10 นาที)

1. **Impulse design**:
   - Input: Image (96×96, RGB)
   - Processing: **Image** (Color depth: RGB)
   - Learning: **Transfer Learning (Images)** หรือ **Classification**

2. **Image** → Generate features
   - ดู feature explorer — ดูว่า classes แยกกันได้ไหม

3. **Transfer Learning** (แนะนำ):
   ```
   Model:           MobileNetV2 (96x96 0.1)
   Model size:      nano  ★ สำคัญ
   Training cycles: 20
   Learning rate:   0.0005
   Processor:       GPU
   ```

⚠️ ถ้าเลือก model ใหญ่กว่านี้ → **จะไม่ลง UNO Q ได้**

เป้าหมายของ V1 คือได้ baseline ที่นำไปทดสอบกับรูปที่ไม่เคยเห็นมาก่อน ไม่ใช่แต่งตัวเลขใน train set

---

### Step 5: Validate (10 นาที)

ดู Confusion Matrix + Test set accuracy

**Sanity check:** ใช้รูปใหม่ที่ไม่ได้ train
- ถ้า Live classification ตรง → ดี
- ถ้าผิด → overfit หรือ training data ไม่ดี

ถ้าความผิดพลาดเกิดเฉพาะในแสงต่ำหรือ background ใหม่ นั่นคือสัญญาณว่าทีมควรกลับไปเก็บ variation เพิ่ม

---

### Step 6: Deploy to UNO Q (15 นาที)

#### Method 1: Arduino App Lab (แนะนำ)

1. Edge Impulse → Deployment → **Arduino UNO Q**
2. Build → Download
3. Arduino App Lab:
   - Import model
   - Configure Bricks:
     - Input: USB Camera
     - Output: Modulino Pixels (RGB) + Buzzer
   - Run on board

#### Method 2: Linux side via edge-impulse-linux

```bash
# บน UNO Q
edge-impulse-linux-runner

# runner จะดาวน์โหลด/รัน model จาก project ที่ผูกไว้
# แล้วเปิด web server สำหรับดูผล real-time
```

ถ้าต้องการไฟล์ `.eim` แยกสำหรับ offline run ให้ export/download จาก Edge Impulse ก่อน แล้วค่อยรันตามเอกสารของ runner รุ่นที่ใช้อยู่

### Step 6.5: Prototype Code Starter

Quick test ยังไม่พอสำหรับ demo สุดท้าย หลังเห็น prediction แล้วให้ map class ไป output อย่างน้อย 1 แบบ เช่น Pixels, Buzzer, LED Matrix หรือ WebUI

เปิดตัวอย่าง logic ได้ที่ [../common/prototype-code-starters.md](../common/prototype-code-starters.md#3-track-b--vision-mapping)

---

### Step 7: 10-Case Testing (40 นาที)

**Test cases ที่ต้องมี:**

| # | Type | ตัวอย่าง (กรณี Recycle) |
|---|---|---|
| 1-3 | Normal | ขวดธรรมดา / กระป๋องใหม่ / กระดาษเรียบ |
| 4-5 | Variation | ขวดบุบ / กระป๋องเปียก |
| 6 | Boundary | กล่องนมกระดาษ (น่าจะกระดาษ) |
| 7 | Different lighting | ในห้องมืด |
| 8 | Different angle | มุมก้ม |
| 9 | Background change | บนพื้นไม้ vs โต๊ะขาว |
| 10 | Adversarial | ขวดที่ไม่เคยเห็น |

---

### Step 8: Iterate to V2 (40 นาที)

จาก analysis V1:
1. ระบุ confusion → เพิ่มรูปในคลาสที่ผิด
2. เพิ่ม variation ที่ขาด (มุม/แสง/background)
3. (ถ้าจำเป็น) แก้ class definition
4. Re-train V2
5. Test เปรียบเทียบ

อย่าปรับหลายอย่างพร้อมกันถ้าไม่จำเป็น เพราะจะอธิบายยากว่าจริง ๆ แล้วอะไรทำให้ V2 ดีขึ้น

---

## 🎨 Output Configuration Examples

### Smart Recycle Bin (Basic)

```cpp
String predictedClass = topLabel;

if (predictedClass == "ขวด") {
  // เปิด servo ฝา 1
  setPixels(0, 0, 255);   // Blue
} else if (predictedClass == "กระป๋อง") {
  // เปิด servo ฝา 2
  setPixels(192, 192, 192);  // Silver
} else if (predictedClass == "กระดาษ") {
  // เปิด servo ฝา 3
  setPixels(139, 90, 43);   // Brown
} else {
  setPixels(120, 120, 120);  // Unknown / retry
}
```

---

## 🚀 ไอเดียต่อยอด

1. **Smart Recycle Bin** — เพิ่ม servo เปิดฝาตามประเภท
2. **Plant Disease Detector** — กล้องส่องใบ → ตรวจโรค
3. **Smart Cashless** — จำสินค้าได้ + ส่งราคาขึ้น display
4. **Inventory Counter** — นับสินค้าในชั้น
5. **Security Camera** — alert เมื่อเห็นสิ่งผิดปกติ

---

## ⚠️ Common Pitfalls

### Problem 1: Train แล้ว memory error
**สาเหตุ:** Model size ใหญ่เกินสำหรับ UNO Q
**แก้:** Model size = nano, resolution ≤ 96×96

### Problem 2: Accuracy ดีบน Studio แต่ใช้จริงห่วย
**สาเหตุ:** Distribution shift — env training ≠ env deployment
**แก้:** เก็บข้อมูลใน environment เดียวกับที่จะ deploy

### Problem 3: Inference ช้า (>1 วินาที)
**สาเหตุ:** Model ใหญ่ / resolution สูง
**แก้:** ลด model size, ลด input size

### Problem 4: Class ไม่ balance
**สาเหตุ:** เก็บ class ไหนได้ง่ายก็เก็บเยอะ
**แก้:** Limit ที่ 80 รูป/class

### Problem 5: Bias ของ background
**สาเหตุ:** เก็บแต่ละ class บน background ต่างกัน
**แก้:** Mix background — เก็บทุก class บนทุก background

---

## 📚 References

- [Edge Impulse Image Classification](https://docs.edgeimpulse.com/docs/tutorials/end-to-end-tutorials/image-classification)
- [Arduino UNO Q + Edge Impulse Vision](https://blog.arduino.cc/2026/03/04/train-and-deploy-your-own-ai-models-in-arduino-app-lab-now-fully-integrated-with-edge-impulse/)
- [Mobilenet variants](https://docs.edgeimpulse.com/docs/edge-impulse-studio/learning-blocks/transfer-learning-images)
