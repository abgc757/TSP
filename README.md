# Algoritmo de Recocido Simulado

## 🔍 **Definición**
Método de **optimización estocástica** inspirado en el proceso físico del **recocido de metales**, donde se calienta y enfría un material controladamente para minimizar sus defectos. En computación, busca el **óptimo global** de una función permitiendo **soluciones temporales peores** para evitar quedar atrapado en óptimos locales.

## 🔄 **Analogía Física**
- **Temperatura alta**: Mayor probabilidad de aceptar soluciones peores (exploración).
- **Temperatura baja**: Menor tolerancia a soluciones peores (explotación).

## ⚙️ **Componentes Clave**
1. **Solución Actual**: Punto inicial en el espacio de búsqueda.
2. **Solución Candidata**: Modificación aleatoria de la solución actual.
3. **Función de Costo**: Evalúa la calidad de una solución (`E`).
4. **Temperatura (`T`)**: Parámetro que controla la exploración.
5. **Schedule de Enfriamiento**: Ritmo de disminución de `T` (ej: `T = α·T`, `0 < α < 1`).

## 📝 **Pasos del Algoritmo**
1. **Inicializar**: Temperatura alta y solución aleatoria.
2. **Generar Vecino**: Crear solución candidata cercana.
3. **Evaluar**: Calcular ΔE = Costo(nuevo) - Costo(actual).
4. **Aceptar/Rechazar**:
   - Si ΔE < 0 → Aceptar siempre.
   - Si ΔE ≥ 0 → Aceptar con probabilidad:  
     ![P = e^{-ΔE / T}](https://latex.codecogs.com/png.latex?P%20%3D%20e%5E%7B-%5CDelta%20E%20/%20T%7D)
5. **Enfriar**: Reducir `T` según el schedule.
6. **Repetir** hasta convergencia o temperatura mínima.

## 📉 **Schedule de Enfriamiento**
- **Geométrico**: `T = T₀ * α^k` (α ≈ 0.85-0.99).
- **Lineal**: `T = T₀ - k·β`.
- **Personalizado**: Basado en el problema.

## ✅ **Ventajas**
- Escapa de **óptimos locales**.
- Útil en problemas **complejos/no lineales**.
- Fácil de implementar.

## ❌ **Desventajas**
- Sensible a la elección de parámetros (`T₀`, α, etc.).
- Costo computacional en problemas muy grandes.

## 🎯 **Aplicaciones Comunes**
- Problema del Viajante (TSP).
- Optimización de rutas logísticas.
- Diseño de circuitos electrónicos.
- Scheduling de tareas.

## 📌 **Consejos Prácticos**
- Ajusta `T₀` para que ~50% de soluciones peores sean aceptadas inicialmente.
- Usa enfriamiento lento para mejores resultados.
- Combina con búsquedas locales para refinamiento final.

# Ejemplo: Optimización de Rutas con Recocido Simulado

Utilizando el dataset [**Traveling Salesman Problem (TSP)**](https://www.kaggle.com/datasets/mexwell/traveling-salesman-problem/data?select=medium.csv), que incluye cuatro conjuntos de coordenadas geográficas simuladas, se implementó un algoritmo para encontrar la **ruta más corta** que visite todos los puntos (ciudades) sin repeticiones.

El Notebook [**TSP.ipynb**](TSP.ipynb) aplica el **algoritmo de recocido simulado**, demostrando su funcionamiento en tres etapas clave:
1. **Fase de Exploración Inicial**: Genera soluciones subóptimas con alta tolerancia a rutas largas.
2. **Enfriamiento Progresivo**: Reduce sistemáticamente la temperatura del sistema, limitando gradualmente la aceptación de soluciones peores.
3. **Convergencia**: Alcanza una configuración estable cercana al óptimo global tras múltiples iteraciones.

Este comportamiento se ilustra en el siguiente gráfico de evolución, donde se observa:
- Alta variabilidad en las distancias iniciales (exploración aleatoria)
- Reducción consistente de la longitud de ruta conforme disminuye la temperatura
- Estabilización en un valor mínimo (explotación del óptimo identificado)

![Evolución de la distancia durante el recocido simulado](images\evolucion_algoritmo.png)  
*Progreso típico del algoritmo: de soluciones caóticas a rutas optimizadas.*

## Solución de Ruta Óptima

## 📌 **Estructura de la Solución**
La ruta más corta encontrada se almacena en el diccionario bajo la clave:  
`resultados['ruta_optima'] = [índice_ciudad1, índice_ciudad2, ..., índice_ciudadN]`  

Esta lista representa el **orden óptimo** para visitar todas las ciudades, garantizando la distancia mínima total.

---

## 📍 **Características Clave**
1. **Ciclo Cerrado**: 
   - La ruta forma un bucle completo (la última ciudad se conecta con la primera).
   - Ejemplo: `[0, 3, 1, 2, 0]` (implícito en la visualización).

2. **Punto de Inicio Arbitrario**:
   - El ciclo puede comenzar en cualquier ciudad (rotación circular equivalente).  
   - *Ejemplo:* `[0, 3, 1, 2]` ≡ `[3, 1, 2, 0]` en distancia total.

---

## 📊 **Interpretación del Gráfico**
![Gráfico de Ruta Óptima](images\ruta_optima.png)
- **Puntos Rojos**: Ciudades (coordenadas geográficas).
- **Línea Azul Punteada**: Trayectoria óptima (`'b--'` en matplotlib).
- **Dirección**: Flecha implícita en el orden de la lista (sigue el sentido del arreglo `ruta_optima`).

---

## 🛠️ **Cómo se Genera**
```python
# Cargar librerias
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# Carga coordenadas en  un DataFrame a partir de un archivo csv. 
df = pd.read_csv('path/file.csv')
coordenadas = df[['x', 'y']].values    # Carga las coordenadas en un array de numpy

# Calcula distancia la ruta óptima, determina la distancia mínima y devuelve la evolución
# Hacer uso de las funciones que encontrará en el notebook TSP.ipynb
ruta_optima, distancia_optima, evolucion = recocido_simulado(coordenadas)

# Generar los gráficos necesarios
plt.figure(figsize=(8, 8))
plt.scatter(coordenadas[:, 0], coordenadas[:, 1], color='red', label..............