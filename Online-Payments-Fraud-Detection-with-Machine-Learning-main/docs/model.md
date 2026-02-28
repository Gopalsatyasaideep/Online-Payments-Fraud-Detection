# Machine Learning Model Documentation

---

## Overview

The fraud detection model is a **Decision Tree Classifier** trained using scikit-learn on the PaySim dataset. It classifies a transaction as either `Fraud` or `No Fraud` based on four transaction features.

---

## Model Details

| Property | Value |
|---|---|
| Algorithm | Decision Tree Classifier (`sklearn.tree.DecisionTreeClassifier`) |
| Serialisation | Python `pickle` (`model.pkl`) |
| Model file path (used by app) | `static/model.pkl` |
| Training notebook | `main.ipynb` |

---

## Features Used

The model is trained on exactly **four features**:

| Feature | Description | Type |
|---|---|---|
| `type` | Transaction type (encoded numerically) | Integer (1–5) |
| `amount` | Transaction amount | Float |
| `oldbalanceOrg` | Sender balance before transaction | Float |
| `newbalanceOrig` | Sender balance after transaction | Float |

### Feature Encoding — Transaction Type

| Raw Value | Encoded Value |
|---|---|
| `CASH_OUT` | `1` |
| `PAYMENT` | `2` |
| `CASH_IN` | `3` |
| `TRANSFER` | `4` |
| `DEBIT` | `5` |

---

## Target Variable

| Label | Encoded Value |
|---|---|
| `No Fraud` | `0` |
| `Fraud` | `1` |

---

## Training Pipeline

The training process is fully documented in `main.ipynb`. Summary:

### 1. Load Dataset
```python
import pandas as pd
data = pd.read_csv('PS_20174392719_1491204439457_log.csv')
```

### 2. Explore the Data
- Check for null values (`data.isnull().sum()`)
- Visualise transaction type distribution using a Plotly pie chart
- Analyse feature correlation with a seaborn heatmap

### 3. Encode Categorical Features
```python
data["type"] = data["type"].map({
    "CASH_OUT": 1, "PAYMENT": 2,
    "CASH_IN": 3,  "TRANSFER": 4,
    "DEBIT": 5
})
data["isFraud"] = data["isFraud"].map({0: "No Fraud", 1: "Fraud"})
```

### 4. Split Data
```python
from sklearn.model_selection import train_test_split
import numpy as np

x = np.array(data[["type", "amount", "oldbalanceOrg", "newbalanceOrig"]])
y = np.array(data[["isFraud"]])

xtrain, xtest, ytrain, ytest = train_test_split(x, y, test_size=0.10, random_state=42)
```

### 5. Train the Model
```python
from sklearn.tree import DecisionTreeClassifier

model = DecisionTreeClassifier()
model.fit(xtrain, ytrain)
```

### 6. Evaluate the Model
```python
print("Training Accuracy:", model.score(xtrain, ytrain))
print("Test Accuracy:",     model.score(xtest, ytest))
```

### 7. Save the Model
```python
import pickle

with open("static/model.pkl", "wb") as f:
    pickle.dump(model, f)
```

---

## Model Loading (in `app.py`)

```python
import pickle

model = pickle.load(open("static/model.pkl", "rb"))
```

---

## Inference

```python
import numpy as np

input_array = np.array([[type_encoded, amount, oldbalanceOrg, newbalanceOrig]])
prediction = model.predict(input_array)
output = prediction[0]   # "No Fraud" or "Fraud"
```

---

## Dataset Reference

- **Source:** Kaggle — PaySim Synthetic Financial Dataset
- **Link:** https://www.kaggle.com/ealaxi/paysim1/download
- **Description:** Simulated mobile money transactions based on a sample of real transactions from a one-month log extracted from a financial service running in an African country.

---

## Limitations

| Limitation | Detail |
|---|---|
| Only 4 features used | Features like `nameDest`, `oldbalanceDest`, `newbalanceDest` are excluded |
| Decision Tree can overfit | No pruning parameters are set in the current training code |
| TRANSFER type bug | In `app.py`, the `CASH_IN` case is checked twice; `TRANSFER` never maps to `4` |
| No probability score | The app returns a hard label (0 or 1), not a confidence score |

---

## Retraining

To retrain the model with new data:

1. Place the new CSV dataset in the project root.
2. Update the file path in `main.ipynb`.
3. Run all notebook cells.
4. The new `model.pkl` will be saved to `static/model.pkl` automatically.
5. Restart `app.py` to load the updated model.
