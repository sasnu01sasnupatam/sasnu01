<!-- workshop-header -->
<img width="1347" height="127" alt="Coding Thailand 2026 header" src="https://github.com/user-attachments/assets/ba5cf267-f460-4fb0-b69b-c461ae061a3b" />

# Team [XX] — [Project Name]

> Coding Thailand 2026 — Edge AI Workshop
> [Track A / B / C / D] — [Basic / Intermediate / Advanced]

README ของทีมควรทำหน้าที่เป็นหน้าเล่าเรื่องหลัก: คนอ่านควรเข้าใจได้ภายในไม่กี่นาทีว่าทีมกำลังแก้ปัญหาอะไร ใช้ model แบบไหน และ V2 ดีขึ้นจาก V1 ยังไง

---

## 👥 ทีมเรา

| ชื่อ | บทบาท 3H | GitHub |
|---|---|---|
| | Hacker | @username |
| | Hipster | @username |
| | Hustler | @username |

---

## 🎯 โจทย์

**Problem:** _ปัญหาที่ทีมจะแก้_

**Target user:** _กลุ่มเป้าหมาย_

**Edge AI value:** _ทำไม Edge ไม่ใช่ Cloud_

---

## 🔬 Approach

### Track เลือก
- [ ] A — Motion (Modulino Movement)
- [ ] B — Vision (USB Webcam)
- [ ] C — Environment (Thermo + Light + Distance)
- [ ] D — Audio (USB Microphone)

### Classes

1. _Class 1_
2. _Class 2_
3. _Class 3_

### Output
- [ ] Modulino Pixels
- [ ] Modulino Buzzer
- [ ] Modulino LED Matrix
- [ ] อื่นๆ: ___

---

## 🔗 Links

- **Edge Impulse Project:** https://studio.edgeimpulse.com/studio/XXXXX
- **Prototype Video (ถ้ามี):** _ใส่ลิงก์เมื่อพร้อม_

---

## 📊 Results

กรอกเฉพาะตัวเลขที่ทีมวัดได้จริงจากการ train และ test ไม่ต้องเดาตัวเลขให้ดูดี

| Metric | V1 | V2 | Δ |
|---|---|---|---|
| Validation accuracy | __% | __% | +/-__% |
| Test accuracy | __% | __% | +/-__% |
| Inference time | __ms | __ms | +/-__ms |
| Model size | __KB | __KB | +/-__KB |

---

## 📂 Repository Structure

```
.
├── README.md                  ← ตอนนี้อ่านอยู่ที่นี่
├── docs/
│   ├── setup-notes.md        ← บันทึก setup
│   ├── track-selection.md    ← เหตุผลเลือก track
│   ├── v1-vs-v2.md          ← เปรียบเทียบ model
│   └── issues-and-fixes.md   ← ปัญหาที่เจอ
├── worksheets/
│   ├── W1-class-design.md
│   ├── W2-data-collection-log.md
│   ├── W3-prediction-log.md
│   └── W4-idea-canvas.md
├── code/
│   ├── v1/                   ← Code Model V1
│   └── v2/                   ← Code Model V2
├── assets/
│   └── workspace.jpg         ← ภาพ workspace
└── logs/
    └── predictions.csv       ← Prediction log (machine-readable)
```

---

## ✅ เช็กก่อนสรุปรอบแรก

- [ ] Worksheet W1-W4 ครบ
- [ ] Edge Impulse project link ใน README
- [ ] Model V1 + V2 deploy ได้
- [ ] Prediction log ≥10 cases
- [ ] V1 vs V2 comparison
- [ ] Commit ≥10 ครั้ง
- [ ] ทุกคนในทีมเป็น Contributor

---

## 🤝 วิธีอัปเดต repo ทีม

```bash
# Clone repo
git clone <url>
cd <repo>

# สร้าง branch ใหม่ก่อนแก้
git checkout -b feat/your-feature

# แก้ไข แล้ว commit
git add .
git commit -m "docs: เพิ่มชื่อทีมและลิงก์โปรเจกต์"
git push -u origin feat/your-feature

# เปิด Pull Request บน GitHub
```

---

## 📜 License

MIT
