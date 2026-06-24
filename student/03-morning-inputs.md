<!-- workshop-header -->
<img width="1347" height="127" alt="Coding Thailand 2026 header" src="https://github.com/user-attachments/assets/ba5cf267-f460-4fb0-b69b-c461ae061a3b" />

# 🔌 ต่อ Input 3 แบบ

วันนี้จะลองต่อให้ครบทั้ง 3 แบบ บ่ายค่อยเลือก 1 อย่างไปเทรน
> UNO Q มีสองสมอง: **sensor (Modulino)** อ่านฝั่ง MCU ส่วน **กล้อง/ไมค์ (USB)** อ่านฝั่ง Linux

---

## Input 1 — Modulino Sensor (Qwiic)

1. ต่อสาย Qwiic: `UNO Q → Modulino Movement` (เสียบผิดด้านเข้าไม่ได้ ปลอดภัย)
2. ใน App Lab เปิด example อ่าน sensor (Movement / Accelerometer) แล้วกด Run
3. ขยับ sensor → ดูค่าขยับตาม

> ✅ **ผ่านเมื่อ:** ขยับ sensor แล้วตัวเลขขยับจริง

---

## Input 2 — USB Webcam

1. เสียบ webcam เข้า **powered USB hub** (hub ที่มีไฟเลี้ยง) แล้วต่อ hub เข้าบอร์ด
2. เปิด example ฝั่งกล้อง (camera) แล้ว Run → เห็นภาพสด

> ⚠️ ตอนใช้กล้อง ให้บอร์ดอยู่บน **Wi-Fi** **อย่าต่อบอร์ดเข้าคอมโดยตรง** ไม่งั้นจะหากล้องไม่เจอ

เช็กบน shell (`>_`) ได้: `lsusb` / `ls /dev/video*`

> ✅ **ผ่านเมื่อ:** เห็นภาพสดจากกล้อง

---

## Input 3 — USB Mic

1. เสียบ mic เข้า powered hub
2. เช็กบน shell: `arecord -l` (เห็น card ไมค์)
3. ลองพูด → เห็นสัญญาณเสียงขยับ

> ✅ **ผ่านเมื่อ:** พูดแล้วเห็นเสียงขยับ

---

ต่อไม่ขึ้น? เปิด [troubleshooting.md](troubleshooting.md) ข้อ 2

เสร็จเช้าแล้ว ลองคิดกันในทีม: อยากสอน AI ให้ **มอง / ฟัง / รู้สึก** อะไร? บ่ายจะได้เลือก input ไปเทรน
