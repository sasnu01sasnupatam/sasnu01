<!-- workshop-header -->
<img width="1347" height="127" alt="Coding Thailand 2026 header" src="https://github.com/user-attachments/assets/ba5cf267-f460-4fb0-b69b-c461ae061a3b" />

# 🌳 Git & GitHub 101 — 15 นาทีเริ่มได้

> สำหรับนักเรียนที่ไม่เคยใช้ Git มาก่อน — อ่าน 15 นาที พอใช้งานใน workshop ได้

---

## ทำไมต้องใช้ Git?

ลองนึกภาพว่าทีมกำลัง train AI:
- Model V1 → accuracy 70%
- ลองแก้ → accuracy เหลือ 50% 😱
- "อยาก revert กลับไป V1!"

**Git** คือเครื่องมือที่ช่วย:
- ✅ บันทึก state ของโค้ด/ข้อมูล แต่ละช่วงเวลา (เรียกว่า **commit**)
- ✅ ย้อนกลับไป state ก่อนหน้าได้
- ✅ ทำงานร่วมกันหลายคนโดยไม่ทับโค้ดกัน

**GitHub** คือ cloud ที่เก็บ Git repository ของทีม

---

## 📚 คำศัพท์ที่ต้องรู้ (อ่านครั้งเดียวพอ)

| คำ | ความหมาย | อธิบายแบบบ้านๆ |
|---|---|---|
| **Repository (Repo)** | โฟลเดอร์โปรเจกต์ที่ติดตาม version | "ตู้เก็บงาน" |
| **Commit** | บันทึก snapshot ของงาน | "ถ่ายรูปงาน ณ ตอนนี้" |
| **Push** | อัพโหลด commit ขึ้น GitHub | "ส่งงานเข้า cloud" |
| **Pull** | ดาวน์โหลดจาก GitHub | "ดึงงานล่าสุดมา" |
| **Fork** | คัดลอก repo คนอื่นเป็นของตัวเอง | "ก๊อปต้นแบบมา" |
| **Clone** | ดาวน์โหลด repo ลงเครื่อง | "เอา repo ลง laptop" |
| **Branch** | สาขางาน (เริ่มที่ `main`) | "ทำหลาย version พร้อมกัน" |
| **Pull Request (PR)** | ขอ merge งานเข้า main | "ขอเอางานของฉันเข้าหลัก" |

---

## 🚀 Hands-On — 5 ขั้นตอน Setup ทีม

### Step 1: สมัคร GitHub (1 ครั้ง, ฟรี)

ทุกคนในทีมต้องมีบัญชี GitHub:
1. ไป https://github.com/signup
2. ใช้ email + username ที่จำง่าย
3. **แนะนำ:** ใช้ email จริง ไม่ใช่ใช้ครั้งเดียวแล้วทิ้ง

### Step 2: ติดตั้ง Git บน laptop

**Windows:** ดาวน์โหลด https://git-scm.com/download/win
**Mac:** เปิด Terminal → `xcode-select --install`
**Linux:** `sudo apt install git`

ทดสอบ:
```bash
git --version
# git version 2.x.x
```

### Step 3: ตั้งชื่อ + email ครั้งแรก

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

### Step 4: สร้าง repo ทีมจาก Team Repo Template

1. ไปที่ repo ของ workshop (ลิงก์ที่ทีมใช้อยู่)
2. เปิดโฟลเดอร์ `templates/team-repo-template/`
3. สร้าง repo ใหม่ใน account ของทีมชื่อ `edge-ai-team-XX` (XX = เลขทีม)
4. คัดลอกไฟล์จาก template ไปใส่ repo ทีม
5. ตั้ง repo เป็น **Public** เพื่อให้เพื่อนร่วมทีมและ TA เห็นความคืบหน้าได้

ถ้ามี template repo แยกต่างหากอยู่แล้ว ให้ใช้ repo นั้นแทนโฟลเดอร์ template ใน workshop repo นี้

### Step 5: Clone ลง laptop

```bash
# เปลี่ยน URL ให้ตรงกับ repo ของทีม
git clone https://github.com/your-team/edge-ai-team-XX.git
cd edge-ai-team-XX
```

---

## 🔄 Daily Workflow ใน Workshop

### Cycle 1: เมื่อทำงานเสร็จส่วนหนึ่ง

```bash
# ดูว่ามีอะไรเปลี่ยนบ้าง
git status

# Add ไฟล์ที่อยากบันทึก
git add .
# หรือเฉพาะไฟล์: git add worksheets/W1-class-design.md

# Commit พร้อมข้อความ
git commit -m "feat: define 3 classes for Track A motion sensor"

# Push ขึ้น GitHub
git push
```

### Cycle 2: ดึงงานล่าสุดจากเพื่อนในทีม

```bash
git pull
```

---

## ✍️ เขียน Commit Message ให้ทีมอ่านแล้วเข้าใจ

นี่คือส่วนสำคัญที่สุดของหัวข้อ **Debug & Explain** (5 คะแนน)

### Format ที่แนะนำ

```
<type>: <สิ่งที่ทำ> เพราะ <เหตุผล>
```

### Types

| Type | ใช้เมื่อ |
|---|---|
| `feat` | เพิ่ม feature ใหม่ |
| `fix` | แก้ bug |
| `data` | เพิ่ม/แก้ dataset |
| `train` | train model ใหม่ |
| `docs` | แก้เอกสาร |
| `test` | เพิ่ม test |
| `refactor` | rearrange โค้ดไม่ได้เปลี่ยนผล |

### ตัวอย่างจริง

❌ **แย่:**
```
update
fix
test123
asdf
```

✅ **ดี:**
```
feat: เพิ่ม class "ล้ม" เพราะโจทย์ต้องการ fall detection
data: เก็บข้อมูล "เดิน" เพิ่ม 30 samples เพราะ V1 confusion สูง
train: ลด model size จาก small เป็น nano เพราะ memory error บน UNO Q
fix: แก้ Qwiic cable ปกติ การต่อ Movement ก่อน Pixels ตามลำดับ I2C
docs: เพิ่มผลการทดสอบ V1 vs V2 ลง docs/v1-vs-v2.md
```

---

## 🤝 ทำงานเป็นทีม — Pull Request Workflow

### เมื่ออยากให้เพื่อนช่วย review งานก่อน merge:

```bash
# สร้าง branch ใหม่
git checkout -b feat/add-fall-class

# ทำงาน...
git add .
git commit -m "feat: เพิ่ม class ล้ม"
git push -u origin feat/add-fall-class
```

แล้วบน GitHub:
1. กด **Compare & pull request**
2. เพิ่มคำอธิบายสั้น ๆ
3. กด **Create pull request**
4. ให้เพื่อนในทีม review + approve
5. Merge

**ทำไมถึงสำคัญ?** Pull Request = หลักฐาน **Participation & Collaboration** (5 คะแนน)

---

## 🆘 Cheat Sheet — Commands ที่ใช้บ่อย

```bash
# ดูสถานะ
git status

# ดู history
git log --oneline

# Add ไฟล์
git add <file>             # ไฟล์เดียว
git add .                  # ทั้งหมด

# Commit
git commit -m "message"

# Push/Pull
git push
git pull

# สร้าง/เปลี่ยน branch
git checkout -b new-branch
git checkout main

# Undo (ยังไม่ commit)
git restore <file>

# ดู remote
git remote -v
```

---

## ❓ ปัญหาที่พบบ่อย

### "เผลอ commit ไฟล์ใหญ่ (เช่น dataset)"

```bash
# ลบไฟล์ออกจาก git แต่เก็บใน folder
git rm --cached big-file.zip
echo "big-file.zip" >> .gitignore
git commit -m "fix: ลบไฟล์ใหญ่ออกจาก tracking"
```

### "Push ไม่ขึ้น เพราะ remote มีอะไรใหม่"

```bash
git pull --rebase
git push
```

### "ลืม commit ก่อนปิด laptop"

ไม่เป็นไร — กลับมาเปิดใหม่ Git ยังจำ state ไว้ ทำ `git status` ดูได้

### "อยากรู้ใครแก้บรรทัดไหน"

```bash
git blame <file>
```

---

## 📚 อยากเก่งกว่านี้?

- **Interactive Tutorial:** https://learngitbranching.js.org/
- **GitHub Docs:** https://docs.github.com/en/get-started
- **Pro Git Book (ฟรี):** https://git-scm.com/book

---

## 🎯 สิ่งที่ทีมต้องทำใน Workshop

- [ ] ทุกคนมี GitHub account
- [ ] สร้าง repo ทีมจาก template
- [ ] ทุกคน commit อย่างน้อย 3 ครั้ง
- [ ] อย่างน้อย 1 Pull Request ที่ทีมรีวิวกัน
- [ ] Commit message ตาม format
- [ ] Push ก่อนเลิก workshop
