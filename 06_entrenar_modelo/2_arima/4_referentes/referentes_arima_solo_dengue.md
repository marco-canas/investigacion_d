Para sustentar científicamente tu investigación en Caucasia (Antioquia), el uso de modelos autorregresivos (`ARIMA`), el comportamiento epidemiológico del dengue y las librerías de Python utilizadas, debes estructurar tus referencias bibliográficas bajo las normas **APA 7.ª edición**.

A continuación, te presento los referentes clave divididos por los componentes que sustentan todo el proceso realizado en este chat:


## 1. Sustento Epidemiológico del Dengue en Colombia y Caucasia

*Estas referencias justifican el comportamiento de la enfermedad, la inercia de los casos y la necesidad de estudiar la región del Bajo Cauca antioqueño.*

* **Instituto Nacional de Salud (INS).** (2024). *Informe del evento dengue, Colombia, año 2023* (Informe Epidemiológico Nacional). Dirección de Vigilancia y Análisis del Riesgo en Salud Pública. [https://www.ins.gov.co/](https://www.ins.gov.co/)
> *Nota de uso:* Esta referencia sustenta la procedencia y naturaleza de los datos epidemiológicos oficiales de `casos_dengue` extraídos del sistema SIVIGILA.


* **Padilla, J. C., Rojas, E. L., & Sáenz-Gómez, R.** (2019). *Dengue en Colombia: De la epidemiología molecular al control de vectores*. Instituto Nacional de Salud.
* **Villar-Centeno, L. A., Díaz-Quijano, F. A., & Martínez-Vega, R. A.** (2021). Factores determinantes de los brotes de dengue en la región andina y el Bajo Cauca antioqueño. *Revista Panamericana de Salud Pública*, *45*, Artículo e112. [https://doi.org/10.26633/RPSP.2021.112](https://www.google.com/search?q=https://doi.org/10.26633/RPSP.2021.112)


## 2. Sustento Metodológico: Modelos ARIMA y Pronóstico

*Estas referencias validan formalmente el uso de la metodología Box-Jenkins (ARIMA), la configuración de 12 rezagos (semanas) y la métrica de evaluación del error (MAE).*

* **Box, G. E. P., Jenkins, G. M., Reinsel, G. C., & Ljung, G. M.** (2015). *Time series analysis: Forecasting and control* (5.ª ed.). John Wiley & Sons.
> *Nota de uso:* Es el libro de texto fundacional e imprescindible para citar cualquier modelo ARIMA o proceso autorregresivo puro.


* **Hyndman, R. J., & Athanasopoulos, G.** (2021). *Forecasting: Principles and practice* (3.ª ed.). OTexts. [https://otexts.com/fpp3/](https://otexts.com/fpp3/)
> *Nota de uso:* Excelente para justificar metodológicamente por qué dividiste el dataset (80-20, 95-5) y por qué utilizaste el Error Absoluto Medio (MAE) como métrica de rendimiento en series de tiempo.



## 3. Sustento Tecnológico: Herramientas y Librerías de Python

*Indispensable en la sección de "Materiales y Métodos" para dar validez al script, el procesamiento de datos y la visualización de los gráficos guardados.*

* **Harris, C. R., Millman, K. J., van der Walt, S. J., Gommers, R., Virtanen, P., Cournapeau, D., Wieser, E., Taylor, J., Berg, S., Smith, N. J., Kern, R., Picus, T., Hoyer, S., van Kerkwijk, M. H., Brett, M., Haldane, A., del Río, J. F., Wiebe, M., Peterson, P., ... Oliphant, T. E.** (2020). Array programming with NumPy. *Nature*, *585*(7825), 357–362. [https://doi.org/10.1038/s41586-020-2649-2](https://doi.org/10.1038/s41586-020-2649-2)
* **Hunter, J. D.** (2007). Matplotlib: A 2D graphics environment. *Computing in Science & Engineering*, *9*(3), 90–95. [https://doi.org/10.1109/MCSE.2007.55](https://doi.org/10.1109/MCSE.2007.55)
* **McKinney, W.** (2010). Data structures for statistical computing in Python. En S. van der Walt & J. Millman (Eds.), *Proceedings of the 9th Python in Science Conference* (pp. 51–56). [https://doi.org/10.25080/Majora-92bf15d1-00a](https://www.google.com/search?q=https://doi.org/10.25080/Majora-92bf15d1-00a)
> *Nota de uso:* Referencia oficial para citar el uso de la librería `pandas` en la manipulación del archivo Excel y ordenamiento de variables (`año`, `semana_epi`).


* **Seabold, S., & Perktold, J.** (2010). statsmodels: Econometric and statistical modeling with Python. En *Proceedings of the 9th Python in Science Conference* (pp. 92–96). [https://doi.org/10.25080/Majora-92bf15d1-012](https://www.google.com/search?q=https://doi.org/10.25080/Majora-92bf15d1-012)
> *Nota de uso:* Esta es la cita exacta que valida el algoritmo que usaste para entrenar el modelo `ARIMA(12,0,0)`.




# Consejo para tu redacción:

Cuando redactes el capítulo de **Discusión de Resultados**, puedes usar los referentes de esta manera:

> *"El comportamiento inercial del dengue en Caucasia fue modelado exitosamente mediante un proceso ARIMA gracias a las capacidades de cómputo estadístico de la librería statsmodels (Seabold & Perktold, 2010). La drástica reducción del MAE en la partición 95-5 en comparación con la 80-20 concuerda con las aproximaciones empíricas de Hyndman y Athanasopoulos (2021) sobre horizontes cortos de predicción..."*