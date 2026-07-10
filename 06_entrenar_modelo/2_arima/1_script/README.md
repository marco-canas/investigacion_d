Prompt 

Dame el script de Python para tomar el dataset de la ubicación "C:\Users\marco\Documentos\investigacion\arima\06_entrenar_modelo\2_arima\2_datos\1_raw\2_meteo_epi_rezagos_2026_1.xlsx" y tomando la variable de casos_dengue y los doce rezagos de ella y solo estas variables, entrenar un modelo ARIMA, no ARIMAX, para predecir la variable `casos_dengue` en términos de los rezagos de ella misma; pero dividiendo el dataset en 80-20, 90-10, 95-5, y 97-3 y graficando el desempeño mae en entrenamiento y testeo para cada una de estas divisiones. Luego de entrenado el modelo, dame la función de usuario de Python que le entre el año y la semana epidemiológica y entregue la predicción de casos de dengue para este año y semana epidemiológica leyendo de este dataset la instancia correspondiente a este año y semana epidemiológica y las 12 semanas anteriores, para poder hacer la predicción usando los rezagos correspondientes a tal semana  y año dado. Guarda los gráficos y el excel de los resultados de desempeño en "C:\Users\marco\Documentos\investigacion\arima\06_entrenar_modelo\2_arima\3_resultados". Siempre usa el nombre de la variable año, con el uso de la  letra eñe (ñ). No le cambies el nombre a la variable `año`.   



Aquí tienes el script completo de Python que cumple con todos tus requerimientos. El script utiliza la librería `statsmodels` para entrenar un modelo puro **ARIMA(12, 0, 0)** (un modelo autorregresivo con 12 rezagos, sin variables exógenas/ARIMAX), realiza las cuatro divisiones de datos solicitadas, calcula el MAE de entrenamiento y testeo, genera y guarda el gráfico y el archivo Excel con los resultados en la ruta especificada, y finalmente define la función de predicción exacta que solicitaste respetando la variable `año`.

```python
import os
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from statsmodels.tsa.arima.model import ARIMA
from sklearn.metrics import mean_absolute_error

# 1. Configuración de rutas y variables
ruta_input = r"C:\Users\marco\Documentos\investigacion\arima\06_entrenar_modelo\2_arima\2_datos\1_raw\2_meteo_epi_rezagos_2026_1.xlsx"
ruta_output_dir = r"C:\Users\marco\Documentos\investigacion\arima\06_entrenar_modelo\2_arima\3_resultados"

# Crear el directorio de resultados si no existe
os.makedirs(ruta_output_dir, exist_ok=True)

# 2. Carga y preparación de los datos
print("Cargando datos...")
df = pd.read_excel(ruta_input)

# CORRECCIÓN: Se cambió 'semana' por 'semana_epi' para el ordenamiento cronológico
df = df.sort_values(by=['año', 'semana_epi']).reset_index(drop=True)

# Seleccionar la serie temporal (ARIMA puro solo requiere la variable objetivo)
serie_dengue = df['casos_dengue']

# Definición de las divisiones de entrenamiento y testeo
divisiones = {
    "80-20": 0.80,
    "90-10": 0.90,
    "95-5":  0.95,
    "97-3":  0.97
}

resultados_mae = []

print("Entrenando modelos ARIMA para las diferentes divisiones...")

# 3. Bucle para evaluar cada división de datos
for nombre, split_ratio in divisiones.items():
    n_train = int(len(serie_dengue) * split_ratio)
    train = serie_dengue.iloc[:n_train]
    test = serie_dengue.iloc[n_train:]
    
    # Entrenar modelo ARIMA(12, 0, 0)
    modelo = ARIMA(train, order=(12, 0, 0))
    modelo_ajustado = modelo.fit()
    
    # Predicciones en entrenamiento
    pred_train = modelo_ajustado.predict(start=0, end=n_train-1)
    # Reajustar para omitir los primeros 12 datos en la métrica debido al calentamiento de los rezagos
    mae_train = mean_absolute_error(train.iloc[12:], pred_train.iloc[12:])
    
    # Predicciones en testeo
    pred_test = modelo_ajustado.forecast(steps=len(test))
    mae_test = mean_absolute_error(test, pred_test)
    
    resultados_mae.append({
        "División": nombre,
        "MAE_Entrenamiento": mae_train,
        "MAE_Testeo": mae_test
    })

# Convertir resultados a DataFrame
df_resultados = pd.DataFrame(resultados_mae)

# 4. Guardar resultados en Excel
ruta_excel = os.path.join(ruta_output_dir, "resultados_desempeño_mae.xlsx")
df_resultados.to_excel(ruta_excel, index=False)
print(f"Resultados guardados en Excel: {ruta_excel}")

# 5. Graficar el desempeño MAE y guardar la imagen
plt.figure(figsize=(10, 6))
x = np.arange(len(df_resultados["División"]))
width = 0.35

plt.bar(x - width/2, df_resultados["MAE_Entrenamiento"], width, label='MAE Entrenamiento', color='#2b5c8f')
plt.bar(x + width/2, df_resultados["MAE_Testeo"], width, label='MAE Testeo', color='#d95f02')

plt.xlabel('Divisiones de Dataset (Train-Test)')
plt.ylabel('MAE (Mean Absolute Error)')
plt.title('Desempeño del Modelo ARIMA por División de Datos')
plt.xticks(x, df_resultados["División"])
plt.legend()
plt.grid(axis='y', linestyle='--', alpha=0.7)

ruta_grafico = os.path.join(ruta_output_dir, "grafico_desempeño_mae.png")
plt.tight_layout()
plt.savefig(ruta_grafico, dpi=300)
plt.close()
print(f"Gráfico guardado en: {ruta_grafico}")


# 6. FUNCIÓN DE USUARIO PARA PREDICCIÓN CON REZAGOS HISTÓRICOS
def predecir_casos_dengue(año, semana_epidemiologica, dataset_path=ruta_input):
    """
    Función para predecir los casos de dengue para un año y semana epidemiológica dados.
    Mantiene la variable 'año' con eñe y busca la columna 'semana_epi' internamente.
    """
    # Cargar el dataset interno de la función
    df_pred = pd.read_excel(dataset_path)
    
    # CORRECCIÓN: Ajustado ordenamiento interno también con 'semana_epi'
    df_pred = df_pred.sort_values(by=['año', 'semana_epi']).reset_index(drop=True)
    
    # Encontrar el índice de la instancia solicitada usando 'semana_epi'
    indice_objetivo = df_pred[(df_pred['año'] == año) & (df_pred['semana_epi'] == semana_epidemiologica)].index
    
    if len(indice_objetivo) == 0:
        return f"Error: No se encontró el año {año} y semana {semana_epidemiologica} en el dataset."
    
    idx = indice_objetivo[0]
    
    # Se requieren las 12 semanas anteriores mínimas
    if idx < 12:
        return "Error: No hay suficientes semanas anteriores (mínimo 12 rezagos) en el dataset para realizar la predicción."
    
    # Extraemos la historia de casos de dengue hasta la semana objetivo (excluyendo la semana objetivo)
    historia_completa = df_pred['casos_dengue'].iloc[:idx].values
    
    # Ajustamos el modelo ARIMA(12, 0, 0)
    modelo_dinamico = ARIMA(historia_completa, order=(12, 0, 0))
    modelo_dinamico_fit = modelo_dinamico.fit()
    
    # Predecir el siguiente paso en el tiempo (la semana objetivo)
    prediccion = modelo_dinamico_fit.forecast(steps=1)[0]
    
    return max(0, round(prediccion, 2))

### Características del Script:

1. **Modelo ARIMA Puro:** Implementa `ARIMA(12, 0, 0)` mediante la librería `statsmodels`. Al ser un modelo autorregresivo puro de orden 12, se alimenta matemáticamente de sus propios 12 rezagos anteriores de forma nativa, asegurando que no se incluyan variables climáticas o exógenas (cumpliendo con la restricción de que **no sea ARIMAX**).
2. **Respeto a las variables:** La variable `año` se mantiene estrictamente escrita con la letra eñe (`ñ`) en todo el flujo del código.
3. **Métricas y Almacenamiento:** Ejecuta de forma secuencial las particiones (80-20, 90-10, 95-5, 97-3), calcula los errores absolutos medios (MAE) tanto en entrenamiento como en evaluación de prueba, y exporta directamente los resultados tabulares y gráficos en formato `.xlsx` y `.png` dentro de la carpeta `3_resultados`.
4. **Función Dinámica de Predicción:** La función `predecir_casos_dengue` toma el `año` y la `semana`, localiza el punto cronológico exacto en el histórico, comprueba que existan al menos 12 semanas de retraso previas disponibles para calcular la estructura autorregresiva y realiza la predicción puntual.