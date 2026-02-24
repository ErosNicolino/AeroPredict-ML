# ✈️ AeroPredict --- Flight Delay Prediction Pipeline

[![Status](https://img.shields.io/badge/status-complete-brightgreen)](https://github.com/ErosNicolino/AeroPredict-ML)
[![Python](https://img.shields.io/badge/python-%3E%3D3.10-blue)](https://www.python.org/)
[![Scikit-learn](https://img.shields.io/badge/scikit--learn-%3E%3D1.3.0-orange)](https://scikit-learn.org/)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

---

## 📌 Overview

AeroPredict é um projeto de Machine Learning Engineering focado na
previsão de atrasos de voos utilizando dados estruturados da aviação.

O projeto aplica boas práticas de engenharia:

- Pipeline de pré-processamento sem data leakage\
- Validação cruzada estratificada\
- Comparação de modelos supervisionados\
- Interpretação de variáveis\
- Discussão crítica de limitações

---

## 📂 Dataset

Fonte: Flight Delays and Cancellations (flights.csv)

Target criada:

IS_DELAYED = 1 se ARRIVAL_DELAY \> 15 minutos

---

## 🧹 Data Preprocessing

1.  Remoção de voos cancelados/desviados\
2.  Tratamento de valores ausentes\
3.  OneHotEncoding para categóricas\
4.  Padronização para variáveis numéricas\
5.  Pipeline com ColumnTransformer + Pipeline

---

## 🤖 Modelos

### Logistic Regression

Modelo linear baseline com class_weight='balanced'

### Random Forest

Modelo ensemble não-linear com análise de importância de features

---

## 🔁 Validação

- Train/Test split estratificado\
- Stratified K-Fold (k=5)\
- Métricas:
  - Accuracy
  - Recall
  - ROC-AUC
  - Confusion Matrix
  - ROC Curve

---

## 📊 Performance

Modelo Accuracy Recall ROC-AUC

---

Logistic Regression \~0.59 \~0.62 \~0.63
Random Forest \~0.65 \~0.52 \~0.64

---

## 📊 Visualizações

### ROC Curve

![ROC](imagens/viz_cell_4_2.png)

### Feature Importance

![Importance](imagens/viz_cell_4_5.png)

### Elbow Method

![Elbow](imagens/viz_cell_5_2.png)

### Delay Map

![Map](imagens/newplot.png)

---

## 📉 Limitações

- Ausência de dados climáticos externos\
- Dataset limitado a um ano\
- Natureza parcialmente estocástica dos atrasos

---

## 🛠 Tech Stack

- Python\
- Pandas / NumPy\
- Scikit-learn\
- Matplotlib / Seaborn\
- Jupyter

---

## 📁 Estrutura

AeroPredict-ML/
│
├── imagens/
├── notebooks/
├── data/
├── README.md
├── LICENSE
└── requirements.txt

---

## 📎 Como Executar

pip install -r requirements.txt

Abrir notebook e executar.

---

## 👤 Autor

Eros Nicolino da Rocha

---

## 📌 Licença

MIT License
