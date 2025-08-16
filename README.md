# Laboratorio-1--estadistica
En este laboratorio exploramos señales fisiológicas de ECG utilizando técnicas de estadística descriptiva y modelos de ruido. El objetivo es entender tanto las características propias de la señal como el impacto del ruido, analizando aspectos como la relación señal-ruido (SNR) 
```python
!pip install wfdb
import matplotlib.pyplot as plt
import numpy as np
import wfdb
import pandas as pd
import os
from scipy.stats import norm
import seaborn as sns

```record_name= '100001_ECG'


señal,caracter = wfdb.rdsamp(record_name)

"""
print("Información del registro:")
print(f"Nombre del registro: {record.record_name}")
print(f"Número de señales: {record.n_sig}")
print(f"Nombres de las señales: {record.sig_name}")
print(f"Frecuencia de muestreo: {record.fs} Hz")
print(f"Duración de la señal: {record.sig_len / record.fs:.2f} segundos")
"""


plt.figure(figsize=(12, 4))
plt.plot(señal)
plt.title("Visualización de la Señal Médica")
plt.xlabel("Muestras")
plt.ylabel("Voltaje(μm)")
plt.grid(True)
plt.show()

caracter

signal=señal[2000000:2010000]


