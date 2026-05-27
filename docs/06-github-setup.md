<!-- workshop-header -->
<img width="1347" height="127" alt="Coding Thailand 2026 header" src="https://github.com/user-attachments/assets/ba5cf267-f460-4fb0-b69b-c461ae061a3b" />

# 🚀 GitHub Team Repo Setup

> ใช้ไฟล์นี้ตอนสร้าง repo ของทีมจาก `templates/team-repo-template/`

---

## Step 1: สร้าง repo ของทีมบน GitHub

1. ไป https://github.com/new
2. **Repository name:** `edge-ai-team-XX`
3. **คำอธิบาย repo:** `Edge AI workshop team repo — [ชื่อทีม]`
4. เลือก **Public**
5. ยังไม่ต้องติ๊กสร้าง README หรือ `.gitignore` ใหม่
6. กด **Create repository**

ถ้าทีมยังไม่มีชื่อ ให้ใช้เลขทีมไปก่อน แล้วค่อยกลับมาแก้ทีหลังได้

---

## Step 2: คัดลอก Team Template ลง repo

1. เปิดโฟลเดอร์ `templates/team-repo-template/` ใน workshop repo
2. คัดลอกไฟล์ทั้งหมดไปใส่ repo ของทีม
3. เปิด `README.md` ของทีมแล้วเปลี่ยน placeholder ให้เป็นชื่อทีม, track, และโจทย์ของตัวเอง

ไฟล์ที่ควรแก้ทันทีมีอย่างน้อย:

- `README.md`
- `docs/track-selection.md`
- `worksheets/W1-class-design.md`

---

## Step 3: Clone ลงเครื่องและ commit แรก

```bash
git clone https://github.com/your-team/edge-ai-team-XX.git
cd edge-ai-team-XX

git add .
git commit -m "init: เริ่ม repo ทีมและใส่ template"
git push -u origin main
```

ถ้า `main` ยังไม่มี ให้สร้างก่อน push:

```bash
git branch -M main
git push -u origin main
```

---

## Step 4: ชวนเพื่อนในทีมและตั้ง workflow ให้ตรงกัน

### เพิ่มเพื่อนเข้า repo

1. GitHub repo → **Settings** → **Collaborators**
2. เชิญเพื่อนในทีมทุกคนเข้า repo
3. ให้ทุกคน clone repo ลงเครื่องตัวเอง

### เลือกวิธีทำงานร่วมกัน

มี 2 แบบที่ใช้ได้:

- แบบเร็ว: commit ลง `main` โดยดึงงานล่าสุดก่อนทุกครั้ง
- แบบแยกงาน: สร้าง branch แล้วเปิด Pull Request ก่อน merge

ถ้าทีมเพิ่งเริ่ม Git ให้ใช้แบบเร็ว แต่ยังควรเขียน commit message ให้ชัด

---

## Step 5: จังหวะ commit ที่ควรมี

ไม่ต้องรอให้เสร็จทั้งวันค่อย push ควรมีหลักฐานเป็นช่วง ๆ เช่น:

- หลังเลือก track และกรอก W1
- หลังเก็บข้อมูลชุดแรก
- หลัง train V1
- หลังบันทึก Prediction Log
- หลังเปรียบเทียบ V1 กับ V2

ตัวอย่าง tag ปิดรอบท้ายวัน:

```bash
git tag day1-final
git push origin day1-final
```

---

## ✅ เช็กว่าพร้อมใช้งานแล้ว

- [ ] repo ทีมถูกสร้างบน GitHub แล้ว
- [ ] ทุกคนในทีมเข้า repo ได้
- [ ] commit แรกขึ้น GitHub แล้ว
- [ ] `README.md` ของทีมถูกแก้ placeholder แล้ว
- [ ] `docs/track-selection.md` และ `worksheets/W1-class-design.md` เริ่มกรอกแล้ว

---

## 🆘 ปัญหาที่พบบ่อย

### Push ไม่ขึ้น (HTTPS)

GitHub ตอนนี้ไม่ใช้ password แบบเดิมแล้ว ถ้าถูกถามเรื่อง auth ให้ใช้ GitHub CLI:

```bash
gh auth login
```

### เผลอ commit ไฟล์ใหญ่

ให้เอาไฟล์นั้นออกจาก git แล้วปล่อยให้ dataset อยู่ที่ Edge Impulse แทน

```bash
git rm --cached path/to/big-file.zip
git commit -m "fix: เอาไฟล์ใหญ่ออกจาก repo"
git push
```

### มี merge conflict

กลับไปดู [05-troubleshooting.md](05-troubleshooting.md) แล้วแก้ทีละไฟล์ อย่ารีบกดทับทั้งก้อน

---

## 📚 References

- [GitHub Docs - Collaborators](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/managing-repository-settings/inviting-collaborators-to-a-personal-repository)
- [GitHub CLI](https://cli.github.com/)
