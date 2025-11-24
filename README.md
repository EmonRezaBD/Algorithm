# Algorithm

Approximation Algorithms for Makespan Scheduling  
Md Rokonuzzaman Reza, Souhardya Saha Dip 

1. Problem Overview
The objective is to assign a set of $n$ jobs, each with a specific processing time, to a set of $m$ machines such that the maximum load on any single machine is minimized.
Formally, given a set of machines $M = \{1, \dots, m\}$ and a set of jobs $J = \{1, \dots, n\}$ where job $j$ requires processing time $p_{ij}$ on machine $i$, we seek an assignment that minimizes the makespan $C_{max}$:

$$C_{max} = \max_{i \in M} \sum_{j \in J_i} p_{ij}$$; where $J_i$ is the subset of jobs assigned to machine $i$.

We can make an intuitive analogy for this problem with "Block Stacking," where jobs are viewed as blocks of varying heights, and machines are stacks. The goal is to distribute the blocks among $m$ stacks so that the height of the tallest stack is as low as possible.

This problem is NP-hard, meaning that no polynomial-time algorithm is known to find the exact optimal solution for all inputs. Consequently, this minipaper focuses on Approximation Algorithms that guarantee a solution within a specific factor (e.g., within 2 times or 1.5 times) of the optimal makespan. We will prove total of 03 algorithms (i)Greedy Job Scheduling (ii)Sorted Greedy Job Scheduling, and (iii)Polynomial Time Approximation Scheme (PTAS) 

