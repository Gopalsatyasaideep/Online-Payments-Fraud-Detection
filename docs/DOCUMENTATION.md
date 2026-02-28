# Online Payments Fraud Detection with Machine Learning
### Complete Project Documentation

---

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [Project Structure](#2-project-structure)
3. [Technology Stack](#3-technology-stack)
4. [Dataset](#4-dataset)
5. [Installation & Setup](#5-installation--setup)
6. [Running the Application](#6-running-the-application)
7. [Using the Web Interface](#7-using-the-web-interface)
8. [API Reference](#8-api-reference)
9. [Machine Learning Model](#9-machine-learning-model)
10. [Training Pipeline](#10-training-pipeline)
11. [Known Issues & Limitations](#11-known-issues--limitations)
12. [Troubleshooting](#12-troubleshooting)

---

## 1. Project Overview

Online payment systems have simplified financial transactions globally, but they have also introduced significant fraud risks — especially in credit card and mobile money transactions. Detecting fraud in real time protects customers from unauthorised charges.

This project builds an **end-to-end Online Payments Fraud Detection system** that:

- Trains a **Decision Tree Classifier** on historical transaction data
- Deploys the trained model as a **Flask web application**
- Accepts live transaction inputs through a clean **web interface**
- Predicts instantly whether a transaction is **Fraudulent** or **Legitimate**

---

## 2. Project Structure

```
Online-Payments-Fraud-Detection-with-Machine-Learning-main/   ← workspace root
│
├── docs/                          ← documentation (this folder)
│   └── DOCUMENTATION.md           ← this file
│
└── Online-Payments-Fraud-Detection-with-Machine-Learning-main/   ← project source
    │
    ├── app.py                     # Flask web application (backend + prediction logic)
    ├── main.ipynb                 # Jupyter Notebook: EDA, training, model export
    ├── README.md                  # Original project README
    ├── model.pkl                  # Trained model (root-level copy)
    │
    ├── static/
    │   ├── model.pkl              # Trained model loaded by app.py
    │   ├── style.css              # CSS stylesheet for the web UI
    │   └── background.jpg         # Background image for the web UI
    │
    └── templates/
        └── index.html             # HTML form template rendered by Flask
```

---

## 3. Technology Stack

| Layer | Technology | Purpose |
|---|---|---|
| Machine Learning | scikit-learn | DecisionTreeClassifier training & inference |
| Model Serialisation | Python `pickle` | Save and load trained model |
| Deep Learning (imported) | Keras / TensorFlow | Imported in app.py (unused in current version) |
| Web Backend | Flask | Serve web pages and handle POST predictions |
| Frontend | HTML5 + CSS3 | User input form and result display |
| Data Analysis | pandas, numpy | Data loading, transformation, feature engineering |
| Visualisation | matplotlib, seaborn, plotly | EDA charts and correlation heatmap |
| Notebook | Jupyter | Interactive training and exploration environment |

---

## 4. Dataset

| Property | Detail |
|---|---|
| Source | Kaggle — PaySim Synthetic Mobile Money Transactions |
| Download | https://www.kaggle.com/ealaxi/paysim1/download |
| Description | Simulated transactions based on a one-month real log from a mobile financial service |

### Dataset Columns

| Column | Description |
|---|---|
| `step` | Unit of time (1 step = 1 hour) |
| `type` | Transaction type: CASH_OUT, PAYMENT, CASH_IN, TRANSFER, DEBIT |
| `amount` | Transaction amount in local currency |
| `nameOrig` | Account ID of the customer initiating the transaction |
| `oldbalanceOrg` | Sender's account balance **before** the transaction |
| `newbalanceOrig` | Sender's account balance **after** the transaction |
| `nameDest` | Account ID of the recipient |
| `oldbalanceDest` | Recipient's balance before the transaction |
| `newbalanceDest` | Recipient's balance after the transaction |
| `isFraud` | **Target label** — `1` = Fraud, `0` = No Fraud |

---

## 5. Installation & Setup

### Prerequisites

| Requirement | Minimum Version |
|---|---|
| Python | 3.8 or higher |
| pip | 21.0 or higher |

### Step 1 — Navigate to the Project Folder

```bash
cd Online-Payments-Fraud-Detection-with-Machine-Learning-main
```

### Step 2 — Create a Virtual Environment

```bash
# Windows
python -m venv venv
venv\Scripts\activate

# macOS / Linux
python3 -m venv venv
source venv/bin/activate
```

### Step 3 — Install Dependencies

```bash
pip install flask numpy scikit-learn keras tensorflow pandas matplotlib seaborn plotly jupyter
```

Or save the following as `requirements.txt` and run `pip install -r requirements.txt`:

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

### Step 4 — Verify the Model File

The application loads the model from:

```
static/model.pkl
```

If this file is missing, retrain the model by running all cells in `main.ipynb` (see [Section 10](#10-training-pipeline)).

---

## 6. Running the Application

```bash
python app.py
```

Expected terminal output:

```
 * Running on http://127.0.0.1:5000
 * Debug mode: on
```

Open your browser and visit **http://127.0.0.1:5000**

---

## 7. Using the Web Interface

The home page shows a form with four input fields.

### Input Fields

| Field | Type | Description | Example |
|---|---|---|---|
| **Type** | Dropdown | Type of transaction | `CASH_OUT` |
| **Amount** | Number | Transaction amount | `50000` |
| **OldbalanceOrg** | Number | Sender's balance before transaction | `50000` |
| **NewbalanceOrig** | Number | Sender's balance after transaction | `0` |

### Transaction Type Options

| Value | Description |
|---|---|
| `CASH_OUT` | Withdrawing cash |
| `PAYMENT` | Paying a merchant |
| `CASH_IN` | Depositing cash |
| `TRANSFER` | Transferring to another account |
| `DEBIT` | Debit card transaction |

### Steps

1. Select a **Transaction Type** from the dropdown.
2. Enter the **Amount**.
3. Enter the **Old Balance** (before transaction).
4. Enter the **New Balance** (after transaction).
5. Click **Submit**.
6. The prediction appears below the form: `Fraud Detection: 0` (No Fraud) or `Fraud Detection: 1` (Fraud).

### Example Scenarios

**Suspicious — likely Fraud:**

| Field | Value |
|---|---|
| Type | CASH_OUT |
| Amount | 500000 |
| OldbalanceOrg | 500000 |
| NewbalanceOrig | 0 |

> The account was completely drained in a single transaction — a common fraud pattern.

**Normal — likely No Fraud:**

| Field | Value |
|---|---|
| Type | PAYMENT |
| Amount | 200 |
| OldbalanceOrg | 5000 |
| NewbalanceOrig | 4800 |

> A proportional payment consistent with normal behaviour.

---

## 8. API Reference

### Base URL

```
http://127.0.0.1:5000
```

---

### `GET /`

Renders the home page with the fraud detection input form.

| Property | Value |
|---|---|
| Method | `GET` |
| Response | `index.html` (HTML page) |

---

### `POST /predict`

Accepts transaction form data and returns a fraud prediction rendered in `index.html`.

| Property | Value |
|---|---|
| Method | `POST` |
| Content-Type | `application/x-www-form-urlencoded` |

#### Request Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `type` | string | Yes | `CASH_OUT`, `PAYMENT`, `CASH_IN`, `TRANSFER`, or `DEBIT` |
| `amount` | float | Yes | Transaction amount |
| `oldbalanceOrg` | float | Yes | Sender balance before transaction |
| `newbalanceOrig` | float | Yes | Sender balance after transaction |

#### Type Encoding (internal)

| Type String | Numeric Value Passed to Model |
|---|---|
| `CASH_OUT` | `1` |
| `PAYMENT` | `2` |
| `CASH_IN` | `3` |
| `TRANSFER` | `4` |
| `DEBIT` | `5` |

#### Response

Returns `index.html` with the `{{ prediction }}` variable filled in.

| Value | Meaning |
|---|---|
| `0` | No Fraud |
| `1` | Fraud |

#### Example — cURL

```bash
curl -X POST http://127.0.0.1:5000/predict \
  -d "type=CASH_OUT&amount=50000&oldbalanceOrg=50000&newbalanceOrig=0"
```

#### Example — Python requests

```python
import requests

response = requests.post("http://127.0.0.1:5000/predict", data={
    "type": "CASH_OUT",
    "amount": 50000,
    "oldbalanceOrg": 50000,
    "newbalanceOrig": 0
})
print(response.text)
```

---

## 9. Machine Learning Model

| Property | Value |
|---|---|
| Algorithm | `sklearn.tree.DecisionTreeClassifier` |
| Serialisation format | Python `pickle` |
| Model file | `static/model.pkl` |
| Input features | 4 (type, amount, oldbalanceOrg, newbalanceOrig) |
| Output | Binary class — `No Fraud` or `Fraud` |

### Feature Vector

```python
input_array = np.array([[type_encoded, amount, oldbalanceOrg, newbalanceOrig]])
prediction = model.predict(input_array)
```

### Model Loading in `app.py`

```python
import pickle
model = pickle.load(open("static/model.pkl", "rb"))
```

---

## 10. Training Pipeline

The full training process lives in `main.ipynb`. Run it with:

```bash
jupyter notebook main.ipynb
```

### Step-by-Step

**Step 1 — Import Libraries**
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

**Step 2 — Load the Dataset**
```python
data = pd.read_csv('PS_20174392719_1491204439457_log.csv')
data.head()
```

**Step 3 — Check for Null Values**
```python
print(data.isnull().sum())
```

**Step 4 — Visualise Transaction Type Distribution**
```python
import plotly.express as px
type_counts = data["type"].value_counts()
figure = px.pie(data, values=type_counts.values, names=type_counts.index,
                hole=0.5, title="Distribution of Transaction Type")
figure.show()
```

**Step 5 — Correlation Heatmap**
```python
correlation = data.corr()
sns.heatmap(correlation, annot=True)
```

**Step 6 — Encode Features**
```python
data["type"] = data["type"].map({
    "CASH_OUT": 1, "PAYMENT": 2,
    "CASH_IN": 3,  "TRANSFER": 4,
    "DEBIT": 5
})
data["isFraud"] = data["isFraud"].map({0: "No Fraud", 1: "Fraud"})
```

**Step 7 — Split Data**
```python
from sklearn.model_selection import train_test_split

x = np.array(data[["type", "amount", "oldbalanceOrg", "newbalanceOrig"]])
y = np.array(data[["isFraud"]])

xtrain, xtest, ytrain, ytest = train_test_split(x, y, test_size=0.10, random_state=42)
```

**Step 8 — Train Model**
```python
from sklearn.tree import DecisionTreeClassifier

model = DecisionTreeClassifier()
model.fit(xtrain, ytrain)
print("Training Accuracy:", model.score(xtrain, ytrain))
print("Test Accuracy:",     model.score(xtest,  ytest))
```

**Step 9 — Save Model**
```python
import pickle

with open("static/model.pkl", "wb") as f:
    pickle.dump(model, f)
```

---

## 11. Known Issues & Limitations

| Issue | Detail |
|---|---|
| Bug — TRANSFER never encoded | In `app.py`, `CASH_IN` is checked twice in the `if/elif` chain; `TRANSFER` always falls to `val = 5` (DEBIT's value) instead of `4` |
| Only 4 features used | Features `nameDest`, `oldbalanceDest`, `newbalanceDest` are excluded; including them may improve accuracy |
| No input validation | Non-numeric inputs for amount/balance fields cause an unhandled `ValueError` |
| No confidence score | The app returns a hard label (`0` or `1`), not a probability |
| Decision Tree may overfit | No `max_depth` or pruning parameters are set |
| Keras/TensorFlow imported but unused | `app.py` imports `keras` and `tensorflow` unnecessarily, increasing startup time |

### Bug Fix — TRANSFER Encoding (`app.py`)

Replace the current `if/elif` block with:

```python
type_map = {
    "CASH_OUT": 1,
    "PAYMENT":  2,
    "CASH_IN":  3,
    "TRANSFER": 4,
    "DEBIT":    5
}
val = type_map.get(type, 5)
```

---

## 12. Troubleshooting

| Problem | Solution |
|---|---|
| `ModuleNotFoundError: flask` | Run `pip install flask` |
| `ModuleNotFoundError: keras` | Run `pip install keras tensorflow` |
| `FileNotFoundError: static/model.pkl` | Retrain the model by running all cells in `main.ipynb` |
| Port 5000 already in use | Edit `app.py`: change `app.run(port=5001, debug=True)` |
| Virtual environment won't activate (Windows PowerShell) | Run `Set-ExecutionPolicy -Scope CurrentUser Unrestricted`, then activate |
| Prediction always shows `None` | Ensure the form `action="/predict"` and the model file path are correct |
| Browser shows old result | Hard-refresh with `Ctrl + Shift + R` |
