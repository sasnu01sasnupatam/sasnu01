<!-- workshop-header -->
<img width="1347" height="127" alt="Coding Thailand 2026 header" src="https://github.com/user-attachments/assets/ba5cf267-f460-4fb0-b69b-c461ae061a3b" />

# 🎨 Day 2 Teaching Slides

## 📑 Section 1: Opening — Day 2 Kickoff

### Day 2 = MVP Prototype Sprint

```
วันนี้ไม่ใช่วันทำระบบให้สมบูรณ์ 100%

แต่เป็นวันที่ทีมต้องเลือก
"ฟังก์ชันหลัก" ที่พิสูจน์ไอเดียได้จริง
ภายในเวลาที่มี

Prototype ที่ดีไม่จำเป็นต้องใหญ่
แต่ต้อง:
✅ ทำงานได้จริง
✅ ทดสอบได้
✅ อธิบายได้
✅ มีหลักฐานครบ
```

---

### ภาพจำของวันนี้

```
Problem → Core Function → Working Demo → Test Evidence → Pitch Story

ถ้าขาดข้อใดข้อหนึ่ง
ระบบจะเล่าได้ไม่ครบ
```

---

### สิ่งที่ทีมต้องถือกลับไปจาก Day 2

```
จบวันนี้ ทีมควรมีอย่างน้อย:

1. Working Prototype ที่ทำงานได้จริง 1 ฟังก์ชันหลัก
2. Test Log V2 อย่างน้อย 5 ครั้ง
3. หลักฐานของ model + output + การทดสอบ
4. Demo สั้นที่โชว์ไอเดียได้ชัด
5. Pitch story สำหรับ Day 3
```

---

### วันนี้ไม่ต้องทำอะไรบ้าง

```
✗ ไม่ต้องทำระบบใหญ่ที่สุด
✗ ไม่ต้องยัด feature ทุกอย่างในไอเดียเดียว
✗ ไม่ต้องทำกล่องสวยก่อนระบบทำงานจริง
✗ ไม่ต้องไล่ accuracy อย่างเดียวโดยไม่มี demo

เป้าหมายคือ:
ทำให้น้อยลง แต่พิสูจน์ให้ชัดขึ้น
```

---

### สิ่งที่มักถูกดูตอนประเมิน Day 2–3

```
กรรมการหรือผู้ฟังมักดู 6 เรื่องนี้ร่วมกัน:

1. Prototype ทำงานจริงไหม
2. Demo ชัดไหม เห็น input → output ไหม
3. Pitch อธิบายปัญหาและวิธีแก้ได้ไหม
4. Q&A ตอบข้อจำกัดและความผิดพลาดได้ไหม
5. Evidence Folder ครบไหม
6. ใช้ทรัพยากรและ token คุ้มค่าไหม
```

อย่าคิดว่าแค่ระบบติดแล้วจบ ต้องเล่าให้เห็นด้วยว่าทำไมระบบนี้จึงมีความหมาย

---

## 📑 Section 2: MVP Thinking

### MVP ของห้องนี้หน้าตาเป็นยังไง

```
MVP ขั้นต่ำของวันนี้ควรมี:

✅ อย่างน้อย 1 AI task
✅ อย่างน้อย 2 classes
✅ รับ input ใหม่แล้วทำนายได้
✅ เห็น prediction / confidence / output
✅ มีหลักฐานของ model และ test
✅ มี test อย่างน้อย 5 ครั้ง
```

จำง่าย:
AI ต้องไม่ใช่แค่ train ได้ แต่ต้องถูกเอาไปใช้กับ input ใหม่จริง

---

### โครง MVP ที่ควรยึด

```
Input Data
   ↓
AI Model / Brick
   ↓
Edge Inference บน UNO Q
   ↓
Prediction / Confidence
   ↓
Output / Alert / Display
```

ถ้ายังวาดเส้นนี้ไม่ออก แปลว่ายัง scope ไม่ชัดพอ

---

### เลือกแค่ 1 Core Function

```
ถามทีมตัวเองให้ชัด:

ฟังก์ชันเดียวที่ต้องสาธิตให้ได้คืออะไร?

ตัวอย่าง:
- ตรวจได้ว่า Safe หรือ Warning
- แยกได้ว่า Healthy หรือ Unhealthy
- ได้ยิน Start หรือ Stop แล้วตอบสนอง
- เจอ Anomaly แล้วแจ้งเตือน
```

ฟังก์ชันเดียวที่ชัด ดีกว่าหลายฟังก์ชันที่ทำไม่สุด

---

### Bronze / Silver / Gold

| ระดับ | สิ่งที่ต้องได้ |
|---|---|
| Bronze | ทำนายได้ มี 2 classes แสดงผลบน Serial/จอ และมี Test Log |
| Silver | ทำนายแล้วสั่ง output ได้ เช่น LED / Buzzer / Display |
| Gold | มี threshold, unknown/retest, dashboard, กล่อง demo หรือ video ที่ดีขึ้น |

กติกาสำคัญ:

```text
ปิด Bronze ให้ได้ก่อนเที่ยง
แล้วค่อยขยายเป็น Silver / Gold
```

---

### วิธีลด Scope เมื่อเริ่มไม่ทัน

```
ถ้าเริ่มเห็นว่าไม่ทัน ให้ตัดตามลำดับนี้:

1. ลดจำนวน features เสริม
2. ลดจำนวน outputs ที่ไม่จำเป็น
3. ลดจำนวน classes ให้เหลือแกนหลัก
4. ลดส่วนตกแต่งที่ไม่กระทบ demo

แต่อย่าตัด:
- core function
- การทดสอบ
- หลักฐาน
```

---

## 📑 Section 3: Checkpoints ของวันนี้

### Day 2 Checkpoints

| เวลา | Checkpoint | สิ่งที่ต้องเห็น |
|---|---|---|
| 09:00–09:30 | Brief + เป้าหมาย MVP | ทุกทีมตอบได้ว่าจะพิสูจน์ฟังก์ชันอะไร |
| 09:30–10:15 | Idea Canvas Review | ปัญหา ผู้ใช้ วิธีแก้ ชัดขึ้น |
| 10:15–11:00 | Prototype Scope | เลือก 1–2 ฟังก์ชันที่จะทำให้จบ |
| 11:00–12:00 | System Architecture | วาด Input / AI / UNO Q / Output ได้ |
| 13:00–14:30 | Build Sprint 1 | ระบบหลักเริ่มทำงาน |
| 14:30–15:45 | Build Sprint 2 | รวมระบบเป็น working prototype |
| 15:45–16:30 | Test & Debug | มี Test Log V2 อย่างน้อย 5 ครั้ง |
| 16:30–17:00 | Evidence Collection | มี code, diagram, screenshot, photo, video |
| 17:00–17:30 | Pitch Draft | เริ่มเล่า Problem, Solution, Tech, Result ได้ |
| 17:30–18:00 | Readiness Check | พร้อมสำหรับ Day 3 |

---

### ตอนเช้า ทีมต้องตอบ 4 คำถามนี้ให้ได้

```
1. ปัญหาคืออะไร?
2. ผู้ใช้คือใคร?
3. ฟังก์ชันหลักที่ต้องสาธิตให้ได้คืออะไร?
4. output ที่ผู้ใช้มองเห็นคืออะไร?
```

ถ้ายังตอบไม่ได้ครบ อย่าเพิ่งลงมือประกอบหรือเขียนเพิ่ม

---

### System Architecture ที่ควรวาดให้ได้

```text
[Input / Sensor]
      ↓
[AI Model / Edge Impulse / Brick]
      ↓
[UNO Q Runtime]
      ↓
[Output: LED / Buzzer / Display / Alert]
      ↓
[User เห็นอะไร / ได้ประโยชน์อะไร]
```

วาดให้สั้นและชัด อย่าวาดระบบทั้งโลก

---

### Feasibility Check ก่อนเริ่ม Build

```
ก่อนเริ่มลงมือจริง ให้เช็กว่า:

✅ Input พร้อมไหม
✅ Model พร้อมหรือยัง ต้อง train เพิ่มไหม
✅ UNO Q อ่าน input ได้จริงไหม
✅ output ต่อครบและทดสอบได้ไหม
✅ เวลาที่เหลือพอสำหรับ test อย่างน้อย 5 ครั้งไหม
✅ ถ้าระบบพัง ยังมีแผนสำรองไหม
```

---

## 📑 Section 4: Build Sprint

### Build Sprint 1 — ก่อนเที่ยงต้องได้อะไร

```
เป้าหมายขั้นต่ำก่อนเที่ยง:

✅ ระบบอ่าน input ได้
✅ model ทำนายจาก input ใหม่ได้
✅ เห็น prediction และ confidence
✅ output อย่างน้อย 1 แบบเริ่มตอบสนอง

ถ้ายังไม่ถึงจุดนี้
อย่าเพิ่งขยาย scope
```

---

### Build Sprint 2 — บ่ายทำอะไรต่อ

```
เมื่อ Bronze ผ่านแล้ว ค่อยเพิ่ม:

1. output ที่ชัดขึ้น
2. threshold เช่น confidence >= 80%
3. unknown / retry state
4. ความนิ่งของระบบ
5. ความพร้อมในการ demo
```

---

### ถ้าจะเพิ่ม output ให้เพิ่มแบบไหนก่อน

```
แนะนำลำดับนี้:

1. Serial / Text output
2. LED / Pixels
3. Buzzer / Alert
4. Display / Dashboard

ยิ่งซับซ้อน ยิ่งต้องถามว่า
มันช่วยพิสูจน์ไอเดียจริงไหม
```

---

### คำถามที่ทีมควรถามตัวเองระหว่าง Build

```
ถ้าเหลือเวลาอีก 2 ชั่วโมง จะตัดอะไรออก?
ถ้าระบบพังตอน demo จะโชว์อะไรแทน?
ถ้ากรรมการถามว่าโมเดลผิดเมื่อไหร่ จะตอบได้ไหม?
ถ้าผู้ใช้ดู 10 วินาทีแรก เขาเข้าใจไหมว่าระบบนี้ช่วยอะไร?
```

---

## 📑 Section 5: Test, Debug, Evidence

### Test Log V2 — ขั้นต่ำ 5 ครั้ง

```
ตอนทดสอบให้บันทึกอย่างน้อย:

- input condition
- predicted class
- confidence
- response time
- output ที่เกิดขึ้นจริง
- actual result
- ผ่าน / ไม่ผ่าน
```

ถ้ามีเวลา ให้เก็บมากกว่า 5 ครั้ง และมีทั้ง easy case กับ edge case

---

### Debug Log — ต้องจดอะไรบ้าง

```
ปัญหาที่ควรจด:

1. อะไรพัง?
2. น่าจะพังเพราะอะไร?
3. ลองแก้อะไรไปแล้ว?
4. ตอนนี้ดีขึ้นไหม?
5. ยังเหลือ risk อะไรอยู่?
```

Debug ที่ดีช่วยตอบ Q&A ได้ดีกว่าระบบที่ดูสวยแต่เล่าอะไรไม่ได้

---

### Day 2 Evidence Pack

```
ก่อนจบวัน ทุกทีมควรมี:

1. Updated Idea Canvas
2. Prototype Scope Sheet
3. System Architecture
4. Working Prototype ที่ทำงานได้จริง
5. Test Log V2 อย่างน้อย 5 ครั้ง
6. Debug Log
7. Model Evidence
8. Token Usage Log
9. Demo Video 15–30 วินาที
10. Pitch Draft
```

ถ้าหลักฐานยังไม่ครบ อย่าเพิ่งคิดว่าจบงาน

---

### Model Evidence ที่ควรมี

```
อย่างน้อยควรเก็บ:

- screenshot หรือ link ของ model / project
- ผล prediction / confidence
- ภาพ output ตอนรันจริง
- บันทึกว่าระบบตอบสนองช้าหรือเร็วแค่ไหน
- สิ่งที่เปลี่ยนจาก V1 ไป V2
```

---

### Token Usage Log สำคัญยังไง

```
Token ไม่ได้มีไว้แค่เบิกของ
แต่ใช้พิสูจน์การตัดสินใจของทีมด้วย

คำถามที่ต้องตอบได้:
- ใช้อุปกรณ์อะไร?
- ใช้ไปทำไม?
- ถ้าไม่ใช้อุปกรณ์ชิ้นนี้ งานยังเดินได้ไหม?
```

อย่าเบิกของเพราะดูเท่ ให้เบิกเพราะช่วยพิสูจน์ไอเดีย

---

## 📑 Section 6: Pitch Readiness

### โครง Pitch Day 3 ที่ควรเริ่มตั้งแต่วันนี้

| ลำดับ | คำถามที่ต้องตอบ |
|---|---|
| 1. Problem | ปัญหาคืออะไร ใครเดือดร้อน |
| 2. User | ผู้ใช้คือใคร ใช้ในสถานการณ์ใด |
| 3. Solution | Prototype ของทีมแก้อะไร |
| 4. Technology | ใช้ Edge AI / UNO Q / Sensor / Model อย่างไร |
| 5. Demo | ระบบทำงานอย่างไร |
| 6. Test Result | ทดสอบแล้วได้ผลอย่างไร |
| 7. Limitation | ระบบยังผิดพลาดตรงไหน |
| 8. Next Step | ถ้าพัฒนาต่อจะปรับอะไร |
| 9. Resource | ใช้ token / อุปกรณ์คุ้มค่าอย่างไร |

---

### คำถามที่ mentor หรือกรรมการมักถาม

```
ฟังก์ชันเดียวที่ต้องสาธิตให้ได้คืออะไร?
ถ้าเหลือเวลา 2 ชั่วโมง จะตัดอะไรออก?
input ของ AI คืออะไร?
output ที่ผู้ใช้เห็นคืออะไร?
จะทดสอบ 5 ครั้งด้วยเงื่อนไขอะไร?
โมเดลผิดพลาดได้เมื่อไหร่?
สิ่งนี้ช่วยผู้ใช้จริงอย่างไร?
```

เตรียมตอบให้ได้ตั้งแต่ตอน build ไม่ใช่ค่อยคิดตอนขึ้นเวที

---

### Example MVPs ที่เหมาะกับ Edge AI

| แนวคิด | MVP ที่ทำทัน | Demo Day 3 |
|---|---|---|
| Smart Safety | ตรวจป้าย Warning / Safe | เจอ Warning แล้ว Buzzer ดัง |
| Smart Factory | ตรวจเสียงหรือการสั่น Normal / Anomaly | Anomaly แล้ว LED แดง |
| Smart Farm | ตรวจภาพใบพืช Healthy / Unhealthy | แสดงผล Healthy / Unhealthy |
| Smart Environment | จำแนกขยะ Plastic / Paper | LED หรือ Servo แยกประเภท |
| Smart Education | ตรวจคำสั่ง Start / Stop | พูด Start แล้วเริ่ม, Stop แล้วหยุด |
| HealthTech | จำแนกเสียง Normal / Alert | เสียง Alert แล้วแจ้งเตือน |

---

### Example Case — Motor Vibration Monitor

```text
โจทย์:
ติด Modulino Movement บนมอเตอร์หรือฐานเครื่อง
เพื่อแยกสถานะ stopped / normal / loose / unbalanced

input:
accX / accY / accZ

output:
Python log / Pixels / Buzzer / WebUI
```

เคสนี้เหมาะกับ Day 2 เพราะเห็น flow ชัด:

```text
เก็บ CSV → train ใน Edge Impulse → deploy กลับ UNO Q → predict → แจ้งผล
```

---

### Example Case Workflow

```text
1. เก็บข้อมูลแยกตาม class เป็นไฟล์ CSV
2. Upload เข้า Edge Impulse แบบ time-series
3. Train model ด้วย Spectral Analysis + Classification
4. Deploy แบบ Arduino Library ลง UNO Q
5. แสดงผลบน log หรือ webapp ให้คนดูเข้าใจทันที
```

ค่าเริ่มต้นที่ใช้ได้เลย:

```text
sample rate = 50 Hz
capture = 10 วินาทีต่อไฟล์
รอบแรก = 5 ไฟล์ต่อ class
```

คู่มือเต็มอยู่ที่:
[labs/track-a-motion/example-case-vibration-monitor.md](../labs/track-a-motion/example-case-vibration-monitor.md)

---

### Closing Mantra

```
เล็ก → ทำงานได้ → ทดสอบได้ → เล่าได้

Build less, prove more.
ทำน้อยลง แต่พิสูจน์ให้ชัดขึ้น
```

---

### Before You End Day 2

```
เช็กครั้งสุดท้ายก่อนเลิก:

□ ระบบรันได้จริงอย่างน้อย 1 ฟังก์ชันหลัก
□ มี Test Log V2 ≥ 5
□ มีหลักฐานพร้อมเปิดให้ดู
□ มีวิดีโอหรือรูปตอนระบบทำงาน
□ Pitch เล่าได้ครบ 5 นาที
□ รู้ว่าพรุ่งนี้จะโชว์อะไร
```
