# Online Payments Fraud Detection with Machine Learning

A machine learning-powered web application that detects fraudulent online payment transactions in real time using a **Decision Tree Classifier** and a **Flask** backend.

---

## Live Demo / Screenshots

> Run locally at **http://127.0.0.1:5000** after following the installation steps below.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Technology Stack](#technology-stack)
- [Dataset](#dataset)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Usage](#usage)
- [Model Details](#model-details)
- [API Reference](#api-reference)
- [Documentation](#documentation)
- [License](#license)

---

## Overview

Online payment systems have made financial transactions significantly more convenient. However, this convenience has also led to a sharp rise in payment fraud, particularly with credit card transactions. Detecting such fraud in real time is critical for financial institutions to protect their customers.

This project implements an **Online Payments Fraud Detection** system using **Machine Learning** and deploys it as a **web application** built with **Flask**. Given a set of transaction parameters, the model predicts whether a transaction is **fraudulent** or **legitimate**.

---

## Features

| Feature | Description |
|---|---|
| Fraud Prediction | Classifies a transaction as **Fraud** or **No Fraud** |
| Web Interface | Simple HTML form to enter transaction details |
| REST Endpoint | `POST /predict` accepts form data and returns a prediction |
| Trained Model | Decision Tree Classifier serialised as `model.pkl` |
| Exploratory Analysis | Full EDA and training steps in `main.ipynb` |

---

## Technology Stack

| Layer | Technology |
|---|---|
| Machine Learning | scikit-learn (`DecisionTreeClassifier`) |
| Model Serialisation | Python `pickle` |
| Backend | Flask (Python) |
| Frontend | HTML5 + CSS3 |
| Data Analysis | pandas, numpy, matplotlib, seaborn, plotly |
| Notebook | Jupyter Notebook |

---

## Dataset

The dataset was sourced from **Kaggle — PaySim** ([link](https://www.kaggle.com/ealaxi/paysim1/download)).  
It contains historical records of mobile money transactions, labelled as fraudulent or non-fraudulent.

### Dataset Columns

| Column | Description |
|---|---|
| `step` | Unit of time (1 step = 1 hour) |
| `type` | Type of transaction |
| `amount` | Transaction amount |
| `nameOrig` | Customer initiating the transaction |
| `oldbalanceOrg` | Sender balance before transaction |
| `newbalanceOrig` | Sender balance after transaction |
| `nameDest` | Recipient of the transaction |
| `oldbalanceDest` | Recipient balance before transaction |
| `newbalanceDest` | Recipient balance after transaction |
| `isFraud` | Target label (1 = Fraud, 0 = No Fraud) |

---

## Project Structure

```
Online-Payments-Fraud-Detection-with-Machine-Learning-main/
│
├── app.py                  # Flask web application (backend)
├── main.ipynb              # Jupyter Notebook (EDA + model training)
├── README.md               # Project README
│
├── static/
│   ├── style.css           # Frontend CSS stylesheet
│   ├── background.jpg      # Background image for the UI
│   └── model.pkl           # Trained ML model used by the app
│
├── templates/
│   └── index.html          # HTML template for the web UI
│
└── docs/                   # Project documentation
    ├── overview.md
    ├── installation.md
    ├── usage.md
    ├── api.md
    └── model.md
```

---

## Installation

### Prerequisites

| Requirement | Minimum Version |
|---|---|
| Python | 3.8+ |
| pip | 21.0+ |
| Git | Any recent version |

### Steps

**1. Clone the repository**

```bash
git clone https://github.com/Gopalsatyasaideep/Online-Payments-Fraud-Detection.git
cd Online-Payments-Fraud-Detection
```

**2. Create and activate a virtual environment (recommended)**

```bash
# Windows
python -m venv venv
venv\Scripts\activate

# macOS / Linux
python3 -m venv venv
source venv/bin/activate
```

**3. Install dependencies**

```bash
pip install flask numpy scikit-learn pandas matplotlib seaborn plotly jupyter
```

**4. Verify the model file**

Ensure `static/model.pkl` exists. If not, run `main.ipynb` to train and generate it.

---

## Usage

**Start the Flask server:**

```bash
python app.py
```

Open your browser and navigate to **http://127.0.0.1:5000**

### Input Fields

| Field | Description | Example |
|---|---|---|
| **Type** | Type of online transaction | `CASH_OUT` |
| **Amount** | Transaction amount | `15000.00` |
| **OldbalanceOrg** | Sender's balance before the transaction | `20000.00` |
| **NewbalanceOrig** | Sender's balance after the transaction | `5000.00` |

### Example — Suspicious Transaction (Fraud)

| Field | Value |
|---|---|
| Type | `CASH_OUT` |
| Amount | `500000` |
| OldbalanceOrg | `500000` |
| NewbalanceOrig | `0` |

> The account was completely drained — this pattern is commonly flagged as **Fraud**.

---

## Model Details

| Property | Value |
|---|---|
| Algorithm | `DecisionTreeClassifier` (scikit-learn) |
| Serialisation | Python `pickle` → `model.pkl` |
| Training Notebook | `main.ipynb` |
| Features Used | `type`, `amount`, `oldbalanceOrg`, `newbalanceOrig` |

### Feature Encoding — Transaction Type

| Raw Value | Encoded Value |
|---|---|
| `CASH_OUT` | `1` |
| `PAYMENT` | `2` |
| `CASH_IN` | `3` |
| `TRANSFER` | `4` |
| `DEBIT` | `5` |

### Output Labels

| Label | Value |
|---|---|
| No Fraud | `0` |
| Fraud | `1` |

---

## API Reference

### `POST /predict`

Accepts form-encoded data and returns a fraud prediction.

**Request Parameters**

| Parameter | Type | Description |
|---|---|---|
| `type` | Integer | Encoded transaction type (1–5) |
| `amount` | Float | Transaction amount |
| `oldbalanceOrg` | Float | Sender balance before transaction |
| `newbalanceOrig` | Float | Sender balance after transaction |

**Response**

Returns the rendered `index.html` page with the prediction result (`0` = No Fraud, `1` = Fraud).

---

## Documentation

Full documentation is available in the [`docs/`](Online-Payments-Fraud-Detection-with-Machine-Learning-main/docs/) folder:

- [Overview](Online-Payments-Fraud-Detection-with-Machine-Learning-main/docs/overview.md)
- [Installation Guide](Online-Payments-Fraud-Detection-with-Machine-Learning-main/docs/installation.md)
- [Usage Guide](Online-Payments-Fraud-Detection-with-Machine-Learning-main/docs/usage.md)
- [API Reference](Online-Payments-Fraud-Detection-with-Machine-Learning-main/docs/api.md)
- [Model Documentation](Online-Payments-Fraud-Detection-with-Machine-Learning-main/docs/model.md)

---

## License

This project is open-source and available under the [MIT License](LICENSE).
