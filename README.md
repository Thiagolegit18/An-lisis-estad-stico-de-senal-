# Analisis-estadistico-de-senal-

# Obtención de la Señal Fisiológica

Se accedió a bases de datos de señales fisiológicas PhysioNet, se seleccionó y descargó una señal fisiológica adecuada para el análisis. Descargamos únicamente archivos `.hea` y `.dat`.
Esta base de datos incluye 35 registros de ECG de ocho minutos de sujetos humanos que experimentaron episodios de taquicardia ventricular sostenida, aleteo ventricular y fibrilación ventricular.

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

<p align="center">
   <img src="https://github.com/user-attachments/assets/b1d37700-06e0-429d-bbd1-487e614f8618">
</p>

## Histograma desde Cero

Se especifica el número de intervalos (`num_bins = 50`). Luego, se encuentra el valor mínimo y máximo de los datos. Crea 50 intervalos (bins) entre `min_val` y `max_val`. Calculamos el histograma y los bordes de los bins usando estos intervalos. Finalmente, se usan barras (`plt.bar()`) para graficar el histograma.

<p align="center">
   <img src="https://github.com/user-attachments/assets/76318e4e-2b88-441f-b40f-585b6979232c")
">
</p>

## Señal Contaminada por Ruido Gaussiano para Obtener un SNR Positivo

- Generamos el ruido gaussiano con media 0 y desviación estándar 1.
- La longitud del ruido es igual a la de `valores_limpios` y luego dividiremos el ruido por 25 para reducir su amplitud, pues con una amplitud menor logramos un SNR positivo.
- Sumamos el ruido generado a la señal original `valores_limpios` para obtener la señal contaminada y luego la graficamos con sus respectivas etiquetas.
  <p align="center">
   <img src="https://github.com/user-attachments/assets/777f6610-81ca-4506-8efc-9f18e596b30a">
  </p>
- Calculamos la potencia de la señal (media de los cuadrados de los valores) y la potencia del ruido. Luego, calculamos la SNR en decibelios (dB) usando la fórmula `10 * log10(potencia señal / potencia ruido)`.
- Luego graficamos únicamente el ruido gaussiano.

  <p align="center">
   <img src="https://github.com/user-attachments/assets/7f078d18-49a2-49ce-97bc-e22cdca8aaeb">
  </p>

## Señal Contaminada por Ruido Gaussiano para Obtener un SNR Negativo

- Generamos el ruido gaussiano con media 0 y desviación estándar 1. Multiplicamos este ruido por 12 para aumentar su amplitud y generar un SNR negativo.
- Sumamos el ruido generado a la señal original `valores_limpios` para obtener la señal contaminada y la graficamos como la segunda señal contaminada.
  <p align="center">
   <img src="https://github.com/user-attachments/assets/d72eb21f-d188-4424-9875-84768cfb4f66">
  </p>
- Calculamos la potencia de la señal y la potencia del ruido como antes, y luego calculamos la SNR en decibelios (dB).
- Por último graficamos el segundo ruido gaussiano generado.
  <p align="center">
   <img src="https://github.com/user-attachments/assets/a97ee9b1-ad76-417f-9602-9bdccd4fb911">
  </p>

## Señal Contaminada por Ruido Impulsivo para Obtener un SNR Positivo

- Definimos la probabilidad de que ocurra un impulso en cualquier punto de la señal.
- Definimos la amplitud del impulso 3 veces la desviación estándar predefinida de la señal.
- Calculamos el número total de impulsos a introducir en la señal multiplicando la probabilidad de impulso por la longitud de la señal y convirtiendo el resultado a un número entero.
- Seleccionamos aleatoriamente los índices o posiciones de la señal donde se introducirán los impulsos. `replace=False` asegura que no se seleccionen las mismas posiciones más de una vez.
- Inicializamos un array de ruido impulsivo con ceros del mismo tamaño que la señal original para luego asignar impulsos de amplitud positiva o negativa aleatorios.
- Sumamos el ruido impulsivo a la señal original para obtener la señal contaminada.
  <p align="center">
   <img src="https://github.com/user-attachments/assets/ab626612-fc58-4e9d-8443-914fba14433a">
  </p>
- Graficamos la señal contaminada con ruido impulsivo y graficamos el ruido impulsivo.
- Calculamos la potencia del ruido impulsivo y luego la SNR en decibelios. La potencia se calcula como la media de los cuadrados de los valores del ruido. La SNR se calcula usando la fórmula antes mencionada.
  <p align="center">
   <img src="https://github.com/user-attachments/assets/0eeb66f9-80a7-4713-9c41-ddf7fbc8c8fb">
  </p>
## Señal Contaminada por Ruido Impulsivo para Obtener un SNR Negativo

- Definimos la amplitud del impulso 40 veces la desviación estándar predefinida de la señal.
  <p align="center">
   <img src="https://github.com/user-attachments/assets/4c3f4323-5a23-4045-9b5e-91cbd9554f1c">
  </p>
- Realizamos exactamente el mismo procedimiento que usamos para la señal con SNR positivo solo que con una amplitud diferente.
  <p align="center">
   <img src="https://github.com/user-attachments/assets/deff4eed-4bc6-4717-9333-cf3d6189f213">
  </p>
## Señal Contaminada con Ruido Tipo Artefacto para Obtener un SNR Negativo

- Definimos la probabilidad de que ocurra un artefacto en la señal.
- Definimos la amplitud del artefacto como 2 veces la desviación estándar de los valores limpios.
- Calculamos el número de artefactos a insertar como la probabilidad de artefacto multiplicada por la longitud de la señal.
- Seleccionamos aleatoriamente las posiciones donde se introducirán los artefactos.
- Inicializamos un array de ceros del mismo tamaño que la señal limpia.
- Introducimos los artefactos en las posiciones seleccionados, con valores positivos o negativos.
- Sumamos el ruido tipo artefacto a la señal original para obtener la señal contaminada.
  <p align="center">
   <img src="https://github.com/user-attachments/assets/2dfecbdb-761e-4645-8cde-50fb9ddf0ee9">
  </p>
- Graficamos la señal contaminada con ruido artefacto y por aparte graficamos el ruido artefacto.
- Calculamos el SNR con la fórmula antes mencionada.
  <p align="center">
   <img src="https://github.com/user-attachments/assets/fdb46c78-ff2a-4b5f-a1ea-603147e5b995">
  </p>

## Señal Contaminada con Ruido Tipo Artefacto para Obtener un SNR Negativo

- Realizamos exactamente el mismo procedimiento anterior únicamente que para la amplitud artefacto multiplicamos 50 veces la desviación estándar predefinida de la señal para obtener un SNR negativo.
  <p align="center">
   <img src="https://github.com/user-attachments/assets/a8694ca1-e674-4f63-8928-eb55711f8e53">
  </p>
- Gráfica ruido tipo artefacto.
   <p align="center">
   <img src="https://github.com/user-attachments/assets/6a19f7f2-e345-439f-95ec-5234797e1c16">
  </p>
