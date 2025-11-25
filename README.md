<center>

# Approximation Algorithms for Makespan Scheduling
**Md Rokonuzzaman Reza, Souhardya Saha Dip**

</center>

---

## **Problem Overview**

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

Therefore, it is proved that the greedy approach outputs a solution with makespan at most 2 times the optimum.

Time & Space Complexity:

(ii)Sorted Greedy Job Scheduling:
The basic Greedy algorithm can fail when a huge job appears at the very end of the list, creating a sudden spike in height. By processing the biggest jobs first:
The large "rocks" form a solid foundation at the bottom. The smaller "sand" (smaller jobs) fills in the gaps at the top, evening out the stacks. This prevents the "unlucky" worst-case scenario where a giant block lands on top of an already high stack.

Procedure:

-Sort all jobs (blocks) from largest to smallest.

-Initialize $m$ machines with 0 load.

-Process the sorted jobs one by one.

-Assign the current job to the machine with the minimum current load.

3. Theorem 2

Statement: The Sorted Greedy Job Scheduling algorithm is a 1.5-approximation means it is at most 1.5 times the optimal makespan.

$$ALG \le 1.5 \cdot OPT$$

4. Proof

Lemma 1: The Basics (Same as Algorithm 1)

Let the machine that finishes last define our makespan ($ALG$).
Let $j_{last}$ be the last job added to this machine, with size $p_{last}$.
Let $L$ be the load of this machine before $j_{last}$ was added.

We know from the previous proof that:

$ALG = L + p_{last}$

$OPT \ge L$ (Because this was the shortest stack, so all others are taller).

Step 2: The Sorting Advantage

Because we sorted the jobs from largest to smallest, every job processed before $j_{last}$ must be at least as big as $j_{last}$.

Since $L$ is the height of the stack under $j_{last}$, there must be at least one job in that stack (unless $L=0$, in which case the solution is optimal).
Therefore, every machine has at least one job on it that is $\ge p_{last}$.

Step 3: The Pigeonhole Principle

Count the "big" jobs involved so far:

We have $m$ machines.

Each machine has at least 1 job $\ge p_{last}$ already on it.

We just added $j_{last}$ (which is also a job of size $p_{last}$).

Total "big" jobs = $m + 1$.

If you have $m$ machines (buckets) and $m+1$ big jobs (items), the Pigeonhole Principle says that at least one machine must hold 2 big jobs in the optimal solution.

Step 4: Bounding the Optimal

Since the optimal solution ($OPT$) must fit these $m+1$ jobs onto $m$ machines, one machine must handle two jobs of size $p_{last}$ (or larger).
Therefore:


$$OPT \ge 2 \cdot p_{last}$$


Or, dividing by 2:


$$p_{last} \le \frac{1}{2} \cdot OPT$$


Finally, Now we substitute this into our original equation ($ALG = L + p_{last}$):

We know $L \le OPT$.

We just proved $p_{last} \le 0.5 \cdot OPT$.

$$ALG \le OPT + 0.5 \cdot OPT$$

$$ALG \le 1.5 \cdot OPT$$

Therefore, it is proved that the sorted greedy approach outputs a solution with makespan at most 1.5 times the optimum.

Time & Space Complexity:


(iii)Polynomial Time Approximation Scheme (PTAS):
Unlike the Greedy algorithms which give a fixed guarantee (like 2x or 1.5x), a PTAS allows us to choose how close we want to be to the perfect solution. We can pick an error parameter $\epsilon$ (epsilon). The algorithm guarantees a solution within $(1+\epsilon)$ of the optimal.

The algorithm guarantees a solution within $(1+\epsilon)$ of the optimal. For a rough answer fast, choose a large $\epsilon$ whereas for a very precise answer, choose a tiny $\epsilon$. Moreover, the algorithm combines three techniques: Binary Search, Rounding, and Dynamic Programming.


Part A: The Geometric Rounding Logic

Goal: Prove that rounding doesn't change the answer by more than a factor of $(1+\epsilon)$.

The Math:
We define a set of interval buckets based on powers of $(1+\epsilon)$:
$[1, 1+\epsilon), [(1+\epsilon), (1+\epsilon)^2), \dots$

If a job has a real size $P_{real}$ that falls into the interval $[X, X(1+\epsilon))$, we simplify it by rounding it down to $X$.

Rounded Size: $P_{rounded} = X$

Real Size: $P_{real} < X(1+\epsilon)$

Therefore, the relationship between the real and rounded size is:


$$P_{real} < P_{rounded} \cdot (1+\epsilon)$$

Conclusion: If we find a valid schedule using the rounded sizes that fits within time $T$, converting those blocks back to their real sizes will expand the total time by at most that same factor:


$$\text{Real Time} \le T \cdot (1+\epsilon)$$

Part B: The Small Jobs Logic

Goal: Prove that adding the "small" jobs (which we ignored earlier) doesn't break the schedule too badly.

The Math:
A "small" job is defined as any job with size $p < \epsilon \cdot T$.
When we greedily add these small jobs to the schedule, two things can happen:

It fits: The job fits into an existing gap below time $T$. (No increase in makespan).

It overflows: The job forces a machine to go above time $T$.

If it overflows, the machine was previously at some height $H \le T$, and we added a small job $p$. The new height is $H + p$.
Since $p < \epsilon \cdot T$, the new height is:


$$\text{New Height} < T + \epsilon \cdot T = T(1+\epsilon)$$

Conclusion: Handling small jobs increases the makespan by at most a factor of $(1+\epsilon)$.

Part C: The Final Algebra

Now we combine Part A (Rounding) and Part B (Small Jobs).
We found a schedule for rounded jobs in time $T$. When we restore the real sizes and add small jobs, the errors multiply.

$$ALG \le T \cdot (1+\epsilon) \cdot (1+\epsilon)$$

$$ALG \le T \cdot (1+\epsilon)^2$$

Expanding the Square:
Using standard algebra: $(1+\epsilon)^2 = 1 + 2\epsilon + \epsilon^2$.

Since $\epsilon$ is a small number (between 0 and 1), its square $\epsilon^2$ is even smaller (e.g., if $\epsilon = 0.1$, then $\epsilon^2 = 0.01$). In approximation theory, we often simplify $\epsilon^2 \le \epsilon$ to get a clean upper bound:


$$1 + 2\epsilon + \epsilon^2 \le 1 + 2\epsilon + \epsilon = 1 + 3\epsilon$$

Final Result:


$$ALG \le (1+3\epsilon) \cdot T^*$$


For any error parameter $0 < \epsilon \le 1$, the algorithm produces a schedule with makespan at most:


$$ALG \le (1+\epsilon)^2 \cdot T^* \approx (1+3\epsilon) \cdot T^*$$

Time & Space Complexity:

$$O(n^{2k} \cdot \lceil \log_2 (1/\epsilon) \rceil)$$; Where $k$ (number of distinct sizes) depends on $1/\epsilon$.






















