<!-- workshop-header -->
<img width="1347" height="127" alt="Coding Thailand 2026 header" src="https://github.com/user-attachments/assets/ba5cf267-f460-4fb0-b69b-c461ae061a3b" />

# 🛟 TA Playbook

> สำหรับทีม TA ใช้ตอนเข้าช่วยทีมที่ค้าง รวบรวมจากหน้างานโดยทีม TA (BUU ENG staff)

## วิธีเข้าช่วย (triage ให้ไว)
1. ถามทีมก่อน: ค้าง step ไหน / error อะไร / ลองอะไรไปแล้ว
2. เทียบกับ checkpoint ที่ทีมควรผ่าน (ดู [run-of-show.md](run-of-show.md)) — รู้ทันทีว่าค้างจุดไหน
3. แก้ตามคู่มือด้านล่าง ถ้าเกิน ~5 นาทียังไม่ขยับ → **สลับบอร์ด/อุปกรณ์สำรอง** ให้ทีมเดินต่อก่อน แล้วค่อยตามแก้ตัวเสีย
4. อย่าให้ 1 ทีมที่ค้างดูดเวลาจนทีมอื่นรอ

---

## 1) บอร์ดไม่ขึ้น / ไม่ boot
เช็ก: สายถูก/เสีย, ต่อ Wi-Fi ได้ไหม
- ⭐ **jumper pin เสียบค้าง** = boot ไม่ขึ้น (pin สำหรับ flash mode) ถอดออกก่อนเลย — เป็นสาเหตุอันดับต้นๆ
- ยังไม่ขึ้น App Lab เลย → สลับบอร์ดสำรอง แล้วเอาตัวเสียไป flash CLI ข้างห้อง

## 2) กล้อง/ไมค์ไม่ขึ้น หรือบอร์ดไม่เชื่อม (รอนาน)
หลักการ: ใช้กล้อง/ไมค์ = บอร์ดอยู่ **Wi-Fi** + peripheral เข้า **powered hub**
- ⚠️ **อย่าต่อบอร์ดเข้าคอมพร้อมใช้กล้อง** — จะหากล้องไม่เจอ
- หาบอร์ดไม่เจอใน App Lab → เดาได้ว่าบอร์ด/คอมคนละ Wi-Fi:
  1. ต่อบอร์ดเข้าคอมตรงๆ → App Lab → **Settings** → SSID/PASSWORD ให้ตรงคอม
  2. ถอด USB กลับไปต่อ Wi-Fi + hub เข้าบอร์ดผ่าน Wi-Fi
- เช็ก: `lsusb`, `ls /dev/video*`, `arecord -l`, สิทธิ์: `sudo usermod -aG video,audio arduino`

## 3) edge-impulse-linux
เปิด shell (`>_`) บนบอร์ด:
```bash
nano run.sh
```
```bash
curl -sL https://deb.nodesource.com/setup_20.x | sudo bash -
sudo apt install -y gcc g++ make build-essential nodejs sox \
  gstreamer1.0-tools gstreamer1.0-plugins-good \
  gstreamer1.0-plugins-base gstreamer1.0-plugins-base-apps
sudo npm install edge-impulse-linux -g --unsafe-perm
ls /dev/video*
arecord -l
```
`ctrl+x` → `y` → `enter` → `bash run.sh` → `edge-impulse-linux`
- login: email/password จาก Edge Impulse → account settings
- `command not found` → `sudo npm install edge-impulse-linux -g --unsafe-perm` ใหม่
- ถ้า pre-install มาแล้ว ข้ามขั้นนี้ได้เลย (แนะนำให้ทีมจัด pre-install ทุกบอร์ด ลด Wi-Fi คอขวด)

## 4) deploy/train พัง
- memory error ตอน deploy → ลด model size หรือลดจำนวน class
- accuracy ใน Studio สูงแต่ของจริงพัง → bias/variation ไม่พอ ให้เก็บข้อมูลหลายเงื่อนไขเพิ่ม
- inference ช้า/แกว่ง → ลด window + เพิ่ม overlap + เช็ก threshold

## ✨ Magic Trick (ก่อน escalate)
ทำ troubleshooting ครบแล้วยังไม่ได้ → **ถอดปลั๊กเสียบใหม่** → ยังไม่ได้ → escalate วิทยากร พร้อมสรุปว่าค้าง step ไหน ลองอะไรแล้ว

## โปรโตคอลบอร์ดสำรอง
- มีบอร์ดสำรอง ≥2 + hub สำรอง พร้อมที่โต๊ะ TA
- สลับให้ทีมก่อนเสมอถ้าตัวเดิมกินเวลาเกิน 5 นาที — งานเด็กอยู่บน Edge Impulse/GitHub (cloud) ไม่หายไปกับบอร์ด
