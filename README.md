# AI-Examination-Seating-Planner-Examatrix

<div align="center">

```
███████╗██╗  ██╗ █████╗ ███╗   ███╗ █████╗ ████████╗██████╗ ██╗██╗  ██╗
██╔════╝╚██╗██╔╝██╔══██╗████╗ ████║██╔══██╗╚══██╔══╝██╔══██╗██║╚██╗██╔╝
█████╗   ╚███╔╝ ███████║██╔████╔██║███████║   ██║   ██████╔╝██║ ╚███╔╝ 
██╔══╝   ██╔██╗ ██╔══██║██║╚██╔╝██║██╔══██║   ██║   ██╔══██╗██║ ██╔██╗ 
███████╗██╔╝ ██╗██║  ██║██║ ╚═╝ ██║██║  ██║   ██║   ██║  ██║██║██╔╝ ██╗
╚══════╝╚═╝  ╚═╝╚═╝  ╚═╝╚═╝     ╚═╝╚═╝  ╚═╝   ╚═╝   ╚═╝  ╚═╝╚═╝╚═╝  ╚═╝
```

### AI-Powered Examination Seating Planner

*Eliminate cheating risk through intelligent seat optimization*

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat-square&logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-3.0-000000?style=flat-square&logo=flask&logoColor=white)
![React](https://img.shields.io/badge/React-18-61DAFB?style=flat-square&logo=react&logoColor=black)
![TailwindCSS](https://img.shields.io/badge/TailwindCSS-3.4-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white)
![EasyOCR](https://img.shields.io/badge/EasyOCR-1.7-FF6B35?style=flat-square)
![OpenCV](https://img.shields.io/badge/OpenCV-4.9-5C3EE8?style=flat-square&logo=opencv&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-22C55E?style=flat-square)

</div>

---

## What is Examatrix?

**Examatrix** is a full-stack AI system that automatically generates optimal exam hall seating arrangements. Upload a photo of your student roster or a CSV/Excel file, and the system reads it, extracts every student's name, ID, course, and friend connections, then runs an AI optimization engine to seat students in a way that mathematically minimizes cheating risk.

No more manual seating charts. No more spreadsheets. No more hoping friends aren't sitting next to each other.

---

## How It Works

```
Upload Roster Image / CSV
         │
         ▼
  ┌─────────────────┐
  │   EasyOCR +     │  ← Reads handwritten or printed tables
  │   OpenCV        │  ← Preprocesses image for accuracy
  └────────┬────────┘
           │  Structured student data
           ▼
  ┌─────────────────┐
  │ Intelligent     │  ← Detects columns: S.No, ID, Name, Course, Friends
  │ Parser          │  ← Resolves friend names → IDs
  └────────┬────────┘
           │  Clean JSON roster
           ▼
  ┌─────────────────┐
  │ Seating         │  ← Simulated Annealing (10,000 iterations)
  │ Optimizer       │  ← Penalty: +9 (friends + same course)
  │                 │              +3 (same course only)
  └────────┬────────┘               0 (different course)
           │  Optimized grid
           ▼
  ┌─────────────────┐
  │ React Dashboard │  ← Live seating chart
  │                 │  ← KNN neighbor risk inspector
  └─────────────────┘  ← Student roster management
```

---

## Key Features

| Feature | Description |
|---|---|
| **OCR Roster Upload** | Photograph any printed student list — EasyOCR extracts all data automatically |
| **CSV / Excel Upload** | Direct import from `.csv`, `.xlsx`, `.xls` with auto column detection |
| **Simulated Annealing** | 10,000-iteration AI optimizer that separates friends and distributes courses |
| **Linear Regression Sort** | Fast one-pass priority seating for large rosters |
| **KNN Risk Inspector** | Click any seat to inspect its 4 cardinal neighbors for cheating vulnerability |
| **Live Penalty Score** | Real-time before/after optimization score displayed on the seating chart |
| **High-Risk Flagging** | Mark students requiring supervision — placed in visible positions automatically |
| **Auto-Fit Grid** | Hall dimensions auto-calculate based on roster size |
| **Manual Override** | Rows, columns, and buffer seats fully configurable |
| **Friend Network** | Bi-directional friend linking with mutual relationship enforcement |

---

## Algorithm Deep Dive

### Simulated Annealing (Primary — "Advanced AI" mode)

The core optimization engine. Works like a smart trial-and-error:

1. Place all students in random seats
2. Calculate total **penalty score** across all adjacent pairs
3. Pick two students and swap their seats
4. If penalty decreased → keep the swap permanently
5. Repeat 10,000 times
6. Return the arrangement with the lowest penalty

**Penalty formula:**
```
For every pair of students seated adjacent (Up/Down/Left/Right):
  same course + are friends  →  +9 pts  (high cheating risk)
  same course only           →  +3 pts  (low risk, tolerated)
  different course           →   0 pts  (always safe)
```

The optimizer will always sacrifice a course-separation to achieve a friend-separation because the 9 vs 3 point gap makes friend-pairs the biggest penalty contributor.

### Linear Regression Sort (Secondary — "Fast Sort" mode)

A single-pass priority algorithm for speed:

1. Calculate a risk score per student: `friends_count × course_match_weight`
2. Sort all students by risk score, highest first
3. Assign high-risk students to maximally separated seats first
4. Fill remaining seats in order

No iteration — completes in a single pass. Best for very large rosters where speed matters more than perfect optimization.

### KNN Neighbor Detection (K = 4)

After seating is generated, clicking any seat runs a K-Nearest Neighbor check with K=4 — the four cardinal directions (Up, Down, Left, Right). Diagonal seats are intentionally excluded as they provide much less opportunity for answer-passing. Each neighbor is checked against the selected student's course and friend list, and labeled **RISK** or **SAFE** instantly.

---

## Tech Stack

```
Backend                          Frontend
───────────────────────          ─────────────────────────────
Python 3.10+                     React 18
Flask 3.0  (REST API)            Vite 5  (build tool)
EasyOCR 1.7  (text detection)    TailwindCSS 3.4  (styling)
OpenCV 4.9  (image processing)   Lucide React  (icons)
NumPy  (array operations)        Fetch API  (HTTP client)
openpyxl  (Excel parsing)
```

---

## Project Structure

```
examatrix/
│
├── backend/
│   ├── app.py              ← Flask API, OCR pipeline, route handlers
│   ├── agent.py            ← SeatingPlanner: Simulated Annealing optimizer
│   ├── environment.py      ← ExamHall: grid representation and seat management
│   ├── models.py           ← Student dataclass
│   └── uploads/            ← Temp storage for uploaded images (auto-cleaned)
│
├── frontend/
│   ├── src/
│   │   ├── App.jsx         ← Main application: roster, grid, setup tabs
│   │   └── main.jsx        ← React entry point
│   ├── index.html
│   ├── package.json
│   └── vite.config.js      ← Proxy: /api/* → localhost:5000
│
└── main.py                 ← Single launcher: starts Flask + Vite together
```

---

## Getting Started

### Prerequisites

- Python 3.10 or higher
- Node.js 18 or higher
- pip

### Installation

**1. Clone the repository**
```bash
git clone https://github.com/your-username/examatrix.git
cd examatrix
```

**2. Install Python dependencies**
```bash
cd backend
pip install flask flask-cors easyocr opencv-python numpy openpyxl
```

**3. Install frontend dependencies**
```bash
cd frontend
npm install
```

**4. Run the application**
```bash
# From the project root — launches both servers simultaneously
python main.py
```

**5. Open in browser**
```
http://localhost:5173
```

The Flask backend runs on `http://localhost:5000`. Vite proxies all `/api/*` requests to it automatically — no manual configuration needed.

---

## API Reference

### `POST /api/upload-roster`

Upload a student roster image or spreadsheet for parsing.

**Request:** `multipart/form-data`

| Field | Type | Description |
|---|---|---|
| `file` | File | Image (JPG/PNG) or spreadsheet (CSV/XLSX/XLS) |
| `subjects` | string[] | Known subject names to help OCR normalization |

**Response:**
```json
{
  "students": [
    {
      "id": "105",
      "name": "Saad",
      "subject": "Compiler Construction",
      "friends": ["106", "114", "112"],
      "isHighRisk": false
    }
  ]
}
```

---

### `POST /api/optimize`

Run the seating optimization algorithm.

**Request:**
```json
{
  "rows": 5,
  "cols": 6,
  "students": [ ...student objects... ],
  "algorithm": "ai"
}
```

**Response:**
```json
{
  "initial_penalty": 440,
  "final_penalty": 18,
  "grid": [
    [ { "id": "105", "name": "Saad", "subject": "Compiler Construction" }, null, ... ],
    ...
  ]
}
```

---

## Roster Image Format

Examatrix OCR works best with tables that have these columns (order flexible, headers required):

```
S.No  |  ID  |  Name  |  Course Assigned  |  Friends (Close Friends from the List)
──────────────────────────────────────────────────────────────────────────────────
  1.  | 105  |  Saad  | Compiler Const.   | Mahad, Saif, Ahmed
  2.  | 106  | Mahad  | Compiler Const.   | Saad, Hamad, Abdullah
```

**Tips for best OCR accuracy:**
- Use a flat, well-lit surface when photographing
- Ensure the table fills most of the frame
- Printed tables work better than handwritten
- CSV/Excel upload is always 100% accurate — use it when available

---

## Screenshots

> Add screenshots here after running the project

| Roster Tab | Seating Chart | KNN Inspector |
|---|---|---|
| `screenshot-roster.png` | `screenshot-grid.png` | `screenshot-knn.png` |

---

## Future Improvements

- [ ] Export seating chart as PDF with seat labels
- [ ] Multi-hall support (distribute students across rooms)
- [ ] GPU-accelerated OCR for faster processing
- [ ] Drag-and-drop manual seat adjustment after optimization
- [ ] Invigilator zone configuration
- [ ] Historical seating records and comparison

---

## Contributing

Pull requests are welcome. For major changes, open an issue first to discuss what you'd like to change.

1. Fork the repository
2. Create your branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m 'Add your feature'`
4. Push to the branch: `git push origin feature/your-feature`
5. Open a pull request

---

## License

This project is licensed under the MIT License. See `LICENSE` for details.

---

<div align="center">

Built with Python, Flask, React, EasyOCR, and OpenCV

*Examatrix — because exam integrity should not depend on a lucky seating chart.*

</div>
