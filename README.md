# Laboratorio 1 estadistica
En este laboratorio exploramos señales fisiológicas de ECG utilizando técnicas de estadística descriptiva y modelos de ruido. El objetivo es entender tanto las características propias de la señal como el impacto del ruido, analizando aspectos como la relación señal-ruido (SNR) 

---
<h1 align="center"><i><b>PARTE A DEL LABORATORIO</b></i></h1>

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
plt.plot(señal)
plt.title("Visualización de la Señal Médica")
plt.xlabel("Muestras")
plt.ylabel("Voltaje(μm)")
plt.grid(True)
plt.show()

```
<p align="center">
<img width="800" height="393" alt="image" src="https://github.com/user-attachments/assets/46baf1ac-03c6-494a-8272-ae6b86bde2b9" />
</p>


Grafica la señal completa, nombra los ejes (Ej:`muestras v.s voltaje`).


---

+ **Selección de un segmento de la señal**
```python
signal=señal[2000000:2010000]
plt.plot(signal)
plt.title("Visualización de la Señal Médica")
plt.xlabel("Muestras")
plt.ylabel("Voltaje(μm)")
plt.grid(True)
plt.show()
```
<p align="center">
<img width="500" height="455" alt="image" src="https://github.com/user-attachments/assets/c08a149a-20e0-46b5-b3e1-1fb07f31fe7a" />
</p>

Recortar la señal entre las muestras `2,000,000` y `2,010,000` (10,000 muestras). Se recomienda no tomar los valores iniciales ya que pueden ser muy variables mientras la extracción de la señal se estabiliza.
Se grafica esta nueva señal llamada `signal`.

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

*Resultado:* promedio= [51.83181363]

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

---

+ **Coeficiente de variación**
```python
Coeficiente=(desviacionEstandar/promedio)*100
print("Coeficiente de Variacion:", Coeficiente)
```
Mide la variabilidad relativa de los datos con respecto a su media en valor porcentual.

---

+ **Histograma y función de probabilidad**
```python
plt.figure()
plt.hist(signal,bins=100,density=0,color='blue')
plt.title("Histograma")
plt.xlabel("Voltaje(μv)")
plt.ylabel("Frecuencia ")
plt.show()
```
<p align="center">
<img width="500" height="455" alt="image" src="https://github.com/user-attachments/assets/dde26283-daea-45b9-83b7-fcc66d5e6ea4" />
</p>

Grafica un histograma y nombra los ejes *x* y *y*.
```python
sns.histplot(signal, kde=True, bins=30, color='red')
plt.hist(signal, bins=30, edgecolor='blue')
plt.title('Función de probabilidad')
plt.xlabel('Voltaje(μv)')
plt.ylabel('Frecuencia')
plt.show()
```

grafica el histograma nuevamente, pero con la curva de la `función de probabilidad`.

---
+ **Curtosis**
```python
numerador = sum((x - promedio)**4 for x in signal) / n
denominador = (sum((x - promedio)**2 for x in signal) / n) ** 2
curtosis= numerador / denominador

print("Curtosis:", curtosis)
```
Calcula la curtosis, la cual describe que tan achatados o afilados son los picos de la señal en comparación con una distribución normal (campana de gauss).

---

<h1 align="center"><i><b>PARTE B DEL LABORATORIO</b></i></h1>

+ **visualizacion de la señal extraida del generador biologico**
```python
df = pd.read_csv('medicion1.csv')
x = df.iloc[:, 0]
y = df.iloc[:, 1]
plt.figure(figsize=(10, 5))
plt.plot(x,y)
plt.title('Señal extraida del generador')
plt.xlabel('Tiempo (s)')
plt.ylabel('Voltaje (mV)')
plt.grid(True)
plt.show()
signal2= df.iloc[:, 1]
```
<p align="center">
<img width="700" height="470" alt="image" src="https://github.com/user-attachments/assets/83db39ad-3948-4408-97e7-d1b2bed5b1cf" />
</p>

+ **promedio o media**
  
```pyton
suma=0
n = 0
for muestra in signal2:
  suma=suma+muestra
  n=n+1

  promedio2=suma/n
print("promedio=", promedio2)
```
+ **desviacion estandar y varianza**
```pyton
suma = 0
n = 0
for muestra in signal2:
    suma += muestra
    n += 1
sumaCuadrados2 = 0
for muestra in signal2:
    diferencia = muestra - promedio
    sumaCuadrados2 += diferencia ** 2

varianza2 = sumaCuadrados2 / n
desviacionEstandar2 = varianza2 ** 0.5

print("Desviación Estándar:", desviacionEstandar2)
print("varianza:", varianza2)
```
+ **coeficiente de variacion**
```pyton
  Coeficiente2=(desviacionEstandar2/promedio2)*100
print("Coeficiente de Variación:", Coeficiente2)
```




<h1 align="center"><i><b>PARTE C DEL LABORATORIO</b></i></h1>
