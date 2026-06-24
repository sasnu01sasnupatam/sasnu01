<!-- workshop-header -->
<img width="1347" height="127" alt="Coding Thailand 2026 header" src="https://github.com/user-attachments/assets/ba5cf267-f460-4fb0-b69b-c461ae061a3b" />

# 🎨 Day 1 Teaching Slides

> 2 deck: **เช้า — คุม UNO Q + input** / **บ่าย — เทรนด้วย Edge Impulse**
> โทน เหลือง/ดำ/ขาว · font Sarabun/Inter · code block พื้นเข้ม · ⭐ = สไลด์หัวใจ ใส่ diagram ช่วย

---

## 📑 Section 1: Opening — 09:00–09:20

**Coding Thailand 2026 — Edge AI Workshop**
*Arduino UNO Q × Modulino × Edge Impulse*

<img src="assets/mentor-sanchaieardprab.jpeg" alt="Mentor portrait of ผศ.ดร.สัญชัย เอียดปราบ" width="240" align="right" />

- Mentor: ผศ.ดร.สัญชัย เอียดปราบ
- วิศวกรรมระบบสมองกลฝังตัวและอิเล็กทรอนิกส์สื่อสาร
- คณะวิศวกรรมศาสตร์ มหาวิทยาลัยบูรพา
- วันที่ ____________

---

### Welcome & House Rules

```
Welcome! 🎉

กฎ:
1. มีคำถาม ยกมือ ไม่ต้องรอ
2. ทำงานเป็นทีม ไม่ใช่แข่งกันคนเดียว
3. ติดเกิน 5 นาที บอกพี่เลี้ยง
4. ใช้ AI/เครื่องมือได้ แต่อธิบายให้ได้ว่าทำอะไร
```

---

### งานนี้เดินยังไง

```
ภาพรวม 3 วัน:
Day 1  สร้างฐาน (วันนี้): setup, input, train, deploy, test
Day 2  ต่อยอด prototype (ทำเอง)
Day 3  demo + pitch

วันนี้ต้องได้ทั้ง "ของที่รันได้" และ "หลักฐานเอาไปเล่าต่อได้"
ทุกทีมเดินเส้นเดียวกัน
```

---

# DECK เช้า — "คุม UNO Q ให้อยู่มือ"

### S4 - What is Edge AI?

```
Cloud AI            Edge AI
ส่งข้อมูลขึ้น cloud      ทำงานบนอุปกรณ์เลย
ช้า ต้องมีเน็ต         เร็ว ไม่ต้องเน็ต
Privacy?            Privacy ok
```
ตัวอย่าง: Fall detector / Smart camera / ฟังเสียงเครื่องจักร

---

### S5 - ทำไม UNO Q: สองสมอง (star)

```
UNO Q = สองสมองในบอร์ดเดียว

MCU side   : อ่าน sensor / คุม output ทันที (Modulino)
Linux side : รัน AI / กล้อง USB / ไมค์ USB / Edge Impulse

จำไว้: input คนละแบบเข้าคนละฝั่ง บ่ายจะได้ไม่งง
```

---

### S6 - วันนี้จะได้อะไร

```
เช้า:
- บอร์ดเป็นของทีม (รหัส/ชื่อ/Wi-Fi)
- ต่อ input ครบ 3 (sensor / กล้อง / ไมค์)

บ่าย:
- model V1 + รันบนบอร์ดจริง
- Prediction Log >=10 cases
- ส่งงานผ่าน fork
```

---

### S7 - ตั้งบอร์ด: ทำไม

```
บอร์ดวนมาจากที่อื่น
รหัส / ชื่อ / Wi-Fi เดิม "ไม่ใช่ของเรา"
=> ตั้งใหม่ให้เป็นของทีม

บอร์ดไม่ boot? เช็ก jumper pin เสียบค้างก่อน (ถอดออก)
```

---

### S8 - ตั้งบอร์ด: ยังไง (ไม่ต้อง flash)

```
App Lab เห็นบอร์ด (login ให้เอง) -> Settings:
- ชื่อบอร์ด -> team-XX-q
- Change password -> ตั้งรหัสทีม (จดไว้!)
- Wi-Fi -> ตั้งให้ตรงกับคอม
- เปิด Remote access (SSH)

ผ่าน = เห็นชื่อทีม + Wi-Fi ติด + รู้รหัส
```

---

### S9 - Modulino: Plug & Play

```
Qwiic เสียบแล้วใช้ ไม่ต้องบัดกรี
เสียบผิดด้านเข้าไม่ได้ (ปลอดภัย)
ต่อเรียง: sensor ก่อน -> output หลัง
```

---

### S10-S12 - ต่อ Input 3 แบบ

```
Input 1 - Sensor (Qwiic, MCU side)
   ต่อ Movement -> อ่านค่า -> ขยับแล้วเลขขยับ

Input 2 - Webcam (USB, Linux side)
   powered USB hub -> เห็นภาพ
   ใช้กล้อง: บอร์ดอยู่ Wi-Fi อย่าต่อบอร์ดเข้าคอม

Input 3 - Mic (USB, Linux side)
   mic เข้า hub -> arecord -l -> พูดแล้ว level ขยับ
```

---

### S13 - เช้า -> บ่าย

```
3 input นี้ บ่ายเลือก 1 อย่างส่งเข้า Edge Impulse
ทีมอยากสอน AI ให้ "มอง / ฟัง / รู้สึก" อะไร?
```

---

# DECK บ่าย — "เทรนจริงด้วย Edge Impulse"

### S14 - Edge Impulse คืออะไร

```
platform เทรน Edge AI ผ่านเว็บ ฟรี (free tier)

Pipeline 4 ขั้น:
Collect -> Train -> Deploy -> Test  (แล้ววนแก้)
```

---

### S15 - Bias: AI จำ shortcut ผิดจุด (star)

```
เก็บจากคนเดียว / ฉากเดียว / ระยะเดียว
=> AI จำ "คน / ฉาก / ระยะ" แทน class จริง
=> เปลี่ยนคน เปลี่ยนห้อง ก็ทายพัง

เช็ก: "เปลี่ยนคนหรือเปลี่ยนห้องแล้ว ยังทายได้ไหม?"
```

---

### S16 - ออกแบบ class

```
ก่อนเก็บข้อมูล ทีมตอบให้ได้:
- มีกี่ class?
- แต่ละ class ต่างกันด้วยอะไร?
- edge case ที่อาจสับสน?
- เก็บกี่ตัวอย่าง/class? จากใคร ที่ไหนบ้าง?
```

---

### S17 - เชื่อม UNO Q เข้า Edge Impulse (star)

```
ทางเข้าต่างกันตาม input:
- กล้อง/ไมค์ -> shell: edge-impulse-linux -> login -> เลือก project
- Modulino  -> ผ่าน App Lab brick หรือ data-forwarder

เห็น data ไหลเข้า Studio ก่อน ค่อยเก็บ dataset
```

---

### S18 - เก็บ data ให้ดี

```
- แต่ละ class จำนวนใกล้กัน
- สลับคน / มุม / แสง / ระยะ / ความเร็ว
- ให้คนที่ไม่ได้เก็บลองใช้ด้วย
```

---

### S19 - Feature Explorer vs Confusion Matrix

```
Feature Explorer = "ข้อมูลพร้อมไหม"
   แยกเป็นก้อน = ดี / ทับกัน = สับสน (แก้ก่อน train)

Confusion Matrix = "โมเดลตอบได้แค่ไหน"
   เส้นทแยงสูง = ดี / นอกเส้นทแยงสูง = สับสน
```

---

### S20 - Train

```
model ขนาดเล็กสุดที่ลง UNO Q ได้ (ดู option บนจอ)
ดู confusion matrix ก่อนดีใจกับ accuracy
memory error? -> ลด model size / ลด class
```

---

### S21 - Deploy ลง UNO Q

```
Edge Impulse: Deployment -> Arduino UNO Q -> Build -> download
App Lab: import model -> เลือก input -> Run

ป้อน input จริง -> model ตอบบนบอร์ดได้
```

---

### S22 - ทดสอบ 10 cases

```
5 ปกติ + 5 ท้าทาย:
ก้ำกึ่ง / เร็วผิดปกติ / คนใหม่ / ใกล้เคียงแต่ไม่ใช่

จดลง predictions.csv -> เคสไหนผิด? เพราะอะไร?
(เหลือเวลา -> เก็บเพิ่ม -> V2)
```

---

### S23 - ส่งงาน + ปิดวัน

```
Fork ครบ: hardware-check + model + predictions + รูปรันบนบอร์ด + ตอบ 3 ข้อ

โชว์ทีมละ 1-2 นาที:
โจทย์ / class / V1 ทำได้แค่ไหน / 1 อย่างที่เรียนรู้

Day 2 = ปล่อยให้ทำเอง
```

---

## เอาไปทำสไลด์จริง
- header banner + โลโก้ Coding Thailand (เหลือง/ดำ/ขาว)
- รูปบอร์ด/Modulino จริงจาก store.arduino.cc
- สไลด์ star (S5, S8, S15, S17) ใส่ diagram ช่วย
- ยกไป Canva/Gamma generate ครั้งแรก แล้วปรับ branding
