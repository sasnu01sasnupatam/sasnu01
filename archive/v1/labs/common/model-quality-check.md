<!-- workshop-header -->
<img width="1347" height="127" alt="Coding Thailand 2026 header" src="https://github.com/user-attachments/assets/ba5cf267-f460-4fb0-b69b-c461ae061a3b" />

# 📈 Model Quality Check — ดูยังไงว่า Model พร้อมใช้จริง

> ใช้ก่อน train, หลัง train V1, และก่อนตัดสินใจทำ V2

คะแนนไม่ได้มาจาก accuracy อย่างเดียว แต่ดูว่าทีมเข้าใจว่าทำไม model ถูก ผิด หรือยังไม่พร้อมใช้จริง

---

## ก่อน Train: Data พร้อมหรือยัง

เช็กก่อนกด train:

- แต่ละ class มีจำนวน sample ใกล้กัน
- class definition ชัด ไม่ทับกันเอง
- มี variation ที่ใช้จริง เช่น คนใหม่ มุมใหม่ แสงใหม่ เสียงใหม่ หรือระยะใหม่
- มี test set ที่ไม่ใช่ข้อมูลชุดเดียวกับ train
- ไม่มี shortcut ที่ทำให้เกิด bias เช่น class A ถ่ายฉากหนึ่ง, class B ถ่ายอีกฉากหนึ่งทั้งหมด

ถ้าข้อนี้ยังไม่ผ่าน ต่อให้ train ได้ accuracy สูงก็ยังเสี่ยงพังตอนใช้จริง

---

## อ่าน Feature Explorer

Feature Explorer ใช้ตอบว่า **ข้อมูลแยกคลาสได้จริงหรือยัง**

| สิ่งที่เห็น | แปลว่า | ควรทำอะไร |
|---|---|---|
| จุดแต่ละ class แยกเป็นก้อน | data มี pattern ที่ model น่าจะเรียนได้ | train V1 ได้ |
| จุดหลาย class ทับกันเยอะ | class definition หรือ data ยังสับสน | กลับไปแก้ W1 หรือเก็บ data ใหม่ |
| จุด class เดียวกระจายหลายก้อน | variation เยอะ แต่ยังไม่สม่ำเสมอ | เพิ่ม sample ใน variation ที่ขาด |
| มีจุดหลุดไกลจากกลุ่ม | มี outlier หรือ sample เพี้ยน | เปิด sample นั้นดู แล้วลบหรือเก็บเพิ่มให้สมดุล |

จำง่าย:

```text
Feature Explorer = ข้อมูลพร้อมไหม
Confusion Matrix = model ตอบได้แค่ไหน
Prediction Log = ใช้จริงแล้วพังตรงไหน
```

---

## อ่าน Confusion Matrix

Confusion Matrix ใช้ดูว่า model สับสน class ไหนกับ class ไหน

| สิ่งที่เห็น | ความหมาย | วิธีแก้ |
|---|---|---|
| diagonal สูง | ทายถูกเยอะ | ไป quick test บน UNO Q ต่อ |
| off-diagonal สูงระหว่าง 2 class | model สับสนสอง class นี้ | เพิ่ม data ที่ทำให้สอง class ต่างกันชัดขึ้น |
| class หนึ่งถูกทายผิดบ่อยมาก | class นั้น data น้อยหรือ definition ไม่ชัด | เพิ่ม sample / แก้นิยาม class |
| accuracy สูงมากแต่ test จริงพัง | overfitting หรือ train/test ง่ายเกิน | เพิ่ม hard cases และ test set ใหม่ |

อย่าดูแค่ตัวเลข accuracy รวม ให้ถามเสมอว่า **ผิดกับ class ไหน และผิดเพราะอะไร**

---

## สัญญาณว่า V1 ยังไม่พร้อม

- ทายถูกเฉพาะคนที่เก็บ data แต่คนอื่นใช้แล้วผิด
- ทายถูกเฉพาะแสงเดิม ห้องเดิม หรือฉากหลังเดิม
- confidence ต่ำในทุก class
- boundary case ถูกบังคับให้เป็น class ใด class หนึ่งเสมอ
- output logic ส่งเสียง/ไฟเตือนทั้งที่ confidence ยังต่ำ

ถ้าเจออย่างน้อยหนึ่งข้อ ให้ทำ V2 โดยเริ่มจาก data ก่อน แล้วค่อยปรับ model parameter

---

## Decision Tree สำหรับแก้ V2

```text
Feature Explorer ปนกัน?
→ กลับไปแก้ class definition หรือเก็บ data เพิ่ม

Confusion Matrix สับสน class เดิมซ้ำ ๆ?
→ เพิ่ม sample เฉพาะ class คู่นั้น และเพิ่ม boundary cases

Studio accuracy ดี แต่ Prediction Log แย่?
→ environment ตอน test ต่างจากตอน train ให้เก็บ variation เพิ่ม

confidence ต่ำแต่ output ยังตัดสิน?
→ เพิ่ม threshold หรือเพิ่ม unknown/retry state

model ใหญ่ / deploy ไม่ได้?
→ ลด model size, ลด input resolution/window, ใช้ target Arduino UNO Q
```

---

## หลักฐานที่ควร Commit

- screenshot หรือ link ของ Feature Explorer
- screenshot หรือค่าจาก Confusion Matrix
- [Prediction Log](../../worksheets/W3-prediction-log.md) อย่างน้อย 10 cases
- สรุป V1 → V2 ใน `docs/v1-vs-v2.md` ของ repo ทีม
- commit message ที่บอกว่าแก้อะไรและเพราะอะไร

ตัวอย่าง commit message:

```text
train: เพิ่มข้อมูล class "เดิน" เพราะ V1 สับสนกับ "วิ่ง"
test: บันทึก 10 cases หลัง deploy บน UNO Q
docs: สรุป V1 vs V2 จาก confusion matrix และ prediction log
```
