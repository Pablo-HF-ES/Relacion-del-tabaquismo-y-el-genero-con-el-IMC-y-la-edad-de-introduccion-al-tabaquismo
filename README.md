# Relacion-del-tabaquismo-y-el-genero-con-el-IMC-y-la-edad-de-introduccion-al-tabaquismo
Análisis estadístico e inferencia en R (ENSE 2023). Aplica Data Wrangling, feature engineering, ANOVA robusto (Welch) y modelado paramétrico Lognormal sobre tabaquismo e IMC. Destaca el rigor matemático frente a heterocedasticidad y asimetrías. Sólidos fundamentos de datos para Machine Learning y Data Science.

# 📊 Análisis de Inferencia Estadística sobre Tabaquismo y Factores Biométricos (ENSE 2023)

[cite_start]Este repositorio contiene un ecosistema de análisis estadístico avanzado e inferencia poblacional implementado en **R**, utilizando los microdatos reales de la **Encuesta Nacional de Salud de Adultos (ENSE 2023)**. [cite_start]El proyecto evalúa de manera rigurosa el impacto multidimensional del tabaquismo, analizando su relación con el Índice de Masa Corporal (IMC) y modelando los patrones demográficos de la edad de inicio al consumo según el género.

---

## 🛠️ Stack Tecnológico & Librerías
* [cite_start]**Lenguaje:** R v4.x [cite: 20]
* **Depuración y Manipulación de Datos:** `tidyverse`, `dplyr`
* [cite_start]**Modelado Paramétrico & Ajuste de Curvas:** `fitdistrplus` [cite: 30]
* [cite_start]**Visualización Avanzada:** `ggplot2` (Histogramas de densidad, Gráficos Q-Q, Boxplots estructurados) [cite: 23, 26, 30]

---

## 📐 Objetivos Estratégicos del Proyecto
1. [cite_start]**Modelado y Análisis de Varianza de Muestras Complejas:** Evaluar el impacto real del hábito tabáquico en el IMC continuo, segmentando el análisis por género para capturar comportamientos biológicos específicos[cite: 11].
2. [cite_start]**Modelado Paramétrico de Tiempos de Espera:** Identificar, ajustar y validar la distribución de probabilidad teórica que rige la edad de iniciación al tabaquismo, contrastando diferencias significativas entre hombres y mujeres[cite: 12, 30].

---

## 🧹 Pipeline de Ingeniería de Datos (Data Wrangling)
[cite_start]Los microdatos originales de la ENSE 2023 se caracterizaban por codificaciones complejas no operativas y variables nominales que limitaban un modelado numérico robusto[cite: 13, 15]. Se diseñó un pipeline de limpieza que incluyó:
* [cite_start]**Ingeniería de Características (Feature Engineering):** Sustitución de la variable cualitativa de IMC original por el cálculo exacto de la métrica continua de IMC $\left(\frac{\text{Peso}}{\text{Altura}^2}\right)$ a partir de los datos cuantitativos crudos de peso (kg) y altura (m)[cite: 15, 16, 17].
* [cite_start]**Tratamiento y Filtrado de Datos Faltantes (Missing Data Handling):** Exclusión controlada de registros sin datos biométricos válidos y filtrado estratégico de la submuestra de "fumadores diarios" para garantizar la consistencia en el análisis de la edad de inicio[cite: 17, 19, 20].
* [cite_start]**Mapeo de Variables:** Recodificación jerárquica de hábitos de tabaquismo en cuatro niveles diferenciados: *Fumador diario, Fumador ocasional, Exfumador y Nunca ha fumado*[cite: 9].

---

## 🧠 Enfoque y Rigor Estadístico (Data Science Highlights)

[cite_start]Lo que diferencia a este proyecto es que **no se aplicaron recetas automáticas**, sino que cada decisión analítica fue respaldada matemáticamente según las propiedades de la muestra[cite: 34, 51]:

### 1. Robustez ante Muestras Masivas (Efecto de Gran Tamaño Muestral)
* [cite_start]Con tamaños muestrales elevados (ej. $n > 6000$ en mujeres que nunca han fumado) [cite: 63][cite_start], los test tradicionales de bondad de ajuste y normalidad tienden a rechazar la hipótesis nula de forma artificial por un exceso de potencia estadística, detectando desviaciones irrelevantes[cite: 38, 42, 46]. 
* [cite_start]**Solución ML/DS:** Se optó por un enfoque basado en estadística descriptiva avanzada, análisis visual de **gráficos Cuantil-Cuantil (Q-Q)** e histogramas con curvas de densidad superpuestas para validar el comportamiento cuasi-gaussiano[cite: 23, 39, 62].

### 2. Gestión de Heterocedasticidad (Homogeneidad de Varianzas)
* [cite_start]Mediante el **Test de Levene** (elegido por su robustez frente al test de Bartlett ante distribuciones ligeramente asimétricas) [cite: 40, 41][cite_start], se descubrió homocedasticidad en el grupo de hombres ($p = 0.538$) pero una severa heterocedasticidad en el de mujeres ($p < 0.001$)[cite: 68].
* [cite_start]**Solución ML/DS:** Se aplicó un **ANOVA tradicional** para hombres [cite: 44] [cite_start]y un **ANOVA de Welch** para mujeres (el cual no asume varianzas iguales y recalcula los grados de libertad)[cite: 45, 72]. 
* [cite_start]**Análisis Post-Hoc:** Para aislar los grupos con medias significativamente distintas, se aplicó la prueba de **TukeyHSD** en hombres [cite: 47] [cite_start]y **contrastes t de Welch con corrección de Bonferroni** en mujeres para mitigar la tasa de error por comparaciones múltiples[cite: 47, 73].

### 3. Modelado Paramétrico y Transformaciones de Datos
* [cite_start]La edad de inicio al tabaquismo arrojó una asimetría positiva intrínseca (tiempos de espera)[cite: 52]. [cite_start]Evaluamos las distribuciones **Weibull, Gamma y Lognormal** con `fitdistrplus`, determinando matemáticamente que la **distribución Lognormal** proporcionaba el ajuste óptimo[cite: 30, 83].
* [cite_start]Para poder aplicar inferencia paramétrica, se realizó una transformación logarítmica natural $Y = \ln(X)$[cite: 31, 54]. [cite_start]Al confirmarse que las varianzas continuaban siendo desiguales [cite: 90][cite_start], se resolvió mediante un **Test t de Welch** sobre las medias logarítmicas[cite: 32, 91].
* [cite_start]**Estimadores Robustos:** Los resultados se devolvieron a su escala original (años) utilizando la **Media Geométrica** en lugar de la media aritmética[cite: 33, 58, 59]. [cite_start]Esto impidió que los valores atípicos de la cola derecha sesgaran la métrica de tendencia central, garantizando un estimador insesgado de la realidad demográfica[cite: 60, 94].

---

## 📈 Conclusiones Clave del Negocio / Investigación
* [cite_start]**Impacto del Tabaquismo en el IMC:** Existen diferencias estadísticamente significativas en el IMC promedio de acuerdo con el hábito de consumo de tabaco tanto en hombres como en mujeres, evidenciando variaciones críticas especialmente en el grupo de exfumadores/as[cite: 70, 72, 73, 104].
* [cite_start]**Comportamiento Demográfico del Consumo:** Las mujeres inician formalmente el hábito a una edad geométrica ligeramente superior a la de los hombres (17.96 años frente a 17.00 años, $p < 0.001$)[cite: 95, 97, 106].
* [cite_start]**Relevancia Estadística vs. Práctica:** A pesar de los p-valores altamente significativos provocados por la escala de la muestra, el impacto real en unidades de IMC o años de diferencia es moderado, una conclusión crítica que demuestra un criterio de interpretación de datos maduro y realista.

---

## 📂 Estructura del Repositorio
* [cite_start]`data/` : Microdatos anonimizados procesados provenientes de la ENSE 2023.
* [cite_start]`src/` : Scripts de R limpios, modulares y comentados que garantizan la reproducibilidad completa del análisis (limpieza, gráficos vectoriales y contrastes)[cite: 20].
* [cite_start]`docs/` : Informe metodológico detallado e inferencias matemáticas del ANOVA de Welch y distribuciones transformadas[cite: 1, 110].
