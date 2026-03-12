# Detección de Fraude en Transacciones con Tarjetas de Crédito
**Isolation Forest – Detección de Anomalías**

Proyecto de portafolio simple para rol de **Machine Learning Engineer**.  
Enfoque en **detección de fraude bancario** con métodos no supervisados en datasets altamente desbalanceados.

---

# Descripción

Implementación de **Isolation Forest** para identificar transacciones fraudulentas en el dataset **Credit Card Fraud Detection** de Kaggle.

## Objetivos principales

- Demostrar manejo de datasets extremadamente desbalanceados (~0.172% fraudes)
- Aplicar buenas prácticas en detección de anomalías no supervisada
- Evaluar trade-offs entre **Recall, Precision y Falsos Positivos**
- Comparar modelo base vs hiperparámetros optimizados con **GridSearchCV**

## Resultados destacados (en conjunto de test)

- **Modelo base:**  
  Recall ~24%  
  Precision ~20%  
  F1 ~22%

- **Mejor modelo con GridSearch:**  
  Recall ~19.5%  
  Precision ~31%  
  F1 ~24%

- **Muy bajo FPR (~0.07–0.17%)**  
  Esto significa pocas alarmas falsas en transacciones legítimas.

### Resumen de impacto y calidad de detección

En un conjunto de test con ~71.200 transacciones (incluyendo 123 fraudes reales), el modelo logra capturar entre el **19.5% y 24%** de los fraudes reales con una precisión de **20–31%** y una tasa de falsos positivos extremadamente baja (**<0.2%** de las transacciones legítimas).

Esto representa un **detector inicial robusto y práctico**: minimiza el impacto negativo en usuarios legítimos (muy pocas verificaciones innecesarias) mientras identifica un porcentaje significativo de fraudes sin necesidad de datos etiquetados masivos.  
Aunque el recall es moderado (típico en métodos no supervisados puros con PCA), el excelente control de falsos positivos y la mejora en precisión tras optimización demuestran **valor real en escenarios productivos** como filtro de primer nivel, complementario a reglas de negocio o modelos supervisados.
---

# Tecnologías utilizadas

- Python 3.11+
- scikit-learn (Isolation Forest, GridSearchCV, métricas)
- pandas
- numpy
- matplotlib
- seaborn
- RobustScaler (preprocesamiento)

---

# Estructura del proyecto

```
fraud-detection-isolation-forest/
│
├── notebooks/
│   └── 01_fraud_detection_isolation_forest.ipynb
│
├── data/
│   └── creditcard.csv          (NO incluido en el repositorio)
│
├── requirements.txt
├── README.md

```

---

# Cómo ejecutar el proyecto

## 1. Clonar el repositorio

```
git clone https://github.com/tu-usuario/fraud-detection-isolation-forest.git
cd fraud-detection-isolation-forest
```

## 2. Crear y activar un entorno virtual (recomendado)

Linux / Mac:

```
python -m venv venv
source venv/bin/activate
```

Windows:

```
python -m venv venv
venv\Scripts\activate
```

## 3. Instalar dependencias

```
pip install -r requirements.txt
```

## 4. Descargar el dataset

Ir al dataset de Kaggle:

https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud

Descargar el archivo:

```
creditcard.csv
```

Colocarlo dentro de la carpeta:

```
data/
```

## 5. Abrir el notebook

Puedes ejecutarlo con Jupyter:

```
jupyter notebook notebooks/01_fraud_detection_isolation_forest.ipynb
```

O abrirlo directamente en **VS Code** con la extensión **Jupyter**.

---

# Resultados clave y visualizaciones

El notebook incluye varios análisis exploratorios y métricas de evaluación:

- Distribución de montos por clase (escala logarítmica)
- Matriz de correlación (limitada utilidad por transformación PCA)
- Matrices de confusión – modelo base y optimizado
- Curva Precision-Recall (AUC-PR ~0.11 en baseline)
- Comparativa de hiperparámetros (contamination y n_estimators)

---

# Lecciones aprendidas y limitaciones

**Isolation Forest** destaca por su excelente control de **falsos positivos** en escenarios iniciales de producción.

Sin embargo:

- Recall relativamente bajo (~20%) es característico de enfoques **no supervisados**
- Detectar fraudes raros sin etiquetas sigue siendo un problema complejo

En escenarios reales se recomienda combinar con:

- **Reglas de negocio**
  - montos altos
  - horarios inusuales
  - geolocalización sospechosa

- **Modelos supervisados**
  - XGBoost
  - LightGBM
  - redes neuronales

- **Calibración de umbrales**
  - basada en curva **Precision-Recall**

---

# Próximos pasos / mejoras posibles

- Probar valores más altos de `contamination` (0.005 – 0.02)
- Implementar **selección de umbral dinámico** basado en curva PR
- Comparar con otros métodos no supervisados:
  - Local Outlier Factor (LOF)
  - Autoencoders
  - One-Class SVM
- Explorar enfoque **semi-supervisado**
- Desplegar como **API simple**
  - FastAPI
  - modelo serializado con **joblib**

---