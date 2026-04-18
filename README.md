# DATA-TELCO_CHURN_ANALYSIS

Análisis exploratorio y modelado predictivo de cancelación de servicios de telecomunicaciones (churn) sobre el conjunto de datos público *Telco Customer Churn* de Kaggle, implementado como un pipeline reproducible en Jupyter.


## Tabla de contenido

1. [Descripción del proyecto](#descripción-del-proyecto)
2. [Estructura del proyecto](#estructura-del-proyecto)
3. [Instalación](#instalación)
4. [Uso](#uso)


---

## Descripción del proyecto

El presente repositorio contiene el desarrollo íntegro de un pipeline de ciencia de datos orientado a la predicción de churn en clientes de una empresa de telecomunicaciones. El conjunto de datos de origen, extraído de [Kaggle — Telco Customer Churn](https://www.kaggle.com/datasets/blastchar/telco-customer-churn), comprende 7 043 observaciones y 21 variables que describen características demográficas, contractuales y de consumo de cada cliente.

### Flujo de datos

El pipeline se articula en cinco etapas secuenciales:

| Etapa | Descripción |
|---|---|
| **Extracción (ETL)** | Carga del CSV original, inspección de tipos de dato, detección y tratamiento de valores nulos, y corrección de tipos (p. ej. `TotalCharges` como numérico). |
| **Transformación** | Codificación de variables categóricas binarias, ordinales y nominales mediante one-hot encoding y mapeos ordinales; discretización de `tenure` en grupos de antigüedad; estandarización con `StandardScaler`. |
| **Análisis Exploratorio (EDA)** | Visualización de distribuciones univariadas y bivariadas, análisis de correlaciones, tasas de churn por segmentos y box plots de variables numéricas. |
| **Reducción de dimensionalidad** | Aplicación de PCA sobre el espacio de características estandarizado; análisis de varianza explicada acumulada y proyección 2D para identificar separabilidad de clases. |
| **Modelado predictivo** | Entrenamiento y evaluación comparativa de tres clasificadores supervisados — Regresión Logística, Random Forest y Gradient Boosting — con balanceo de clases mediante SMOTE y métricas de Accuracy, Precision, Recall, F1-Score y ROC-AUC. |

El modelo de mejor desempeño se selecciona dinámicamente bajo el criterio de mayor Recall, priorizando la detección de clientes en riesgo real de cancelación. Los artefactos intermedios y procesados se persisten en el directorio `data/` para garantizar la reproducibilidad del pipeline sin necesidad de reejecutar etapas anteriores.



---

## Estructura del proyecto

```
telco-churn-analysis/
│
├── notebook_analisis.ipynb          # Notebook principal — pipeline completo EDA → modelado
│
├── requirements.txt                 # Dependencias del proyecto con versiones fijadas
│
├── data/
│   ├── raw/
│   │   └── WA_Fn-UseC_-Telco-Customer-Churn.csv   # Dataset original sin modificaciones (Kaggle)
│   │
│   ├── interim/
│   │   └── telco_interim.csv                       # Dataset limpio tras la etapa de ETL
│   │
│   └── processed/
│       └── telco_processed.csv                     # Dataset final codificado y listo para modelado
│
└── venv/                            # Entorno virtual de Python (excluido del control de versiones)
```



&nbsp;

---

## Instalación

### Requisitos previos

- Python 3.11 o superior
- `pip` y `venv` disponibles en el sistema
- Git instalado

### Pasos

**1. Clonar el repositorio**

```bash
git clone https://github.com/andresm-data/data-telco-churn-analysis telco-churn-analysis
cd telco-churn-analysis
```

**2. Crear y activar el entorno virtual**

```bash
python -m venv venv
source venv/bin/activate          # Linux / macOS
# venv\Scripts\activate           # Windows
```

**3. Instalar las dependencias**

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

Las dependencias principales incluyen `polars`, `numpy`, `scikit-learn`, `imbalanced-learn`, `matplotlib`, `seaborn` y `jupyterlab`, entre otras. Todas las versiones se encuentran fijadas en `requirements.txt` para garantizar la reproducibilidad del entorno.

---

## Uso

### Ejecución mediante JupyterLab

La forma principal de interactuar con el proyecto es a través del notebook `notebook_analisis.ipynb`. Para iniciarlo, con el entorno virtual activo, se ejecuta el siguiente comando:

```bash
jupyterlab notebook_analisis.ipynb
```

Una vez abierto en el navegador, las celdas se ejecutan de forma secuencial desde la primera hasta la última mediante la opción **Run → Run All Cells**, o de forma individual utilizando `Shift + Enter`. Cada sección del notebook puede ejecutarse de manera independiente siempre que las etapas previas hayan depositado sus artefactos en el directorio `data/`.

### Ejecución no interactiva (headless)

Para la ejecución completa del notebook sin interfaz gráfica, por ejemplo en un servidor remoto o entorno de integración continua, se puede usar `nbconvert`:

```bash
jupyter-nbconvert --to notebook --execute notebook_analisis.ipynb \
    --output notebook_analisis_ejecutado.ipynb
```

Este comando genera un nuevo archivo `.ipynb` con todos los resultados y gráficas embebidas, sin necesidad de intervención manual.

### Navegación por secciones

El notebook se encuentra estructurado en las siguientes secciones principales, accesibles desde el panel de tabla de contenido de JupyterLab:

| Sección | Contenido |
|---|---|
| Preparación del entorno | Importación de librerías y configuración de rutas |
| Proceso de ETL | Carga, inspección y limpieza del dataset crudo |
| Análisis Exploratorio de Datos | Visualizaciones univariadas, bivariadas y de correlación |
| Reducción de Dimensionalidad | PCA, scree plot y proyección 2D |
| Modelado Predictivo | Entrenamiento, evaluación comparativa y selección del modelo final |
| Modelo aplicado | Predicción sobre perfiles hipotéticos de clientes |
| Conclusiones | Hallazgos, comparación de modelos y recomendaciones de negocio |

---

_Richard Andrés M._
