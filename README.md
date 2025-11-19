# Laboratorio de Señales 5: Variabilidad de la Frecuencia Cardíaca (HRV) y balance autonómico 
**Universidad Militar Nueva Granada**  
**Asignatura:** Laboratorio de Procesamiento Digital de Señales  
**Estudiantes:** [Maria Jose Peña Velandia, Joshara Valentina Palacios, Lina Marcela Pabuena]  
**Fecha:** Octubre 2025  
**Título de la práctica:** Análisis espectral de la voz 
## Objetivo
Identificar cambios en el balance autonómico mediante análisis temporal de la
variabilidad de la frecuencia cardíaca (HRV).

## PARTE A 
**a. Fundamento teórico**
1. Actividad simpática y parasimpática del sistema nervioso autónomo
   El sistema nervioso autónomo (SNA) regula funciones involuntarias como la frecuencia cardíaca, presión arterial y respiración. Está compuesto por dos ramas:

**Sistema Nervioso Simpático (SNS)**
Activa el cuerpo para situaciones de estrés o demanda energética (“lucha o huida”).
Efectos principales:
- Aumento de la frecuencia cardíaca (FC)
- Incremento de la contractilidad
- Vasoconstricción
- Liberación de adrenalina/noradrenalina

**Sistema Nervioso Parasimpático (SNP)**
Favorece la relajación (“reposo y digestión”).
Efectos principales:
- Disminución de la FC
- Aumento de la HRV
- Recuperación y ahorro energético
- El equilibrio entre SNS y SNP determina el estado fisiológico y la capacidad adaptativa del organismo.

2. Efecto del sistema autónomo en la frecuencia cardíaca

La frecuencia cardíaca es modulada por la interacción dinámica entre ambas ramas:

SNS ↑ → Aumenta la FC

SNP ↑ → Disminuye la FC

El SNP (nervio vago) regula cambios rápidos latido a latido, mientras que el SNS genera ajustes más lentos.
Esta oscilación entre ambas ramas produce variaciones en los intervalos R-R, base del análisis de HRV.

3. Variabilidad de la Frecuencia Cardíaca (HRV) a partir del ECG

La HRV (Heart Rate Variability) mide los cambios en el tiempo entre latidos consecutivos usando los intervalos R-R del electrocardiograma (ECG).

**¿Qué indica la HRV?**

HRV alta: buen estado fisiológico, predominio parasimpático, adaptabilidad.
HRV baja: estrés, fatiga, predominio simpático, posible disfunción autonómica.

Proceso para obtener la HRV:

- Adquirir señal ECG.
- Detectar picos R (Pan-Tompkins u otro algoritmo).
- Extraer la serie temporal de intervalos R-R.
- Analizar mediante: Dominio del tiempo: SDNN, RMSSD, pNN50; Dominio de la frecuencia: LF, HF, relación LF/HF; Métodos no lineales: Poincaré, entropía, DFA; La HRV es usada en medicina, deporte, psicología, cardiología y estudios de estrés.

4. Diagrama de Poincaré (RRₙ vs RRₙ₊₁)
El Diagrama de Poincaré es un análisis no lineal que representa gráficamente la relación entre dos intervalos cardíacos consecutivos.

Cómo se construye:

Eje X → R-Rₙ

Eje Y → R-Rₙ₊₁
Cada punto representa un par de latidos consecutivos.

Parámetros:

- SD1: variabilidad de corto plazo (actividad vagal)
- SD2: variabilidad de largo plazo (componente simpático)
- SD1/SD2: índice de balance autonómico

Interpretación:

- Elipse estrecha → baja HRV → predominio simpático
- Elipse grande/redonda → alta HRV → buen control parasimpático
- Distribución caótica → arritmias o disfunción autonómica

## Diagrama de Flujo 
<img width="283" height="794" alt="image" src="https://github.com/user-attachments/assets/9085273b-b344-4423-8e45-74ba560ce252" />


**b. Adquisición de la señal ECG**


La señal electrocardiográfica (ECG) utilizada en este proyecto fue adquirida siguiendo el protocolo establecido en la guía de práctica.
Se seleccionó un sujeto de prueba y se realizó un registro de 4 minutos, dividido en dos fases:

**Fase 1: Reposo (0–2 min)**

El participante permaneció inmóvil, en silencio y en condición basal para registrar la actividad cardíaca sin estímulos externos.

**Fase 2: Lectura en voz alta (2–4 min)**

El participante leyó un fragmento de texto seleccionado, con el fin de inducir cambios fisiológicos asociados al esfuerzo cognitivo y la modulación autonómica.

La frecuencia de muestreo de 1000 Hz garantiza una resolución temporal suficiente para detectar los picos R con precisión y permite el análisis de HRV sin distorsiones.
<img width="1621" height="456" alt="image" src="https://github.com/user-attachments/assets/a035e0dc-409a-418f-bce1-0c9f6dd8754f" />
## PARTE B
### C. Pre procesamiento de la señal, electrocardiográfica y diseño del filtro digital. 

Esta parte es muy importante para aislar el complejo QRS, que es la manifestación eléctrica de la despolarización ventricular de los artefactos que le están acompañando y así poder hacer una detección de picos R más precisa.

#### Diseño del filtro

Este filtro se diseñó e implementó como un filtro IIR ( infinite impulse response) qué se relaciona con el Butter World en configuración pasa banda. Nuestra elección del Butterworth es debido a su respuesta en frecuencia plana en la banda. De paso, qué minimiza la distorsión de la onda QRS, que para nosotros como futuros ingenieros biomédicos es vital.


<img width="698" height="275" alt="image" src="https://github.com/user-attachments/assets/38cc5a3f-52df-495b-a9c4-dcea71150905" />


Nuestra frecuencia de muestreo fue de 1000 Hertz, las frecuencias de corte fueron de 0.5 Hz a 40 Hz. 

El orden del filtro fue de orden cuatro para la función de transferencia que se dio en el diseño, ya que este es un filtro, pasa banda, quiere decir que es un filtro IIR de octavo orden, porque hay cuatro polos para el corte bajo y cuatro para el corte alto lo que nos da una atenuación lo suficientemente amplia en la rechaza banda sin introducir una complejidad computacional.

#### Implementación y ecuación en diferencias

El filtro se implementó a través de su ecuación de diferencias, utilizando los coeficientes normalizados B, del numerador y A, del denominador que se obtuvieron en el diseño. A continuación, la ecuación fundamental.

<img width="337" height="80" alt="image" src="https://github.com/user-attachments/assets/833e0ca3-8884-4c59-9b04-ebb8ab5b4480" />

En esta ecuación x[n] es la señal de entrada y y[n] es la es la señal ya filtrada. En la programación, la implementación con la función ‘lfilter’ asumir la señal en reposo, estableciendo las condiciones iniciales en cero tal como nos lo indica la guía. 
Como resultado, tenemos la gráfica del segmento electrocardiográfica filtrado que nos demuestra un excelente atenuación del ruido de la línea base y una alta definición de los picos R, lo que rectifica el funcionamiento del diseño del filtro IIR.

#### Detección de picos R y generación de la serie RR.

La señal filtrada que es aproximadamente de unos 244.35 segundos de duración, se segmentó en dos bloques de 120 segundos o dos minutos para permitir un análisis comparativo de la HR V. A lo largo del tiempo.

<img width="700" height="425" alt="image" src="https://github.com/user-attachments/assets/3ccf8867-eb3c-4e48-9dc7-fabf02907b6d" />

Para la detección de los picos RS empleó la función de ‘find_peaks’ , la distancia mínima fue de 0.3 segundos y este valor evita la detección de artefactos que no son correspondientes a la onda QRS, como si fueran latidos independientes porque un corazón humano no puede tirar frecuencia superiores a los 3.33 latidos por segundo o 200 latidos por minuto de forma sostenida, se utilizó un umbral dinámico, basado en las estadísticas de los segmentos, en donde se adaptaron las variaciones de la amplitud y el ruido residual.

En el primer segmento de cero a dos minutos se detectaron 196 picos R. Y en el segundo segmento de dos a cuatro minutos se detectaron 207 picos R.

Los tiempos de ocurrencia de los picos se utilizaron para calcular la serie de intervalos RR, este intervalo se calcula como la diferencia de tiempo entre los dos picos R sucesivos.

<img width="694" height="573" alt="image" src="https://github.com/user-attachments/assets/e0850577-4a69-4029-b4bf-4f23c9935f67" />

La gráfica de la serie RR, como por ejemplo, en el segmento uno ilustrar las fluctuaciones en el ritmo cardíaco de largo del tiempo que son estudio de la HRV, las caídas bruscas observadas en los puntos podrían indicar artefactos, latidos prematuros o errores en la detección que requieren corrección de artefactos.

## DIAGRAMA DE FLUJO
![diagrama 1](https://github.com/user-attachments/assets/acc5f0bf-08fd-41f0-af0c-8fc3bce0b0c3)


### D. Análisis cualitativo de la variabilidad de la frecuencia cardiaca.

Esta parte se centró en la estadística de los intervalos R R. Generada en la etapa del pre procesamiento, comparando el segmento uno y el segmento dos.

La tabla, a continuación resumir los valores obtenidos para ambos segmentos, en todas las medidas. La diferencia de S2 versus S1, fue de reducción.

<img width="369" height="104" alt="image" src="https://github.com/user-attachments/assets/88d8607f-fa29-4a15-8c05-62d81d39df64" />

Este análisis de HRV en el dominio del tiempo indica un claro cambio en el estado fisiológico de nuestra persona prueba entre los dos segmentos.
- Segmento 1 (base) : se caracteriza por una mayor variabilidad y un ritmo cardiaco más lento, indicando un mayor tono vagal o estado de reposo relativo.
- ⁠Segmento 2 (Cambio) : se característica por un ritmo cardiaco, acelerado y una reducción significativa de la retirada parasimpática y la reducción de la variabilidad total. Estos hallazgos son consistentes con la respuesta de estrés a la lectura, ya que hay un aumento de la actividad simpática que fue inducida por el cambio emocional o la respuesta a un estímulo externo en la medición.

##  DIAGRAMA DE FLUJO
![diagrama 2](https://github.com/user-attachments/assets/ae0435d0-c0c7-4fde-a88c-5a09a1ceca8d)


## PARTE C
**e.Construcción del diagrama de Poincaré**

Para cada tramo de 2 minutos de la señal ECG se generó el diagrama de Poincaré, el cual se construye representando los intervalos  en el eje horizontal y los intervalos  en el eje vertical. Esta gráfica permite evaluar la variabilidad del ritmo cardiaco y distinguir componentes del sistema nervioso autónomo:

SD1, asociado a variabilidad de corto plazo (actividad vagal),

SD2, relacionado con la variabilidad de largo plazo (componente simpático + vagal).


**Comparación de la dispersión entre segmentos**

**Segmento 1 (0–2 min):**

  SD1  = 0.04678571245372775 s
  
  SD2  = 0.07732998998284517 s
  
  CVI  = -2.441538798771597
  
  CSI  = 1.6528548124457114
  

**Segmento 2 (2–4 min):**

  SD1  = 0.06144787404210659 s
  
  SD2  = 0.11152520584667719 s
  
  CVI  = -2.1641201046178127
  
  CSI  = 1.8149562956442655
  
  

En los dos segmentos analizados se observa un conjunto principal de puntos alrededor de valores entre 0.60 y 0.70 segundos. Sin embargo, el Segmento 2 (2–4 min) presenta una dispersión ligeramente mayor, lo que indica un aumento tanto de la variabilidad rápida como de la lenta. Esto coincide con los valores de SD1 y SD2, los cuales aumentan respecto al Segmento 1.

**Interpretación de los resultados**

<img width="590" height="590" alt="17635189634558261947150282215344" src="https://github.com/user-attachments/assets/1706f13a-d8e4-47b6-9e37-bf49d5ac3516" />

<img width="590" height="590" alt="17635189358827406964224730213446" src="https://github.com/user-attachments/assets/2a1f699f-c3a1-41ff-9a97-c8a4dff7372c" />





SD1 aumenta en el Segmento 2, lo cual refleja mayor fluctuación de corto plazo vinculada a la actividad vagal.

SD2 también incrementa, indicando mayor variabilidad global del ritmo cardiaco.

CVI (índice vagal) se vuelve menos negativo → leve incremento de actividad parasimpática.

CSI (índice simpático) aumenta → incremento de la actividad simpática.


En conjunto, los valores sugieren que durante el segundo período analizado el sistema nervioso autónomo está más activo, tanto en su componente simpático como parasimpático, lo cual se refleja en una nube de puntos más dispersa en el diagrama.


## Diagrama de flujo
![Imagen de WhatsApp 2025-11-18 a las 21 27 00_814e9610](https://github.com/user-attachments/assets/0df6f1d5-fae9-4bd6-87d7-77bde42fbfbb)







