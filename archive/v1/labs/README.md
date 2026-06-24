<!-- workshop-header -->
<img width="1347" height="127" alt="Coding Thailand 2026 header" src="https://github.com/user-attachments/assets/ba5cf267-f460-4fb0-b69b-c461ae061a3b" />

# 🧪 Labs — Day 1 Edge AI Workshop

> เริ่มจาก setup กลาง แล้วค่อยเลือก track ที่ทีมอยากทำจริง

หน้านี้คือทางเข้าของ lab ทั้งหมด ใช้เช็กว่าแต่ละทีมควรเปิดไฟล์ไหนต่อ และควรมีหลักฐานอะไรหลังทำแต่ละช่วงจบ

---

## ลำดับที่แนะนำ

1. เปิด [setup/connect-edge-impulse.md](setup/connect-edge-impulse.md) เพื่อเชื่อม UNO Q กับ Edge Impulse ให้ผ่านก่อน
2. ดู [anchor-demo/README.md](anchor-demo/README.md) เพื่อเห็นภาพรวม Data → Train → Deploy → Test
3. เลือก track จาก 4 track ด้านล่าง แล้วทำตาม lab ของ track นั้น
4. ใช้คู่มือกลางใน [common/](common/) ระหว่าง train, deploy, test, และ iterate
5. บันทึกผลลง worksheet และ commit หลักฐานเข้า repo ทีม

ถ้า UNO Q ยังไม่ขึ้นใน Edge Impulse Studio หรือ data ยังไม่ไหลเข้า Studio ให้หยุดที่ setup ก่อน อย่าเพิ่งเริ่มเก็บ dataset ของ track

---

## คู่มือกลางที่ใช้ทุก Track

| ไฟล์ | ใช้ตอนไหน | ช่วยตอบคำถาม |
|---|---|---|
| [common/post-deploy-testing.md](common/post-deploy-testing.md) | หลัง build/deploy model | Deploy แล้วต้องเขียน code ไหม? ต้อง test อะไรบน UNO Q? |
| [common/model-quality-check.md](common/model-quality-check.md) | ก่อน train, หลัง train, ก่อนทำ V2 | Feature Explorer / Confusion Matrix บอกอะไร? ควรแก้ data หรือ train ใหม่? |
| [common/prototype-code-starters.md](common/prototype-code-starters.md) | หลัง quick test ผ่าน | จะ map prediction → output / log / webapp ยังไง? |

---

## เลือก Track

| Track | เหมาะกับทีมที่อยาก... | Lab |
|---|---|---|
| A — Motion | เริ่มไว เห็นผลเร็ว ใช้ Modulino Movement | [track-a-motion/README.md](track-a-motion/README.md) |
| B — Vision | ทำงานกับภาพ วัตถุ กล้อง หรือ classification จากภาพ | [track-b-vision/README.md](track-b-vision/README.md) |
| C — Environment | รวมหลาย sensor แล้วอธิบาย sensor fusion | [track-c-environment/README.md](track-c-environment/README.md) |
| D — Audio | ทำ keyword / sound classification | [track-d-audio/README.md](track-d-audio/README.md) |

ไม่มี track ไหนได้คะแนนมากกว่ากัน คะแนนมาจาก process, หลักฐาน, การทดสอบ, และการอธิบายงาน

---

## หลังจบ Lab ควรมีอะไร

- Edge Impulse project ที่มี dataset และ model V1/V2
- UNO Q ที่รัน inference ได้จริงอย่างน้อย 1 รอบ
- output logic เบื้องต้น เช่น Pixels, Buzzer, Serial log หรือ LED Matrix
- Prediction Log อย่างน้อย 10 cases
- สรุป V1 → V2 ว่าแก้อะไร เพราะอะไร และดีขึ้นจริงไหม
- commit ใน repo ทีมที่อธิบายงานแต่ละช่วง

---

## ถ้าติดตรงไหน

| อาการ | เปิดไฟล์นี้ก่อน |
|---|---|
| UNO Q ไม่ขึ้นใน Studio | [setup/connect-edge-impulse.md](setup/connect-edge-impulse.md) |
| Deploy แล้วไม่รู้ test ยังไง | [common/post-deploy-testing.md](common/post-deploy-testing.md) |
| Accuracy สูงแต่ใช้จริงพัง | [common/model-quality-check.md](common/model-quality-check.md) |
| ไม่รู้จะเลือก track ไหน | [../docs/02-tracks.md](../docs/02-tracks.md) |
| ปัญหา hardware/network | [../docs/05-troubleshooting.md](../docs/05-troubleshooting.md) |
