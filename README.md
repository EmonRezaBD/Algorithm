# Algorithm

Approximation Algorithms for Makespan Scheduling  
Md Rokonuzzaman Reza, Souhardya Saha Dip 

1. Problem Overview
The objective is to assign a set of $n$ jobs, each with a specific processing time, to a set of $m$ machines such that the maximum load on any single machine is minimized.
Formally, given a set of machines $M = \{1, \dots, m\}$ and a set of jobs $J = \{1, \dots, n\}$ where job $j$ requires processing time $p_{ij}$ on machine $i$, we seek an assignment that minimizes the makespan $C_{max}$:

$$C_{max} = \max_{i \in M} \sum_{j \in J_i} p_{ij}$$; where $J_i$ is the subset of jobs assigned to machine $i$.

We can make an intuitive analogy for this problem with "Block Stacking," where jobs are viewed as blocks of varying heights, and machines are stacks. The goal is to distribute the blocks among $m$ stacks so that the height of the tallest stack is as low as possible.

This problem is NP-hard, meaning that no polynomial-time algorithm is known to find the exact optimal solution for all inputs. Consequently, this minipaper focuses on Approximation Algorithms that guarantee a solution within a specific factor (e.g., within 2 times or 1.5 times) of the optimal makespan. We will prove total of 03 algorithms (i)Greedy Job Scheduling (ii)Sorted Greedy Job Scheduling, and (iii)Polynomial Time Approximation Scheme (PTAS) 

(i)Greedy Job Scheduling:
This algorithm called Greedy because it makes the best immediate choice at every single step. When the algorithm picks a machine for a job, it looks for the "cheapest" option available right now (the machine with the lowest load). It does not look ahead.

Procedure:

-Initialize $m$ machines with 0 load.

-Process jobs one by one in arbitrary order.

-Assign the current job to the machine with the minimum current load.

2. Theorem 1

Statement: The Greedy Job Scheduling algorithm is a 2-approximation means it is at most twice the optimal makespan.

$$ALG \le 2 \cdot OPT$$

3. Proof

Lemma 1: Lower Bounds on Optimal

The optimal makespan ($OPT$) is limited by two physical constraints:

Max Job Size: $OPT$ must be at least as large as the largest single job ($p_{max}$).


$$OPT \ge p_{max}$$

Average Load: $OPT$ cannot be less than the average load of all jobs distributed perfectly evenly.


$$OPT \ge \frac{\text{Total Work}}{m}$$

Now, consider the machine that finishes last. Its load is the makespan ($ALG$).
Let $j_{last}$ be the last job added to this machine, with size $p_{last}$.
Let $L$ be the load of this machine before $j_{last}$ was added.

We can express the final makespan as:


$$ALG = L + p_{last}$$


Moreover, when the algorithm assigned job $j_{last}$, it chose this machine because it had the minimum load. Therefore, all other machines had a load of at least $L$.

Total Work $\ge m \cdot L$

Average Work $\ge L$

From Lemma 1, we know $OPT \ge \text{Average Work}$, so:


$$OPT \ge L$$


Finally, substituting our bounds into the makespan equation:

$ALG = L + p_{last}$

Since $L \le OPT$ and $p_{last} \le p_{max} \le OPT$:


$$ALG \le OPT + OPT$$

$$ALG \le 2 \cdot OPT$$

