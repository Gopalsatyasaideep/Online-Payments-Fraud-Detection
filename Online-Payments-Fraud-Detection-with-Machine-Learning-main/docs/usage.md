# Usage Guide

This guide explains how to use the Online Payments Fraud Detection web application.

---

## Starting the Application

```bash
python app.py
```

Then open your browser and go to:  
**http://127.0.0.1:5000**

---

## Using the Web Interface

The home page displays a form with four input fields:

### Input Fields

| Field | Type | Description | Example |
|---|---|---|---|
| **Type** | Dropdown | Type of online transaction | `CASH_OUT` |
| **Amount** | Number | Transaction amount in currency units | `15000.00` |
| **OldbalanceOrg** | Number | Sender's account balance before the transaction | `20000.00` |
| **NewbalanceOrig** | Number | Sender's account balance after the transaction | `5000.00` |

### Transaction Type Options

| Option | Description |
|---|---|
| `CASH_OUT` | Withdrawing cash from an account |
| `PAYMENT` | Making a payment to a merchant |
| `CASH_IN` | Depositing cash into an account |
| `TRANSFER` | Transferring money to another account |
| `DEBIT` | Debit card transaction |

---

## Making a Prediction

1. Select a **Transaction Type** from the dropdown.
2. Enter the transaction **Amount**.
3. Enter the **Old Balance** (balance before the transaction).
4. Enter the **New Balance** (balance after the transaction).
5. Click **Submit**.

The result is displayed on the same page under **Fraud Detection:**

### Possible Outputs

| Output | Meaning |
|---|---|
| `0` or `No Fraud` | The transaction is predicted as **legitimate** |
| `1` or `Fraud` | The transaction is predicted as **fraudulent** |

---

## Example Scenarios

### Scenario 1 — Suspicious CASH_OUT (likely Fraud)

| Field | Value |
|---|---|
| Type | CASH_OUT |
| Amount | 500000 |
| OldbalanceOrg | 500000 |
| NewbalanceOrig | 0 |

The account was completely drained in a single transaction — this pattern is commonly flagged as fraud.

---

### Scenario 2 — Normal PAYMENT (likely No Fraud)

| Field | Value |
|---|---|
| Type | PAYMENT |
| Amount | 200 |
| OldbalanceOrg | 5000 |
| NewbalanceOrig | 4800 |

A small, proportional payment — typical of a normal transaction.

---

## Using the Notebook for Training

To retrain or explore the model:

```bash
jupyter notebook main.ipynb
```

Run the cells in order to:
1. Load and explore the dataset
2. Visualise transaction distributions
3. Check for null values
4. Encode categorical features
5. Train the Decision Tree model
6. Evaluate model accuracy
7. Save the trained model as `model.pkl`
