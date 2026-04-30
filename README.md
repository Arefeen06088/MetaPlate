# MetaPlate
<p align="center">
<img width="1536" height="1024" alt="Image" src="https://github.com/user-attachments/assets/fd0de8ae-3dd1-4afd-b5ea-0761abe2112a" />
</p>

# 🍽️ Personalized Meal Generation via Counterfactual Modeling + LLMs

This repository presents an end-to-end pipeline for **predicting postprandial glucose response**, generating **counterfactual dietary recommendations**, and translating them into **realistic meal plans using Large Language Models (LLMs)**.

<p align="center">
<img width="1196" height="341" alt="Image" src="https://github.com/user-attachments/assets/d65a7cdb-3e11-4387-b174-fec2c151e064" />
</p>

---

## 🧠 Overview

This project combines:

* 📈 **Machine Learning (LightGBM)** → Predict glucose response
* 🔁 **Counterfactual Optimization** → Adjust macronutrients (carbs, protein, fat)
* 🤖 **LLM-based Translation** → Convert macros into realistic meals

The goal is to move from **prediction → intervention → actionable recommendation**.

---

## 📂 Repository Structure

```
.
├── forecasting.ipynb                  # Model training & evaluation
├── Counterfactual_generation.ipynb    # Counterfactual macro optimization
├── meal_generation_with_LLM_RAG.ipynb # LLM-based meal generation
│
├── artifacts/                         
│   ├── lgbm_model.pkl                 # model (NOT committed)
│   ├── counterfactuals.csv
│   └── meals/
│
├── all_api.env                        # API keys (NOT committed)
└── README.md
```

---

## ⚙️ Pipeline

### 1️⃣ Forecasting (Glucose Prediction)

* Train multiple models (LightGBM, XGBoost, RF, etc.)
* Perform hyperparameter tuning
* Select best LightGBM model
* Evaluate using:

  * RMSE, MAE, R²
  * MAPE, SMAPE
  * Pearson correlation

📌 Output:

* Trained model (`lgbm_model.pkl`)
* Predictions + evaluation metrics

---

### 2️⃣ Counterfactual Generation

* For each test sample:

  * Identify high predicted glucose responses
  * Optimize macronutrients to reduce predicted glucose
* Uses:

  * `scipy.optimize.differential_evolution`
* Constraints:

  * Reduce carbs primarily
  * Keep meals realistic (bounded protein/fat)

📌 Output:

* `counterfactuals.csv`
* Metrics:

  * % achieving target glucose threshold
  * Average carb reduction
  * Predicted glucose drop

---

### 3️⃣ LLM-based Meal Generation (RAG-style)

* Convert optimized macros → realistic meals
* Uses a **single unified prompt** across LLMs
* Models supported:

  * GPT-5.4 / GPT-5.4-mini
  * Gemini 2.5 Flash
  * Groq-hosted LLaMA

📌 Key design:

* Only **2 counterfactuals per model** (for controlled comparison)
* Structured JSON output
* USDA FoodData-based constraints

📌 Output:

* `artifacts/meals/*.csv`

---

## 🧑‍🍳 Example Flow

```
Input:
  Meal → Features → Model predicts high glucose

↓

Counterfactual:
  Reduce carbs, adjust macros

↓

LLM:
  "Chicken + brown rice + broccoli" style meal
```

---

## 🔑 API Setup

Create a file named:

```
all_api.env
```

Add your keys:

```
GROQ_API_KEY=your_groq_key
OPENAI_API_KEY=your_openai_key
GEMINI_API_KEY=your_gemini_key
```

⚠️ Do NOT commit this file.

---

## ▶️ How to Run

### 1. Train model

Open:

```
forecasting.ipynb
```

Run all cells.

---

### 2. Generate counterfactuals

Open:

```
Counterfactual_generation.ipynb
```

Run all cells.

---

### 3. Generate meals

Open:

```
meal_generation_with_LLM_RAG.ipynb
```

Run all cells.

---

## 📊 Evaluation

### ML Model

* RMSE ≈ ~17.5 mg/dL (LightGBM)
* Strong correlation with observed glucose

### Counterfactuals

* Significant reduction in predicted glucose
* Controlled macronutrient adjustments

### LLM Outputs

* Structured JSON
* Realistic meal composition
* Macro adherence within tolerance

---

## ⚠️ Limitations

* Counterfactuals are **associational**, not causal
* LLM outputs depend on prompt quality
* API rate limits (Gemini, Groq) may require retries
* Some models may not always be available (e.g., Kimi K2)

---

## 🚀 Future Work

* Personalized constraints (dietary preferences, allergies)
* Multi-meal planning (daily diet)
* Integration with real-time glucose monitoring
* Automated evaluation of meal realism

---

## 🤝 Acknowledgments

* LightGBM / XGBoost / Scikit-learn
* Groq, OpenAI, Google Gemini APIs
* USDA FoodData Central

---

## 📌 Notes

* This repo is structured for **research + experimentation**
* Not intended for clinical deployment without validation

---

## 📬 Contact

For questions or collaboration, feel free to reach out.
