# Proyecto Integrador – SIES Bogotá
## Fundamentos de Big Data

### 📌 Descripción general
Este proyecto analiza los registros de emergencias de la línea 123 de Bogotá, con datos públicos del **Sistema Integrado de Emergencias y Seguridad (SIES)**.  
El objetivo es aplicar técnicas de **análisis exploratorio de datos (EDA)** y comprender patrones temporales, geográficos y de tipo de incidente.

Los datos se obtienen del portal de [Datos Abiertos Bogotá](https://datosabiertos.bogota.gov.co/dataset/llamadas-de-urgencias-y-emergencias-que-ingresan-a-traves-de-la-linea-123).

---

### ⚙️ Configuración del entorno

El proyecto se ejecuta en **Google Colab**, conectado con **Google Drive** para almacenar y procesar los archivos CSV.

```python
from google.colab import drive
drive.mount('/content/drive')
```

Estructura recomendada:
```
/MyDrive/BigData_Emergencias_123/
 ├── llamadas_123_sep_2025.csv
 ├── llamadas_123_oct_2025.csv
 └── llamadas_123_nov_2025.csv
```

---

### 📂 Estructura del proyecto

```
BigData_SIES_Bogota/
│
├── notebooks/
│   └── SIES_Bogota_EDA.txt     # Notebook principal con el análisis exploratorio, deje el archivo .txt porque con ipynb genera cobro
│
├── data/
│   ├── llamadas_123_sep_2025.csv  # Datos originales descargados
│   ├── llamadas_123_oct_2025.csv
│   └── llamadas_123_nov_2025.csv
│
├── outputs/
│   ├── Resultados_Actividad_SIES_Bogota.docx  # Informe con hallazgos
│   └── Graficos_SIES.png                      # Visualizaciones generadas
│
└── README.md
```

---

### 🧪 Ejecuciones principales del Notebook

#### **1️⃣ Montar Google Drive y verificar datos**
```python
from google.colab import drive
drive.mount('/content/drive')
!ls "/content/drive/MyDrive/BigData_Emergencias_123"
```

#### **2️⃣ Cargar datos**
```python
import pandas as pd
df = pd.read_csv("/content/drive/MyDrive/BigData_Emergencias_123/llamadas_123_sep_2025.csv", 
                 sep=';', encoding='latin1')
```

#### **3️⃣ Exploración básica**
```python
df.info()
df.describe()
df.isnull().sum()
```

#### **4️⃣ Conteos y análisis inicial**
```python
df['LOCALIDAD'].value_counts().head(10)
df['TIPO_INCIDENTE'].value_counts().head(10)
```

#### **5️⃣ Limpieza de texto y codificación**
```python
df['LOCALIDAD'] = (df['LOCALIDAD']
                   .str.replace('µ','Á')
                   .str.replace('Ö','Ó')
                   .str.replace('à','Á')
                   .str.replace('','Á'))
```

#### **6️⃣ Conversión de fechas**
```python
df['FECHA_INICIO_DESPLAZAMIENTO_MOVIL'] = pd.to_datetime(
    df['FECHA_INICIO_DESPLAZAMIENTO_MOVIL'], errors='coerce'
)
```

#### **7️⃣ Visualizaciones**
```python
import matplotlib.pyplot as plt

plt.figure(figsize=(10,5))
df['LOCALIDAD'].value_counts().head(10).plot(kind='bar', title='Top 10 Localidades con más emergencias')
plt.xlabel('Localidad')
plt.ylabel('Número de emergencias')
plt.show()

plt.figure(figsize=(10,5))
df['TIPO_INCIDENTE'].value_counts().head(10).plot(kind='bar', title='Top 10 Tipos de Incidentes')
plt.xlabel('Tipo de incidente')
plt.ylabel('Cantidad')
plt.show()
```

#### **8️⃣ Exportar resultados**
```python
from docx import Document

doc = Document()
doc.add_heading('Resultados Análisis SIES Bogotá', level=1)
doc.add_paragraph('Resumen del análisis exploratorio y hallazgos principales...')
doc.save('/content/drive/MyDrive/BigData_Emergencias_123/Resultados_SIES.docx')
```

---

### 📊 Resultados principales
- Total de registros: **11.433**
- Promedio de edad: **45 años**
- Localidades con más emergencias: **Kennedy, Engativá, Suba**
- Tipos de incidente más frecuentes: **Heridos accidentales, inconscientes, eventos respiratorios**
- Cobertura temporal: **01/09/2025 – 01/10/2025**

---

### 💡 Próximos pasos
- Unir varios meses para análisis temporal extendido.
- Realizar clustering o predicción de demanda.
- Generar dashboard en Power BI o Google Data Studio.
