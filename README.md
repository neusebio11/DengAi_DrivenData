# DengAI: Predicting Disease Spread

Solução completa para a competição [DengAI](https://www.drivendata.org/competitions/44/dengai-predicting-disease-spread/) da DrivenData.

## Descrição do Problema

O objectivo é prever o número semanal de casos de dengue em duas cidades tropicais:

| Cidade | País | Período de treino | Período de teste |
|--------|------|:-----------------:|:----------------:|
| San Juan (sj) | Puerto Rico | 1990–2008 (936 sem.) | 2008–2013 (260 sem.) |
| Iquitos (iq) | Peru | 2000–2010 (520 sem.) | 2010–2013 (156 sem.) |

**Métrica de avaliação:** MAE (Mean Absolute Error) em escala original de casos/semana.  
**Referência competitiva:** MAE < 25 (agregado).

### Dados disponíveis

Cada observação é uma semana numa cidade, com 20 variáveis climáticas e ambientais:

- **NDVI** (Normalized Difference Vegetation Index) — 4 quadrantes geográficos, por satélite
- **Temperatura** — estação local (°C) e reanálise NOAA (K)
- **Humidade** — relativa (%), específica (g/kg), temperatura do ponto de orvalho (K)
- **Precipitação** — mm, kg/m²
- **Amplitude térmica diurna** — diferença máx./mín. diária

---

## Estrutura do Projecto

```
DengAI/
├── database/
│   ├── dengue_features_train.csv   # variáveis climáticas de treino
│   ├── dengue_labels_train.csv     # target (total_cases) de treino
│   ├── dengue_features_test.csv    # variáveis climáticas de teste
│   ├── submission_format.csv       # formato exacto de submissão
│   ├── processed/                  # datasets após pré-processamento (gerados pelo NB2)
│   │   ├── train_sj.csv
│   │   ├── train_iq.csv
│   │   ├── test_sj.csv
│   │   └── test_iq.csv
│   └── models/                     # modelos serializados (gerados pelo NB4)
│       ├── xgb_sj.pkl
│       ├── xgb_iq.pkl
│       ├── lgbm_sj.pkl
│       └── lgbm_iq.pkl
├── 01_EDA.ipynb                    # Análise Exploratória de Dados
├── 02_Preprocessing.ipynb          # Pré-processamento e Feature Engineering
├── 03_Baseline.ipynb               # Modelos Baseline (NB Regression + Random Forest)
├── 04_Advanced_Models.ipynb        # Modelos Avançados (XGBoost, LightGBM, SARIMAX)
├── 05_Ensemble_Submission.ipynb    # Ensemble e geração do submission.csv
├── submission.csv                  # ficheiro de submissão final
└── README.md                       # este ficheiro
```

### O que faz cada notebook

| Notebook | Fase | Conteúdo principal |
|----------|------|--------------------|
| `01_EDA.ipynb` | Exploração | Missing values, distribuição do target, séries temporais, sazonalidade, correlações, análise de NDVI/temperatura/humidade |
| `02_Preprocessing.ipynb` | Engenharia | Separação por cidade, interpolação linear, lag features (1–4 sem.), rolling averages (3–4 sem.), encoding cíclico sin/cos, transformação log1p |
| `03_Baseline.ipynb` | Modelação | Negative Binomial Regression (statsmodels), Random Forest (scikit-learn), TimeSeriesSplit 5-fold, feature importance |
| `04_Advanced_Models.ipynb` | Modelação | XGBoost + GridSearchCV, LightGBM + GridSearchCV, SARIMAX, tabela comparativa |
| `05_Ensemble_Submission.ipynb` | Submissão | Ensemble ponderado por 1/MAE, validação do formato, geração de `submission.csv` |

---


### Benchmark e Competição

- DrivenData Benchmark: https://drivendata.co/blog/dengue-benchmark
- Competição: https://www.drivendata.org/competitions/44/dengai-predicting-disease-spread/

### Bibliotecas

| Biblioteca | Versão recomendada | GitHub |
|------------|:-----------------:|--------|
| pandas | 2.x | https://github.com/pandas-dev/pandas |
| numpy | 1.24+ | https://github.com/numpy/numpy |
| matplotlib | 3.7+ | https://github.com/matplotlib/matplotlib |
| seaborn | 0.12+ | https://github.com/mwaskom/seaborn |
| scikit-learn | 1.3+ | https://github.com/scikit-learn/scikit-learn |
| xgboost | 1.7+ | https://github.com/dmlc/xgboost |
| lightgbm | 3.3+ | https://github.com/microsoft/LightGBM |
| statsmodels | 0.14+ | https://github.com/statsmodels/statsmodels |
| missingno | 0.5+ | https://github.com/ResidentMario/missingno |
| scipy | 1.10+ | https://github.com/scipy/scipy |

---

