# Laboratorio 1 Estadística
En este laboratorio exploramos señales fisiológicas de ECG utilizando técnicas de estadística descriptiva y modelos de ruido. El objetivo es entender tanto las características propias de la señal como el impacto del ruido, analizando aspectos como la relación señal-ruido (SNR) 

---
<h1 align="center"><i><b>PARTE A DEL LABORATORIO</b></i></h1>

```mermaid

graph TD
    A([Inicio]) --> B[Instalar e importar librerías]
    B --> C[Cargar señal ECG]
    C --> D[Graficar señal completa]
    D --> E[Seleccionar segmento]
    E --> F[Graficar segmento]
    F --> G[Calcular media]
    G --> H[Calcular varianza y Desv. Estándar]
    H --> I[Coeficiente de variación]
    I --> J[Histograma]
    J --> K[Funcion de probabilidad]
    K --> L[Calcular curtosis]
    L --> M([Fin])

```


+ **importación de librerias y carga de señal**
```python
!pip install wfdb
import matplotlib.pyplot as plt
import numpy as np
import wfdb
import pandas as pd
import os
from scipy.stats import norm
import seaborn as sns
```
Se instalan la libreria `wfdb` para leer archivos locales (.hea, .dat, etc.) del directorio de trabajo. 
Se importan diferentes librerias necesarias para desarrollar el código con sus funciones.

```python
record_name= '100001_ECG'
señal,caracter = wfdb.rdsamp(record_name)
```
Carga la señal, `record_name` debe ser el nombre del archivo (ej: `100001_ECG`).
Nombra la señal `señal`.

---
+ **visualización señal**
```python
plt.figure(figsize=(12, 4))
plt.plot(señal,color="deeppink")
plt.title("Visualización de la Señal Médica")
plt.xlabel("Muestras")
plt.ylabel("Voltaje(μm)")
plt.grid(True)
plt.show()

```
**Resultado:**

<p align="center">
<img width="1034" height="300" alt="image" src="https://github.com/user-attachments/assets/2727c7c5-2d33-49d2-bd44-a4012c0ecd10" />
</p>




Grafica la señal completa, nombra los ejes (Ej:`muestras v.s voltaje`).


---

+ **Selección de un segmento de la señal**
```python
signal=señal[2000000:2010000]
plt.plot(signal,color="deeppink")
plt.title("Visualización de la Señal Médica")
plt.xlabel("Muestras")
plt.ylabel("Voltaje(μm)")
plt.grid(True)
plt.show()
```

Recortar la señal entre las muestras `2,000,000` y `2,010,000` (10,000 muestras). Se recomienda no tomar los valores iniciales ya que pueden ser muy variables mientras la extracción de la señal se estabiliza.
Se grafica esta nueva señal llamada `signal`.

**Resultado:**

<p align="center">
<img width="583" height="400" alt="image" src="https://github.com/user-attachments/assets/ab0ca852-91ff-4aa7-9ead-cb52de1835a7" />

</p>

---

+ **Promedio o media**
```python
suma = 0
n = 0
for muestra in signal:
  suma=suma+muestra
  n=n+1
  promedio=suma/n

print("promedio=", promedio)
```
Se calcula de forma manual la media usando un bucle `for` y finalmente se imprime este valor.

`suma=0`:inicializacion de la variable suma para acomular las sumas de las muestras. `n = 0:` inicializa un contador para llevar el número total de muestras. `for muestra in signal:` recorre cada muestra en la señal.`suma = suma + muestra`:  suma el valor de la muestra a la variable suma. `n = n + 1`: incrementa el contador n en uno por cada muestra.`promedio = suma / n`: calcula el promedio dividiendo la suma total entre la cantidad de muestras.

**Resultado:** promedio= [51.83181363]


---


+ **Varianza y desviación estándar**
```python
sumaCuadrados = 0
for muestra in signal:
    diferencia = muestra - promedio
    sumaCuadrados += diferencia ** 2

varianza = sumaCuadrados / n
desviacionEstandar = varianza ** 0.5

print("Desviación Estándar:", desviacionEstandar)
print("varianza:", varianza)
```
Calcula la `varianza`, que es la dispersión de los datos con respecto a la media, y posteriormente calcula la `Desviación estándar` que es la raiz cuadrada de la varianza. 

`sumaCuadrados = 0:`  crea una variable para guardar la suma de las diferencias al cuadrado.`for muestra in signal:` recorre cada dato (muestra) en la señal.`diferencia = muestra - promedio:` calcula cuánto se aleja la muestra del promedio.`sumaCuadrados += diferencia ** 2:` eleva la diferencia al cuadrado y se acumula en sumaCuadrados.
`varianza = sumaCuadrados / n:` calcula la varianza dividiendo la suma total por el número de datos.`desviacionEstandar = varianza ** 0.5:` obtiene la desviación estándar sacando la raíz cuadrada de la varianza.`print("Desviación Estándar:", desviacionEstandar):` muestra el valor de la desviación estándar.`print("varianza:", varianza):` muestra el valor de la varianza.

**Resultados:**

*Desviación Estándar: [312.3767537]

*varianza: [97579.23625063]

---

+ **Coeficiente de variación**
```python
Coeficiente=(desviacionEstandar/promedio)*100
print("Coeficiente de Variacion:", Coeficiente)
```
Mide la variabilidad relativa de los datos con respecto a su media en valor porcentual.

`Coeficiente = (desviacionEstandar / promedio) * 100:`calcula el coeficiente de variación dividiendo la desviación estándar entre el promedio y multiplicando por 100 para obtener un porcentaje.
`print("Coeficiente de Variacion:", Coeficiente):`Se muestra el resultado del coeficiente de variación en pantalla.

**Resultado:**

Coeficiente de Variacion: [602.6737863]

---

+ **Histograma y función de probabilidad**
```python
plt.figure()
plt.hist(signal,bins=100,density=0,color='deeppink')
plt.title("Histograma")
plt.xlabel("Voltaje(μv)")
plt.ylabel("Frecuencia ")
plt.show()
```
Grafica un histograma y nombra los ejes *x* y *y*.

**Resultado:**

<p align="center">
<img width="585" height="455" alt="image" src="https://github.com/user-attachments/assets/23d93aed-f0e0-49fb-8b69-e6fc61813c20" />
</p>

```python
plt.figure(figsize=(10, 5))
sns.histplot(signal, bins=100, stat="count", kde=True)
plt.title('Histograma con función KDE')
plt.xlabel('Voltaje (μV)')
plt.ylabel('Frecuencia')
plt.grid(True)
plt.show()
```
grafica el histograma nuevamente, pero con la curva de la `función de probabilidad`.

**Resultado:**

<p align="center">
<img width="862" height="400" alt="image" src="https://github.com/user-attachments/assets/cbeb7cbb-7da1-4ebc-9f79-3a2211490135" />
</p>

`sns.histplot`:Es una funcion de seaborn para realizar histogramas y de forma estilizada.

`signal`: se guardan los datos obtenidos de la señal descargada.

 `bins=100`: divide los datos en 100 barras (más detalle en el histograma).
 
`stat="count"`: el eje Y muestra la cantidad de veces que aparece cada valor (frecuencia).

`kde=True`: agrega la curva de funcion de probabilidad encima del histograma realizado.

---
+ **Curtosis**
```python
numerador = sum((x - promedio)**4 for x in signal) / n
denominador = (sum((x - promedio)**2 for x in signal) / n) ** 2
curtosis= numerador / denominador

print("Curtosis:", curtosis)
```
Calcula la curtosis, la cual describe que tan achatados o afilados son los picos de la señal en comparación con una distribución normal (campana de gauss).

**Resultado:**

*Curtosis: [15.20632096]

---

<h1 align="center"><i><b>PARTE B DEL LABORATORIO</b></i></h1>

```mermaid
graph TD
    A[Inicio] --> B[Extracción de señal]
    B --> C[Leyendo datos con pandas]
    C --> D[Visualización de señal]
    D --> E[Cálculo estadísticos]
    E --> F[Histograma y función de probabilidad]
    F --> G[Fin]
 ```
 
+ **visualizacion de la señal extraida del generador biologico**
  
Inicialmente se extrajo la señal desde un generador de señales, usando un DAQ. Para esto se descargó el programa NI DAQ MX y se configuro como finite samples para extraer 100 muestras. Después en mathlab con Data acquisition toolbox se definió la frecuencia de muestreo. Estos datos organizados en una tabla se exportaron como archivo `.csv `para finalmente abrir la tabla en Excel.

posteriormente se lee el registro usando una función de pandas llamada `pd.read_csv()`, y se nombran las columnas x y y, para despues guardarlo en la variable `signal2`.

```python
df = pd.read_csv('medicion1.csv')
x = df.iloc[:, 0]
y = df.iloc[:, 1]
plt.figure(figsize=(10, 5))
plt.plot(x,y,color='springgreen')
plt.title('Señal extraida del generador')
plt.xlabel('Tiempo (s)')
plt.ylabel('Voltaje (mV)')
plt.grid(True)
plt.show()
signal2= df.iloc[:, 1]
```

<p align="center">
<img width="750" height="470" alt="image" src="https://github.com/user-attachments/assets/3497e712-6952-402e-af17-60c4e2ee5619" />
</p>

  Se vuelven a calcular manualmente los estadisticos descriptivos de la señal como se hizo en la parte A.
  
+ **promedio o media**
  
```python
suma=0
n = 0
for muestra in signal2:
  suma=suma+muestra
  n=n+1

  promedio2=suma/n
print("promedio=", promedio2)
```
**Resultado:**

promedio= 1.2196760663788881

+ **desviacion estandar y varianza**
```python
suma = 0
n = 0
diferencia= 0
for muestra in signal2:
    suma += muestra
    n += 1
sumaCuadrados2 = 0
for muestra in signal2:
    diferencia = muestra - promedio2
    sumaCuadrados2 += diferencia ** 2

varianza2 = sumaCuadrados2 / n
desviacionEstandar2 = varianza2 ** 0.5

print("Desviación Estándar:", desviacionEstandar2)
print("varianza:", varianza2)
```
**Resultados**

Desviación Estándar: [0.40117251725441216]

varianza: [0.16093938860024162]

+ **coeficiente de variacion**
```python
  Coeficiente2=(desviacionEstandar2/promedio2)*100
print("Coeficiente de Variación:", Coeficiente2)
```
**Resultado:**

Coeficiente de Variación: [32.891726607824516]

+ **histograma y funcion de probabilidad**
```python
plt.figure()
plt.hist(signal2,bins=100,density=0,color='springgreen')
plt.title("Histograma señal generador")
plt.xlabel("Voltaje(μv)")
plt.ylabel("Frecuencia ")
plt.show()
```
**Resultado:**

<p align="center">
<img width="577" height="455" alt="image" src="https://github.com/user-attachments/assets/3b08dc29-e108-40db-bf3e-ffa6b5d50d04" />
</p>

<h1 align="center"><i><b>PARTE C DEL LABORATORIO</b></i></h1>

```mermaid
graph TD
    A[Inicio] --> B[Cargar señal desde CSV]
    B --> C[Visualizar señal original]
    
    C --> D[Agregar ruido gaussiano]
    D --> D1[Generar ruido normal media 0]
    D1 --> D2[Sumar ruido a la señal]
    D2 --> D3[Calcular SNR]
    D3 --> D4[Graficar resultado]
    
    C --> E[Agregar ruido impulsivo]
    E --> E1[Definir probabilidad de impulso]
    E1 --> E2[Insertar impulsos aleatorios]
    E2 --> E3[Calcular SNR]
    E3 --> E4[Graficar resultado]

    C --> F[Agregar ruido de artefacto]
    F --> F1[Generar señal senoidal baja frecuencia]
    F1 --> F2[Sumar artefacto a la señal]
    F2 --> F3[Calcular SNR]
    F3 --> F4[Graficar resultado]

    D4 --> G[Fin]
    E4 --> G
    F4 --> G
    
```


A la señal de la parte B (signal2) se le contamina con 3 tipos de ruido diferentes para despues calcular su valor SNR.

---
+ **Ruido gaussiano**
El ruido gaussiano es un tipo de ruido aleatorio cuyas variaciones siguen una distribución normal.Se define por su media (0 en este caso) y su desviación estándar (0.1, que controla su intensidad). Es común en señales fisiológicas debido a la electrónica del sistema de adquisición y otras fuentes de interferencia aleatoria.
```python
fs = 1000  
t = np.arange(len(signal2)) / fs * 1000   

ruido = np.random.normal(0, 0.1, len(signal2))
señal_rgauss = signal2 + ruido

pot_signal = np.mean(signal2**2)
pot_ruido  = np.mean(ruido**2)
SNR = 10 * np.log10(pot_signal / pot_ruido)

plt.figure(figsize=(12,6))
plt.plot(t, señal_rgauss, color="red", linewidth=0.8, label="Ruido gaussiano")
plt.plot(t, signal2, color="teal", linewidth=1, label="Señal fisiológica")

plt.title(f"Señal con ruido gaussiano, SNR = {SNR:.3f} dB")
plt.xlabel("Tiempo [ms]")
plt.ylabel("Voltaje [mV]")
plt.legend()
plt.grid(True)
plt.show()
```
resultado:
<p align="center">
   <img width="950" height="547" alt="image" src="https://github.com/user-attachments/assets/f4916579-6f3b-4683-a9f1-7c8316098115" />
    </p>

+ `fs:` Frecuencia de muestreo en hz.

+ `ruido = np.random.normal:` Se genera un ruido gaussiano.

+ `señal_rgauss = signal2 + ruido:` señal con ruido gaussiano.

+ `pot_signal:` Se utiliza para calcular el SNR.

+ `SRN:` 21.713 dB
---

+ **Ruido impulso**
Este ruido se caracteriza por picos abrupto y repentinos en la señal, generados aquí con una probabilidad del 8% (prob_impulso = 0.08). La función np.random.choice determina en qué puntos aparecen los impulsos (1 o 0), y la amplitud se asigna aleatoriamente con valores de ±0.2. Este ruido suele deberse a interferencias externas o fallos en la transmisión de datos.
```python
fs = 1000   
t = np.arange(len(signal2)) / fs * 1000   

ruido_impulso = np.zeros(len(signal2))
num_impulsos = int(0.1 * len(signal2))  
posiciones = np.random.randint(0, len(signal2), num_impulsos)
ruido_impulso[posiciones] = np.random.choice([-1, 1], size=num_impulsos) * np.random.uniform(0.5, 1.5, num_impulsos)
señal_rimpulso = signal2 + ruido_impulso
pot_signal = np.mean(signal2**2)
pot_ruido  = np.mean(ruido_impulso**2)
SNR = 10 * np.log10(pot_signal / pot_ruido)

plt.figure(figsize=(12,6))
plt.plot(t, señal_rimpulso, color="syan", linewidth=0.8, label="Ruido impulsivo")
plt.plot(t, signal2, color="teal", linewidth=1, label="Señal fisiológica")

plt.title(f"Señal con ruido impulsivo, SNR = {SNR:.3f} dB")
plt.xlabel("Tiempo [ms]")
plt.ylabel("Voltaje [mV]")
plt.legend()
plt.grid(True)
plt.show()
```
resultado:
<p align="center">
  <img width="950" height="547" alt="image" src="https://github.com/user-attachments/assets/7397d085-c913-4d82-9b36-a786b417e21d" />
</p>

+ `fs:` Frecuencia de muestreo en hz.

+ `ruido_impulso = np.zeros(len(signal2)):` Se genera un ruido de impulso.

+ `señal_rimpulso = signal2 + ruido_impulso:` señal con ruido de impulso.

+ `pot_signal:` Se utiliza para calcular el SNR.

+ `SRN:` 11.675dB 

---

+ **Ruido artefacto**
Este ruido representa alteraciones no deseadas en la señal, que no se encuentran presentes en la fuente original si no que se deben a alteraciones externas a dicha fuente, como movimientos del paciente o fallos en los electrodos. Es similar al ruido de impulso, pero con una mayor probabilidad de ocurrencia (prob_imp = 0.15). Se genera con la misma lógica de np.random.choice, agregando perturbaciones aleatorias.
```python
fs = 1000 
t = np.arange(len(signal2)) / fs * 1000 
freq_artefacto = 0.5 
ruido_artefacto = 0.3 * np.sin(2 * np.pi * freq_artefacto * (t/1000))

señal_artefacto = signal2 + ruido_artefacto
pot_signal = np.mean(signal2**2)
pot_ruido  = np.mean(ruido_artefacto**2)
SNR = 10 * np.log10(pot_signal / pot_ruido)

plt.figure(figsize=(12,6))
plt.plot(t, señal_artefacto, color="lime", linewidth=0.8, label="Ruido artefacto")
plt.plot(t, signal2, color="deeppink", linewidth=1, label="Señal fisiológica")

plt.title(f"Señal con ruido de artefacto, SNR = {SNR:.3f} dB")
plt.xlabel("Tiempo [ms]")
plt.ylabel("Voltaje [mV]")
plt.legend()
plt.grid(True)
plt.show()
```
resultado:
<p align="center">
   <img width="950" height="547" alt="image" src="https://github.com/user-attachments/assets/5e2d1a7a-0544-4d59-b81d-26ce885452c3" />
</p>

+ `fs:` Frecuencia de muestreo en hz.

+ `freq_artefacto:` Se genera un ruido de artefacto donde es de baja frecuencia senoida.

+ `señal_artefacto:` signal2 + ruido_artefacto: Ruido con ruido de artefacto.

+ `pot_signal:` Se utiliza para calcular el SNR.

+ `SNR:` 27,627 dB


 <h1 align="center"><i><BIBLIOGRAFIA</b></i></h1>
     
     https://physionet.org/about/database/
     https://docs.github.com/es/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax
     https://www.w3schools.com/python/default.asp
 
