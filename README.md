# Relacion-del-tabaquismo-y-el-genero-con-el-IMC-y-la-edad-de-introduccion-al-tabaquismo
Análisis estadístico e inferencia en R (ENSE 2023). Aplica Data Wrangling, feature engineering, ANOVA robusto (Welch) y modelado paramétrico Lognormal sobre tabaquismo e IMC. Destaca el rigor matemático frente a heterocedasticidad y asimetrías. Sólidos fundamentos de datos para Machine Learning y Data Science.

# Análisis de Inferencia Estadística sobre Tabaquismo y Factores Biométricos (ENSE 2023)

Este repositorio contiene un ecosistema de análisis estadístico avanzado e inferencia poblacional implementado en **R**, utilizando datos reales de la **Encuesta Nacional de Salud de Adultos (ENSE 2023)**. El proyecto evalúa de manera rigurosa el impacto multidimensional del tabaquismo, analizando su relación con el Índice de Masa Corporal (IMC) y modelando los patrones demográficos de la edad de inicio al consumo según el género.

---

## Stack Tecnológico & Librerías
* **Lenguaje:** R
* **Depuración y Manipulación de Datos:** `tidyverse`, `dplyr`
* **Modelado Paramétrico & Ajuste de Curvas:** `fitdistrplus` 
* **Visualización Avanzada:** `ggplot2` (Histogramas de densidad, Gráficos Q-Q, Boxplots estructurados)

---

## Objetivos Estratégicos del Proyecto
1. **Modelado y Análisis de Varianza de Muestras Complejas:** Evaluar el impacto real del hábito tabáquico en el IMC, segmentando el análisis por género para capturar comportamientos biológicos específicos.
2. **Modelado Paramétrico de Tiempos de Espera:** Identificar, ajustar y validar la distribución de probabilidad teórica que rige la edad de iniciación al tabaquismo, contrastando diferencias significativas entre hombres y mujeres.

---

## Pipeline de Ingeniería de Datos (Data Wrangling)
Los datos originales de la ENSE 2023 se caracterizaban por codificaciones complejas no operativas y variables nominales que limitaban un modelado numérico robusto. Se diseñó un pipeline de limpieza que incluyó:
* **Ingeniería de Características (Feature Engineering):** Sustitución de la variable cualitativa de IMC original por el cálculo exacto de la métrica continua de IMC $\left(\frac{\text{Peso}}{\text{Altura}^2}\right)$ a partir de los datos cuantitativos crudos de peso (kg) y altura (m).
* **Tratamiento y Filtrado de Datos Faltantes (Missing Data Handling):** Exclusión controlada de registros sin datos biométricos válidos y filtrado estratégico de la submuestra de "fumadores diarios" para garantizar la consistencia en el análisis de la edad de inicio.
* **Mapeo de Variables:** Recodificación jerárquica de hábitos de tabaquismo en cuatro niveles diferenciados: *Fumador diario, Fumador ocasional, Exfumador y Nunca ha fumado*[cite: 9].

---

## Enfoque y Rigor Estadístico (Data Science Highlights)

Lo que diferencia a este proyecto es que **no se aplicaron recetas automáticas**, sino que cada decisión analítica fue respaldada matemáticamente según las propiedades de la muestra:

### 1. Robustez ante Muestras Masivas (Efecto de Gran Tamaño Muestral)
* Con tamaños muestrales elevados (ej. $n > 6000$ en mujeres que nunca han fumado), los test tradicionales de bondad de ajuste y normalidad tienden a rechazar la hipótesis nula de forma artificial por un exceso de potencia estadística, detectando desviaciones irrelevantes. 
* **Solución ML/DS:** Se optó por un enfoque basado en estadística descriptiva avanzada, análisis visual de **gráficos Cuantil-Cuantil (Q-Q)** e histogramas con curvas de densidad superpuestas para validar el comportamiento cuasi-gaussiano.

### 2. Gestión de Heterocedasticidad (Homogeneidad de Varianzas)
* Mediante el **Test de Levene** (elegido por su robustez frente al test de Bartlett ante distribuciones ligeramente asimétricas), se descubrió homocedasticidad en el grupo de hombres ($p = 0.538$) pero una severa heterocedasticidad en el de mujeres ($p < 0.001$).
* **Solución ML/DS:** Se aplicó un **ANOVA tradicional** para hombres y un **ANOVA de Welch** para mujeres (el cual no asume varianzas iguales y recalcula los grados de libertad).
* **Análisis Post-Hoc:** Para aislar los grupos con medias significativamente distintas, se aplicó la prueba de **TukeyHSD** en hombres y **contrastes t de Welch con corrección de Bonferroni** en mujeres para mitigar la tasa de error por comparaciones múltiples.

### 3. Modelado Paramétrico y Transformaciones de Datos
* La edad de inicio al tabaquismo arrojó una asimetría positiva intrínseca (tiempos de espera). Evaluamos las distribuciones **Weibull, Gamma y Lognormal** con `fitdistrplus`, determinando matemáticamente que la **distribución Lognormal** proporcionaba el ajuste óptimo.
* Para poder aplicar inferencia paramétrica, se realizó una transformación logarítmica natural $Y = \ln(X)$. Al confirmarse que las varianzas continuaban siendo desiguales, se resolvió mediante un **Test t de Welch** sobre las medias logarítmicas.
* **Estimadores Robustos:** Los resultados se devolvieron a su escala original (años) utilizando la **Media Geométrica** en lugar de la media aritmética. Esto impidió que los valores atípicos de la cola derecha sesgaran la métrica de tendencia central, garantizando un estimador insesgado de la realidad demográfica.

---

## Conclusiones Clave de la Investigación
* **Impacto del Tabaquismo en el IMC:** Existen diferencias estadísticamente significativas en el IMC promedio de acuerdo con el hábito de consumo de tabaco tanto en hombres como en mujeres, evidenciando variaciones críticas especialmente en el grupo de exfumadores/as.
* **Comportamiento Demográfico del Consumo:** Las mujeres inician formalmente el hábito a una edad geométrica ligeramente superior a la de los hombres (17.96 años frente a 17.00 años, $p < 0.001$).
* **Relevancia Estadística vs. Práctica:** A pesar de los p-valores altamente significativos provocados por la escala de la muestra, el impacto real en unidades de IMC o años de diferencia es moderado, una conclusión crítica que demuestra un criterio de interpretación de datos maduro y realista.
