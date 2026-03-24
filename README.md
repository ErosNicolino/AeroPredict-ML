# ✈️ AeroPredict — Flight Delay Prediction Pipeline

[![Status](https://img.shields.io/badge/status-concluido-brightgreen)](.)
[![Python](https://img.shields.io/badge/python-3.10%2B-blue)](https://www.python.org/)
[![Scikit-learn](https://img.shields.io/badge/scikit--learn-ML-orange)](https://scikit-learn.org/)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE.txt)

## 📌 Visão Geral

O **AeroPredict** é um pipeline de Machine Learning para previsão de atrasos de voos nos EUA, com foco em práticas de pré-processamento, validação robusta, comparação de modelos supervisionados e análise não supervisionada de aeroportos.

- **Autor:** Eros Nicolino da Rocha
- **Dataset principal:** Flight Delays and Cancellations (`flights.csv`)
- **Volume original:** 5.819.079 voos
- **Target:** `IS_DELAYED = 1` se `ARRIVAL_DELAY > 15` minutos

## 🧾 Base de Dados e Definição do Problema

O objetivo supervisionado é classificar se um voo chegará com atraso relevante (mais de 15 minutos). A modelagem foi realizada com variáveis disponíveis no contexto operacional e avaliação estratificada para lidar com desbalanceamento.

## 🧹 Pré-processamento Implementado

As etapas efetivamente implementadas no pipeline foram:

1. Preenchimento de `CANCELLATION_REASON` com `'None'`.
2. Preenchimento de colunas de atraso (`AIR_SYSTEM_DELAY`, `SECURITY_DELAY`, `AIRLINE_DELAY`, `LATE_AIRCRAFT_DELAY`, `WEATHER_DELAY`) com `0`.
3. Remoção de voos cancelados e desviados (`CANCELLED = 0` e `DIVERTED = 0`).
4. `dropna` em `ARRIVAL_DELAY` e `DEPARTURE_DELAY`.
5. Base final limpa com **5.714.008 voos**.
6. `merge` com `airlines.csv` para obter nome completo das companhias (`AIRLINE_NAME`).
7. Amostragem de **5%** para modelagem supervisionada (**285.700 linhas**) visando viabilidade computacional da validação cruzada.
8. Pipeline com `ColumnTransformer`:
   - `StandardScaler` para variáveis numéricas.
   - `OneHotEncoder` para variáveis categóricas.

## 🧠 Feature Engineering

- `IS_DELAYED`: variável alvo binária (`ARRIVAL_DELAY > 15`).
- `TIME_OF_DAY`: `Morning`, `Afternoon`, `Evening`, `Night` (a partir de `SCHEDULED_DEPARTURE`).
- `SEASON`: `Winter`, `Spring`, `Summer`, `Autumn` (meses do hemisfério norte).

## 🧪 Features Utilizadas nos Modelos Supervisionados

- `AIRLINE_NAME`
- `DAY_OF_WEEK`
- `MONTH`
- `TIME_OF_DAY`
- `SEASON`
- `DISTANCE`

## 🤖 Modelagem Supervisionada

### Configuração de validação

- Split holdout estratificado: **80/20**
- Validação cruzada: **Stratified K-Fold (k = 5)**
- Desbalanceamento no teste: **46.971 negativos vs 10.169 positivos**

### Resultados reais

| Modelo              | Configuração                                                  | CV ROC-AUC (média +/- desvio) | Holdout Accuracy | Holdout Precision (classe 1) | Holdout Recall (classe 1) | Holdout F1 (classe 1) |
| ------------------- | ------------------------------------------------------------- | ----------------------------- | ---------------- | ---------------------------- | ------------------------- | --------------------- |
| Logistic Regression | `class_weight='balanced'`, `max_iter=1000`                    | `0.6315 +/- 0.0045`           | `0.59`           | `0.24`                       | `0.62`                    | `0.35`                |
| Random Forest       | `class_weight='balanced'`, `n_estimators=100`, `max_depth=15` | `0.6395 +/- 0.0026`           | `0.65`           | `0.26`                       | `0.52`                    | `0.35`                |

### Interpretação objetiva

- O `RandomForestClassifier` apresentou melhor desempenho global em acurácia e ROC-AUC.
- Ambos os modelos têm poder discriminativo moderado (ROC-AUC na faixa de ~0.63 a ~0.64), coerente com a complexidade e incerteza inerente ao problema.

## 🌲 Importância de Variáveis (Random Forest)

Após agrupar importâncias por variável original (revertendo expansões do One-Hot Encoding), a ordem de maior relevância observada foi:

1. `DISTANCE`
2. `AIRLINE_NAME`
3. `MONTH`

## 🛰️ Modelagem Não Supervisionada: K-Means em Aeroportos

Pipeline aplicado:

1. Agregação por `ORIGIN_AIRPORT` com médias de `DEPARTURE_DELAY`, `ARRIVAL_DELAY`, `TAXI_OUT` e contagem de voos.
2. Filtro para aeroportos com mais de `1.000` voos.
3. Normalização com `StandardScaler`.
4. Método do cotovelo testado de `K=2` a `K=7`.
5. Escolha de `K=3` (cotovelo identificado em `K=3`).

### Perfis médios dos clusters

| Cluster | Perfil        | DEPARTURE_DELAY | ARRIVAL_DELAY | TAXI_OUT |
| ------- | ------------- | --------------- | ------------- | -------- |
| Grupo A | Eficiente     | `4.09`          | `-0.02`       | `12.88`  |
| Grupo B | Crítico       | `9.14`          | `6.02`        | `13.87`  |
| Grupo C | Intermediário | `7.13`          | `1.63`        | `19.08`  |

Visualizações implementadas:

- Scatter plot dos clusters com `hue=cluster` e `size=TAXI_OUT`.
- Mapa geográfico interativo (Plotly) dos aeroportos por cluster nos EUA.

## ⚠️ Limitações Reais

- Ausência de dados climáticos externos, fator importante para atrasos.
- Base limitada ao ano de 2015.
- Necessidade de amostragem de 5% por limitação de memória.
- Features pré-voo disponíveis têm poder preditivo moderado (ROC-AUC ~0.63-0.64).
- Natureza parcialmente estocástica dos atrasos reduz teto de precisão.
- `TAXI_OUT` pode ser explorada como feature adicional no cenário supervisionado.

## 🚀 Próximos Passos Sugeridos

- Incluir `TAXI_OUT` nas features dos modelos supervisionados.
- Testar modelos de boosting como **XGBoost** e **LightGBM**.
- Incorporar dados climáticos externos via API.
- Expandir o recorte temporal para múltiplos anos.
- Criar feature de rota (`ORIGIN_AIRPORT + DESTINATION_AIRPORT`).

## 🛠️ Tech Stack

- Python 3.10+
- Pandas
- NumPy
- Scikit-learn
- Matplotlib
- Seaborn
- Plotly
- Jupyter

## 📁 Estrutura do Projeto

```text
AeroPredict-ML/
├── data/
│   ├── flights.csv
│   ├── airlines.csv
│   └── airports.csv
├── images/
├── notebooks/
│   └── 01_eda.ipynb
├── README.md
├── LICENSE.txt
└── requirements.txt
```

## ▶️ Como Executar

```bash
pip install -r requirements.txt
```

Em seguida:

1. Abra `notebooks/01_eda.ipynb`.
2. Execute todas as células em ordem.

## 👤 Autor

**Eros Nicolino da Rocha**

## 📄 Licença

Este projeto está licenciado sob os termos descritos em `LICENSE.txt`.
