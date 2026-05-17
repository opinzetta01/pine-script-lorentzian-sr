# Konkorde NextGen - Independent Pro (Pine Script v6)

**Konkorde Pro** es un indicador técnico avanzado de grado cuantitativo para TradingView escrito en **Pine Script v6**. Esta versión evoluciona la metodología clásica de Konkorde (basada en el flujo de dinero institucional y retail) para convertirla en un sistema predictivo y robusto con filtros algorítmicos contra falsos giros del mercado.

---

## 🚀 Características Clave

### 1. Filtro de Tendencia Multi-Timeframe (MTF) Dinámico
El indicador incorpora la potencia de `request.security` nativa de Pine Script v6. Permite traer de forma dinámica la tendencia desde un **Timeframe Institucional Superior** (por defecto de 1 Hora o 60 minutos) para pintar las zonas de compra fuerte de la "Montaña" únicamente si están apoyadas por la macro-tendencia. Esto reduce drásticamente el ruido y las señales falsas en marcos temporales menores.

### 2. Detección de Divergencias mediante Extremos Locales
A diferencia de los detectores de divergencia tradicionales que reaccionan ante el ruido de 1 sola barra (1-bar noise), este script utiliza un algoritmo quant de **Extremos Locales** con una ventana retrospectiva (`lookback`) de 20 barras:
*   🔴 **Agotamiento Alcista (X Roja)**: Ocurre cuando el precio hace un nuevo máximo local de 20 velas, pero el volumen institucional (Mano Fuerte) ha colapsado más del 50% de su propio pico del periodo y el momentum de la tendencia (RSI/Montaña) ha decaído más del 20%. Esto evita dar falsos avisos en subidas parabólicas sanas.
*   🟢 **Agotamiento Bajista (Triángulo Verde ▲)**: Ocurre cuando el precio marca un nuevo mínimo de 20 velas, pero la presión de venta institucional disminuye en más de un 50% y la caída de la tendencia se frena por completo.

### 3. Patrones Visuales Clásicos Incorporados
*   ⚫ **Primavera (Círculo Celeste en Base)**: Detección de acumulación. La Mano Fuerte se posiciona compradora mientras la Mano Débil (retail) vende con pánico.
*   ⭐ **Abrazo (Estrella Amarilla en Techo)**: Compra institucional extrema que supera la tendencia de la montaña.

### 4. Logging de Grado Profesional
Utiliza la característica nativa de depuración `log.warning` and `log.info` de Pine Script v6, enviando mensajes estructurados con precios exactos al panel de *Pine Logs* al detectarse los agotamientos para facilitar el backtesting manual y la auditoría del script.

---

## 🛠️ Parámetros de Configuración

| Parámetro | Valor por Defecto | Descripción |
| :--- | :--- | :--- |
| **Fuente de Datos** | `hlc3` | Base de cálculo para los indicadores de flujo y momento. |
| **Periodo Vigía** | `15` | Longitud del RSI que compone el núcleo de la tendencia. |
| **Suavizado Tendencia** | `20` | Longitud del suavizado EMA para la "Montaña". |
| **Mano Fuerte (MFI)** | `14` | Longitud del Money Flow Index para rastrear instituciones. |
| **Mano Débil (Stoch)** | `14` | Longitud del Estocástico para rastrear inversores minoristas. |
| **Timeframe Institucional** | `60` (1H) | Marco temporal superior de referencia para filtrar el ruido. |

---

## 📈 Interpretación de Señales

1.  **Compra Anticipada (Giro al Alza)**: Aparición de un triángulo verde (`▲`) en la parte inferior del oscilador. Indica que el precio está cayendo por inercia pero las instituciones están acumulando fuertemente en secreto.
2.  **Venta Anticipada (Giro a la Baja)**: Aparición de una `X` roja en el techo del oscilador. Indica agotamiento del movimiento alcista; el precio sube con debilidad institucional.
3.  **Compra Confirmada (Clásica)**: Cruce de la línea de la Montaña por encima de su línea de señal roja.
4.  **Venta Confirmada (Clásica)**: Cruce de la línea de la Montaña por debajo de su línea de señal roja.

---

## 📝 Requisitos de Sistema
*   Plataforma: **TradingView**
*   Compilador: **Pine Editor (v6)**

Desarrollado bajo principios de robustez y control estadístico en el entorno de pair-programming de **Antigravity**.
