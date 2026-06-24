<!-- workshop-header -->
<img width="1347" height="127" alt="Coding Thailand 2026 header" src="https://github.com/user-attachments/assets/ba5cf267-f460-4fb0-b69b-c461ae061a3b" />

# 🔧 Setup Notes

> บันทึกการ setup hardware + software ของทีม

จดเฉพาะสิ่งที่ทีมใช้จริง จะช่วยให้ย้อนกลับมาแก้ setup หรืออธิบายงานตอนสรุปผลได้ง่ายขึ้น

---

## Hardware ที่ใช้

- [ ] Arduino UNO Q ___GB
- [ ] Modulino Movement
- [ ] Modulino Distance
- [ ] Modulino Thermo
- [ ] Modulino Light
- [ ] Modulino Buttons
- [ ] Modulino Knob
- [ ] Modulino Buzzer
- [ ] Modulino Pixels
- [ ] USB Webcam: ___
- [ ] USB Microphone: ___
- [ ] USB Hub (powered): ___

## Connection Topology

```
[วาด ASCII art โครงสร้างการต่อ Modulino + UNO Q]

ตัวอย่าง:
UNO Q ─Qwiic─→ Movement ─Qwiic─→ Pixels ─Qwiic─→ Buzzer
```

## ภาพ Workspace

ใส่ภาพไว้ที่ `assets/workspace.jpg` แล้วเปิดบรรทัดด้านล่างเมื่อมีรูปจริง:

```md
![Workspace](../assets/workspace.jpg)
```

## Software ที่ติดตั้ง

- [ ] Arduino App Lab v___
- [ ] Edge Impulse CLI (`edge-impulse-linux`)
- [ ] Git v___
- [ ] (อื่นๆ)

## Edge Impulse Project

- **URL:** https://studio.edgeimpulse.com/studio/_____
- **Project name:** team-XX-_____
- **Owner account:** _____@____

## ปัญหา Setup ที่เจอ

ดู `docs/issues-and-fixes.md`
