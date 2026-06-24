<!-- workshop-header -->
<img width="1347" height="127" alt="Coding Thailand 2026 header" src="https://github.com/user-attachments/assets/ba5cf267-f460-4fb0-b69b-c461ae061a3b" />

# 📅 ตารางวันนี้

ทีมเดินไปพร้อมกันทั้งห้อง ทำทีละช่วง เช็คให้ผ่านก่อนค่อยไปต่อ

## เช้า — ทำให้ UNO Q อยู่มือ

| เวลา | ทำอะไร | คู่มือ |
|---|---|---|
| 09:00–09:20 | เปิดงาน + ดูตัวอย่างปลายทาง | — |
| 09:20–09:40 | รู้จัก UNO Q (สองสมอง MCU/Linux) | — |
| 09:40–10:30 | ตั้งบอร์ดให้เป็นของทีม | [02-setup-board.md](02-setup-board.md) |
| 10:30–11:00 | ต่อ Input 1 — Sensor | [03-morning-inputs.md](03-morning-inputs.md) |
| 11:00–11:30 | ต่อ Input 2 — Webcam | [03-morning-inputs.md](03-morning-inputs.md) |
| 11:30–11:50 | ต่อ Input 3 — Mic | [03-morning-inputs.md](03-morning-inputs.md) |
| 11:50–12:00 | คิดว่าบ่ายจะใช้ input ไหนเทรน | — |

## บ่าย — เทรนจริงด้วย Edge Impulse

| เวลา | ทำอะไร | คู่มือ |
|---|---|---|
| 13:00–13:30 | รู้จัก Edge Impulse + ออกแบบ class | [04-edge-impulse.md](04-edge-impulse.md) |
| 13:30–14:00 | เชื่อมบอร์ดเข้า Edge Impulse | [04-edge-impulse.md](04-edge-impulse.md) |
| 14:00–14:45 | เก็บข้อมูล + เทรน V1 | [04-edge-impulse.md](04-edge-impulse.md) |
| 14:45–15:30 | ลง model บนบอร์ด + ลองจริง | [04-edge-impulse.md](04-edge-impulse.md) |
| 15:30–16:00 | ทดสอบ 10 cases + จดผล | [04-edge-impulse.md](04-edge-impulse.md) |
| 16:00–16:30 | ส่งงาน + โชว์ทีมละ 1–2 นาที | [05-submit.md](05-submit.md) |

## เคล็ดลับ

- ติดเกิน 5 นาที จดไว้ก่อนว่าติดอะไร error อะไร ลองอะไรไปแล้ว
- อย่าดูแค่ accuracy ใน Studio ดูตอนลองจริงด้วย
- ถ้าเวลาไม่พอ ลด scope ให้จบก่อน (ลด class / ลดจำนวนตัวอย่าง) ดีกว่าทำใหญ่แล้วไม่จบ
