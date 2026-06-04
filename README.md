# 🏘️ Proyecto: Real Estate Time-Memory AI
**Predicción de tendencias inmobiliarias con Redes Neuronales Recurrentes (RNN)**

*Autores: Carlos Álvarez Gómez, Fernando Lledó Romero, Sara Talbi Martín*
*Fecha: 21/04/2026*

## 📖 Descripción del escenario
El proyecto "Real Estate Time-Memory AI" utiliza técnicas de Deep Learning para predecir el comportamiento del mercado inmobiliario. El objetivo principal es superar la capacidad de los modelos estáticos mediante el análisis de la inercia temporal y la detección de tendencias estructurales en el sector inmobiliario.

## 📊 Origen de los datos
Los datos utilizados corresponden a la evolución de los precios de las viviendas compradas (Precios de transacción de las viviendas).
* **Fuente:** Instituto Nacional de Estadística (INE) a través del portal de datos abiertos del Gobierno de España.
* **Limpieza y estructura:** Se realizó una limpieza de nulos y ajuste de formatos numéricos para asegurar una serie temporal continua de 76 trimestres.

## ⚙️ Variables del sistema
* **Entrada (X):** Una ventana de tiempo histórica (secuencia) de los últimos 8 trimestres. La transformación de la serie resultó en un tensor 3D con la configuración (68, 8, 1).
* **Salida (Y):** Está constituida por 68 valores de predicción correspondientes al valor proyectado del Índice de Precios de Vivienda para el próximo trimestre.

## 🚀 Arquitectura y metodología (Fases del proyecto)

### Fase I: Modelado de Secuencias (Data preparation)
* **Limpieza y escalamiento:** Se aplicó MinMaxScaler para situar los datos en un rango de [0, 1], requisito indispensable para la estabilidad de la función de activación de la capa LSTM.
* **Generación de muestras:** Se generaron 68 secuencias operativas a partir de los 76 datos originales.

### Fase II: Diseño de la arquitectura RNN
* **Capa recurrente:** Se implementó una capa LSTM con 100 unidades de memoria. Esta configuración permite al modelo retener patrones complejos de ciclos anteriores sin sufrir el problema del gradiente desvanecido.
* **Capa de salida:** Una capa Dense (1 unidad) que consolida la información de la memoria en la predicción final del precio.
* **Compilación:** Se utilizó el optimizador RMSprop por su eficiencia en el ajuste de pesos en series temporales y la función de pérdida MSE.

### Fase III: Entrenamiento temporal
* **División cronológica:** Se respetó la cronología de los datos para evitar el sesgo de anticipación. El dataset se dividió en 54 muestras de entrenamiento (pasado) y 14 muestras de validación (futuro).
* **Ajuste:** El entrenamiento se ejecutó durante 100 épocas con un batch size de 8.

### Fase IV: El Detector de inflexión (Análisis estratégico)
* **Evaluación de pereza:** Al observar la comparativa en el conjunto de test (14 trimestres de validación), se evidencia que el modelo no es perezoso (Naïve). Nuestra red LSTM con 100 unidades logra proyectar la pendiente de crecimiento.
* **Detección de ciclos:** El modelo demuestra ser un excelente confirmador de tendencias estructurales, ayudando a ignorar el "ruido" de caídas temporales para centrarse en el ciclo largo.

## 📈 Resultados y conclusión estratégica
* **Comportamiento predictivo:** Aunque existe una brecha (bias) donde el modelo es ligeramente más conservador que la realidad, la red es capaz de captar la "fuerza" del mercado alcista actual, ajustando su predicción de forma dinámica.
* **Reacción ante giros:** Durante un escenario crítico de inversión (contracción entre los trimestres 4 y 5), la red no logró predecir la caída exacta debido a su "memoria histórica" fuertemente alcista de los 8 trimestres anteriores. Sin embargo, suavizó su crecimiento tras el giro, amortiguando la predicción.
* **Valor para el inversor:** El modelo es un estabilizador de decisiones. Al identificar cuándo una subida pierde fuerza, permite prever el agotamiento de un boom inmobiliario antes de que ocurra una caída sistémica.

## 💻 Tecnologías utilizadas
* Python
* TensorFlow / Keras (Sequential, LSTM, Dense)
* Pandas & NumPy
* Scikit-Learn (MinMaxScaler)
* Matplotlib