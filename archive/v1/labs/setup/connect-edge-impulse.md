<!-- workshop-header -->
<img width="1347" height="127" alt="Coding Thailand 2026 header" src="https://github.com/user-attachments/assets/ba5cf267-f460-4fb0-b69b-c461ae061a3b" />

# 🔌 Connect Edge Impulse to UNO Q

> เชื่อม UNO Q ให้ขึ้นใน Edge Impulse Studio และทดสอบ data flow ให้ผ่านก่อนเริ่ม lab ของ track

---

## Overview

UNO Q เป็นบอร์ดสองสมอง — เราจะใช้ **Linux side** เป็น Edge Impulse device

เป้าหมายคือให้ทีมเชื่อมบอร์ด, login, และเห็น data flow เข้า Edge Impulse Studio ได้จริง

---

## Step 1: Boot UNO Q

1. เสียบ USB-C เข้า UNO Q + power adapter (5V/3A)
2. รอ ~30 วินาทีให้ Linux boot
3. ดู LED indicator → ติดต่อเนื่อง = พร้อมใช้

ถ้าบอร์ดยังไม่พร้อมใน step นี้ อย่าเพิ่งข้ามไป Wi-Fi หรือ CLI เพราะจะทำให้ debug ยากขึ้น

---

## Step 2: เชื่อม Wi-Fi

### Method A: ผ่าน Arduino App Lab (ง่ายสุด)

1. เปิด Arduino App Lab บน laptop
2. UNO Q จะปรากฏใน device list
3. คลิก device → Setup → Wi-Fi
4. เลือก network → ใส่ password

### Method B: ผ่าน ADB

```bash
# บน laptop
adb devices
adb shell

# ใน UNO Q shell
sudo nmcli device wifi connect "SSID" password "PASSWORD"
```

ถ้าทีมต้องการแค่ใช้งานให้ทันช่วงเช้า ให้เลือก Method A ก่อน แล้วค่อยใช้ Method B เมื่อจำเป็น

---

## Step 3: SSH เข้า UNO Q

```bash
# หา IP ของ UNO Q
adb shell ip addr show wlan0 | grep inet

# SSH (default: arduino / arduino)
ssh arduino@<IP>
```

> ⚠️ ถ้า password ผิด → reset ผ่าน `adb shell sudo passwd arduino`

จบ step นี้แล้ว ทีมควรมี shell ที่เข้า UNO Q ได้จริงอย่างน้อย 1 ช่องทาง

---

## Step 4: ติดตั้ง Edge Impulse CLI

ใน UNO Q terminal:

```bash
sudo apt update

# Node.js 20
curl -sL https://deb.nodesource.com/setup_20.x | sudo bash -

# Dependencies
sudo apt install -y gcc g++ make build-essential nodejs sox \
  gstreamer1.0-tools gstreamer1.0-plugins-good \
  gstreamer1.0-plugins-base gstreamer1.0-plugins-base-apps

# Edge Impulse Linux client
sudo npm install edge-impulse-linux -g --unsafe-perm
```

⏱️ ใช้เวลา ~5-10 นาที (ขึ้นกับ network)

ถ้า CLI ติดตั้งช้าผิดปกติ ให้เช็ก network ก่อนลงลึกกับคำสั่งอื่น

---

## Step 5: Connect Camera/Mic (ถ้าใช้)

ก่อนรัน Edge Impulse ต้องเสียบ peripherals ผ่าน USB Hub:

```bash
# เช็ค camera
ls /dev/video*

# เช็ค audio
arecord -l
```

---

## Step 6: Login + เลือก Project

```bash
edge-impulse-linux
```

จะเปิด wizard:
1. Email + password (จาก Edge Impulse account)
2. เลือก project ของทีม
3. ตั้งชื่อ device (เช่น `team-XX-unoq`)

✅ จะเห็นข้อความ "Your device is now connected to Edge Impulse"

ถ้ายังไม่เห็นข้อความนี้ ให้ถือว่ายังไม่จบ setup และอย่าเพิ่งไปเก็บ data ใน Studio

---

## Step 7: ทดสอบ Data Acquisition

ใน Edge Impulse Studio:
1. Devices tab → จะเห็น UNO Q connected
2. Data acquisition → Start sampling
3. เลือก sensor / camera / microphone
4. กด Start → เห็น data flow มาจาก UNO Q ✓

นี่คือ checkpoint สำคัญที่สุดของไฟล์นี้: ถ้า data ยังไม่ไหลเข้า Studio ให้แก้ตรงนี้ก่อนเริ่ม lab ของ track

---

## ⚠️ Troubleshooting

### "edge-impulse-linux command not found"

```bash
# เช็ค path
which edge-impulse-linux

# ถ้าไม่เจอ install ใหม่
sudo npm install edge-impulse-linux -g --unsafe-perm
```

### "Camera not detected"

```bash
# เช็ค USB
lsusb

# เช็ค driver
dmesg | tail -20

# ลอง re-plug + reboot
```

### "Permission denied" ตอน access camera/mic

```bash
sudo usermod -aG video,audio arduino
# logout แล้ว login ใหม่
```

### "32-bit OS error"

```
"You are attempting to deploy an .eim Edge Impulse model file
to a 32-bit operating system running on a 64-bit CPU."
```

→ ใช้ deployment target = **Arduino UNO Q** (ไม่ใช่ "Linux") ใน Studio

### Connection drop บ่อย

- เช็ค Wi-Fi signal
- เปลี่ยน Wi-Fi channel (5GHz ดีกว่า 2.4GHz)
- ลอง LAN cable แทน Wi-Fi

---

## ⏱️ Time Budget

ใน workshop ใช้ช่วง 09:30-10:00:
- 5 นาที: Boot + connect Wi-Fi
- 5 นาที: SSH access
- 10 นาที: Install dependencies (รอ apt)
- 5 นาที: Login Edge Impulse
- 5 นาที: Test data acquisition

ถ้าทีมใช้เกิน 30 นาทีแล้วยังไม่เห็น data ใน Studio ให้เรียก TA พร้อมบอกว่าไปค้างที่ step ไหนและลองอะไรมาแล้วบ้าง

---

## 📚 Reference

- [Arduino UNO Q docs](https://docs.edgeimpulse.com/hardware/boards/arduino-uno-q)
- [Edge Impulse Linux SDKs](https://docs.edgeimpulse.com/tools/libraries/sdks/inference/linux)
