**Problem Description**

The multi-knapsack problem is a generalization of the classical knapsack problem.

We are given:

-A set of items, each with a value and weights across multiple dimensions 

-Several knapsacks, each with limited capacity per dimension

The goal is to assign each item to at most one knapsack (or leave it unassigned) in order to maximize the total value of the packed items, while respecting all capacity constraints in every dimension.

------

**Results Summary**

-For Problem 1 (20 items, 3 knapsacks, 2 dimensions), the algorithm achieved a total value of **1 065**.
All items were placed optimally across the three knapsacks, with no remaining unassigned items.

-For Problem 2 (100 items, 10 knapsacks, 10 dimensions), the solver reached a total value of **52 620**.
The slower cooling schedule (cooling_rate = 0.998) enhanced convergence stability, and once again all items were successfully packed while maintaining feasibility.

-For Problem 3, a large-scale test involving 5 000 items, 100 knapsacks, and 100 dimensions, the algorithm remained stable and efficient, reaching a total value of **2 490 698**.
Even at this scale, the SA procedure managed to assign every single item to a knapsack without exceeding any capacity limits.

------

**Main Implementation Choices**

1. Choice of Simulated Annealing

In line with the methodology discussed in the course, we chose Simulated Annealing (SA) as the optimization framework.
This choice allows balancing exploration and exploitation through probabilistic acceptance of non-improving moves, helping the search escape local optima and converge toward near-optimal solutions.
Simulated Annealing was implemented in its classical form, with:
A gradually decreasing temperature that controls acceptance probability,
A simple neighborhood operator (tweak) that slightly perturbs the current solution,
And an exponential cooling schedule ensuring convergence over time.
This approach aligns with course guidelines that emphasize using metaheuristics capable of adapting to complex combinatorial problems without requiring exhaustive enumeration.




2. Unified and Parameterized Design

Instead of building separate algorithms for each problem size, a single general-purpose solver was designed.
The same function, simulated_annealing_knapsack(), handles all configurations (small, medium, and large instances) by automatically adapting to:
The number of knapsacks,
The number of items,
The number of dimensions.
Only hyperparameters (iterations, temperature, cooling rate) are adjusted depending on the problem scale.
This ensures flexibility, scalability, and comparability across experiments.



3. Cost Function

The objective function returns a negative total value (since the algorithm minimizes)
plus a penalty term proportional to constraint violations.
A fixed penalty factor (penalty_factor = 10.0) was chosen to discourage infeasible solutions while preserving exploration in borderline cases.



4. Neighborhood Operator (Tweak)

The neighborhood generation strategy randomly reassigns a fraction of items to different knapsacks or to “not taken” (–1).
The tweak strength is reduced over time, allowing:
Broad exploration in the early phase,
Fine local optimization in later iterations.



5. Cooling Schedule

The temperature update rule temp *= cooling_rate was chosen for simplicity and stability.
A slower decay (cooling rate closer to 1.0) provides more extensive exploration for larger problems, which aligns with the heuristic guidelines presented in class.

