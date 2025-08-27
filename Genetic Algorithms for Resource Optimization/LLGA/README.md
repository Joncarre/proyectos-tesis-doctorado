# LLGA: Lower-Level Genetic Algorithm for Resource Optimization

This Java project implements the **Lower-Level Genetic Algorithm (LLGA)**.

> [!NOTE]
> LLGA is designed to solve resource optimization problems, particularly aiming to maximize outcomes based on specific social welfare functions. It's utilized across numerous tests to identify optimal values in diverse resource allocation scenarios.

## Algorithm Configuration

> [!IMPORTANT]
> The core configuration involves evolving the population over **5,000 iterations**. The primary goal during this process is to maximize the fitness values calculated in each evolutionary cycle.

## Genetic Algorithm Phases

El sistema está compuesto por las siguientes clases principales:

### 1. `Main.java`
- **Propósito**: Punto de entrada del programa
- **Funcionalidad**: Instancia la clase `Engine` y maneja las excepciones principales

### 2. `Engine.java`
- **Propósito**: Controlador principal del flujo de ejecución
- **Parámetros de configuración**:
  - `generations = 100,000`: Número máximo de generaciones
  - `popSize = 30`: Tamaño de la población
  - `numUsers = 8`: Número de agentes/usuarios
  - `pfcIterations = 20`: Iteraciones de preferencias
- **Funcionalidades**:
  - Lectura y generación de datos de preferencias
  - Control del bucle principal de evolución
  - Gestión de iteraciones promedio para obtener resultados robustos

### 3. `Individual.java`
- **Propósito**: Representa una solución candidata (individuo)
- **Estructura**:
  - `genes[]`: Array de enteros binarios (0 o 1) de longitud 25
  - `fitness`: Valor de aptitud del individuo
- **Funcionalidades**:
  - Generación aleatoria de individuos
  - Lectura/escritura desde/hacia archivos
  - Gestión de genes y cálculo de fitness

### 4. `Population.java`
- **Propósito**: Maneja una colección de individuos
- **Funcionalidades**:
  - Creación de poblaciones iniciales
  - Identificación del individuo más apto
  - Gestión de individuos en la población

### 5. `FitnessCalc.java`
- **Propósito**: Calculadora de fitness y gestión de preferencias
- **Componentes principales**:
  - `M_preferences`: Preferencias modificadas de los agentes
  - `O_preferences`: Preferencias originales (agente 0)
  - **Función de fitness**: Implementa el criterio **maximin** (maximizar el mínimo)
- **Funcionalidades**:
  - Lectura/escritura de preferencias desde/hacia archivos
  - Cálculo del fitness basado en el principio igualitario
  - Gestión de variaciones en las preferencias

### 6. `Algorithm.java`
- **Propósito**: Implementa los operadores genéticos avanzados
- **Características destacadas**:

#### Parámetros Adaptativos:
- **Tasa de mutación dinámica**: Comienza en 3% y decrece gradualmente
- **Elitismo**: Preserva los 3 mejores individuos
- **Presión de selección**: 1.8 (balanceada)

#### Mecanismos Avanzados:

1. **Control de Estancamiento**:
   - Detección automática de convergencia prematura
   - Incremento de mutación cuando se detecta estancamiento
   - Contador de generaciones sin mejora

2. **Sistema de Catástrofes**:
   - Cada 3,000 generaciones se produce una "catástrofe" parcial
   - Mantiene la élite pero perturba el resto de la población
   - Ayuda a escapar de óptimos locales

3. **Fases de Exploración**:
   - **Fase inicial** (1,000 generaciones): Alta exploración con mutación incrementada
   - **Fase normal**: Mutación decreciente con recocido simulado

4. **Operadores de Cruce Múltiples**:
   - **Cruce uniforme** (60%): Intercambio gen a gen
   - **Cruce de un punto** (30%): División en un punto
   - **Cruce de dos puntos** (10%): División en dos puntos

5. **Mutaciones Especializadas**:
   - **Mutación tradicional**: Cambio de un gen (70%)
   - **Mutación de genes consecutivos** (20%)
   - **Inversión de segmentos** (10%)
   - **Mutación disruptiva**: Para alta exploración inicial

6. **Selección Híbrida**:
   - Combinación de selección determinista y probabilística
   - Sistema de torneo mejorado
   - Selección ponderada con ranking exponencial

### 7. `WeightedRandomSelect.java`
- **Propósito**: Implementa selección ponderada por fitness
- **Funcionalidad**: Permite selección probabilística donde individuos con mejor fitness tienen mayor probabilidad de ser elegidos

## Algoritmo Principal

### Flujo de Ejecución:

1. **Inicialización**:
   - Carga de preferencias de agentes desde archivos
   - Creación de población inicial aleatoria

2. **Bucle Evolutivo** (100,000 generaciones):
   - Evaluación de fitness de todos los individuos
   - Ordenamiento por fitness (bubble sort)
   - Detección de estancamiento
   - Aplicación de catástrofes si es necesario
   - Selección de padres con presión adaptativa
   - Cruce con operadores múltiples
   - Mutación con esquemas adaptativos
   - Preservación de élite
   - Reemplazo generacional

3. **Optimización Continua**:
   - Ajuste dinámico de parámetros
   - Monitoreo de progreso
   - Escape de óptimos locales

## Función de Fitness (Criterio Igualitario)

La función de fitness implementa el **principio maximin**:

```java
// Para cada individuo, calcula la satisfacción de cada agente
double[] assignments = new double[numUsers];
for (int i = 0; i < geneLength; i++) {
    for(int j = 0; j < numUsers; j++)
        assignments[j] += individual.getGene(i) * preferences[j][i]; 
}
// Retorna el mínimo (enfoque igualitario)
return getMinValue(assignments);
```

Esta función busca **maximizar el bienestar del agente menos satisfecho**, garantizando soluciones más igualitarias.

## Características Técnicas Avanzadas

### 1. **Recocido Simulado**
- Factor de "temperatura" que decrece con las generaciones
- Mutación más agresiva al inicio, más refinada al final

### 2. **Diversidad Poblacional**
- Monitoreo de desviación estándar en la élite
- Incremento automático de mutación cuando hay poca diversidad

### 3. **Elitismo Inteligente**
- Preservación de los mejores individuos
- Verificación de que la élite se mantiene tras la evolución

### 4. **Parámetros Auto-adaptativos**
- Tasa de mutación que evoluciona con el tiempo
- Presión de selección que se ajusta según el progreso
- Transición de exploración a explotación

## Resultados y Salida

El sistema proporciona:
- Mejor fitness encontrado en cada iteración
- Representación binaria de la mejor solución
- Análisis de preferencias originales vs. modificadas
- Monitoreo del progreso evolutivo

## Aplicación

Este algoritmo genético está específicamente diseñado para:
- **Negociación automática** entre múltiples partes
- **Distribución igualitaria** de recursos
- **Toma de decisiones colectivas** con criterio de equidad
- **Resolución de conflictos** con enfoque maximin

## Ventajas del Diseño

1. **Robustez**: Múltiples mecanismos anti-estancamiento
2. **Adaptabilidad**: Parámetros que se ajustan automáticamente  
3. **Eficiencia**: Balance óptimo entre exploración y explotación
4. **Escalabilidad**: Diseño modular que permite extensiones
5. **Equidad**: Enfoque específico en soluciones igualitarias

Este algoritmo genético representa una implementación sofisticada que combina técnicas clásicas con innovaciones modernas para resolver problemas complejos de optimización multi-objetivo con restricciones de equidad.
