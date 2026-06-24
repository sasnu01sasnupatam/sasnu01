<!-- workshop-header -->
<img width="1347" height="127" alt="Coding Thailand 2026 header" src="https://github.com/user-attachments/assets/ba5cf267-f460-4fb0-b69b-c461ae061a3b" />

# 🎯 Track C — Multi-Sensor Environment

> **Sensors:** Modulino Thermo + Light + Distance (combo)
> **Output:** Modulino Pixels + Buzzer
> **Difficulty:** ⭐⭐⭐ Advanced concept (multi-sensor fusion)

Track นี้เหมาะกับทีมที่อยากลอง multi-sensor fusion และพร้อมอธิบายว่า sensor แต่ละตัวช่วยแยก class ยังไง

---

## 🎓 ทำไมเลือก Track C

- ✅ ใช้ Modulino ล้วน ไม่ต้อง USB peripherals
- ✅ สอน **multi-sensor fusion** ได้
- ✅ ข้อมูล dense → train เร็ว
- ✅ Real-world applications เยอะใน IoT

---

## 💡 โจทย์แนะนำตามระดับ

### 🟢 Basic: Day/Night Detector

**Sensors ใช้:** Light + Thermo
**Classes:** กลางวัน / กลางคืน / เปิดไฟ

### 🟡 Intermediate: Fridge Door Alert

**Sensors ใช้:** Thermo + Light
**Classes:** ปิด / เปิดสั้น (< 30s) / เปิดทิ้ง (> 30s)

### 🔴 Advanced: Smart Zone Detector

**Sensors ใช้:** Distance + Light + (Thermo optional)
**Classes:** ห้องว่าง / มีคน 1 คน / มีคนหลายคน

---

## 🛠️ Lab Steps

## ✅ ก่อนเริ่ม track นี้ ทีมควรมี

- repo ทีมพร้อมใช้งานและมี commit แรกแล้ว
- W1 ที่ระบุชัดว่าแต่ละ class ต่างกันเพราะ signal ไหน
- ทีมเข้าใจว่าจะใช้ sensor อะไรบ้าง ไม่จำเป็นต้องใช้ทุกตัวถ้าโจทย์ไม่ต้องการ

### Step 1: Hardware Setup (5 นาที)

```
UNO Q ─Qwiic─→ Thermo ─Qwiic─→ Light ─Qwiic─→ Distance ─Qwiic─→ Pixels
```

⚠️ I2C address ของแต่ละ Modulino ต่างกัน — ไม่ชนกัน

ทดสอบ:
```cpp
// อ่านค่าทั้ง 3 sensor
float temp = thermo.getTemperature();
float humidity = thermo.getHumidity();
int light = light.getLight();
int distance = distance.getDistance();
```

ดูค่าใน Serial Monitor ก่อนเริ่ม

จบ step นี้แล้ว ทีมควรพอมองออกว่าค่า sensor เปลี่ยนยังไงเวลา environment เปลี่ยนจริง

---

### Step 2: Edge Impulse Project (5 นาที)

1. https://studio.edgeimpulse.com → New Project
2. ตั้งชื่อ `team-XX-environment`
3. Target = **Arduino UNO Q**

---

### Step 3: Data Collection (60 นาที)

#### Sampling Settings:

| Parameter | ค่า |
|---|---|
| Sample length | **10,000 ms (10 วินาที)** |
| Sample frequency | **2 Hz** (อ่านทุก 0.5 วินาที) |
| Sensors | Temp, Humidity, Light, Distance (เลือกตามโจทย์) |

#### เป้าหมายรอบแรก: 20 recordings/class

| Class | จำนวน recordings | ความยาวต่อไฟล์ | รวมเวลาโดยประมาณ |
|---|---|---|---|
| Class 1 | 20 | 10 วินาที | ~3.5 นาที |
| Class 2 | 20 | 10 วินาที | ~3.5 นาที |
| Class 3 | 20 | 10 วินาที | ~3.5 นาที |

ตัวเลขนี้เป็นรอบแรกที่ทำทันใน workshop ถ้าต้องการ model ที่นิ่งขึ้น ให้เพิ่มเป็น 40-60 recordings/class โดยเปลี่ยนสภาพแวดล้อมให้หลากหลายขึ้น

**ทำไมต้องเก็บหลาย recordings:** เพราะ sensor data เปลี่ยนตามเวลา แสง ระยะ และอุณหภูมิจริงในห้อง

ถ้าทีมยังอธิบายไม่ได้ว่า class ไหนพึ่งพา sensor ตัวไหนเป็นหลัก ให้หยุดกลับไปทบทวน W1 ก่อนเก็บเยอะ

#### Data Collection Strategy:

**โจทย์ Basic (Day/Night):**
- กลางวัน: เก็บที่หน้าต่าง + ในห้อง + กลางแจ้ง
- กลางคืน: ในห้องปิดไฟ + ใต้โต๊ะ
- เปิดไฟ: หลอด LED + tungsten + neon

**โจทย์ Advanced (Smart Zone):**
- ห้องว่าง: distance สูงตลอด, ค่านิ่ง
- มีคน 1 คน: distance เปลี่ยนเป็นช่วงๆ
- มีคนหลายคน: distance เปลี่ยนเร็ว, light ลด (มีเงา)

---

### Step 4: Create Impulse (10 นาที)

1. **Impulse design**:
   - Input: Time series (10000ms window, 1000ms increase)
   - Processing: **Flatten** (แนะนำสำหรับ temp/light/distance ที่เปลี่ยนช้า)
   - Learning: **Classification (NN)**

ใช้ **Spectral Analysis** เฉพาะกรณีที่ signal มี pattern เป็นจังหวะหรือคลื่นชัดเจน ไม่ใช่แค่ค่าค่อย ๆ เปลี่ยนตามสภาพแวดล้อม

2. **Flatten** features:
   - Generate features
   - ดู feature explorer → classes แยกได้ไหม?

3. **NN Classifier**:
   ```
   Model size:      nano (2.4M)
   Training cycles: 50
   Learning rate:   0.0005
   Processor:       GPU
   ```

💡 **หมายเหตุ:** Multi-sensor data มักต้องการ epochs เยอะกว่า (50 vs 30)

เป้าหมายของ V1 คือเห็นว่าข้อมูลจากหลาย sensor ช่วยแยก class ได้จริง ไม่ใช่ใส่ sensor เยอะไว้ก่อนโดยไม่รู้ว่าตัวไหนมีประโยชน์

---

### Step 5: Validate (5 นาที)

ดู Confusion Matrix
- ถ้าทุก class ระบุได้ → ดี
- ถ้า class ที่ "อยู่ติดกัน" (เช่น เปิดสั้น vs เปิดทิ้ง) สับสน → ต้องเพิ่ม sample ของช่วง boundary

เวลาวิเคราะห์ ให้ดูด้วยว่าความสับสนเกิดจาก class definition แคบเกินไป หรือเกิดจาก sensor ที่เลือกยังไม่พอ

---

### Step 6: Deploy to UNO Q (10 นาที)

ทำเหมือน Track อื่น:
1. Deployment → Arduino UNO Q
2. Model size: nano
3. Build → Download
4. Arduino App Lab:
   - Input: Modulino Thermo + Light + Distance (custom block)
   - Output: Pixels + Buzzer

ถ้า App Lab ไม่มี block รวม sensor ที่ทีมต้องการ ให้ใช้ Arduino Library จาก Edge Impulse แล้วเขียน code อ่าน sensor เอง จากนั้นส่ง feature เข้า classifier

### Step 6.5: Prototype Code Starter

หลังเห็น prediction แล้ว ให้ map class ไป output เช่น `ปิด → เขียว`, `เปิดสั้น → เหลือง`, `เปิดทิ้ง → แดง + buzzer`

เปิดตัวอย่าง logic ได้ที่ [../common/prototype-code-starters.md](../common/prototype-code-starters.md#4-track-c--environment-mapping)

---

### Step 7: 10-Case Testing (40 นาที)

**Test cases ที่ต้องมี:**

| # | Type | ตัวอย่าง (กรณี Fridge Door) |
|---|---|---|
| 1-3 | Normal | ปิดสนิท / เปิด 5s / เปิด 60s |
| 4 | Boundary | เปิด 25s, 30s, 35s — ดู confidence |
| 5 | Lighting | ตู้เย็นในห้องสว่าง vs ห้องมืด |
| 6 | Door swing | เปิด-ปิด-เปิด-ปิด เร็วๆ |
| 7 | Empty | ตู้เย็นว่างเปล่า |
| 8 | Full | ตู้เย็นเต็มของ |
| 9 | After cleaning | ตู้เย็นเช็ดด้วยน้ำเย็น (humidity เปลี่ยน) |
| 10 | Stress | เปิดทิ้ง 5 นาที ดูว่า detect ได้ไหม |

---

### Step 8: Iterate to V2 (40 นาที)

Multi-sensor มักมี "weight" ที่ต่างกัน:
- Class A อาจขึ้นกับ Light เป็นหลัก
- Class B อาจขึ้นกับ Distance เป็นหลัก

ลอง:
1. Remove sensor ที่ไม่ contribute → simpler model
2. หรือ add sensor ใหม่ที่ track ไม่ได้คิดถึง
3. เปรียบเทียบ V1 vs V2

ยิ่งทีมอธิบายได้ชัดว่า sensor ไหนช่วยจริง V2 ของทีมจะยิ่งน่าเชื่อถือเวลาทดสอบและอธิบายผล

---

## 🎨 Output Configuration Examples

### Fridge Door Alert

```cpp
String predictedClass = topLabel;

if (predictedClass == "ปิด") {
  setPixels(0, 255, 0);   // Green — OK
} else if (predictedClass == "เปิดสั้น") {
  setPixels(255, 255, 0); // Yellow — Warning
} else if (predictedClass == "เปิดทิ้ง") {
  setPixels(255, 0, 0);   // Red — Alert
  buzzer.tone(2000, 500);
   // ส่ง notification เพิ่มภายหลังได้
} else {
   setPixels(120, 120, 120); // Unknown / retry
}
```

---

## 🚀 ไอเดียต่อยอด

1. **Smart Classroom Monitor** — ตรวจว่าห้องว่างไหม → ปิดแอร์/ไฟอัตโนมัติ
2. **Fridge Guardian** — alert ถ้าเปิดทิ้ง 30s+ (ประหยัดไฟ)
3. **Smart Parking** — แต่ละช่องจอด detect รถ
4. **Greenhouse Monitor** — ตรวจ environment เกษตร
5. **Hospital Room Monitor** — track patient ในห้อง

---

## ⚠️ Common Pitfalls

### Problem 1: Class boundary ไม่ชัด
**ตัวอย่าง:** "เปิดสั้น" 29 วินาที vs "เปิดทิ้ง" 31 วินาที — แทบไม่ต่าง
**แก้:**
- ทำ class boundary ห่างกันมากขึ้น (เปิด < 15s vs > 60s)
- หรือใช้ regression แทน classification

### Problem 2: Light sensor saturate
**สาเหตุ:** กลางแจ้งแดดจัด → ค่า max หมด
**แก้:** เพิ่ม noise/baseline data ที่ saturate ลงไป

### Problem 3: Temperature drift
**สาเหตุ:** Modulino Thermo อ่านอุณหภูมิห้อง ไม่ใช่ object
**แก้:** ติดให้ใกล้ object ที่ต้องการ measure

### Problem 4: Sample length ผิด
**ถ้าสั้นเกิน:** จับ pattern ไม่ทัน
**ถ้ายาวเกิน:** ตอบสนองช้า
**แก้:** เริ่มที่ 10s แล้วปรับตาม use case

---

## 📚 References

- [Modulino Distance](https://store.arduino.cc/products/modulino-distance)
- [Modulino Thermo](https://store.arduino.cc/products/modulino-thermo)
- [Modulino Light](https://store.arduino.cc/products/modulino-light)
- [Edge Impulse Sensor Fusion guide](https://docs.edgeimpulse.com/docs/tutorials/end-to-end-tutorials/sensor-fusion)
