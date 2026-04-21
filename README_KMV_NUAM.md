# KMV_Nuam_FinalVersion - Modelo de Riesgo de Crédito KMV

## Descripción General

Este notebook implementa el modelo de Merton (KMV) para calcular la probabilidad de incumplimiento (Default Probability) de cinco emisores en el mercado de capitales colombiano. El modelo combina técnicas de teoría de opciones con datos financieros para estimar el riesgo de crédito de cada empresa.

### Emisores Analizados

1. **Bancolombia** - Datos XBRL (Superintendencia Financiera)
2. **Ecopetrol** - Datos de informes financieros
3. **Nutresa** - Datos XBRL (Superintendencia Financiera)
4. **Cementos Argos** - Datos de informes financieros
5. **Mineros** - Datos de informes financieros

## Metodología del Modelo KMV

### 1. Fundamento Teórico

El modelo KMV se basa en el modelo de Merton (1974), que trata el patrimonio de una empresa como una opción call europea sobre sus activos. El incumplimiento ocurre cuando el valor de los activos cae por debajo del valor de las deudas.

**Ecuación fundamental:**
```
E = A × N(d₁) - D × e^(-rT) × N(d₂)

Donde:
  E = Valor del patrimonio (Market Cap)
  A = Valor de activos
  D = Punto de incumplimiento (Default Point)
  r = Tasa libre de riesgo (5% asumida)
  T = Horizonte de 1 año
  N(d₁), N(d₂) = Distribución acumulada normal estándar
```

### 2. Proceso de Cálculo

#### Paso 1: Recopilación de Datos

**Para Bancolombia y Nutresa (datos XBRL):**
- Archivos XBRL enviados a la Superintendencia Financiera de Colombia
- Extracción de conceptos financieros conforme estándar IFRS:
  - `ifrs:Equity` - Patrimonio (instant)
  - `ifrs:Liabilities` - Pasivos totales (instant)
  - `ifrs:ProfitLoss` - Ganancia neta (duration)
  - `ifrs:DepositsFromCustomers` - Pasivos corrientes

**Para otros emisores (informes de resultados):**
- Extracción manual de estados financieros publicados
- Información desde archivos Excel estructurados con datos trimestrales/anuales
- Conceptos capturados: Equity, Pasivo Corriente, Pasivo No Corriente

**Precios de acciones:**
- Fuente: Investing.com
- Frecuencia: Datos históricos diarios en archivo `Prices.xlsx`
- Período de análisis: Mínimo 252 días (1 año) para cálculo de volatilidad

#### Paso 2: Cálculo de Volatilidad de Patrimonio

La volatilidad del patrimonio (Equity Volatility) se calcula a partir de los retornos históricos de precio:

```
Retorno diario = P_t / P_t-1 - 1
Volatilidad = STDEV(Retornos diarios) × √252

Donde:
  P_t = Precio de cierre en día t
  √252 = Factor de anualización (252 días de negociación por año)
```

#### Paso 3: Estimación del Valor de Activos

Los activos iniciales se aproximan como:
```
A_inicial = Market Cap + Pasivos Corrientes + Pasivos No Corrientes
```

#### Paso 4: Definición del Default Point

El punto de incumplimiento representa el nivel de pasivos en el que técnicamente ocurriría el default:

```
Default Point = Pasivos Corrientes + 0.5 × Pasivos No Corrientes
```


#### Paso 5: Algoritmo Iterativo de Merton

El notebook ejecuta un algoritmo iterativo que converge simultáneamente en:
- **Asset Value (A):** Valor total de activos
- **Asset Volatility (σ_A):** Volatilidad de los activos

Relación fundamental entre volatilidades:
```
σ_E = (A / E) × N(d₁) × σ_A
```

**Proceso iterativo:**
1. Inicializar A y σ_A
2. Calcular d1 y d2 usando valores actuales
3. Resolver simultáneamente para nuevo A y σ_A
4. Repetir hasta convergencia (epsilon < 0.00001)

**Criterios de convergencia:**
```
Función 1: E = A × N(d₁) - D × e^(-rT) × N(d₂)
Función 2: σ_E = N(d₁) × (A/E) × σ_A
```


#### Paso 6: Cálculo de Probabilidad de Default

Después de convergencia:

**Distancia al Default:**
```
d2 = [ln(A/D) + (r - 0.5×σ_A²)×T] / (σ_A × √T)
```

**Probabilidad de Default (usando t-student con 5 grados de libertad):**
```
PD = t_dist.cdf(-d2, df=5) aproximación
```


## Estructura del Notebook

### Sección 1: Librerías y Configuración
- Imports de librerías necesarias
- Configuración de compatibilidad para Arelle
- Setup de rutas y variables globales

### Sección 2: Bancolombia (XBRL)
- Lectura de archivos XBRL desde Superintendencia Financiera
- Extracción de conceptos IFRS (2020-2025)
- Cálculo de volatilidad
- Merge de datos financieros con precios históricos
- Aplicación del modelo KMV
- Visualización de probabilidad de default

### Sección 3: Ecopetrol (Datos Informes)
- Carga de datos desde archivo `EEFF_Emisores_KMV.xlsx`
- Ingesta de precios desde `Prices.xlsx`
- Preparación de datos para iteración KMV
- Cálculos de activos, pasivos y volatilidades
- Visualización de evolución de default

### Sección 4: Nutresa (XBRL)
- Lectura de archivos XBRL de Nutresa
- Extracción de balance y P&L (2020-2025)
- Iteración del modelo KMV
- Generación de gráficos de probabilidad
- Tabla de resultados consolidados

### Secciones 5-6: Emisores Adicionales (Excel)
- Patrón similar a Ecopetrol
- Datos desde informes de resultados publicados
- Cálculos KMV completos por emisor

## Fuentes de Datos

### 1. Archivos XBRL
Ubicación: `./Informes Bancolombia/` y `./Informes Nutresa/`

Formato XBRL (eXtensible Business Reporting Language) conforme a normativa de la Superintendencia Financiera:
- Períodos: Cierres anuales (31 de diciembre) desde 2020
- Taxonomía: IFRS
- Conceptos extraídos programáticamente mediante Arelle

Ejemplo de archivo:
```
0054371409_0001_000007_0000_000000_000000_C-C_2025-12-31.xbrl
```

### 2. Informes Financieros (Excel)
Ubicación: `./EEFF_Emisores_KMV.xlsx`

Estructura de archivo:
- Hoja 1: Bancolombia
- Hoja 2: Ecopetrol 
- Hoja 3-5: Otros emisores

Columnas requeridas por hoja:
- Fecha
- Patrimonio (Equity)
- Pasivo Corriente
- Pasivo No Corriente (o Pasivo total para deducir)
- ProfitLoss / Resultado Neto

### 3. Precios de Acciones
Ubicación: `./Prices.xlsx`

Hoja "Prices" con estructura:
- Columna A: Fecha (diaria)
- Columna B: CIBEST (Bancolombia)
- Columna C: ECOPETROL
...


## Salidas del Notebook

### 1. Tablas de Resultados

**KMV_DATA** por emisor:
```
Date              | Assets      | Liabilities | Market_Cap | Asset_Vol | Default_Prob
2025-12-31        | 150,000 M   | 85,000 M    | 45,000 M   | 0.1234    | 2.34%
2024-12-31        | 148,000 M   | 80,000 M    | 52,000 M   | 0.1187    | 1.89%
```

Columnas principales:
- **Date:** Fecha de reporte
- **Assets:** Valor total estimado de activos (M)
- **Liabilities:** Pasivos totales o Default Point
- **Market_Cap:** Capitalización bursátil (precio × acciones en circulación)
- **Equity_Vol:** Volatilidad del patrimonio (estimada)
- **Asset_Vol:** Volatilidad de activos (calculada iterativamente)
- **Default_Prob:** Probabilidad de default en 1 año (%)
- **d2:** Distancia al default en desviaciones estándar

### 2. Gráficos

**Probabilidad de Default sobre el tiempo:**
- Tipo: Gráfico de línea con marcadores
- Eje X: Fecha
- Eje Y: Probabilidad de Default (%)
- Escala: Automática según rango de valores
- Línea punteada en color distintivo por emisor

**Comparativas y análisis:**
- Evolución de volatilidad de activos
- Valor de activos vs pasivos
- Distancia al default

## Parámetros Configurables

Los siguientes parámetros pueden ajustarse según necesidades:

```python
# Tasa libre de riesgo (anual)
r = 0.05  # 5% Nivel de tasas actual

# Horizonte temporal (años)
T = 1  # 1 año

# Convergencia iterativa
epsilon_threshold = 0.00001  # Tolerancia de convergencia

# Distribución para PD
degrees_of_freedom = 5  # t-student grados de libertad

# Ventana de volatilidad
volatility_window = 252  # Días para STDEV de retornos

# Tolerancia de merge de precios
price_tolerance = pd.Timedelta('5D')  # 5 días hacia atrás
```


## Dependencias

```
Python 3.8+
arelle              # Para parsing XBRL
pandas              # Manipulación de datos
numpy               # Cálculos numéricos
scipy               # Distribuciones estadísticas
matplotlib          # Visualización
openpyxl            # Lectura de Excel
isodate             # Procesamiento de fechas ISO
```

Instalación:
```bash
pip install arelle pandas numpy scipy matplotlib openpyxl isodate
```

## Ejecución

### Opción 1: Jupyter Notebook
```bash
jupyter notebook KMV_Nuam_FinalVersion.ipynb
```


---

**Última actualización:** Abril 2026
**Período de análisis:** 2020-2025
**Precisión de datos:** Conforme a reportes públicos (XBRL y Investing.com)
