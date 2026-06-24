<!-- workshop-header -->
<img width="1347" height="127" alt="Coding Thailand 2026 header" src="https://github.com/user-attachments/assets/ba5cf267-f460-4fb0-b69b-c461ae061a3b" />

# 🧩 Prototype Code Starters — Prediction → Output

> ใช้หลังจาก quick test ผ่านแล้ว และทีมอยากให้ผลทำนายสั่ง output จริง เช่น log, Pixels, Buzzer, LED Matrix หรือ WebUI

Quick test แค่พิสูจน์ว่า model รันได้ แต่ prototype ต้องตอบคำถามนี้ให้ได้:

```text
ถ้า model ทำนาย class นี้ ด้วย confidence เท่านี้
ผู้ใช้ควรเห็นอะไร?
```

---

## 1. Logic กลางที่ทุก track ควรมี

```text
prediction label
   ↓
confidence score
   ↓
threshold check
   ↓
class-to-output mapping
   ↓
log / pixels / buzzer / display / webapp
```

ค่าที่เริ่มได้เร็ว:

```text
confidence >= 0.75 → แสดงผลตาม class
confidence < 0.75  → UNKNOWN / RETRY / สีเทา / ไม่ส่งเสียงเตือน
```

อย่าให้ buzzer ดังทุกครั้งที่ confidence ต่ำ เพราะจะทำให้ผู้ใช้คิดว่าเป็น danger ทั้งที่ model ยังไม่มั่นใจ

---

## 2. Python Receiver สำหรับดู prediction ใน log

ตัวอย่างนี้ใช้เมื่อ `sketch.ino` หรือ App Lab side ส่งผลมาทาง `Bridge.notify("ai_prediction", label, score)`

```python
import time
from arduino.app_utils import App, Bridge

THRESHOLD = 0.75

latest = {
    "label": "-",
    "score": 0.0,
    "status": "WAITING",
}


def decide_status(label, score, ok_labels, warning_labels, danger_labels):
    if score < THRESHOLD:
        return "UNKNOWN"
    if label in ok_labels:
        return "OK"
    if label in warning_labels:
        return "WARNING"
    if label in danger_labels:
        return "DANGER"
    return "UNKNOWN"


def on_prediction(label, score):
    global latest

    label = str(label)
    score = float(score)

    latest = {
        "label": label,
        "score": score,
        "status": decide_status(
            label,
            score,
            ok_labels=["normal", "safe", "closed", "background"],
            warning_labels=["warning", "open_short", "speech"],
            danger_labels=["danger", "fault", "open_long", "cough", "help"],
        ),
    }


Bridge.provide("ai_prediction", on_prediction)


def loop():
    print(
        f"status={latest['status']} | "
        f"predict={latest['label']} | "
        f"confidence={latest['score']:.2f}"
    )
    time.sleep(0.5)


App.run(user_loop=loop)
```

ถ้า label ใน Edge Impulse เป็นภาษาไทยหรือชื่ออื่น ให้แก้ list ใน `ok_labels`, `warning_labels`, `danger_labels` ให้ตรงกับ project ของทีม

---

## 3. Track B — Vision Mapping

ตัวอย่าง Smart Recycle Bin:

```python
THRESHOLD = 0.75

VISION_OUTPUTS = {
    "ขวด": {
        "status": "PLASTIC",
        "color": "blue",
        "message": "ส่งไปช่องพลาสติก",
    },
    "กระป๋อง": {
        "status": "CAN",
        "color": "silver",
        "message": "ส่งไปช่องโลหะ",
    },
    "กระดาษ": {
        "status": "PAPER",
        "color": "brown",
        "message": "ส่งไปช่องกระดาษ",
    },
}


def vision_decision(label, score):
    if score < THRESHOLD:
        return {
            "status": "UNKNOWN",
            "color": "gray",
            "message": "วัตถุยังไม่ชัด ลองจัดแสงหรือมุมกล้องใหม่",
        }

    return VISION_OUTPUTS.get(label, {
        "status": "UNKNOWN",
        "color": "gray",
        "message": "class นี้ยังไม่ได้ map output",
    })
```

Demo ที่ดีควรเห็นภาพเดียวกัน 3 อย่าง:

- ภาพวัตถุหน้ากล้อง
- label + confidence
- output เช่น สี, ช่องแยก, หรือข้อความบนจอ

---

## 4. Track C — Environment Mapping

ตัวอย่าง Fridge Door Alert:

```python
THRESHOLD = 0.75

ENV_OUTPUTS = {
    "ปิด": {
        "status": "OK",
        "color": "green",
        "alarm": False,
    },
    "เปิดสั้น": {
        "status": "WARNING",
        "color": "yellow",
        "alarm": False,
    },
    "เปิดทิ้ง": {
        "status": "DANGER",
        "color": "red",
        "alarm": True,
    },
}


def environment_decision(label, score):
    if score < THRESHOLD:
        return {
            "status": "UNKNOWN",
            "color": "gray",
            "alarm": False,
        }

    return ENV_OUTPUTS.get(label, {
        "status": "UNKNOWN",
        "color": "gray",
        "alarm": False,
    })
```

สำหรับ multi-sensor อย่าดูแค่ label ให้ log ค่า sensor ไปด้วย เช่น `temp`, `humidity`, `light`, `distance` เพื่ออธิบายได้ว่า sensor ไหนช่วยแยก class

---

## 5. Track D — Audio Mapping

ตัวอย่าง Cough Detector:

```python
THRESHOLD = 0.80

AUDIO_OUTPUTS = {
    "ไอ": {
        "status": "ALERT",
        "color": "red",
        "alarm": True,
    },
    "จาม": {
        "status": "NOTICE",
        "color": "orange",
        "alarm": False,
    },
    "พูด": {
        "status": "SPEECH",
        "color": "green",
        "alarm": False,
    },
    "background": {
        "status": "IDLE",
        "color": "off",
        "alarm": False,
    },
}


def audio_decision(label, score):
    if score < THRESHOLD:
        return {
            "status": "UNKNOWN",
            "color": "gray",
            "alarm": False,
        }

    return AUDIO_OUTPUTS.get(label, {
        "status": "UNKNOWN",
        "color": "gray",
        "alarm": False,
    })
```

Audio ควรใช้ threshold สูงกว่า track อื่นเล็กน้อย เพราะ false positive ทำให้ demo ดูไม่น่าเชื่อถือเร็วมาก

---

## 6. สิ่งที่ต้องแก้ให้ตรงกับ project ของทีม

- ชื่อ label ต้องตรงกับ Edge Impulse ทุกตัวอักษร
- threshold ต้องเหมาะกับ test log ของทีม ไม่ใช่เดา
- output ควรช่วยให้ผู้ใช้เข้าใจทันที ไม่ใช่แค่สวย
- unknown/retry ต้องมีเสมอ โดยเฉพาะ Vision และ Audio

ก่อนขึ้น demo ให้ลองอย่างน้อย 5 ครั้งแล้วจดลง Prediction Log:

```text
input condition | predicted label | confidence | output | expected result | pass/fail
```
