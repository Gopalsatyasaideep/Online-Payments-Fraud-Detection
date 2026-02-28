# API Reference

The Flask backend exposes two HTTP endpoints.

---

## Base URL

```
http://127.0.0.1:5000
```

---

## Endpoints

### `GET /`

Renders the home page with the fraud detection input form.

**Response:** Returns `index.html` — the main web UI page.

**Example:**
```
GET http://127.0.0.1:5000/
```

---

### `POST /predict`

Accepts transaction data from the HTML form and returns a fraud prediction.

#### Request

| Property | Value |
|---|---|
| Method | `POST` |
| Content-Type | `application/x-www-form-urlencoded` (HTML form submission) |

#### Form Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `type` | string | Yes | Transaction type: `CASH_OUT`, `PAYMENT`, `CASH_IN`, `TRANSFER`, or `DEBIT` |
| `amount` | float | Yes | Transaction amount |
| `oldbalanceOrg` | float | Yes | Sender balance before transaction |
| `newbalanceOrig` | float | Yes | Sender balance after transaction |

#### Transaction Type Encoding (internal)

The application maps the `type` string to a numerical value before passing it to the model:

| Type | Encoded Value |
|---|---|
| `CASH_OUT` | `1` |
| `PAYMENT` | `2` |
| `CASH_IN` | `3` |
| `TRANSFER` | `4` |
| `DEBIT` | `5` |

#### Response

Returns `index.html` with the `prediction` template variable populated.

| Prediction Value | Meaning |
|---|---|
| `0` | No Fraud |
| `1` | Fraud |

#### Example cURL Request

```bash
curl -X POST http://127.0.0.1:5000/predict \
  -d "type=CASH_OUT&amount=50000&oldbalanceOrg=50000&newbalanceOrig=0"
```

#### Example Python `requests` Call

```python
import requests

payload = {
    "type": "CASH_OUT",
    "amount": 50000,
    "oldbalanceOrg": 50000,
    "newbalanceOrig": 0
}

response = requests.post("http://127.0.0.1:5000/predict", data=payload)
print(response.text)
```

---

## Internal Prediction Logic (`app.py`)

```python
# Build the feature array
input_array = np.array([[val, amount, oldbalanceOrg, newbalanceOrig]])

# Run inference
prediction = model.predict(input_array)

# Return the result
output = prediction[0]   # 0 = No Fraud, 1 = Fraud
```

The model loaded from `static/model.pkl` is a serialised scikit-learn `DecisionTreeClassifier`.

---

## Error Handling

The current implementation does not include explicit error handling. Common issues:

| Scenario | Behaviour |
|---|---|
| Missing form field | Flask raises a `400 Bad Request` |
| Invalid non-numeric amount | Python raises a `ValueError` |
| `model.pkl` not found | Flask raises a `500 Internal Server Error` |

For production use, wrap the prediction logic in a `try/except` block and return appropriate error messages.
