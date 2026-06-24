<!-- workshop-header -->
<img width="1347" height="127" alt="Coding Thailand 2026 header" src="https://github.com/user-attachments/assets/ba5cf267-f460-4fb0-b69b-c461ae061a3b" />

# 🤖 Coding Thailand 2026 — Edge AI Workshop
> **Day 1 Workshop Materials**
> Arduino UNO Q × Modulino × Edge Impulse
> Regional Coding & AI Competition — ห้อง Edge AI

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Arduino UNO Q](https://img.shields.io/badge/Arduino-UNO%20Q-00979D)](https://www.arduino.cc/)
[![Edge Impulse](https://img.shields.io/badge/Edge%20Impulse-Studio-3B4252)](https://edgeimpulse.com/)

---

## 📋 ภาพรวม Workshop

Workshop 6 ชั่วโมง (09:00–17:00) ที่สอนให้นักเรียน
สร้าง Edge AI prototype ด้วย Arduino UNO Q + Modulino + Edge Impulse จบใน 1 วัน — เพื่อเตรียมไปแข่ง Prototype Day ในวันถัดไป

Repo นี้ควรอ่านเหมือนคู่มือระหว่างเรียน: เปิดดูตามลำดับ, ทำทีละช่วง, แล้วเช็กว่า repo ทีมมีหลักฐานครบตามที่ต้องส่ง

### 🎯 เป้าหมายการเรียนรู้

หลังจบ Day 1 นักเรียนจะ:
- เข้าใจ pipeline ของ Edge AI: **Data → Train → Deploy → Inference**
- ออกแบบ class & dataset ที่ลดอคติ (bias-aware)
- Train โมเดลผ่าน Edge Impulse Studio + Deploy ลง UNO Q
- จดบันทึก Prediction Log อย่างเป็นระบบ
- ใช้ Git/GitHub ในการเก็บหลักฐานและ version model

### 📊 เกณฑ์คะแนน Skill Assessment (30 คะแนน)

| หัวข้อ | น้ำหนัก | คะแนนเต็ม |
|---|---|---|
| Setup & Safety | 1.25 | 5 |
| Core Implementation | 2.5 | 10 |
| Data & Testing | 1.25 | 5 |
| Debug & Explain | 1.25 | 5 |
| Participation & Collaboration | 1.25 | 5 |

สำหรับเกณฑ์ Day 2–3 ดูเพิ่มที่ [docs/07-day2-day3-rubric.md](docs/07-day2-day3-rubric.md)

---

## 🗂️ โครงสร้าง Repository

```
coding-thailand-edge-ai-workshop/
├── README.md                          ← ตอนนี้อ่านอยู่ที่นี่
├── docs/                              ← เอกสารหลัก
│   ├── 01-schedule.md                 ← Timeline 6 ชม.
│   ├── 02-tracks.md                   ← 4 Tracks ให้เลือก
│   ├── 03-rubric.md                   ← เกณฑ์ให้คะแนน
│   ├── 04-git-basics.md               ← Git 101 สำหรับนักเรียน
│   ├── 05-troubleshooting.md          ← ปัญหาที่พบบ่อย
│   ├── 06-github-setup.md             ← การตั้งค่า GitHub ก่อนเริ่ม
│   └── 07-day2-day3-rubric.md         ← เกณฑ์รอบ prototype + pitch
├── slides/                            ← Slide outline
│   ├── day1-outline.md
│   └── day2-outline.md
├── worksheets/                        ← ใบงานนักเรียน
│   ├── W1-class-design.md
│   ├── W2-data-collection-log.md
│   ├── W3-prediction-log.md
│   └── W4-idea-canvas.md
├── labs/                              ← Lab manual แต่ละ track
│   ├── README.md                      ← ทางเข้า lab ทั้งหมด
│   ├── common/                        ← คู่มือกลางที่ใช้ทุก track
│   ├── anchor-demo/                   ← Gesture Wand (demo เปิด)
│   ├── track-a-motion/                ← Motion classification
│   ├── track-b-vision/                ← Vision classification
│   ├── track-c-environment/           ← Multi-sensor fusion
│   └── track-d-audio/                 ← Audio classification
├── templates/                         ← Template สำหรับสร้าง repo ทีม
│   ├── team-repo-template/            ← Template repo ของทีม
│   └── pull-request-template.md
└── assets/                            ← รูป diagram
```

---

## 🚀 เริ่มตรงนี้ (สำหรับนักเรียน)

ถ้าเพิ่งเข้ามาใน workshop นี้ ให้ทำตามลำดับนี้:

1. ดู [Timeline วันนี้](docs/01-schedule.md) เพื่อรู้ว่าช่วงไหนต้องทำอะไร
2. เลือก [Track ของทีม](docs/02-tracks.md) ให้เหมาะกับเวลาที่มีและความถนัดของทีม
3. เรียน [Git พื้นฐาน 15 นาที](docs/04-git-basics.md) ถ้ายังไม่คล่อง Git/GitHub
4. สร้าง repo ทีมใหม่จาก [Team Repo Template](templates/team-repo-template/)
5. เปิด [Labs Overview](labs/README.md) แล้วทำตาม lab manual ของ track ที่เลือก

เริ่มจากคัดลอกไฟล์ในโฟลเดอร์ template ไปใส่ repo ใหม่ของทีมบน GitHub แล้วค่อย clone repo นั้นลงเครื่อง

ถ้าติดขัดระหว่างทาง ให้เปิด [Troubleshooting Guide](docs/05-troubleshooting.md) ก่อนขอความช่วยเหลือ

## ✅ ก่อนจบวัน ทีมควรมี

- [ ] Dataset ที่ใช้ train (อยู่ใน `dataset/` หรือ link Edge Impulse project)
- [ ] Model V1 และ V2 (export จาก Edge Impulse)
- [ ] Prediction Log อย่างน้อย 10 cases (`logs/predictions.csv`)
- [ ] Idea Canvas สำหรับรอบต่อยอด (`worksheets/W4-idea-canvas.md`)
- [ ] README ของทีม อธิบายโจทย์, classes, ผลลัพธ์

## 🧰 ถ้าต้องเตรียมเวิร์กช็อป

เวลาเตรียมเวิร์กช็อป ให้เริ่มจาก:

1. เตรียมของที่จะแจกแต่ละทีมตาม [docs/00-equipment-checklist.md](docs/00-equipment-checklist.md)
2. ปรับ [docs/01-schedule.md](docs/01-schedule.md) และ [slides/day1-outline.md](slides/day1-outline.md) ให้ตรงกับบริบทของห้อง
3. ใช้ [docs/03-rubric.md](docs/03-rubric.md) เป็นเกณฑ์ประเมินกลาง
4. ใช้ [docs/07-day2-day3-rubric.md](docs/07-day2-day3-rubric.md) สำหรับรอบ prototype + pitch

ไฟล์ที่ใช้ตอนเตรียมห้องและเปิดเวิร์กช็อป:
- [docs/00-equipment-checklist.md](docs/00-equipment-checklist.md)
- [slides/day1-outline.md](slides/day1-outline.md)
- [slides/day2-outline.md](slides/day2-outline.md)
- [labs/anchor-demo/README.md](labs/anchor-demo/README.md)

---

## 🛠️ Tech Stack

- **Hardware**: Arduino UNO Q (Qualcomm Dragonwing QRB2210) + Modulino nodes
- **ML Platform**: Edge Impulse Studio
- **Deploy**: Arduino App Lab
- **Version Control**: Git + GitHub

---

## 📜 License

MIT — ใช้สอนต่อ ปรับแต่งได้อย่างเสรี

## 🙏 Credits

จัดทำสำหรับ **โครงการ Coding Thailand 2026 — AI Inspires the Future** โดย DEPA และทีมพัฒนาเนื้อหา

---

**Questions?** เปิด [Issue](https://github.com/SANCHAIE/coding-thailand-edge-ai-workshop/issues) หรือ [Discussion](https://github.com/SANCHAIE/coding-thailand-edge-ai-workshop/discussions)
