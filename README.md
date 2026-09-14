# 📱 ConnectaTel: Análisis, Limpieza y Segmentación de Clientes

Este repositorio contiene el desarrollo y los hallazgos del proyecto de análisis de datos para **ConnectaTel**, una empresa de telecomunicaciones en Latinoamérica. El objetivo principal es evaluar el comportamiento de los usuarios registrados hasta el año 2024, construir perfiles estadísticos detallados, limpiar anomalías estructurales de los datos y diseñar una estrategia de negocio basada en segmentos de consumo y demográficos.

---

## 🎯 Objetivo del Proyecto

El propósito de este análisis es transformar los datos crudos de consumo e información de usuarios en insights estratégicos accionables. Específicamente, el proyecto resuelve:
1. **Calidad de Datos (Data Cleaning):** Identificar y mitigar inconsistencias operativas en las bases de datos (IDs corruptos, fechas anómalas, valores ausentes).
2. **Perfilamiento Estadístico:** Comprender la distribución real del uso de minutos y mensajería en la población.
3. **Análisis de Valores Atípicos:** Evaluar el comportamiento de usuarios de consumo extremo (*heavy users*) y su impacto en la infraestructura y facturación.
4. **Segmentación de Clientes:** Crear clústeres demográficos (por edad) y operativos (por nivel de uso) para proponer mejoras comerciales en la oferta de planes actuales.

---

## 💾 Datasets Utilizados

El análisis se fundamenta en tres conjuntos de datos interconectados:

* **`plans.csv`**: Información técnica y comercial de los planes actuales (tarifas base, minutos incluidos, GB incluidos y costos por consumo excedente).
* **`users.csv`**: Datos demográficos y contractuales de los clientes (edad, ciudad de residencia, fecha de registro, tipo de plan y estatus de cancelación `churn`).
* **`usage.csv`**: Historial detallado del uso real de los servicios, consolidando las interacciones individuales de llamadas (duración en minutos) y mensajes (longitud de texto).

---

## 🔄 Etapas del Análisis Realizadas

El proyecto se desarrolló siguiendo un flujo de trabajo analítico riguroso estructurado en el notebook:

1. **Exploración Inicial (EDA):** Carga de datos, inspección de tipos de variables (`dtypes`) y evaluación de estadísticas descriptivas con `.describe()`.
2. **Limpieza e Ingeniería de Datos:**
    * **Corrección de Primary Keys:** Identificación y solución de un desfase de escala en `usage['user_id']` (multiplicado por 10 accidentalmente) mediante división entera (`// 10`), permitiendo un `merge` exitoso sin pérdida de datos.
    * **Tratamiento de Fechas Anómalas:** Identificación de registros futuros inconsistentes en `reg_date` (superiores a 2024), anulados mediante `pd.NA` para proteger el análisis de la cohorte 2018.
    * **Resolución de Ausentes Cruzados:** Validación matemática mediante funciones `lambda` demostrando que los nulos en `duration` y `length` son mutuamente excluyentes debido a la naturaleza técnica de cada servicio (*call* o *text*).
3. **Análisis de Outliers (Valores Atípicos):** Visualización mediante diagramas de caja (*boxplots*) y toma de decisión fundamentada para conservar a los usuarios intensivos de voz (entre 400 y 490 minutos) por su alto valor estratégico.
4. **Segmentación y Visualización:** Creación de grupos de consumo y rangos de edad (`Joven`, `Adulto`, `Adulto Mayor`) utilizando `.apply()`, y generación de reportes visuales de frecuencia ordenados mediante `sns.countplot()`.
5. **Generación de Conclusiones Ejecutivas:** Documentación analítica de los sesgos detectados en las solicitudes de umbrales del negocio frente a la distribución real de los cuartiles de la población.

---

## 🚀 Cómo Ejecutar el Notebook

El proyecto está diseñado para ejecutarse de forma interactiva en la nube o de manera local:

### Opción Recomendada: Google Colab
1. Descarga el archivo `.ipynb` de este repositorio.
2. Ve a [Google Colab](https://colab.research.google.com/).
3. Selecciona la pestaña **Subir** y selecciona el archivo `S7 Version-Estudiante-Project-ConnectaTel.ipynb`.
4. Sube los archivos CSV (`plans.csv`, `users.csv`, `usage.csv`) a la sección de archivos del panel izquierdo en Colab.
5. Ejecuta las celdas secuencialmente presionando `Shift + Enter`.

### Opción Local (Jupyter Notebook / VS Code)
1. Clona este repositorio o descarga los archivos en una carpeta local.
2. Asegúrate de tener instalada la distribución de Anaconda o las librerías necesarias (`pandas`, `numpy`, `seaborn`, `matplotlib`).
3. Abre tu terminal y ejecuta: `jupyter notebook`.
4. Abre el archivo `.ipynb` y ejecuta el entorno.

---

## 🛠️ Guía de Reproducción

Para garantizar que los resultados y gráficas se generen exactamente igual a los del reporte ejecutivo, sigue estas pautas:

1. **Mantén la Jerarquía de Archivos:** Coloca los tres archivos CSV en la misma ruta o directorio raíz donde se encuentra el notebook para evitar errores de lectura (`FileNotFoundError`).
2. **Orden de Ejecución Crítico:** Ejecuta el bloque de código de **limpieza de IDs** (`usage['user_id'] // 10`) **antes** de intentar cualquier operación de `merge` o `join`. Si se invierte el orden, el cruce generará un DataFrame vacío.
3. **Versiones de Librerías:** El código utiliza la sintaxis moderna de Pandas para conversión de tipos de datos. Se recomienda utilizar versiones de `pandas >= 1.5.0` y `seaborn >= 0.12.0` para asegurar que el ordenamiento de los gráficos por frecuencia con `.index.tolist()` renderice correctamente y sin nulos implícitos.
4. **Verificación Matemática:** Puedes correr la celda de validación de hipótesis (`print` de frecuencias acumuladas de nulos y usuarios) para auditar que la población final activa se mantenga exactamente en **400 usuarios consolidados** (200 Adultos, 110 Adultos Mayores y 90 Jóvenes).

---
*Análisis desarrollado como parte de la especialización en Data Analytics. Diseñado para la optimización comercial y retención estratégica de clientes.*

