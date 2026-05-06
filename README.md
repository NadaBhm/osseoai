<div align="center">
  <img width="60" src="https://img.icons8.com/fluency/96/stethoscope.png" alt="OsseoAI Logo"/>
  <h1>OsseoAI</h1>
  <p><strong>AI-Powered Bone Fracture Detection Platform for Clinical Use</strong></p>
  <p>
    <img src="https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=white" />
    <img src="https://img.shields.io/badge/FastAPI-Python-009688?logo=fastapi&logoColor=white" />
    <img src="https://img.shields.io/badge/PostgreSQL-18-336791?logo=postgresql&logoColor=white" />
    <img src="https://img.shields.io/badge/PyTorch-MobileNetV2-EE4C2C?logo=pytorch&logoColor=white" />
    <img src="https://img.shields.io/badge/License-MIT-green" />
  </p>
</div>

---

## Overview

OsseoAI is a secure, full-stack medical imaging platform that allows doctors to upload X-ray images and receive AI-powered fracture diagnoses in real time. It combines a modern React interface with a custom deep learning pipeline built on the MURA musculoskeletal dataset.

The system uses a **two-stage AI pipeline**:
1. A **general multi-task model** (MultiTask AttentionMIL) automatically identifies the bone type from the X-ray
2. A **specialist model** (per-bone AttentionMIL on MobileNetV2) runs fracture detection with bone-specific preprocessing

---

## Features

- **Doctor authentication** — JWT-based login, registration, and admin approval workflow
- **Automatic bone type detection** — general model routes to the correct specialist automatically
- **7 specialist models** — Shoulder, Wrist, Elbow, Finger, Forearm, Hand, Humerus
- **Per-patient X-ray storage** — every scan organized by patient folder
- **Analysis history & reports** — full audit trail of all diagnoses per doctor
- **Admin panel** — manage doctor accounts and monitor all system scans
- **Settings & 2FA** — doctor profile management with two-factor authentication setup
- **Documentation & Roadmap** — built-in pages for platform guidance
- **Dark UI** — professional medical-grade interface built with Tailwind + Shadcn

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 19, TypeScript, Tailwind CSS, Shadcn UI, Vite |
| Backend | FastAPI (Python), SQLAlchemy ORM |
| AI Models | PyTorch, MobileNetV2, AttentionMIL, Frangi filter preprocessing |
| Database | PostgreSQL (production) / SQLite (development) |
| Auth | JWT tokens, bcrypt password hashing, 2FA support |

---

## AI Pipeline

X-ray uploaded
↓
General Model (MultiTaskAttentionMIL)
↓              ↓
bone_type      quick fracture check
↓
Specialist Model (bone-specific AttentionMIL + Frangi preprocessing)
↓
Final result: bone type + fracture probability + confidence score

All models are trained on the **MURA v1.1** dataset using Multiple Instance Learning (MIL) with attention pooling, allowing the system to process full study folders (multiple views) as a single bag.

---

## Project Structure

osseoai/
├── src/
│   ├── components/
│   │   ├── Navbar.tsx
│   │   ├── ProtectedRoute.tsx
│   │   └── TwoFactorSetupModal.tsx
│   ├── hooks/
│   │   └── useAuth.tsx
│   └── pages/
│       ├── Home.tsx
│       ├── Auth.tsx
│       ├── Dashboard.tsx
│       ├── Patients.tsx
│       ├── Reports.tsx
│       ├── Admin.tsx
│       ├── Settings.tsx
│       ├── Documentation.tsx
│       ├── Roadmap.tsx
│       └── Pending.tsx
├── components/ui/              # Shadcn UI components
│   ├── button.tsx
│   ├── card.tsx
│   ├── dialog.tsx
│   ├── input.tsx
│   ├── select.tsx
│   ├── table.tsx
│   └── ...
├── lib/
│   └── utils.ts
├── main.py                     # FastAPI — all API routes
├── predictor.py                # Full AI inference pipeline
├── database.py                 # SQLAlchemy models + PostgreSQL
├── auth.py                     # JWT auth logic
├── migrate_db.py               # Database migration utility
├── checkpoints/                # Trained .pth model files (not committed)
│   ├── general_best.pth
│   ├── shoulder_best.pth
│   ├── wrist_best.pth
│   └── ...
└── uploads/                    # Patient X-ray images (auto-created)
└── patient_{id}/
---

## Getting Started

### Prerequisites
- Python 3.8+
- Node.js 16+
- PostgreSQL 18 (or SQLite for development)

### 1. Clone the repo

```bash
git clone https://github.com/NadaBhm/osseoai.git
cd osseoai
```

### 2. Set up the Python backend

```bash
pip install -r requirements.txt
```

Configure your database in `database.py`:
```python
DATABASE_URL = "postgresql://postgres:YOUR_PASSWORD@localhost/osseoai"
```

Add your trained model checkpoints to `checkpoints/`:
checkpoints/
├── general_best.pth
├── shoulder_best.pth
├── wrist_best.pth
├── elbow_best.pth
├── finger_best.pth
├── forearm_best.pth
├── hand_best.pth
└── humerus_best.pth

Start the API server:
```bash
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

Or use the included runner:
```bash
python run.py
```

### 3. Set up the React frontend

```bash
npm install
npm run dev
```

### 4. Open the app
http://localhost:5173

API documentation available at:
http://localhost:8000/docs

---

## Database

Tables are created automatically on first server start via SQLAlchemy. Run migrations if needed:

```bash
python migrate_db.py
```

| Table | Description |
|---|---|
| `doctors` | Doctor accounts with role and approval status |
| `patients` | Patient records linked to their doctor |
| `xrays` | Uploaded X-ray images per patient |
| `analyses` | AI results linked to each X-ray |

---

## Models

All models use **AttentionMIL** architecture on top of **MobileNetV2** backbone, trained with the MURA v1.1 musculoskeletal radiograph dataset.

| Model | Bone | Architecture |
|---|---|---|
| General | All 7 bones | MultiTask AttentionMIL (bone classification + fracture detection) |
| Shoulder | Shoulder | AttentionMIL + Frangi preprocessing |
| Wrist | Wrist | AttentionMIL + Frangi preprocessing |
| Elbow | Elbow | AttentionMIL + Frangi preprocessing |
| Finger | Finger | AttentionMIL + Frangi preprocessing |
| Forearm | Forearm | AttentionMIL + Frangi preprocessing |
| Hand | Hand | AttentionMIL + Frangi preprocessing |
| Humerus | Humerus | AttentionMIL + Frangi preprocessing |

---

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Commit your changes (`git commit -m 'Add your feature'`)
4. Push to the branch (`git push origin feature/your-feature`)
5. Open a Pull Request

---

## License

MIT License — see [LICENSE](LICENSE) for details.

---

<div align="center">
  <p>Built for clinical use · Trained on MURA v1.1 · Made with ❤️ for better diagnostics</p>
</div>
