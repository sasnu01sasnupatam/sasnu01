<!-- workshop-header -->
<img width="1347" height="127" alt="Coding Thailand 2026 header" src="https://github.com/user-attachments/assets/ba5cf267-f460-4fb0-b69b-c461ae061a3b" />

# 📅 Day 1 Schedule — Edge AI Workshop (6 ชั่วโมง)

> **เป้าหมาย:** ทุกทีมจบวันด้วย Model V2 + Prediction Log + Idea Canvas พร้อมต่อยอดในรอบถัดไป

---

## ภาพรวม Timeline

```
09:00 ─┬─ 09:30  นำเข้า + Anchor Demo (30 นาที)
       │
09:30 ─┼─ 10:00  UNO Q Setup + Modulino Tour + Git 101 (30 นาที)
       │
10:00 ─┼─ 10:30  Track Selection + Class Design Workshop (30 นาที)
       │
10:30 ─┼─ 12:00  Data Collection Hands-on (90 นาที) ★
       │
12:00 ─┼─ 13:00  พักเที่ยง
       │
13:00 ─┼─ 14:00  Train V1 + Deploy to UNO Q (60 นาที)
       │
14:00 ─┼─ 15:30  10-Case Testing + Iterate to V2 (90 นาที) ★
       │
15:30 ─┼─ 16:00  Cross-Track Demo (2 นาที/ทีม)
       │
16:00 ─┼─ 16:30  Wrap Up + Idea Canvas (30 นาที)
       │
16:30 ─┴─ 17:00  Maker Space + Token Strategy (30 นาที)
```

★ = ช่วงสำคัญที่สุด สำหรับเก็บคะแนน

---

## 📍 รายละเอียดแต่ละช่วง

### ⏰ 09:00–09:30 | นำเข้าสู่บทเรียน + Anchor Demo

ช่วงนี้ควรได้ 3 อย่าง:
1. เห็นภาพรวมของ Edge AI pipeline ตั้งแต่เก็บข้อมูลจน deploy
2. เห็นตัวอย่าง prototype ที่จบ flow ได้จริง
3. รู้ว่าตลอดวันทีมต้องเก็บหลักฐานอะไรไว้ใน GitHub repo

สิ่งที่ควรสังเกตจากตัวอย่างเปิด:
- เขาเก็บข้อมูลแบบไหน
- เขาดูผล train ตรงไหน
- ตอน deploy สำเร็จ เขาตรวจว่า "ใช้ได้จริง" ยังไง

---

### ⏰ 09:30–10:00 | UNO Q Setup + Modulino Tour + Git 101

**Part A — UNO Q + Modulino (15 นาที):**
- เชื่อม USB-C, รอ boot
- ต่อ Modulino ผ่าน Qwiic — เน้น polarized connector (ผิดด้านเสียบไม่ได้)
- เปิด Arduino App Lab, login
- ลอง read sensor ดิบจาก Modulino Movement (sample sketch มีให้)

**Part B — Git 101 (15 นาที):**
- ดู [`04-git-basics.md`](04-git-basics.md)
- ทุกทีม:
  1. สร้าง repo ทีมใหม่จาก [team-repo-template](../templates/team-repo-template/)
  2. Clone repo ทีมลง laptop
  3. แก้ README ใส่ชื่อทีม + commit แรก

ก่อนจบช่วงนี้ ทีมควรมี:
- UNO Q ที่เปิดติดและคุยกับอุปกรณ์ได้
- repo ทีมที่มี commit แรกขึ้น GitHub แล้ว

---

### ⏰ 10:00–10:30 | Track Selection + Class Design

**Part A — เลือก Track (10 นาที):**
- ดู [`02-tracks.md`](02-tracks.md)
- แต่ละทีมเลือก 1 track + ตัดสินใจระดับความท้าทาย (Basic/Intermediate/Advanced)
- เขียนเหตุผลในไฟล์ `docs/track-selection.md` ของ repo ทีม
- Commit + Push

**Part B — Class Design Workshop (20 นาที):**
- ใช้ Worksheet W1 [`worksheets/W1-class-design.md`](../worksheets/W1-class-design.md)
- แต่ละทีม define classes ของตัวเอง พร้อมตอบ:
  - แต่ละ class แยกกันด้วย feature อะไร?
  - มี edge case อะไรที่อาจทำให้สับสน?
  - เก็บกี่ตัวอย่าง/class? ทำไม?
- ให้เพื่อนอีกคนหรือ TA ช่วยเช็กก่อนเริ่มเก็บข้อมูลจริง

⚠️ **อย่าเพิ่งเก็บข้อมูลถ้า class ยังไม่ชัด** — ถ้านิยาม class ซ้อนกันตั้งแต่ต้น ทีมจะเสียเวลาตอน train และ debug มาก

---

### ⏰ 10:30–12:00 | Data Collection Hands-on ★

**สิ่งที่ต้องทำ:**
- เริ่ม Edge Impulse Studio → New Project → ตั้งชื่อ
- เชื่อม UNO Q กับ Edge Impulse (ดูขั้นตอนใน [`labs/setup/connect-edge-impulse.md`](../labs/setup/connect-edge-impulse.md))
- เก็บข้อมูลตามเป้าหมาย:
  - **Track A (Motion):** 50 samples × class, แต่ละ sample 2 วินาที
  - **Track B (Vision):** 100 images × class
  - **Track C (Environment):** 200 samples × class
  - **Track D (Audio):** 50 samples × class, แต่ละ sample 1 วินาที

**เช็กความครบของข้อมูลระหว่างทาง:**
- แต่ละ class มีจำนวนใกล้กันไหม
- มี variation ของมุม, แสง, ระยะ, คนใช้ หรือ environment ครบพอไหม
- ถ้าเอาไปใช้กับคนหรือสถานที่อื่น ยังพอมีโอกาสทำงานได้ไหม

**สิ่งที่ควรอัปเดตลง repo:**
- Commit Worksheet W2 [`worksheets/W2-data-collection-log.md`](../worksheets/W2-data-collection-log.md)
- Link Edge Impulse project ใน README ของทีม

---

### ⏰ 12:00–13:00 | พักเที่ยง

ช่วงพักนี้ควรใช้เช็กสั้น ๆ ว่า README ทีม, link project และ Worksheet W2 ถูก push ขึ้น repo แล้ว

---

### ⏰ 13:00–14:00 | Train V1 + Deploy

**เป้าหมายช่วงนี้:** ให้ทีมมี V1 ที่ deploy ลง UNO Q ได้จริง

**Step-by-Step:**
1. ใน Edge Impulse → **Create Impulse**
   - เลือก processing block ตาม sensor
   - เลือก learning block (Classification)
2. **Train**
   - ตั้ง model size = **nano (2.4M)** เพื่อให้ลง UNO Q ได้
   - Training processor = **GPU**
   - กด Save & Train
3. **อ่าน Confusion Matrix** (ใช้ 10 นาทีทำความเข้าใจทั้งห้อง)
   - True Positive / False Positive
   - Accuracy ที่ดูได้ vs ที่ใช้ได้จริง
4. **Deploy**
   - Build → Arduino UNO Q (C++ Library หรือ via App Lab)
   - Test inference บนบอร์ดจริง

⚠️ ถ้า train ผิด memory → ลด model size หรือลด class count

---

### ⏰ 14:00–15:30 | 10-Case Testing + Iterate to V2 ★

นี่คือช่วงที่ทีมจะเห็นชัดที่สุดว่า model ใช้ได้จริงหรือยัง และต้องปรับอะไรต่อ

**Part A — 10-Case Test (40 นาที):**
- ใช้ Worksheet W3 [`worksheets/W3-prediction-log.md`](../worksheets/W3-prediction-log.md)
- ทดสอบขั้นต่ำ **10 cases** แยก:
  - 5 cases ที่คาดว่าโมเดลจะทำได้
  - 5 cases ที่ท้าทาย (edge case)
- บันทึก: timestamp, คำอธิบาย input, predicted class, confidence, actual class, ถูก/ผิด

**Part B — Iterate V2 (50 นาที):**
- วิเคราะห์ Prediction Log
- ระบุ pattern ที่ผิด → เก็บข้อมูลเพิ่ม / แก้ class
- Train V2
- ทดสอบ 10 cases เดิม + 5 cases ใหม่
- เปรียบเทียบ V1 vs V2 ใน `docs/v1-vs-v2.md`

**Commit ทุก 30 นาที** เพื่อมี history

---

### ⏰ 15:30–16:00 | Cross-Track Show & Tell

แต่ละทีม **2 นาที**:
- โจทย์คืออะไร
- Classes อะไรบ้าง
- Accuracy V1 → V2 เปลี่ยนเท่าไหร่
- 1 thing learned

ช่วงนี้คือเวลาที่ทีมจะโชว์ทั้ง "สิ่งที่ทำได้" และ "สิ่งที่เรียนรู้"

---

### ⏰ 16:00–16:30 | Wrap Up + Idea Canvas

ช่วงนี้ควรสรุปว่า prototype ของทีมจะต่อยอดเป็นอะไรต่อได้บ้าง เช่น:
- **เกษตร:** ตรวจโรคพืชจากใบ (Vision)
- **สุขภาพ:** Fall detector สำหรับผู้สูงอายุ (Motion)
- **อุตสาหกรรม:** ตรวจเครื่องจักรเสียจากเสียง (Audio)

จากนั้นร่าง Idea Canvas สำหรับรอบถัดไป แล้ว commit Worksheet W4

---

### ⏰ 16:30–17:00 | Maker Space + Token Strategy

ถ้าทีมจะต่อยอดต่อ ช่วงนี้คือเวลาคิดว่าจะต้องใช้อุปกรณ์อะไรเพิ่ม และเพราะอะไร

---

## ✅ End of Day 1 — Checklist สำหรับทุกทีม

- [ ] Repo ทีมมี commit อย่างน้อย 10 ครั้ง
- [ ] README ของทีม update ครบ (โจทย์, classes, ผลลัพธ์)
- [ ] Dataset link Edge Impulse ใส่ใน README
- [ ] `worksheets/W1-class-design.md` ✓
- [ ] `worksheets/W2-data-collection-log.md` ✓
- [ ] `worksheets/W3-prediction-log.md` ≥10 cases ✓
- [ ] `worksheets/W4-idea-canvas.md` ✓
- [ ] Compare `docs/v1-vs-v2.md` ✓
- [ ] Model V2 deploy ลง UNO Q ได้

---

## 🎯 Tips สำหรับนักเรียน

1. ถ้าติดเกิน 5 นาที ให้เขียนก่อนว่าติดตรงไหน, error อะไร, และลองอะไรไปแล้ว
2. อย่าดูแค่ accuracy ใน Studio ให้ดู Prediction Log และ case ที่ model พลาดด้วย
3. ถ้าเวลาเริ่มไม่พอ ให้ลด scope ก่อน เช่น ลดจำนวน class หรือใช้โจทย์ Basic แล้วทำให้จบ
4. Commit เป็นช่วง ๆ ระหว่างวัน จะช่วยให้ทั้งทีมตามงานกันทันและอธิบายสิ่งที่ทำได้ง่ายขึ้น
