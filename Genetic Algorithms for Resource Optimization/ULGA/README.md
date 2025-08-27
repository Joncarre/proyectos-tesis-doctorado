# ULGA: Upper-Level Genetic Algorithm for Strategic Preference Misrepresentation 🤖

This project features the **Upper-Level Genetic Algorithm (ULGA)**, implemented in Java.

> [!TIP]
> The main objective of ULGA is to discover an optimal set of preferences (which might be strategically false) for a specific agent. The idea is that when the Lower-Level Genetic Algorithm (LLGA) performs resource allocation using these potentially false preferences, the outcome maximizes the *actual* utility for this "lying" agent, considering their *true* underlying preferences.

## Key Differences from LLGA

> [!CAUTION]
> Unlike LLGA where genes typically represent discrete entities (like agents, often encoded as integers), the genes within ULGA individuals are **real numbers**. These real numbers directly represent the preferences an agent declares for each available resource.

## Evolutionary Goal

The population within ULGA evolves iteratively. The driving force behind this evolution is the continuous effort to improve the utility gained by the agent through strategic misrepresentation of their preferences.
