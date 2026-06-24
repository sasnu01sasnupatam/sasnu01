<!-- workshop-header -->
<img width="1347" height="127" alt="Coding Thailand 2026 header" src="https://github.com/user-attachments/assets/ba5cf267-f460-4fb0-b69b-c461ae061a3b" />

# ☀️ เทรน + Deploy + ทดสอบ (Edge Impulse)

บ่ายนี้เลือก **1 input** (จาก 3 ตัวที่ต่อเช้า) มาสอน AI แล้วเอาลงบอร์ดให้รันจริง

---

## 1. ออกแบบ class

ทีมตัดสินใจว่าจะสอน AI ให้แยกอะไรบ้าง:
- มีกี่ class? แต่ละ class ต่างกันด้วยอะไร?
- จะเก็บกี่ตัวอย่าง/class? เก็บจากใคร ที่ไหนบ้าง?

> ⚠️ **ระวัง bias:** ถ้าเก็บจากคนเดียว / ฉากเดียว / ระยะเดียว AI จะจำ "คน/ฉาก/ระยะ" แทน class จริง
> เช็กง่ายๆ: *"ถ้าเปลี่ยนคนหรือเปลี่ยนห้องแล้ว ยังทายได้ไหม?"*

---

## 2. เชื่อมบอร์ดเข้า Edge Impulse

ทางเข้าต่างกันตาม input ที่เลือก:

- **กล้อง / ไมค์:** เปิด shell (`>_`) บนบอร์ด → พิมพ์ `edge-impulse-linux` → login (เอา email/password จากหน้า Edge Impulse → account settings) → เลือก project
- **Modulino sensor:** ส่งตรงแบบกล้องไม่ได้ ใช้ผ่าน App Lab Edge Impulse brick หรือ data-forwarder

> ✅ **ผ่านเมื่อ:** เห็น input ตัวเอง (ภาพ/เสียง/ตัวเลข) ไหลเข้า Edge Impulse Studio
> ยังไม่เห็น? อย่าเพิ่งเก็บข้อมูล เปิด [troubleshooting.md](troubleshooting.md) ข้อ 3

---

## 3. เก็บข้อมูล + เทรน V1

1. **Data acquisition** → เก็บตาม class ที่ออกแบบ (สลับคน/มุม/แสง/ระยะ/ความเร็ว ให้หลากหลาย แต่ละ class จำนวนใกล้กัน)
2. **Create Impulse** → processing block ตาม input + learning block = Classification
3. **Generate features** → ดู Feature Explorer: จุดแต่ละ class แยกเป็นก้อน = ดี / ทับกัน = ข้อมูลยังสับสน กลับไปแก้ก่อน
4. **Train** → ได้ V1
   - เลือก model ขนาดเล็กที่สุดที่ลง UNO Q ได้ (ดู option ตอน deploy บนจอ)
   - อ่าน **Confusion Matrix:** เส้นทแยงสูง = ดี / นอกเส้นทแยงสูง = สับสน

> ✅ **ผ่านเมื่อ:** มี model V1 + อ่าน confusion matrix ของตัวเองออก

---

## 4. ลง model บนบอร์ด

1. Edge Impulse → **Deployment** → target **Arduino UNO Q** → Build → download
2. App Lab → import model → เลือก input ให้ตรง → Run บนบอร์ด
3. ป้อน input จริง → ดู predicted class + confidence

> ✅ **ผ่านเมื่อ:** ป้อน input จริงแล้ว model ตอบบนบอร์ดได้

อยากให้ตอบเป็นไฟ/เสียง (Pixels/Buzzer)? map `class → output` + ตั้ง threshold (เช่น confidence < 0.75 → ไม่ตัดสิน)

---

## 5. ทดสอบ 10 cases

จดลง `team-template/afternoon/predictions.csv`:
- 5 เคสปกติ (ทำตามที่ train)
- 5 เคสท้าทาย: ก้ำกึ่งระหว่าง class / เร็วผิดปกติ / คนหรือของที่ไม่ได้เก็บ / ใกล้เคียงแต่ไม่ใช่

ดู: เคสไหนผิด? เพราะอะไร? (เหลือเวลา → เก็บข้อมูลเพิ่มในจุดที่ผิด → เทรน V2)

> ✅ **ผ่านเมื่อ:** มี prediction log ≥10 cases ที่มีทั้งถูกและผิด พร้อมเหตุผลที่น่าจะพลาด

ต่อไป → [05-submit.md](05-submit.md)
