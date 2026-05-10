# Proyecto Final SBD — Agricultura y Ganadería Global

> **Curso de Especialización en Inteligencia Artificial y Big Data**  
> IES Azarquiel · David Menéndez Rodríguez · 2025-2026

Pipeline completo de Big Data y Machine Learning aplicado a datos agrícolas y ganaderos de 12 países. El proyecto cubre desde la ingesta y limpieza de datos heterogéneos (CSV, JSON, XML, TXT) hasta el entrenamiento de 6 modelos de ML y la generación de una capa de negocio financiera lista para consumir en Power BI.

---

## Tabla de Contenidos

- [Descripción del Proyecto](#descripción-del-proyecto)
- [Estructura del Repositorio](#estructura-del-repositorio)
- [Instalación](#instalación)
- [Orden de Ejecución](#orden-de-ejecución)
- [Modelos de Machine Learning](#modelos-de-machine-learning)
- [Dashboard Power BI](#dashboard-power-bi)
- [Principales Librerías](#principales-librerías)
- [Control de Versiones](#control-de-versiones)

---

## Descripción del Proyecto

El objetivo del proyecto es construir un sistema de análisis e inteligencia agrícola capaz de responder preguntas clave de negocio:

- ¿Qué país y cultivo ofrecen un beneficio estimado del 5–10%?
- ¿Cuántas hectáreas se necesitan para maximizar el rendimiento?
- ¿Qué tipo de ganado y cuántas cabezas maximizan el retorno?
- ¿En qué región o país se cumplen simultáneamente las condiciones óptimas?

Para ello se integran datos de **producción agrícola y ganadera**, **condiciones climáticas**, **políticas de subsidios**, **precios de mercado**, **reportes de plagas** y **noticias del sector**, procesados mediante un pipeline ETL modular y analizados con 6 modelos de Machine Learning independientes.

---

## Estructura del Repositorio

```
Proyecto-Final-SBD/
│
├── data/
│   ├── processed/               # Datasets maestros integrados y CSVs de ROI para Power BI
│   └── raw/                     # Excluido del repositorio (.gitignore)
│       ├── structured/          # Datasets estructurados originales (CSV)
│       ├── semi_structured/     # Datos semi-estructurados originales (JSON, XML)
│       └── unstructured/        # Datos no estructurados originales (TXT, JSONL)
│
├── documentacion/
│   ├── figures/                 # Gráficas generadas durante EDA, ETL y ML
│   ├── memoria.odt              # Memoria del proyecto (word editable)
│   └── memoria.pdf              # Memoria del proyecto (versión final)
│
├── models/
│   ├── P1/                      # Modelos y resultados — Predicción de Rendimiento
│   ├── P2/                      # Modelos y resultados — Clasificación Riesgo Plagas
│   ├── P3/                      # Modelos y resultados — Segmentación de Países
│   ├── P4/                      # Modelos y resultados — Predicción de Precios
│   ├── P5/                      # Modelos y resultados — Detección de Anomalías
│   └── P6/                      # Modelos y resultados — Recomendación de Cultivos
│
├── notebooks/
│   ├── Preprocesamiento y Limpieza/
│   │   ├── 01_Unstructured_Data.ipynb      # ETL datos no estructurados (TXT, JSONL + NLP)
│   │   ├── 02_Semi-Structured_Data.ipynb   # ETL datos semi-estructurados (JSON, XML)
│   │   ├── 03_Structured_Data.ipynb        # ETL datos estructurados (CSV)
│   │   ├── 04_Integración_Data.ipynb       # Integración y generación de datasets maestros
│   │   └── 05_EDA.ipynb                    # Análisis exploratorio de datos
│   │
│   ├── Machine Learning/
│   │   ├── 06_P1_prediccion_rendimiento.ipynb
│   │   ├── 07_P2_clasificacion_riesgos_plagas.ipynb
│   │   ├── 08_P3_segmentacion_paises_cluster.ipynb
│   │   ├── 09_P4_prediccion_precio_mercado.ipynb
│   │   ├── 10_P5_deteccion_anomalias.ipynb
│   │   └── 11_P6_recomendacion_cultivos.ipynb
│   │
│   └── 12_preparacion_dashboard_BI.ipynb   # Capa de negocio financiera (ROI) para Power BI
│
├── src/
│   └── powerBI.pbix             # Dashboard interactivo Power BI
│
├── .gitignore
├── requirements.txt
└── README.md
```

---

## Instalación

### Requisitos previos

- Python 3.13
- [Anaconda](https://www.anaconda.com/) o [Miniconda](https://docs.conda.io/en/latest/miniconda.html)
- [Visual Studio Code](https://code.visualstudio.com/) con la extensión Jupyter
- [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (solo para visualizar el dashboard)

### Configuración del entorno

```bash
# 1. Clonar el repositorio
git clone https://github.com/TU_USUARIO/Proyecto-Final-SBD.git
cd Proyecto-Final-SBD

# 2. Crear el entorno conda
conda create -n sbd-agriculture python=3.13

# 3. Activar el entorno
conda activate sbd-agriculture

# 4. Instalar las dependencias
pip install -r requirements.txt
```

> **Nota:** Los datos crudos (`data/raw/`) están excluidos del repositorio por su tamaño.  
> Deben colocarse manualmente en las subcarpetas correspondientes antes de ejecutar los notebooks.

---

## Orden de Ejecución

 **Los notebooks están numerados y deben ejecutarse en orden estrictamente secuencial**, ya que cada fase consume los outputs de la anterior.

### Fase 1 — Preprocesamiento y Limpieza (notebooks 01–04)

| Nº | Notebook | Entrada | Salida |
|----|----------|---------|--------|
| 01 | `01_Unstructured_Data.ipynb` | `data/raw/unstructured/` | `reportes_plagas_processed.csv`, `noticias_processed.csv` |
| 02 | `02_Semi-Structured_Data.ipynb` | `data/raw/semi_structured/` | `condiciones_climaticas_processed.csv`, `politicas_*.csv` |
| 03 | `03_Structured_Data.ipynb` | `data/raw/structured/` | `produccion_agricola_processed.csv`, `master_ganadero_base.csv`, `precios_mercado_processed.csv` |
| 04 | `04_Integración_Data.ipynb` | Todos los processed anteriores | `master_agricola.csv`, `master_ganadero.csv` |

### Fase 2 — Análisis Exploratorio (notebook 05)

| Nº | Notebook | Descripción |
|----|----------|-------------|
| 05 | `05_EDA.ipynb` | Análisis multivariante, correlaciones, distribuciones y visualizaciones exploratorias. Genera las figuras en `documentacion/figures/` |

### Fase 3 — Machine Learning (notebooks 06–11)

Estos notebooks son **independientes entre sí** y pueden ejecutarse en cualquier orden, pero requieren que la Fase 1 esté completada.

| Nº | Notebook | Problema |
|----|----------|----------|
| 06 | `06_P1_prediccion_rendimiento.ipynb` | Regresión — predicción de rendimiento (ton/ha) |
| 07 | `07_P2_clasificacion_riesgos_plagas.ipynb` | Clasificación — riesgo de brote de plagas |
| 08 | `08_P3_segmentacion_paises_cluster.ipynb` | Clustering — segmentación de países |
| 09 | `09_P4_prediccion_precio_mercado.ipynb` | Series temporales — tendencia de precios |
| 10 | `10_P5_deteccion_anomalias.ipynb` | Detección de anomalías en producción |
| 11 | `11_P6_recomendacion_cultivos.ipynb` | Recomendación — cultivo óptimo por condiciones |

### Fase 4 — Dashboard (notebook 12)

| Nº | Notebook | Entrada | Salida |
|----|----------|---------|--------|
| 12 | `12_preparacion_dashboard_BI.ipynb` | `master_agricola.csv`, `master_ganadero.csv`, `precios_mercado_processed.csv` | `BI_agricola_roi.csv`, `BI_ganadero_roi.csv` |

---

## Modelos de Machine Learning

| Problema | Algoritmo Principal | Métrica Clave |
|----------|--------------------|--------------:|
| P1 · Predicción de Rendimiento | Random Forest / XGBoost | RMSE, R² |
| P2 · Clasificación Riesgo Plagas | Random Forest / XGBoost | F1-score, AUC-ROC |
| P3 · Segmentación de Países | K-Means + análisis de silueta | Silhouette Score |
| P4 · Predicción de Precios | Prophet / XGBoost | MAE, Directional Accuracy |
| P5 · Detección de Anomalías | Isolation Forest | Precision@k, F1 anomalías |
| P6 · Recomendación de Cultivos | Random Forest / KNN | Top-3 Accuracy |

Los modelos entrenados se serializan en `models/Px/` en formato `.pkl` / `.joblib`. Estos archivos están excluidos del repositorio y se regeneran ejecutando el notebook correspondiente.

---

## Dashboard Power BI

El archivo `src/powerBI.pbix` contiene el dashboard interactivo. Para abrirlo correctamente:

1. Ejecutar el notebook `12_preparacion_dashboard_BI.ipynb` para generar los CSVs de ROI.
2. Abrir `powerBI.pbix` con Power BI Desktop.
3. En la cinta superior ir a **Inicio → Transformar datos → Configuración del origen de datos**.
4. Actualizar las rutas de los siguientes archivos apuntando a `data/processed/` local:
   - `BI_agricola_roi.csv`
   - `BI_ganadero_roi.csv`
   - `master_agricola.csv`
   - `master_ganadero.csv`
5. Hacer clic en **Actualizar** para cargar los datos.

---

## Principales Librerías

| Categoría | Librería | Versión |
|-----------|----------|---------|
| Manipulación de datos | `pandas` | 2.x |
| Cálculo numérico | `numpy` | 2.4.3 |
| Machine Learning | `scikit-learn` | 1.8.0 |
| Gradient Boosting | `xgboost`, `lightgbm` | — |
| Deep Learning | `tensorflow` / `keras` | 2.21.0 / 3.14.0 |
| Series temporales | `prophet` | 1.3.0 |
| NLP | `spacy`, `nltk`, `gensim` | — |
| Análisis de sentimiento | `pysentimiento` | 0.7.3 |
| Visualización | `matplotlib`, `seaborn`, `plotly` | 3.10.8 / — / — |
| Geoespacial | `geopandas`, `shapely` | 1.1.3 / 2.1.2 |
| Interpretabilidad ML | `shap` | — |
| Datos desbalanceados | `imbalanced-learn` | 0.14.1 |
| Estadística | `statsmodels`, `scipy` | — / 1.17.1 |
| XML / HTML | `lxml`, `beautifulsoup4` | 6.0.2 / — |
| Modelos Transformer | `transformers`, `huggingface_hub` | 5.6.2 / 1.12.0 |

> El archivo `requirements.txt` incluye la lista completa de dependencias del entorno.

---

## Control de Versiones

El proyecto está gestionado con **Git** y alojado en GitHub, desarrollado con **Visual Studio Code**.

El archivo `.gitignore` excluye:

```
venv/                    # Entorno virtual
__pycache__/             # Caché de Python
.ipynb_checkpoints/      # Checkpoints de Jupyter
data/raw/                # Datos crudos (tamaño excesivo para el repo)
models/**/*.pkl          # Modelos entrenados (regenerables)
models/**/*.joblib       # Modelos entrenados (regenerables)
*.DS_Store               # Metadatos macOS
Thumbs.db                # Metadatos Windows
```

---

*IES Azarquiel · Curso Especialización IA y Big Data · 2025-2026*
