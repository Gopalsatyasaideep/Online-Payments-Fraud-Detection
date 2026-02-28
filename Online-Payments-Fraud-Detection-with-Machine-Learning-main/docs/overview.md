# Online Payments Fraud Detection — Project Overview

## Introduction

Online payment systems have made financial transactions significantly more convenient. However, this convenience has also led to a sharp rise in payment fraud, particularly with credit card transactions. Detecting such fraud in real time is critical for financial institutions to protect their customers.

This project implements an **Online Payments Fraud Detection** system using **Machine Learning** and deploys it as a **web application** built with **Flask**. Given a set of transaction parameters, the model predicts whether a transaction is fraudulent or legitimate.

---

## Project Structure

```
Online-Payments-Fraud-Detection-with-Machine-Learning-main/
│
├── app.py                  # Flask web application (backend)
├── main.ipynb              # Jupyter Notebook (EDA + model training)
├── README.md               # Project README
├── model.pkl               # Trained ML model (root-level copy)
│
├── static/
│   ├── style.css           # Frontend CSS stylesheet
│   ├── background.jpg      # Background image for the UI
│   └── model.pkl           # Trained ML model used by the app
│
├── templates/
│   └── index.html          # HTML template for the web UI
│
└── docs/                   # Project documentation (this folder)
    ├── overview.md         # Project overview (this file)
    ├── installation.md     # Setup and installation guide
    ├── usage.md            # How to use the application
    ├── api.md              # Flask API reference
    └── model.md            # Machine learning model documentation
```

---

## Key Features

| Feature | Description |
|---|---|
| Fraud Prediction | Classifies a transaction as Fraud or No Fraud |
| Web Interface | Simple HTML form to enter transaction details |
| REST Endpoint | POST `/predict` accepts form data and returns a prediction |
| Trained Model | Decision Tree Classifier serialised as `model.pkl` |
| Exploratory Analysis | Full EDA and training steps in `main.ipynb` |

---

## Technology Stack

| Layer | Technology |
|---|---|
| Machine Learning | scikit-learn (DecisionTreeClassifier) |
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

## Quick Navigation

- [Installation Guide](installation.md)
- [Usage Guide](usage.md)
- [API Reference](api.md)
- [Model Documentation](model.md)
