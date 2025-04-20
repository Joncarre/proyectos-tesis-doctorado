# LLGA: Lower-Level Genetic Algorithm for Resource Optimization 🧐

This Java project implements the **Lower-Level Genetic Algorithm (LLGA)**.

> [!NOTE]
> LLGA is designed to solve resource optimization problems, particularly aiming to maximize outcomes based on specific social welfare functions. It's utilized across numerous tests to identify optimal values in diverse resource allocation scenarios.

## Algorithm Configuration

> [!IMPORTANT]
> The core configuration involves evolving the population over **5,000 iterations**. The primary goal during this process is to maximize the fitness values calculated in each evolutionary cycle.

## Genetic Algorithm Phases

Each iteration within the LLGA follows the standard phases characteristic of genetic algorithms:

1.  **Initialization**: Setting up the initial population.
2.  **Fitness Evaluation**: Calculating the fitness of each individual in the population.
3.  **Selection**: Choosing elite individuals and parents for the next generation.
4.  **Crossover**: Combining genetic material from parents to create offspring.
5.  **Mutation**: Introducing random changes into the offspring's genetic material.
6.  **Fitness Re-evaluation**: Assessing the fitness of the newly created generation.
