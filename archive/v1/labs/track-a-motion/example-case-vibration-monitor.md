<!-- workshop-header -->
<img width="1347" height="127" alt="Coding Thailand 2026 header" src="https://github.com/user-attachments/assets/ba5cf267-f460-4fb0-b69b-c461ae061a3b" />

# 🧪 Example Case — Motor Vibration Monitor

> ตัวอย่างนี้เหมาะกับทีมที่อยากต่อยอด Track A ไปทางโรงงาน, predictive maintenance, หรือการเฝ้าระวังเครื่องจักร
> เป้าหมายคือใช้ **Modulino Movement + Arduino UNO Q + Edge Impulse** แยกสถานะของมอเตอร์จากการสั่น แล้วแสดงผลบน log, output device, หรือ webapp

---

## 🎯 ฟังก์ชันหลักของเคสนี้

ฟังก์ชันเดียวที่ควรพิสูจน์ให้ได้:

```text
รับสัญญาณการสั่นจากมอเตอร์
→ จำแนกสถานะด้วยโมเดล Edge AI
→ แจ้งผลให้ผู้ใช้เห็นทันที
```

สถานะเริ่มต้นที่แนะนำ:

- `stopped`
- `normal`
- `loose`
- `unbalanced`

ถ้าเวลาน้อย ให้เริ่มแบบ 2 classes ก่อน:

- `normal`
- `fault`

แล้วค่อยแตก `fault` เป็น `loose` กับ `unbalanced` ในรอบถัดไป

---

## 🧭 ภาพรวมระบบ

```text
Motor / Machine
   ↓
Modulino Movement (accX, accY, accZ)
   ↓
UNO Q เก็บข้อมูลเป็น CSV
   ↓
Upload เข้า Edge Impulse
   ↓
Train time-series model
   ↓
Deploy กลับมาที่ UNO Q
   ↓
แสดงผลผ่าน Python log / Pixels / Buzzer / WebUI
```

---

## 1. จะเก็บค่าไปเทรนทำยังไง

### รูปแบบ CSV ที่ Edge Impulse อ่านได้

สำหรับ time-series sensor ให้เก็บ **1 ไฟล์ = 1 sample** และใช้หัวตารางแบบนี้:

```csv
timestamp,accX,accY,accZ
0,-0.23,-0.25,-0.90
20,-0.25,-0.24,-0.92
40,-0.27,-0.34,-0.86
```

หลักสำคัญ:

- คอลัมน์แรกต้องชื่อ `timestamp`
- หน่วยเป็น millisecond
- ตามด้วย channel sensor ที่จะใช้ train
- อย่า upload log ที่มีข้อความอย่าง `status=` หรือ `rms=` ปนอยู่

---

### แผนเก็บข้อมูลรอบแรก

เริ่มง่าย ๆ แบบนี้ก่อน:

```text
stopped      5 ไฟล์ × 10 วินาที
normal       5 ไฟล์ × 10 วินาที
loose        5 ไฟล์ × 10 วินาที
unbalanced   5 ไฟล์ × 10 วินาที
```

ค่าที่แนะนำสำหรับรอบแรก:

- `CAPTURE_SECONDS = 10`
- `SAMPLE_INTERVAL_MS = 20`  → 50 Hz
- เก็บหลายรอบโดยเปลี่ยนความแรงของมอเตอร์หรือการยึดฐานเล็กน้อย

ถ้าต้องการให้ model นิ่งขึ้น ค่อยเพิ่มเป็น 10-20 ไฟล์ต่อ class

---

### ตัวอย่าง `python/main.py` สำหรับบันทึก CSV

ตัวอย่างนี้สมมติว่า `sketch.ino` ส่งค่า `ax, ay, az, vib` มาที่ Python ผ่าน `Bridge.provide("motion_reading", ...)`

```python
import time
import csv
from arduino.app_utils import App, Bridge

LABEL = "normal"          # stopped / normal / loose / unbalanced
CAPTURE_SECONDS = 10
SAMPLE_INTERVAL_MS = 20    # 50 Hz

samples = []
start_time = None
saved = False


def on_motion(ax, ay, az, vib):
    global start_time

    if saved:
        return

    if start_time is None:
        start_time = time.time()

    timestamp = len(samples) * SAMPLE_INTERVAL_MS
    samples.append([timestamp, float(ax), float(ay), float(az)])


Bridge.provide("motion_reading", on_motion)


def save_csv():
    filename = f"/home/arduino/{LABEL}.{int(time.time())}.csv"

    with open(filename, "w", newline="") as file:
        writer = csv.writer(file)
        writer.writerow(["timestamp", "accX", "accY", "accZ"])
        writer.writerows(samples)

    print(f"Saved CSV: {filename}")
    print(f"Samples: {len(samples)}")


def loop():
    global saved

    if start_time is None:
        print(f"Ready to capture label={LABEL}")
        time.sleep(1)
        return

    if time.time() - start_time >= CAPTURE_SECONDS and not saved:
        save_csv()
        saved = True

    time.sleep(0.05)


App.run(user_loop=loop)
```

หลังรันครบเวลาจะได้ไฟล์ประมาณนี้:

```text
/home/arduino/normal.1760000000.csv
```

ถ้าจะเก็บ class อื่น ให้เปลี่ยนแค่ค่า `LABEL` แล้วรันใหม่

---

## 2. เอาไฟล์เข้า Edge Impulse ยังไง

ใน Edge Impulse ไปที่:

```text
Data acquisition
→ Upload data
→ เลือกไฟล์ CSV
```

ถ้าตั้งชื่อไฟล์ขึ้นต้นด้วย label เช่น `normal.001.csv` หรือ `loose.001.csv` ตัว uploader มักจับ label ให้ได้อัตโนมัติ

ค่าที่แนะนำตอนสร้าง Impulse:

```text
Input type: Time series
Frequency: 50 Hz
Window size: 1000-2000 ms
Window increase: 500 ms
Processing: Spectral Analysis
Learning: Classification
Classes: stopped / normal / loose / unbalanced
```

อย่าไล่ accuracy อย่างเดียว ให้ดูด้วยว่าแยก class ได้จริงใน test ที่ไม่ใช่ข้อมูลเดิมหรือไม่

---

## 3. ได้โมเดลแล้วมา predict และแสดงผลได้ไหม

ได้ เส้นทางที่เหมาะกับเคสนี้คือ:

```text
Edge Impulse
→ Deployment: Arduino Library
→ Download .zip
→ เพิ่ม library ใน App Lab
→ sketch.ino เรียก run_classifier()
→ ส่งผลไป Python ผ่าน Bridge.notify()
→ แสดงผลด้วย log / pixels / buzzer / webapp
```

### สิ่งที่ `sketch.ino` ต้องทำ

ขั้นต่ำควรมี 4 อย่าง:

1. อ่านค่า IMU เป็น frame ตาม sampling rate ของโมเดล
2. เรียก `run_classifier()`
3. หา class ที่คะแนนสูงสุด
4. ส่งผลไป Python

ตัวอย่าง notification ที่ควรมี:

```cpp
Bridge.notify("ai_prediction", best_label, best_score, last_ax, last_ay, last_az);
```

ถ้าอยากให้มี alarm เพิ่มได้ เช่น:

- `normal` / `stopped` → สีเขียว
- `loose` / `unbalanced` → สีแดง + buzzer

---

### ตัวอย่าง `python/main.py` สำหรับแสดงผล predict

```python
import time
from arduino.app_utils import App, Bridge

latest = None
last_print = 0


def on_prediction(label, score, ax, ay, az):
    global latest
    latest = {
        "label": str(label),
        "score": float(score),
        "ax": float(ax),
        "ay": float(ay),
        "az": float(az),
    }


Bridge.provide("ai_prediction", on_prediction)


def loop():
    global last_print

    now = time.time()

    if latest is not None and now - last_print >= 0.5:
        label = latest["label"]
        score = latest["score"]

        if label in ["normal", "stopped"]:
            status = "OK"
        elif label in ["loose", "unbalanced", "fault"]:
            status = "FAULT"
        else:
            status = "UNKNOWN"

        print(
            f"status={status} | predict={label} | confidence={score:.2f} | "
            f"ax={latest['ax']:.2f}, ay={latest['ay']:.2f}, az={latest['az']:.2f}"
        )
        last_print = now

    time.sleep(0.05)


App.run(user_loop=loop)
```

ผลที่ควรเห็น:

```text
status=OK | predict=normal | confidence=0.91 | ax=-0.21, ay=-0.18, az=-0.95
status=FAULT | predict=unbalanced | confidence=0.84 | ax=-1.10, ay=-0.31, az=0.22
status=FAULT | predict=loose | confidence=0.79 | ax=-0.70, ay=-0.84, az=-0.15
```

---

## 4. เปิด webapp บน UNO Q ทำยังไง

สำหรับเคสนี้ไม่ต้องทำ Flask เอง ให้ใช้ **WebUI - HTML Brick** ใน App Lab

ขั้นตอนคือ:

```text
Add Brick
→ WebUI - HTML
→ Run
→ ดู URL ที่ Python console แสดง
```

โครงไฟล์ที่มักใช้:

```text
python/main.py
ui/index.html
ui/app.js
ui/style.css
sketch/sketch.ino
```

แนวคิดคือ:

- `sketch.ino` ส่ง `ai_prediction` มาที่ Python
- `python/main.py` รับค่าแล้วส่งต่อไปหน้าเว็บ
- `ui/app.js` อัปเดต badge / label / confidence / accX accY accZ

ตัวอย่างโค้ดส่งข้อมูลจาก Python ไปหน้าเว็บ:

```python
ui.send_message("prediction_update", latest)
```

ตัวอย่างที่ควรแสดงบน webapp:

- สถานะ `OK / FAULT / UNKNOWN`
- label ปัจจุบัน
- confidence
- ค่า `accX`, `accY`, `accZ`
- ข้อความแนะนำ เช่น `Check motor mounting or balance`

---

## 5. Demo Day 2-3 ควรโชว์อะไร

Demo ที่กระชับสำหรับเคสนี้:

1. โชว์มอเตอร์และจุดติดตั้งเซนเซอร์
2. ทำสถานะ `normal` ให้ดู
3. เปลี่ยนเป็น `loose` หรือ `unbalanced`
4. ให้ระบบแจ้งผลบน log, buzzer, pixels หรือ webapp
5. สรุปว่าผู้ใช้จะเอาไปใช้ในงานจริงอย่างไร

ถ้าโชว์สดเสี่ยง ให้ถ่ายวิดีโอ backup 15-30 วินาทีเก็บไว้ด้วย

---

## 6. เช็กลิสต์ก่อนถือว่าเคสนี้พร้อม

- [ ] มี CSV แยกตาม class ครบ
- [ ] อัปโหลดเข้า Edge Impulse ได้จริง
- [ ] train และ export model ได้
- [ ] UNO Q predict จาก input ใหม่ได้
- [ ] มี output อย่างน้อย 1 แบบที่คนดูเห็นทันที
- [ ] มี test log อย่างน้อย 5 ครั้ง
- [ ] ตอบได้ว่าระบบยังพลาดในกรณีไหน

---

## ⚠️ ข้อผิดพลาดที่เจอบ่อย

1. **เอา log ที่มีข้อความปนไป upload**
   → ใช้เฉพาะ CSV ที่มีหัวตาราง `timestamp,accX,accY,accZ`
2. **เก็บทุก class ในสภาพเดียว**
   → ลองเปลี่ยนความแรงมอเตอร์หรือการยึดฐานบ้าง
3. **อยากได้ 4 classes ตั้งแต่แรกแต่ข้อมูลยังน้อย**
   → เริ่ม 2 classes ก่อนแล้วค่อยขยาย
4. **train ดีแต่ demo ไม่ชัด**
   → เพิ่ม output ที่ผู้ใช้เห็นทันที เช่น LED, buzzer, web badge

สรุปสั้น ๆ: เคสนี้เหมาะมากกับ Day 2 เพราะพิสูจน์ไอเดียได้เร็ว, เห็น input → prediction → output ชัด, และโยงกับงานจริงได้ง่าย