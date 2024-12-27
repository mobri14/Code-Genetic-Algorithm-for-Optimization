# Code-Genetic-Algorithm-for-Optimization
Code: Genetic Algorithm for Optimization


```python
import random

# Genetic Algorithm settings
POPULATION_SIZE = 100  # Number of individuals in the population
GENERATIONS = 500  # Number of generations to evolve
TARGET_SUM = 200  # Target sum for optimization
NUMBERS = [random.randint(1, 50) for _ in range(20)]  # Random set of numbers

def fitness(individual):
    """Calculate the fitness score of an individual."""
    total = sum([n * g for n, g in zip(NUMBERS, individual)])
    return abs(TARGET_SUM - total)

def mutate(individual, mutation_rate=0.1):
    """Apply mutation to an individual."""
    return [gene if random.random() > mutation_rate else 1 - gene for gene in individual]

def crossover(parent1, parent2):
    """Perform crossover between two parents to produce offspring."""
    crossover_point = random.randint(1, len(parent1) - 1)
    child1 = parent1[:crossover_point] + parent2[crossover_point:]
    child2 = parent2[:crossover_point] + parent1[crossover_point:]
    return child1, child2

def select_population(population):
    """Select the best individuals from the population."""
    return sorted(population, key=lambda x: fitness(x))[:POPULATION_SIZE]

# Initialize the population with random binary chromosomes
population = [[random.randint(0, 1) for _ in NUMBERS] for _ in range(POPULATION_SIZE)]

# Start the evolutionary process
for generation in range(GENERATIONS):
    # Sort the population and select the best individuals
    population = select_population(population)
    new_population = population[:10]  # Preserve the top 10 individuals

    # Create new individuals through crossover and mutation
    while len(new_population) < POPULATION_SIZE:
        parent1, parent2 = random.sample(population, 2)
        child1, child2 = crossover(parent1, parent2)
        new_population.extend([mutate(child1), mutate(child2)])

    population = new_population

    # Display the best result of the current generation
    best_individual = population[0]
    best_fitness = fitness(best_individual)
    print(f"Generation {generation + 1}: Best Fitness = {best_fitness}")

    # Stop if the optimal solution is found
    if best_fitness == 0:
        break

# Display the final result
best_solution = population[0]
print("\nBest Solution Found:")
print(f"Numbers: {NUMBERS}")
print(f"Selection: {best_solution}")
print(f"Sum: {sum([n * g for n, g in zip(NUMBERS, best_solution)])}")
```
