# Installation Guide

This guide walks you through setting up the Online Payments Fraud Detection project on your local machine.

---

## Prerequisites

| Requirement | Minimum Version |
|---|---|
| Python | 3.8+ |
| pip | 21.0+ |
| Git (optional) | Any recent version |

---

## Step 1 — Clone or Download the Repository

**Option A — Clone via Git:**
```bash
git clone https://github.com/<your-username>/Online-Payments-Fraud-Detection-with-Machine-Learning.git
cd Online-Payments-Fraud-Detection-with-Machine-Learning-main
```

**Option B — Download ZIP:**  
Download the ZIP from GitHub and extract it to a folder of your choice.

---

## Step 2 — Create a Virtual Environment (Recommended)

```bash
# Windows
python -m venv venv
venv\Scripts\activate

# macOS / Linux
python3 -m venv venv
source venv/bin/activate
```

---

## Step 3 — Install Required Dependencies

Install all required Python packages:

```bash
pip install flask numpy scikit-learn keras tensorflow pickle5
```

Or create a `requirements.txt` with the following content and run `pip install -r requirements.txt`:

```
flask
numpy
scikit-learn
keras
tensorflow
pandas
matplotlib
seaborn
plotly
jupyter
```

```bash
pip install -r requirements.txt
```

---

## Step 4 — Verify the Model File

Ensure the trained model file exists at the correct path used by the application:

```
static/model.pkl
```

If the model file is missing, re-run the training notebook:

```bash
jupyter notebook main.ipynb
```

Run all cells in sequence to train and save the model.

---

## Step 5 — Run the Application

```bash
python app.py
```

You should see output similar to:

```
 * Running on http://127.0.0.1:5000
 * Debug mode: on
```

Open your browser and navigate to **http://127.0.0.1:5000** to use the application.

---

## Common Installation Issues

| Issue | Solution |
|---|---|
| `ModuleNotFoundError: No module named 'flask'` | Run `pip install flask` |
| `ModuleNotFoundError: No module named 'keras'` | Run `pip install keras tensorflow` |
| `FileNotFoundError: model.pkl` | Ensure `static/model.pkl` exists; retrain via `main.ipynb` if missing |
| Port 5000 already in use | Change the port: `app.run(port=5001, debug=True)` in `app.py` |
| Virtual environment not activating (Windows) | Run `Set-ExecutionPolicy Unrestricted` in PowerShell, then retry |

---

## Directory After Setup

```
Online-Payments-Fraud-Detection-with-Machine-Learning-main/
├── app.py
├── main.ipynb
├── model.pkl
├── static/
│   ├── model.pkl        ← required by app.py
│   ├── style.css
│   └── background.jpg
└── templates/
    └── index.html
```
