<!-- workshop-header -->
<img width="1347" height="127" alt="Coding Thailand 2026 header" src="https://github.com/user-attachments/assets/ba5cf267-f460-4fb0-b69b-c461ae061a3b" />

# 🧰 Facilitator Prep — เตรียมก่อนวันงาน

> สำหรับทีมจัด/วิทยากร (ไม่ใช่เด็ก) เป้าหมาย: กันจุดที่พังหน้างาน

## ⚠️ Reset บอร์ด — 2 ระดับ อย่าสับสน

บอร์ดวนมาจากที่อื่น รหัส/ชื่อ/Wi-Fi เดิมไม่รู้

**ระดับเบา — App Lab → Settings (ใช้กับเด็กในห้อง)**
ไม่ต้อง jumper ไม่ต้องโหลด image เด็กเข้า Settings ตั้ง ชื่อ / Change password / Wi-Fi เองได้ (App Lab login เข้าบอร์ดให้อัตโนมัติ แม้ไม่รู้รหัสเดิม) → นี่คือ path ที่ให้เด็กทำ

**ระดับ nuclear — Flasher CLI + jumper (ห้ามให้เด็ก 20 ทีมทำพร้อมกัน)**
short 2 ขาเข้า flash mode + โหลด image >1GB/เครื่อง + flash 5–10 นาที ได้ factory state สะอาด (รหัสว่าง) แต่ 20 เครื่องพร้อมกัน = หายนะ → ใช้เฉพาะเครื่องตายสนิทที่ App Lab มองไม่เห็น ให้ TA ทำข้างห้อง

### ❗ ต้อง verify บนบอร์ดจริง 2 ตัวก่อนวันงาน
- App Lab → Settings → **Change password** ตั้งรหัสใหม่ได้โดยไม่ต้องกรอกรหัสเก่าไหม?
  - **ได้** → ระดับเบาพอ ปล่อยเด็กทำเอง
  - **ไม่ได้ (ต้องรหัสเก่า)** → ต้อง pre-reflash บอร์ดทั้งหมดคืนก่อนวันงาน (ทีมจัดทำ ไม่ใช่เด็กหน้างาน)

## 📶 Wi-Fi — จุดพังเงียบ

3 ช่วงโดนถล่มพร้อมกัน: first boot (check update), ต่อ Edge Impulse, upload data
- [ ] **Pre-update บอร์ดทุกตัวก่อนวันงาน** (ให้ check-for-updates จบ)
- [ ] **Pre-install** สคริปต์ `edge-impulse-linux` ลงบอร์ดทุกตัว (ดู [ta-playbook.md](ta-playbook.md))
- [ ] แยก SSID/AP สำหรับ workshop + hotspot สำรอง 1–2 ตัว

## 📦 อุปกรณ์ต่อทีม
- [ ] UNO Q + USB-C + power (5V/3A)
- [ ] Modulino kit (อย่างน้อย Movement + Pixels + Buzzer) + Qwiic ≥3
- [ ] **Powered USB hub** (จำเป็น) + USB Webcam + USB Mic
- [ ] Laptop (ลง App Lab ไว้ล่วงหน้า)

ของกลาง: projector, power strip, โต๊ะ TA + บอร์ดสำรอง ≥2 + hub สำรอง

## ✈️ Pre-Flight
- [ ] verify reset บนบอร์ด 2 ตัว (ด้านบน)
- [ ] บอร์ดทุกตัว pre-update + pre-install EI CLI
- [ ] บัญชี Edge Impulse + project ตัวอย่างพร้อมโชว์
- [ ] ลองครบ flow เอง 1 รอบ (reset → 3 input → train → deploy) — อย่าเจอ bug ครั้งแรกพร้อมเด็ก
- [ ] แชร์ลิงก์ repo + วิธี fork ให้ทุกทีม
