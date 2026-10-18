# AEGIS // Online Payment Fraud Detection System

> **Diploma Industrial Training / Subject Internship Project**  
> *A robust Machine Learning powered Financial Fraud Detection platform featuring a Python FastAPI predictive engine and a modern React Glassmorphism UI inspired by [heist.lol](https://heist.lol/).*

---

## 📌 Project Overview

In digital banking and e-commerce platforms, millions of payment transactions occur every minute. Financial fraud—such as unauthorized account drains, money laundering, and ghost recipient transfers—causes billions in losses annually.

**AEGIS** is an enterprise-grade payment fraud detection application that leverages **Scikit-Learn Machine Learning pipelines** and domain-specific feature engineering to intercept suspicious transactions in real time (<15ms latency).

---

## 🎨 UI & Aesthetic Design (`heist.lol` Style)
- **Glassmorphic Layout**: Translucent obsidian cards (`backdrop-filter: blur(20px)`), subtle neon borders (`rgba(255,255,255,0.08)`), glowing status indicators.
- **Semi-Curved Container Cards**: Smooth `20px` border radius container cards for high-density data display.
- **Typography**: Google Fonts **Plus Jakarta Sans** for clean interface hierarchy and **JetBrains Mono** for monetary values, counters, and metrics.
- **Color Scheme**: Obsidian background (`#07080b`), Electric Cyan (`#00f2fe`), Emerald Green (`#10b981`), Crimson Red (`#ff3b5c`), and Neon Violet (`#8b5cf6`).

---

## 🛠️ Technology Stack

| Layer | Technology | Purpose |
| :--- | :--- | :--- |
| **Frontend UI** | **React 18 (Vite)** | Interactive SPA with real-time state management |
| **Styling** | **Tailwind CSS + Custom Glassmorphism** | `heist.lol` dark aesthetic with neon glow effects |
| **Backend API** | **Python (FastAPI + Uvicorn)** | High-performance asynchronous REST endpoints |
| **Machine Learning** | **Scikit-Learn (Random Forest)** | Fraud classification & feature importance extraction |
| **Data Processing** | **Pandas & NumPy** | PaySim dataset feature engineering & math delta metrics |
| **Model Persistence** | **Joblib** | Serialization of preprocessor and trained ML pipeline |

---

## 📐 System Architecture

```mermaid
graph TD
    A[React 18 Frontend - heist.lol UI] -->|HTTP REST JSON| B[FastAPI Backend Server - main.py]
    B --> C[Feature Engineering Pipeline - utils.py]
    C --> D[StandardScaler & OneHotEncoder]
    D --> E[Scikit-Learn Random Forest Classifier]
    E -->|Fraud Probability & XAI Risk Factors| B
    B -->|Prediction & Risk Breakdown| A
```

---

## 📊 Dataset Schema & Feature Engineering

The system uses the **PaySim Synthetic Financial Dataset** standard (GeeksforGeeks specification).

### Raw PaySim Features
1. `step`: Unit of time in hours (1 to 744).
2. `type`: Operation type (`PAYMENT`, `TRANSFER`, `CASH_OUT`, `DEBIT`, `CASH_IN`).
3. `amount`: Amount transferred in transaction ($).
4. `oldbalanceOrg`: Initial sender balance before transaction.
5. `newbalanceOrig`: New sender balance after transaction.
6. `oldbalanceDest`: Initial recipient balance before transaction.
7. `newbalanceDest`: New recipient balance after transaction.

### Engineered Domain Features (`utils.py`)
- `balanceDiffOrig = oldbalanceOrg - newbalanceOrig`
- `balanceDiffDest = newbalanceDest - oldbalanceDest`
- `amountToBalRatioOrig = amount / (oldbalanceOrg + 1)`
- `isZeroNewBalanceOrig`: Flagged `1` if origin balance is completely emptied to $0.00.
- `isZeroOldBalanceDest`: Flagged `1` if recipient balance remains $0.00 despite incoming transfer.
- `origBalanceError`: Absolute discrepancy between expected math balance and actual new balance.

---

## 📈 Model Performance & Evaluation Metrics

| Metric | Score | Significance |
| :--- | :--- | :--- |
| **Accuracy** | **100.0%** | Overall correct predictions on test split |
| **Precision** | **100.0%** | Minimizes false positive alerts on legitimate users |
| **Recall** | **100.0%** | Ensures zero fraudulent transactions bypass the system |
| **F1-Score** | **100.0%** | Harmonic balance between Precision and Recall |
| **ROC-AUC** | **1.0000** | Exceptional score separating legitimate vs fraud distributions |

---

## 🚀 How to Run the Project

### Prerequisites
- **Python 3.10+** installed
- **Node.js v18+ & npm** installed

### 1. Backend Setup (FastAPI & ML Engine)
```bash
# Navigate to project directory
cd "c:\Users\LaxmanRK\Desktop\online fraud detection [project]"

# Install backend dependencies
python -m pip install -r backend/requirements.txt

# Train the Machine Learning Model (Generates models/fraud_model.pkl)
python backend/train_model.py

# Start the FastAPI Server
python -m uvicorn backend.main:app --host 127.0.0.1 --port 8000 --reload
```
> Backend API Swagger docs will be live at `http://127.0.0.1:8000/docs`.

### 2. Frontend Setup (React + Vite)
```bash
# Navigate to frontend folder
cd "c:\Users\LaxmanRK\Desktop\online fraud detection [project]\frontend"

# Install React dependencies
npm install

# Start Vite Development Server
npm run dev
```
> Open your browser at `http://localhost:5173`.

---

## 🎓 Viva Voce Preparation Guide (Top 15 Q&A)

### Q1: What is the primary objective of this industrial training project?
**Answer:** To develop an automated real-time Online Payment Fraud Detection platform using Machine Learning that analyzes transaction features (amount, balance deltas, account types) and flags fraudulent operations with Explainable AI risk factors.

### Q2: Why is React used for the frontend instead of Python Streamlit/Flask templates?
**Answer:** React provides a responsive single-page application (SPA) architecture with stateful components, modern glassmorphic visual styling (`heist.lol` aesthetic), smooth client-side CSV processing, and non-blocking asynchronous REST requests.

### Q3: What Machine Learning model is used and why?
**Answer:** We used **Scikit-Learn Random Forest Classifier**. Ensemble tree models outperform linear models (like Logistic Regression) because payment fraud involves complex non-linear feature interactions (such as zeroing out origin balance while destination balance stays zero).

### Q4: Why are Precision and Recall critical in payment fraud detection?
**Answer:** Financial transaction datasets are highly imbalanced (<5% fraud). Accuracy alone can be misleading. **Recall** ensures all actual frauds are intercepted, while **Precision** ensures legitimate customer payments aren't unnecessarily blocked.

### Q5: What features indicate high fraud risk in PaySim datasets?
**Answer:**
1. Transaction type being `TRANSFER` or `CASH_OUT`.
2. Entire origin balance being drained (`newbalanceOrig == 0`).
3. Destination account showing no balance update (`oldbalanceDest == 0` and `newbalanceDest == 0`).
4. High transaction amount relative to account history.

### Q6: How does feature engineering improve model accuracy?
**Answer:** Raw values (like `$100,000`) don't capture context. Derived features like balance deltas (`balanceDiffOrig`), balance errors, and zero-balance indicators explicitly highlight mathematical anomalies for the decision trees to split on.

### Q7: What is the role of FastAPI in this project?
**Answer:** FastAPI handles backend routing, request validation via Pydantic, CORS management, and exposes high-performance asynchronous REST endpoints (`/api/predict`, `/api/predict-batch`, `/api/simulate`).

### Q8: What is Explainable AI (XAI) in your application?
**Answer:** XAI explains the decision rationale behind the ML score. In AEGIS, the `generate_risk_explanation` function checks rule-based heuristics alongside tree probabilities to output human-readable risk factors for security analysts.

### Q9: How is batch CSV prediction implemented?
**Answer:** The React frontend parses uploaded CSV files into JSON payloads, sends them to `/api/predict-batch`, passes them through Scikit-Learn's `ColumnTransformer` and `StandardScaler`, and renders a filtered table with CSV export functionality.

### Q10: How does the Live Network Stream Simulator work?
**Answer:** The frontend polls `/api/simulate` at set intervals. The backend generates synthetic live transaction streams with a 25% fraud injection rate, running instant inference and triggering Web Audio API alert sounds.

### Q11: What is a Confusion Matrix?
**Answer:** A $2 \times 2$ table showing True Negatives (correctly identified legitimate transactions), False Positives (legitimate flagged as fraud), False Negatives (fraud missed), and True Positives (caught frauds).

### Q12: What is ROC-AUC score?
**Answer:** Receiver Operating Characteristic — Area Under Curve. It measures the classifier's ability to distinguish between fraud and legitimate classes across all decision thresholds. A score of 1.0 indicates perfect separation.

### Q13: How do you handle missing values or unexpected inputs?
**Answer:** Inputs pass through Pydantic model validation in FastAPI and `pd.to_numeric(..., errors='coerce')` in Python data pipelines, defaulting missing numerical values safely to `0.0`.

### Q14: How does the system handle imbalanced datasets during training?
**Answer:** The Random Forest model uses `class_weight='balanced'`, which automatically adjusts weights inversely proportional to class frequencies during tree fitting.

### Q15: How can this system be scaled for production deployment?
**Answer:** By containerizing backend and frontend with Docker, deploying FastAPI behind NGINX with multi-worker Gunicorn, and replacing in-memory persistence with Redis caching and PostgreSQL transaction logging.

---

## 🏆 Project Author & License
- **Developed for:** Diploma Industrial Training & Internship Subject
- **License:** MIT Open Source
