# MTF Lorentzian Key S/R Level Detector

## ¿Qué hace este script?
Este script es un indicador avanzado para TradingView (escrito en Pine Script v6) que utiliza aprendizaje automático (Machine Learning), específicamente el algoritmo **K-Means Clustering** combinado con una medida de **Distancia Lorentziana**, para detectar y dibujar automáticamente niveles institucionales clave de Soporte y Resistencia.

Se destaca por incluir:
- **Análisis Multi-Timeframe (MTF) a prueba de fallos:** Permite recopilar la acción de precio real en un marco temporal mayor (por ejemplo, Diario) y verla reflejada con total exactitud en tu gráfico de temporalidad menor (por ejemplo, 15m), sin problemas de pérdida de contexto histórico (repainting o vanishing levels).
- **Selección Automática (Elbow Method):** El algoritmo determina automáticamente cuántas líneas (K) debe dibujar buscando el punto dulce matemático (Sharpness Heuristic) para evitar la saturación visual y el ruido en el gráfico.
- **Puntaje de Fuerza Dinámico:** Evalúa cada soporte/resistencia con un puntaje de fortaleza (0-100) basado en su concentración, desviación estándar y cantidad de toques, ajustando visualmente el grosor de la línea en consecuencia.

---

## Interpretación de las Etiquetas en el Gráfico

Junto a cada línea dibujada en tu gráfico, verás una pequeña caja de texto que contiene información clave (ej: `R2 2423.79 | Score: 50/100`). Así se lee:

- **S o R (Soporte o Resistencia):** 
  - Las etiquetas marcadas con **R (Rojo)** indican *Resistencias* (niveles que están por encima del precio actual). 
  - Las marcadas con **S (Verde)** indican *Soportes* (niveles que están por debajo del precio actual). El color cambiará dinámicamente si el precio las cruza.
- **Score (Puntaje de Fuerza 0-100):** Un puntaje de convicción matemática basado en la "estrechez" y densidad de la consolidación de precios. Un nivel cercano a 100 representa un muro institucional altamente respetado y comprimido. Un nivel por debajo de 50 indica una zona más dispersa o débil. (Las líneas con más de 80 puntos se dibujarán mucho más gruesas).
- **Touches (Toques):** El número de veces que el precio máximo/mínimo ha interactuado físicamente o rebotado en esta franja de precios durante las últimas 250 barras (o tu valor de "Sample Size").
- **% (Densidad de la Muestra):** Indica el porcentaje de tiempo que el precio de la moneda ha estado oscilando exactamente dentro de los límites matemáticos de este clúster durante toda la ventana histórica analizada. 

---

## Interpretación del "Cerebro Matemático" (Tabla WCSS)

El indicador despliega una tabla de diagnóstico de Inteligencia Artificial que te permite ver "qué está pensando el algoritmo" y cómo está decidiendo cuántos niveles debe trazar en tu pantalla. 

### Desglose de las Columnas:
* **K (Número de Clústers):** Representa el número total de líneas (soportes + resistencias) que el algoritmo está simulando. El sistema siempre compite probando trazar 2, 3, 4, 5 y hasta 6 líneas.
* **WCSS (Within-Cluster Sum of Squares):** Es la medida del "Error" o "Dispersión". Mide qué tan lejos o fuera de lugar quedan las velas reales respecto a las líneas simuladas. *Regla general:* A medida que agregas más líneas (K mayor), el error siempre baja. Sin embargo, llenar el gráfico de líneas (K=6) significa trazar mucho "ruido" inútil.
* **Sharpness (Nitidez o "El Codo"):** Esta es la "magia" del indicador (la segunda derivada de la curva de error). En lugar de buscar el error nulo, busca el momento exacto en donde la caída del WCSS frena de golpe. El nivel K que tenga el **Sharpness más alto** representa el punto de eficiencia perfecto.
* **Selected:** Una marca (✓) que indica qué número de líneas (K) ganó el torneo matemático y se acaba de dibujar en tu pantalla.

### ¿Cómo interpretar la tabla para la Toma de Decisiones?

Puedes utilizar este diagnóstico como un **termómetro oculto de la Estructura de Mercado**:

1. **Reconocer el Entorno (Tendencia vs Rango):**
   - **K ganador bajo (K=2 o K=3):** El mercado está extremadamente ordenado. Hay zonas de acumulación amplias y limpias separadas por el vacío. Este es un entorno perfecto para operar *Range Trading* y rebotes en los bordes.
   - **K ganador alto (K=5 o K=6):** El mercado está fragmentado, errático ("choppy") o subiendo por micro-escaleras. Aquí hay alto riesgo de ser cazado (stop-hunts). Hay que ser precavido operando rebotes.

2. **Fiabilidad Absoluta del Nivel:**
   - Si el nivel elegido gana por un **margen de Sharpness aplastante** (ej: gana K=3 con un Sharpness de 800, mientras K=4 tiene 50), la estructura actual del mercado es innegable. Operar esos niveles es altamente recomendado, las instituciones los están mirando.
   - Si el margen es apretado (ej: gana K=4 con 388 vs K=5 con 365), la red neuronal del algoritmo duda de si hay una quinta zona oculta. El mercado se está reestructurando.

3. **Modo Manual:** El algoritmo se autoajusta. Sin embargo, si detectas en la tabla que el K=4 fue el ganador y las líneas intermedias de tu gráfico te estorban para hacer un trade direccional a largo plazo, puedes abrir las Opciones del script, desmarcar "Auto K" y fijarlo manualmente en 2 o 3 para ver solo el suelo y techo macros. La tabla te da los datos para tomar esa decisión sin operar a ciegas.