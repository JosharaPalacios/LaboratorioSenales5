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






