# LLGA: Lower-Level Genetic Algorithm for Resource Optimization

This Java project implements the **Lower-Level Genetic Algorithm (LLGA)**.

> [!NOTE]
> LLGA is designed to solve resource optimization problems, particularly aiming to maximize outcomes based on specific social welfare functions. It's utilized across numerous tests to identify optimal values in diverse resource allocation scenarios.

## Algorithm Configuration

> [!IMPORTANT]
> The core configuration involves evolving the population over **5,000 iterations**. The primary goal during this process is to maximize the fitness values calculated in each evolutionary cycle.

## Genetic Algorithm Phases

The system is composed of the following main classes:

### 1. `Main.java`
- **Purpose**: Program entry point
- **Functionality**: Instantiates the `Engine` class and handles main exceptions

### 2. `Engine.java`
- **Purpose**: Main controller for execution flow
- **Configuration parameters**:
    - `generations = 100,000`: Maximum number of generations
    - `popSize = 30`: Population size
    - `numUsers = 8`: Number of agents/users
    - `pfcIterations = 20`: Preference iterations
-
  - Reading and generation of preference data
  - Control of the main evolution loop
  - Management of average iterations for robust results

### 3. `Individual.java`
- **Purpose**: Represents a candidate solution (individual)
- **Structure**:
  - `genes[]`: Array of binary integers (0 or 1) of length 25
  - `fitness`: Fitness value of the individual
- **Features**:
  - Random generation of individuals
  - Reading/writing from/to files
  - Gene management and fitness calculation

### 4. `Population.java`
- **Purpose**: Manages a collection of individuals
- **Features**:
  - Creation of initial populations
  - Identification of the fittest individual
  - Management of individuals in the population

### 5. `FitnessCalc.java`
- **Purpose**: Fitness calculator and preference management
- **Main components**:
  - `M_preferences`: Modified agent preferences
  - `O_preferences`: Original preferences (agent 0)
  - **Fitness function**: Implements the **maximin** criterion (maximize the minimum)
- **Features**:
  - Reading/writing preferences from/to files
  - Fitness calculation based on egalitarian principle
  - Management of preference variations

### 6. `Algorithm.java`
- **Purpose**: Implements advanced genetic operators
- **Key features**:

#### Adaptive Parameters:
- **Dynamic mutation rate**: Starts at 3% and gradually decreases
- **Elitism**: Preserves the top 3 individuals
- **Selection pressure**: 1.8 (balanced)

#### Advanced Mechanisms:

1. **Stagnation Control**:
   - Automatic detection of premature convergence
   - Increased mutation when stagnation is detected
   - Counter for generations without improvement

2. **Catastrophe System**:
   - Every 3,000 generations, a partial "catastrophe" occurs
   - Elite is preserved but the rest of the population is perturbed
   - Helps escape local optima

3. **Exploration Phases**:
   - **Initial phase** (1,000 generations): High exploration with increased mutation
   - **Normal phase**: Decreasing mutation with simulated annealing

4. **Multiple Crossover Operators**:
   - **Uniform crossover** (60%): Gene-by-gene exchange
   - **One-point crossover** (30%): Division at one point
   - **Two-point crossover** (10%): Division at two points

5. **Specialized Mutations**:
   - **Traditional mutation**: Single gene change (70%)
   - **Consecutive gene mutation** (20%)
   - **Segment inversion** (10%)
   - **Disruptive mutation**: For high initial exploration

6. **Hybrid Selection**:
   - Combination of deterministic and probabilistic selection
   - Enhanced tournament system
   - Weighted selection with exponential ranking

### 7. `WeightedRandomSelect.java`
- **Purpose**: Implements fitness-weighted selection
- **Functionality**: Enables probabilistic selection where individuals with better fitness have a higher chance of being chosen

## Main Algorithm

### Execution Flow:

1. **Initialization**:
   - Loading agent preferences from files
   - Creation of initial random population

2. **Evolutionary Loop** (100,000 generations):
   - Evaluation of fitness for all individuals
   - Sorting by fitness (bubble sort)
   - Stagnation detection
   - Application of catastrophes if necessary
   - Parent selection with adaptive pressure
   - Crossover with multiple operators
   - Mutation with adaptive schemes
   - Elite preservation
   - Generational replacement

3. **Continuous Optimization**:
   - Dynamic parameter adjustment
   - Progress monitoring
   - Escaping local optima

## Fitness Function (Egalitarian Criterion)

The fitness function implements the **maximin principle**:

```java
// For each individual, calculate the satisfaction of each agent
double[] assignments = new double[numUsers];
for (int i = 0; i < geneLength; i++) {
    for(int j = 0; j < numUsers; j++)
        assignments[j] += individual.getGene(i) * preferences[j][i]; 
}
// Return the minimum (egalitarian approach)
return getMinValue(assignments);
```

This function aims to **maximize the welfare of the least satisfied agent**, ensuring more egalitarian solutions.

## Advanced Technical Features

### 1. **Simulated Annealing**
- "Temperature" factor decreases with generations
- More aggressive mutation at the start, more refined at the end

### 2. **Population Diversity**
- Monitoring of standard deviation in the elite
- Automatic increase of mutation when diversity is low

### 3. **Intelligent Elitism**
- Preservation of the best individuals
- Verification that the elite is maintained after evolution

### 4. **Self-adaptive Parameters**
- Mutation rate evolves over time
- Selection pressure adjusts according to progress
- Transition from exploration to exploitation

## Results and Output

The system provides:
- Best fitness found in each iteration
- Binary representation of the best solution
- Analysis of original vs. modified preferences
- Monitoring of evolutionary progress

## Application

This genetic algorithm is specifically designed for:
- **Automated negotiation** between multiple parties
- **Egalitarian resource distribution**
- **Collective decision-making** with fairness criteria
- **Conflict resolution** with a maximin approach

## Design Advantages

1. **Robustness**: Multiple anti-stagnation mechanisms
2. **Adaptability**: Automatically adjusting parameters  
3. **Efficiency**: Optimal balance between exploration and exploitation
4. **Scalability**: Modular design allows extensions
5. **Fairness**: Specific focus on egalitarian solutions

This genetic algorithm represents a sophisticated implementation that combines classical techniques with modern innovations to solve complex multi-objective optimization problems with fairness constraints.
