<!-- workshop-header -->
<img width="1347" height="127" alt="Coding Thailand 2026 header" src="https://github.com/user-attachments/assets/ba5cf267-f460-4fb0-b69b-c461ae061a3b" />

# Edge AI Pipeline Diagram

> Reference diagram สำหรับ slide / docs

## Full Pipeline

```mermaid
flowchart LR
    A[📥 Data Collection] --> B[🧠 Edge Impulse Studio]
    B --> C[⚙️ Create Impulse]
    C --> D[🏋️ Train Model<br/>nano + GPU]
    D --> E[📊 Validate<br/>Confusion Matrix]
    E -->|accuracy ok| F[📤 Deploy to UNO Q]
    E -->|accuracy low| A
    F --> G[🧪 10-Case Test]
    G --> H{Pass criteria?}
    H -->|Yes| I[🎉 V1 Complete]
    H -->|No| J[🔄 Iterate]
    J --> A
    I --> K[Continue to V2]
```

## Hardware Flow (Track A)

```mermaid
flowchart LR
    M[Modulino<br/>Movement] -->|Qwiic I2C| U[UNO Q]
    U -->|inference| L[Linux side]
    L -->|class| O[Modulino<br/>Pixels + Buzzer]
```

## Team Workflow

```mermaid
flowchart TD
    T[Team forms] --> R[Create Team Repo]
    R --> S[Setup UNO Q + Edge Impulse]
    S --> W1[W1: Class Design]
    W1 --> D[Data Collection]
    D --> W2[W2: Data Log]
    W2 --> TR[Train V1]
    TR --> DP[Deploy V1]
    DP --> W3[W3: Prediction Log]
    W3 --> IT[Iterate V2]
    IT --> CMP[V1 vs V2]
    CMP --> W4[W4: Idea Canvas]
    W4 --> END[Day 1 Complete]
```

## Skill Assessment Mapping

```mermaid
flowchart LR
    A[Hardware Setup] --> S1[Setup & Safety<br/>5 pts]
    B[Train + Deploy V1+V2] --> S2[Core Implementation<br/>10 pts]
    C[Prediction Log + Compare] --> S3[Data & Testing<br/>5 pts]
    D[Commit msgs + docs] --> S4[Debug & Explain<br/>5 pts]
    E[Team commits + PRs] --> S5[Participation<br/>5 pts]
    S1 & S2 & S3 & S4 & S5 --> T[Total 30 pts]
```

---

**Note:** GitHub renders Mermaid natively — ไฟล์นี้แสดงเป็น diagram บน GitHub
