<!-- workshop-header -->
<img width="1347" height="127" alt="Coding Thailand 2026 header" src="https://github.com/user-attachments/assets/ba5cf267-f460-4fb0-b69b-c461ae061a3b" />

# 🚀 Post-Deploy Testing — หลัง Deploy แล้ว UNO Q ต้องทำอะไร

> ใช้หลังจาก Edge Impulse build model แล้ว import เข้า Arduino App Lab หรือรันบน UNO Q ได้แล้ว

คำถามสำคัญคือ deploy แล้วจบหรือยัง? คำตอบคือยังไม่จบ จบก็ต่อเมื่อทดสอบบนข้อมูลจริงและบันทึกผลได้แล้ว

---

## แยกให้ชัด: Deploy, Quick Test, Prototype Test

| ระดับ | ต้องเขียน code ไหม | เป้าหมาย |
|---|---|---|
| Deploy | ยังไม่จำเป็น | ทำให้ model ไปอยู่บน UNO Q หรือ App Lab ได้ |
| Quick test | ยังไม่จำเป็น | ดูว่า model ทาย class และ confidence จาก input จริงได้ไหม |
| Prototype test | ต้องเขียน logic เพิ่ม | ให้ผลทำนายสั่ง output เช่น Pixels, Buzzer, LED Matrix หรือ Serial log |

จำง่าย: **deploy = model รันได้** แต่ **prototype = model สั่งงานอุปกรณ์ได้จริง**

---

## Quick Test — ยังไม่ต้องเขียน Code

ทำเพื่อเช็กว่า model รันบน UNO Q ได้และรับ input จริงได้

1. Edge Impulse → Deployment → เลือก **Arduino UNO Q**
2. Build → Download
3. Arduino App Lab → Import model
4. เลือก input ให้ตรง track:
   - Motion: Modulino Movement
   - Vision: USB Camera
   - Environment: Thermo / Light / Distance
   - Audio: USB Mic
5. กด Run หรือ Live classification
6. ทำ input จริง 3-5 ครั้ง แล้วดู:
   - predicted class
   - confidence
   - response time
   - case ที่ทายผิด

ถ้ายังไม่เห็น predicted class หรือ confidence ให้ถือว่ายังไม่ผ่าน quick test

---

## Prototype Test — ต้องเขียน Logic เพิ่ม

ทำเมื่อ quick test ผ่านแล้ว และต้องการให้ UNO Q ตอบสนองเป็น output จริง

logic ขั้นต่ำที่ควรมี:

```text
1. อ่านผลทำนายจาก model
2. หา class ที่ confidence สูงสุด
3. เช็ก threshold เช่น confidence >= 80%
4. map class ไป output
5. ถ้า confidence ต่ำ ให้แสดง unknown / ลองใหม่ / ไม่ส่งสัญญาณเตือน
```

ตัวอย่างสำหรับ Gesture Wand:

```text
ถ้า class = "นิ่ง" และ confidence >= 80% → Pixels สีเขียว
ถ้า class = "วงกลม" และ confidence >= 80% → Pixels สีฟ้า
ถ้า class = "Z-shape" และ confidence >= 80% → Pixels สีม่วง
ถ้า confidence < 80% → Pixels สีเทา หรือแสดงว่าให้ลองใหม่
```

ตัวอย่างสำหรับ track อื่น:

| Track | output logic ที่เริ่มได้เร็ว |
|---|---|
| Motion | class ปลอดภัย → เขียว, class เสี่ยง → แดง + Buzzer |
| Vision | object/class ที่ต้องคัดแยก → สีต่างกันบน Pixels หรือ LED Matrix |
| Environment | normal/warning/danger → เขียว/เหลือง/แดง |
| Audio | keyword ที่ถูกต้อง → ไฟเขียว, noise/unknown → ไฟเทา |

---

## Test Cases อย่างน้อย 10 ครั้ง

หลัง prototype logic ทำงานแล้ว ให้ทดสอบและบันทึกลง [../../worksheets/W3-prediction-log.md](../../worksheets/W3-prediction-log.md)

ชุด test ที่ควรมี:

| # | Type | ตัวอย่าง |
|---|---|---|
| 1-3 | Normal | input แบบที่ train มา |
| 4-5 | Variation | คนใหม่ / มุมใหม่ / แสงใหม่ / เสียงใหม่ |
| 6-7 | Boundary | case ที่อยู่ระหว่าง 2 classes |
| 8 | Speed / distance | เร็วขึ้น ช้าลง ใกล้ขึ้น ไกลขึ้น |
| 9 | Environment change | โต๊ะใหม่ ห้องใหม่ background ใหม่ |
| 10 | Adversarial | คล้าย class หนึ่ง แต่จริง ๆ ไม่ใช่ |

อย่าทดสอบเฉพาะ best case เพราะจะไม่รู้ว่า model พังตรงไหน

---

## Checkpoint ก่อนถือว่า Deploy สำเร็จ

- [ ] UNO Q รัน model ได้จริง
- [ ] เห็น predicted class + confidence
- [ ] input จริงเปลี่ยนแล้ว prediction เปลี่ยนตาม
- [ ] output logic ทำงาน เช่น Pixels / Buzzer / Serial log
- [ ] มี test log อย่างน้อย 10 cases
- [ ] มี case ที่ผิดหรือไม่มั่นใจ และทีมอธิบายได้ว่าจะ iterate อะไร

ถ้า deploy ได้แต่ยังไม่มี test log ยังถือว่าไม่จบงาน Day 1

---

## ถ้า Test แล้วไม่ตรง

| อาการ | สาเหตุที่พบบ่อย | วิธีแก้ |
|---|---|---|
| confidence ต่ำทุก class | data ยังแยกไม่ชัด หรือ input ไม่เหมือนตอน train | กลับไปดู Feature Explorer และเก็บ data เพิ่ม |
| ทาย class เดิมตลอด | dataset ไม่ balance หรือ input sensor ไม่เปลี่ยนจริง | เช็ก sensor/camera/mic และ class balance |
| accuracy ใน Studio สูง แต่บนบอร์ดพัง | environment ตอน deploy ต่างจากตอน train | เก็บ variation ในสภาพจริงเพิ่ม |
| output ไม่เปลี่ยนสี/เสียง | logic map class ไม่ตรงชื่อ class หรือ threshold สูงเกิน | print class/confidence ลง Serial log ก่อน |
| memory/build error | model ใหญ่เกิน | ลด model size เป็น nano หรือใช้ target Arduino UNO Q |
