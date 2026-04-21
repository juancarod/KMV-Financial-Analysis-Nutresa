# NUTRESA FMV-FA: Análisis de Riesgo de Crédito
## Modelo KMV + Análisis Financiero 2020-2025

---

## 📋 Tabla de Contenidos

1. [Descripción General](#descripción-general)
2. [Modelo de Default KMV](#modelo-de-default-kmv)
3. [Análisis Financiero](#análisis-financiero)
4. [Interpretación de Resultados](#interpretación-de-resultados)
5. [Estructura del Proyecto](#estructura-del-proyecto)
6. [Requisitos y Dependencias](#requisitos-y-dependencias)
7. [Cómo Usar](#cómo-usar)

---

## 📊 Descripción General

Este proyecto realiza un análisis integral del **riesgo de crédito de NUTRESA** utilizando dos enfoques complementarios:

1. **Modelo Merton (KMV)**: Estimación de probabilidad de default basada en teoría de opciones
2. **Análisis Financiero Tradicional**: Evaluación de indicadores de solvencia, rentabilidad y liquidez

**Período de Análisis**: 2020-2025 (datos de cierre de año fiscal)

**Fuente de Datos**: Reportes XBRL enviados a Superfinanciera de Colombia

---

## 🔬 Modelo de Default KMV

### ¿Qué es el Modelo KMV?

El modelo **Merton - KMV** es un enfoque de **teoría de opciones** que trata el capital de una empresa como una opción call europea sobre sus activos. Fue desarrollado por Kealhofer, McQuown y Vasicek en 1974 y es ampliamente utilizado por instituciones crediticias para medir el riesgo de insolvencia.



### Variables Clave del Modelo

1. **Valor de Activos (VA)**: Estimado como VA = E + D
   - E (Equity) = Valor de mercado del patrimonio
   - D (Debt) = Valor contable de la deuda

2. **Volatilidad de Activos (σ_A)**: 
   - Mide la incertidumbre en los retornos de activos
   - Calculada a partir de: `σ_A = (E/A) × σ_E`

3. **Distancia al Default (DD)**:
   ```
   DD = [ln(A/D) + (μ - 0.5×σ_A²)×T] / (σ_A × √T)
   ```
   - Mide cuántas desviaciones estándar está la empresa de insolvencia
   - **DD > 2.0**: Bajo riesgo
   - **DD entre 1.0-2.0**: Riesgo moderado
   - **DD < 1.0**: Alto riesgo de incumplimiento

4. **Probabilidad de Default (PD)**:
   ```
   PD = N(-DD)
   ```
   - Calificación dentro del modelo KMV de Moody's Analytics


---

## 💰 Análisis Financiero

El análisis complementa el modelo KMV con métricas tradicionales de solvencia y desempeño.

### 1. **Indicador de Apalancamiento**

#### Deuda / EBITDA
**Fórmula:**
```
Deuda/EBITDA = Pasivos Financieros / EBITDA
```

**Interpretación:**
- **< 2.0x**: Bajo apalancamiento (saludable)
- **2.0-3.0x**: Moderado (aceptable)
- **3.0-5.0x**: Alto (requiere vigilancia)
- **> 5.0x**: Muy alto (riesgo significativo)

**Para NUTRESA 2025:**
- Relación: **5.61x**
- **Estado**: 🔴 Crítico - Deuda muy elevada relativa a flujo operativo

#### Liabilities / Equity (Ratio D/E)
**Fórmula:**
```
Liabilities/Equity = Pasivos Totales / Patrimonio Total
```

**Interpretación:**
- **< 0.5x**: Muy conservador
- **0.5-1.0x**: Conservador (bajo riesgo)
- **1.0-2.0x**: Moderado (riesgo aceptable)
- **> 2.0x**: Agresivo (riesgo elevado)

**Para NUTRESA 2025:**
- Relación: **4.06x**
- **Estado**: 🔴 Crítico - Estructura de capital comprometida

### 2. **Capacidad de Cobertura de Intereses**

#### EBITDA / Intereses (Interest Coverage Ratio)
**Fórmula:**
```
EBITDA/Intereses = EBITDA / Gastos de Intereses
```

**Interpretación:**
- **> 5.0x**: Excelente cobertura
- **3.0-5.0x**: Buena cobertura
- **1.5-3.0x**: Débil (riesgo de estrés)
- **< 1.5x**: Crítico (dificultad para pagar intereses)

**Para NUTRESA 2025:**
- Relación: **2.32x**
- **Estado**: 🔴 Débil - Capacidad limitada de cobertura

### 3. **Desempeño Operacional**

#### Revenue, Margins & Cash Flow
- **Revenue (Ingresos)**: Crecimiento de operaciones
- **Gross Margin (Margen Bruto)**: Eficiencia productiva
  - **Fórmula**: (Ganancia Bruta / Revenue) × 100
- **Net Margin (Margen Neto)**: Rentabilidad final
  - **Fórmula**: (Ganancia Neta / Revenue) × 100
- **Operating Cash Flow**: Generación de caja operativa

**Para NUTRESA 2025:**
| Métrica | 2025 | Evolución 2020-2025 |
|---------|------|-------------------|
| Revenue | $20.7B | ✓ +84.6% |
| Gross Margin | 39.14% | ✓ Estable |
| Net Margin | 6.54% | ✓ Mejora vs 2024 |
| Operating CF | $1.25B | ✓ +113% |

---

## 📈 Interpretación de Resultados

### Análisis Integral NUTRESA 2025

| Dimensión | Métrica | 2025 | Estado |
|-----------|---------|------|--------|
| **Solvencia** | Liabilities/Equity | 4.06x | 🔴 Crítico |
| **Apalancamiento** | Deuda/EBITDA | 5.61x | 🔴 Crítico |
| **Cobertura** | EBITDA/Intereses | 2.32x | 🔴 Débil |
| **KMV Default** | Distancia al Default | ? | ⏳ Calculado |
| **KMV PD** | Probabilidad de Default | ? | ⏳ Calculado |
| **Operaciones** | Revenue Growth | +84.6% | ✓ Positivo |

---

## 🗂️ Estructura del Proyecto

```
Prueba_Estructurador_Deuda/
├── KMV_Nuam_FinalVersion.ipynb         # Análisis KMV y métricas financieras
├── Nutresa_FMV_FA.ipynb                # Notebook de trabajo
├── README.md                            # Este archivo
├── Informes Nutresa/                    # Datos XBRL (2020-2025)
│   ├── *_2020-12-31.xbrl
│   ├── *_2021-12-31.xbrl
│   ├── ... 
│   └── *_2025-12-31.xbrl
├── Prices.xlsx                          # Histórico de precios de acciones
└── Outputs/                             # Resultados y gráficos
    ├── XBRL_Nutresa_2025-12-31_COMPLETO.xlsx
    └── [Gráficos y reportes generados]
```

---

## 🔧 Requisitos y Dependencias

### Python 3.8+

```bash
# Bibliotecas principales
pandas>=1.3.0
numpy>=1.21.0
matplotlib>=3.4.0
scipy>=1.7.0
openpyxl>=3.6.0

# Para XBRL parsing
arelle
isodate
```

### Instalación

```bash
pip install arelle pandas numpy matplotlib scipy openpyxl isodate
```

---

## 💻 Cómo Usar

### 1. Preparar datos
- Colocar archivos XBRL en carpeta `Informes Nutresa/`
- Colocar histórico de precios en `Prices.xlsx`

### 2. Ejecutar análisis KMV

```python
python
>>> exec(open('KMV_Nuam_FinalVersion.ipynb').read())
```

O desde Jupyter:
```bash
jupyter notebook KMV_Nuam_FinalVersion.ipynb
```

### 3. Interpretar resultados

**Salidas clave:**
- **Gráfico Deuda/EBITDA**: Evolución del apalancamiento
- **Gráfico EBITDA/Intereses**: Capacidad de cobertura
- **Gráfico Liabilities/Equity**: Estructura de capital
- **Métricas Operacionales**: Revenue, márgenes, cash flow
- **Scores KMV**: Distancia al default y probabilidad de insolvencia

---

## 📊 Métodos y Fórmulas

### Cálculo de Volatilidad de Patrimonio (σ_E)

De retornos históricos de precio de acciones:
```
σ_E = STDEV(ln(P_t / P_t-1))
```

### Iteración para Volatilidad de Activos

```
σ_A = σ_E × (E / (E + D × (1 - tax_rate)))
```

Iteración hasta convergencia.

### Distancia al Default (DD)

```
DD = [ln(A/D) + (r - 0.5×σ_A²)×T] / (σ_A × √T)

Donde:
- A = Valor total de activos
- D = Pasivos totales
- r = Tasa libre de riesgo (2.5% asumido)
- T = 1 año (horizonte de análisis)
```

### Probabilidad de Default

```
PD = N(-DD)
```

Donde N es la distribución normal acumulada estándar.

---

## 📝 Conclusiones

**NUTRESA en 2025 presenta:**

✓ **Fortalezas:**
- Ingresos crecientes (+84.6% desde 2020)
- Márgenes operativos estables (~39% bruto)
- Generación de caja operativa positiva

🔴 **Debilidades Críticas:**
- Apalancamiento extremadamente alto (Deuda/EBITDA = 5.61x)
- Estructura de capital deteriorada (L/E = 4.06x)
- Cobertura de intereses débil (2.32x)
- Salto súbito de deuda en 2025 (+138%)

**Veredicto**: Empresa bajo estrés financiero severo. Requiere monitoreo intensivo y planes inmediatos de reducción de deuda.

---


## 📚 Referencias

1. **Modelo Merton**: Merton, R. C. (1974). "On the Pricing of Corporate Debt: The Risk Structure of Interest Rates"
2. **KMV Model**: Vasicek, O. (2002). "The Loan Loss Distribution"
3. **XBRL Standard**: https://www.xbrl.org/
4. **Colombian Financial Regulations**: Superfinanciera de Colombia

---

**Última actualización**: Abril 2026
**Período de datos**: 2020-2025 (año fiscal cerrado 31 de diciembre)
**Precisión**: Basado en reportes XBRL oficiales de NUTRESA
