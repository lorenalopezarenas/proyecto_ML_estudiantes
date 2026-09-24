# PROYECTO MACHINE LEARNING ESTUDIANTES


## Objetivo del proyecto 🎯

Este proyecto tiene como objetivo analizar los factores académicos y personales que influyen en el rendimiento de los estudiantes, desarrollando modelos capaces tanto de proyectar sus calificaciones exactas como de predecir de forma temprana escenarios de suspenso.

A través de un análisis exploratorio de datos (EDA) y la implementación de algoritmos de Machine Learning, se busca:

- Identificar las variables críticas que impactan la nota final de los estudiantes (asistencia, horas de estudio, dificultad, etc.).

- Desarrollar un modelo de regresión robusto para estimar la nota final en una escala continua de 0 a 100.

- Desarrollar un modelo de clasificación binaria para predecir si un estudiante aprobará (nota \(\geq 60\)) o suspenderá el curso.

- Diseñar una estrategia de preprocesamiento e ingeniería de datos metodológicamente correcta para lidiar con el desbalanceo de clases y los valores atípicos.

---------------------------------------------------------------------------------------------------------

## Fuentes de datos 📂

Para la realización de este proyecto se ha utilizado un conjunto de datos que consolida variables cuantitativas y cualitativas sobre los hábitos y el rendimiento de los estudiantes:

### Dataset: Registro Académico Estudiantil (`estudiantes.csv`)

Contiene registros que abarcan las siguientes dimensiones:

- **Variables Académicas:** `horas_estudio_semanal`, `tasa_asistencia`, `nota_anterior`, `nivel_dificultad` e información sobre si el alumno cuenta con tutoría (`tiene_tutor`).

- **Variables Personales/Hábitos:** `edad`, `horas_sueno`, `horario_estudio_preferido` y `estilo_aprendizaje`.

- **Variables Objetivo:** 
    - `nota_final` para regresión 
    
    - `aprobado` variable binaria para clasificación.

---------------------------------------------------------------------------------------------------------

## Metodología 🧪

El proyecto se ha desarrollado en varias fases:

### 1. Análisis Exploratorio

- **Detección de Anomalías:** Identificación de valores atípicos (*outliers*) mediante los métodos estadísticos IQR y Z-Score. 

### 2. Preprocesamiento

- **Tratamiento de Outliers:** Aplicación de *Winsorization* (topamiento al 1% en los extremos) exclusivo para el modelo de regresión, protegiendo las pendientes lineales sin reducir la muestra.

- **Codificación de Categorías:** Transformación de variables cualitativas a formatos numéricos mediante *Ordinal Encoding* (mapeo manual jerárquico para `nivel_dificultad`) y *One-Hot Encoding* para características nominales.

- ***Data Leakage*:** Eliminación estricta de `nota_final` y variables vinculadas en el flujo de clasificación. Separación de datos en Entrenamiento (80%) y Prueba (20%) de forma previa a cualquier escalado.

### 3. Modelo de Regresión 

- Entrenamiento de una Regresión Lineal y comparación experimental contra un modelo no lineal (*Random Forest Regressor*).

### 4. Modelo de Clasificación

- **Feature Scaling:** Implementación de `RobustScaler` (basado en mediana e IQR) sobre las variables continuas para conservar el peso predictivo de los alumnos con comportamientos extremos reales.

- **Algoritmo y Validación:** Entrenamiento de una Regresión Logística. Evaluación final mediante matrices de confusión y métricas ponderadas.

---------------------------------------------------------------------------------------------------------

## Estructura del Proyecto 🗂️

├── data/

│   ├── raw/          # Dataset original sin procesar

│   └── output/       # Datasets exportados (df_regresion.csv y df_clasificacion.csv)

├── notebooks/

│   ├── 01. Análisis_exploratorio.ipynb

│   ├── 02. Preprocesamiento.ipynb

│   ├── 03. Modelo_regresion.ipynb

│   └── 04. Modelo_clasificacion.ipynb

├── models/           # Modelos entrenados y serializados (.pkl)

├── README.md         # Descripción general del proyecto

└── requirements.txt  # Librerías del proyecto


---------------------------------------------------------------------------------------------------------

## Instalación y requisitos 🛠️

Este proyecto usa Python 3.14.0 y requiere las siguientes bibliotecas:

- `pandas`
- `numpy`
- `scipy`
- `scikit-learn`
- `matplotlib`
- `seaborn`

---------------------------------------------------------------------------------------------------------

## Resultados y conclusiones 📊

El proyecto demostró de forma contundente que el rendimiento académico está fuertemente condicionado por factores estructurales y de dedicación, arrojando conclusiones clave en cada  modelo:

### Modelo de Regresión Lineal

- **Métricas:** \(R^2 = 0.36\) en Test (explicación del 36% de la varianza), \(MAE = 5.83\) puntos de error promedio y un \(RMSE = 7.26\). 

- **Conclusión:** La Regresión Lineal demostró ser un modelo altamente estable y libre de sobreajuste. Al comparar experimentalmente con *Random Forest Regressor*, este último sufrió de un severo *overfitting* (\(R^2\) cayó de 0.75 en Train a 0.32 en Test). El análisis de coeficientes determinó que el nivel de dificultad representa el mayor impacto negativo en la nota final, mientras que las variables cuantitativas (asistencia e historial) perdieron peso lineal debido a la redundancia de datos (multicolinealidad) absorbida por las variables categóricas.

### Modelo de Clasificación (Regresión Logística)

- **Métricas Ponderadas:** \(Accuracy = 0.92\), \(Precision = 0.90\), \(Recall = 0.92\) y \(F1\text{-score} = 0.90\) en el conjunto de prueba.

- **Conclusión:** El uso de métricas ponderadas transparentó con rigor analítico el impacto del desbalanceo de clases expuesto en la matriz de confusión (donde el modelo mantiene una postura conservadora que favorece el acierto de los alumnos aprobados pero requiere un monitoreo estricto de las alertas de suspenso). A diferencia de la regresión, el gráfico de importancia aquí rescató con éxito a las horas de estudio semanal como el pilar fundamental para predecir el éxito del alumno.

---------------------------------------------------------------------------------------------------------

### Autor ✒️

Lorena López Arenas
