# Analisis-estadistico-de-senal-
# Obtención de la Señal Fisiológica

Se accedió a bases de datos de señales fisiológicas PhysioNet, se seleccionó y descargó una señal fisiológica adecuada para el análisis. Descargamos únicamente archivos `.hea` y `.dat`.

## Importación y Visualización de la Señal

La señal fue importada en Python utilizando bibliotecas estándar.

- La librería `wfdb` (WaveForm DataBase) es utilizada para trabajar con datos fisiológicos o biomédicos que provienen de la base de datos PhysioNet.
- `NumPy` es muy útil para realizar cálculos lógicos y matemáticos sobre cuadros y matrices, realizándolos de manera rápida y eficaz.
- Se graficó la señal usando la biblioteca `matplotlib`, proporcionando una representación visual de los datos.

### Limpieza de Datos

Cuando trabajamos con datos, a menudo se encuentran valores faltantes o no definidos, representados como `NaN` (Not a Number). Es importante limpiar estos valores antes de realizar análisis adicionales para evitar errores y obtener resultados precisos. Esto lo hicimos a través de la función `np.isnan()` que pertenece a la librería `numpy`, identificando los valores `NaN` en un array y devolviendo un array booleano del mismo tamaño que el array de entrada. El resultado es un nuevo array `valores_limpios` que contiene solo los elementos de `valores` que no son `NaN`.

## Cálculo de Estadísticos desde Cero

- **Cálculo de la Media**: Suma de todos los números de la lista `valores_limpios` y luego se divide esa suma entre la cantidad de números en la lista.
- **Cálculo de la Suma de los Cuadrados de las Diferencias con la Media**: Primero, se resta el promedio de cada número en la lista `valores_limpios`, luego se eleva al cuadrado cada una de esas diferencias y finalmente, se suman todos esos valores cuadrados.
- **Cálculo de la Desviación Estándar**: Divide la suma de los cuadrados entre la cantidad de números en la lista. Luego, se toma la raíz cuadrada de ese resultado.
- **Cálculo del Coeficiente de Variación**: Se divide la desviación estándar entre el promedio y se multiplica el resultado por 100. Esto da el coeficiente de variación, que expresa la desviación estándar como un porcentaje del promedio, facilitando la comparación de variabilidad entre diferentes conjuntos de datos.

## Cálculo de Estadísticos con Funciones Predefinidas

- **Cálculo de la Media** usando una Función Predefinida: La función `np.mean()` calcula el promedio de los números en la lista `valores_limpios`.
- **Cálculo de la Desviación Estándar** usando una Función Predefinida: La función `np.std()` calcula la desviación estándar de los números en la lista `valores_limpios`.
- **Cálculo del Coeficiente de Variación**: El coeficiente de variación se calcula dividiendo la desviación estándar entre la media y multiplicando el resultado por 100 para expresarlo como un porcentaje. En este caso, usamos las versiones predefinidas de la media y la desviación estándar.

## Histograma con Función Predefinida

Usamos la función `plt.hist` de la biblioteca `matplotlib` para crear un histograma. Luego se obtuvieron los límites del eje x del histograma (`xmin` y `xmax`), se crearon 100 puntos igualmente espaciados entre `xmin` y `xmax` y se calculó la función de densidad de probabilidad de una distribución normal con la media y desviación estándar calculadas previamente.

## Histograma desde Cero

Se especifica el número de intervalos (`num_bins = 50`). Luego, se encuentra el valor mínimo y máximo de los datos. Crea 50 intervalos (bins) entre `min_val` y `max_val`. Calculamos el histograma y los bordes de los bins usando estos intervalos. Finalmente, se usan barras (`plt.bar()`) para graficar el histograma.

## Señal Contaminada por Ruido Gaussiano para Obtener un SNR Positivo

- Generamos el ruido gaussiano con media 0 y desviación estándar 1.
- La longitud del ruido es igual a la de `valores_limpios` y luego dividiremos el ruido por 25 para reducir su amplitud, pues con una amplitud menor logramos un SNR positivo.
- Sumamos el ruido generado a la señal original `valores_limpios` para obtener la señal contaminada y luego la graficamos con sus respectivas etiquetas.
- Calculamos la potencia de la señal (media de los cuadrados de los valores) y la potencia del ruido. Luego, calculamos la SNR en decibelios (dB) usando la fórmula `10 * log10(potencia señal / potencia ruido)`.
- Luego graficamos únicamente el ruido gaussiano.

## Señal Contaminada por Ruido Gaussiano para Obtener un SNR Negativo

- Generamos el ruido gaussiano con media 0 y desviación estándar 1. Multiplicamos este ruido por 12 para aumentar su amplitud y generar un SNR negativo.
- Sumamos el ruido generado a la señal original `valores_limpios` para obtener la señal contaminada y la graficamos como la segunda señal contaminada.
- Calculamos la potencia de la señal y la potencia del ruido como antes, y luego calculamos la SNR en decibelios (dB).
- Por último graficamos el segundo ruido gaussiano generado.

## Señal Contaminada por Ruido Impulsivo para Obtener un SNR Positivo

- Definimos la probabilidad de que ocurra un impulso en cualquier punto de la señal.
- Definimos la amplitud del impulso 3 veces la desviación estándar predefinida de la señal.
- Calculamos el número total de impulsos a introducir en la señal multiplicando la probabilidad de impulso por la longitud de la señal y convirtiendo el resultado a un número entero.
- Seleccionamos aleatoriamente los índices o posiciones de la señal donde se introducirán los impulsos. `replace=False` asegura que no se seleccionen las mismas posiciones más de una vez.
- Inicializamos un array de ruido impulsivo con ceros del mismo tamaño que la señal original para luego asignar impulsos de amplitud positiva o negativa aleatorios.
- Sumamos el ruido impulsivo a la señal original para obtener la señal contaminada.
- Graficamos la señal contaminada con ruido impulsivo y graficamos el ruido impulsivo.
- Calculamos la potencia del ruido impulsivo y luego la SNR en decibelios. La potencia se calcula como la media de los cuadrados de los valores del ruido. La SNR se calcula usando la fórmula antes mencionada.

## Señal Contaminada por Ruido Impulsivo para Obtener un SNR Negativo

- Definimos la amplitud del impulso 40 veces la desviación estándar predefinida de la señal.
- Realizamos exactamente el mismo procedimiento que usamos para la señal con SNR positivo solo que con una amplitud diferente.

## Señal Contaminada con Ruido Tipo Artefacto para Obtener un SNR Negativo

- Definimos la probabilidad de que ocurra un artefacto en la señal.
- Definimos la amplitud del artefacto como 2 veces la desviación estándar de los valores limpios.
- Calculamos el número de artefactos a insertar como la probabilidad de artefacto multiplicada por la longitud de la señal.
- Seleccionamos aleatoriamente las posiciones donde se introducirán los artefactos.
- Inicializamos un array de ceros del mismo tamaño que la señal limpia.
- Introducimos los artefactos en las posiciones seleccionados, con valores positivos o negativos.
- Sumamos el ruido tipo artefacto a la señal original para obtener la señal contaminada.
- Graficamos la señal contaminada con ruido artefacto y por aparte graficamos el ruido artefacto.
- Calculamos el SNR con la fórmula antes mencionada.

## Señal Contaminada con Ruido Tipo Artefacto para Obtener un SNR Negativo

- Realizamos exactamente el mismo procedimiento anterior únicamente que para la amplitud artefacto multiplicamos 50 veces la desviación estándar predefinida de la señal para obtener un SNR negativo.
