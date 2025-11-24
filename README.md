# Algorithm

Approximation Algorithms for Makespan Scheduling  
Md Rokonuzzaman Reza, Souhardya Saha Dip 

Overview of the Minimum Makespan Scheduling Problem

1. Problem Definition

The Minimum Makespan Scheduling Problem is a classic optimization problem in computer science and operations research. The objective is to assign a set of $n$ jobs, each with a specific processing time (or "length"), to a set of $m$ machines such that the maximum load on any single machine is minimized.

Formally, given a set of machines $M = \{1, \dots, m\}$ and a set of jobs $J = \{1, \dots, n\}$ where job $j$ requires processing time $p_{ij}$ on machine $i$, we seek an assignment that minimizes the makespan $C_{max}$:


$$C_{max} = \max_{i \in M} \sum_{j \in J_i} p_{ij}$$


where $J_i$ is the subset of jobs assigned to machine $i$.

A common intuitive analogy for this problem is "Block Stacking," where jobs are viewed as blocks of varying heights, and machines are stacks. The goal is to distribute the blocks among $m$ stacks so that the height of the tallest stack is as low as possible.
