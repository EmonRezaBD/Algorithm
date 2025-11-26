<center>

# Approximation Algorithms for Makespan Scheduling
**Md Rokonuzzaman Reza, Souhardya Saha Dip**

</center>

---

## **Problem Overview**

This minipaper briefly illustrates the theoretical foundation, proof, and examples for solving Makespan scheduling. The objective is to assign a set of $n$ jobs, each with a specific processing time, to a set of $m$ machines such that the maximum load on any single machine is minimized. Formally, given a set of machines $M = \{1, \dots, m\}$ and a set of jobs $J = \{1, \dots, n\}$ where job $j$ requires processing time $p_{j}$ on machine $i$, we seek an assignment that minimizes the makespan $C_{max}$:

$$C_{max} = \max_{i \in M} \sum_{j \in J_i} p_{j}$$

where $J_i$ is the subset of jobs assigned to machine $i$.

We can make an intuitive analogy for this problem with "Block Stacking," where jobs are viewed as blocks of varying heights, and machines are stacks. The goal is to distribute the blocks among $m$ stacks so that the height of the tallest stack is as low as possible. Makespan Scheduling is classified as NP-Hard, which means it is computationally intractable to find the perfect, optimal solution efficiently for every possible input. This hardness is theoretically proven by observing that scheduling jobs on just two identical machines is mathematically equivalent to the Partition Problem, a famous NP-Complete puzzle that asks if a set of numbers can be split into two exactly equal sums. Consequently, this minipaper focuses on Approximation Algorithms that guarantee a solution within a specific factor (e.g., within 2 times or 1.5 times) of the optimal makespan. Thus, we will prove total of 3 algorithms 
(i) Greedy Job Scheduling 
(ii) Sorted Greedy Job Scheduling, and 
(iii) Polynomial Time Approximation Scheme (PTAS) 

## **Greedy Job Scheduling:**

This algorithm called Greedy because it makes the best immediate choice at every single step. When the algorithm picks a machine for a job, it looks for the "cheapest" option available right now (the machine with the lowest load). It does not look ahead.

**Procedure:**
* Initialize $m$ machines with 0 load.
* Process jobs one by one in arbitrary order.
* Assign the current job to the machine with the minimum current load.

**Theorem**

Statement: The Greedy Job Scheduling algorithm is a 2-approximation means it is at most twice the optimal makespan.

$$ALG \le 2 \cdot OPT$$

**Proof**

Lemma 1: Lower Bounds on Optimal <br>
The optimal makespan ($OPT$) is limited by two physical constraints: 
<br> 
(i) Max Job Size: $OPT$ must be at least as large as the largest single job ($p_{max}$). $$OPT \ge p_{max}$$ 

(ii) Average Load: $OPT$ cannot be less than the average load of all jobs distributed perfectly evenly. $$OPT \ge \frac{\text{Total Work}}{m}$$ 

Now, consider the machine that finishes last. Its load is the makespan ($ALG$). Let $j_{last}$ be the last job added to this machine, with size $p_{last}$. Also, let $L$ be the load of this machine before $j_{last}$ was added. We can express the final makespan as: $$ALG = L + p_{last}$$ 

Moreover, when the algorithm assigns job $j_{last}$, it chose the machine with the minimum load. Therefore, all other machines had a load of at least $L$. So it can be said that Total Work $\ge m \cdot L$ and Average Work $\ge L$

From Lemma 1, we know $OPT \ge \text{Average Work}$, so: $$OPT \ge L$$ 
Finally, substituting our bounds into the makespan equation: $ALG = L + p_{last}$ Since $L \le OPT$ and $p_{last} \le p_{max} \le OPT$:

$$ALG \le OPT + OPT$$

$$ALG \le 2 \cdot OPT$$

Therefore, it is proved that the greedy approach outputs a solution with makespan at most 2 times the optimum.

**Time & Space Complexity:**

- **Time:** $O(n \log m)$ — We iterate through $n$ jobs, and for each, we find the minimum load among $m$ machines. Using a Min-Heap takes $O(\log m)$.

- **Space:** $O(m)$ — We can only store the current load of the $m$ machines.

## **Sorted Greedy Job Scheduling**
The basic Greedy algorithm can fail when a huge job appears at the very end of the list, creating a sudden spike in height. By processing the biggest jobs first, the large blocks form a solid foundation at the bottom. The smaller  jobs fills in the gaps at the top, evening out the stacks. This prevents the worst-case scenario where a large block lands on top of an already high stack.

**Procedure:**

* Sort all jobs (blocks) from largest to smallest.

* Initialize $m$ machines with 0 load.

* Process the sorted jobs one by one.

* Assign the current job to the machine with the minimum current load.

**Theorem 2**

Statement: The Sorted Greedy Job Scheduling algorithm is a 1.5-approximation.

$$ALG \le 1.5 \cdot OPT$$

**Proof**

Step 1: 
Let the machine that finishes last define our makespan ($ALG$).
Let $j_{last}$ be the last job added to this machine, with size $p_{last}$.
Let $L$ be the load of this machine before $j_{last}$ was added.

We know from the previous proof that:

$ALG = L + p_{last}$

$OPT \ge L$ (Because this was the shortest stack, so all others are taller).

Step 2: The Sorting Advantage

Because we sorted the jobs from largest to smallest, every job processed before $j_{last}$ must be at least as big as $j_{last}$. Since $L$ is the height of the stack under $j_{last}$, there must be at least one job in that stack (unless $L=0$, in which case the solution is optimal). Therefore, every machine has at least one job on it that is $\ge p_{last}$.

Step 3: The Pigeonhole Principle

Count the "big" jobs involved so far: We have $m$ machines. Each machine has at least 1 job $\ge p_{last}$ already on it. We just added $j_{last}$ (which is also a job of size $p_{last}$). Total "big" jobs = $m + 1$. If you have $m$ machines (buckets) and $m+1$ big jobs (items), the Pigeonhole Principle says that at least one machine must hold 2 big jobs in the optimal solution.

Step 4: Bounding the Optimal

Since the optimal solution ($OPT$) must fit these $m+1$ jobs onto $m$ machines, one machine must handle two jobs of size $p_{last}$ (or larger).
Therefore:

$$OPT \ge 2 \cdot p_{last}$$

Or, dividing by 2:

$$p_{last} \le \frac{1}{2} \cdot OPT$$

Finally, now we substitute this into our original equation ($ALG = L + p_{last}$):

We know $L \le OPT$.

We just proved $p_{last} \le 0.5 \cdot OPT$.

$$ALG \le OPT + 0.5 \cdot OPT$$

$$ALG \le 1.5 \cdot OPT$$


Therefore, it is proved that the sorted greedy approach outputs a solution with makespan at most 1.5 times the optimum.

**Time & Space Complexity:**

- **Time:** $O(n \log n)$ — Sorting the jobs takes $O(n \log n)$. Assigning jobs with a min-heap over $m$ machines costs $O(n \log m)$. For $n \ge m$: $O(n \log n + n \log m) = O(n \log n)$.

- **Space:** $O(n)$ — For $n \ge m$, Store the sorted job list, $O(n)$ and the machine loads $O(m)$; total is $O(n)$.

## **(iii) Polynomial Time Approximation Scheme (PTAS):**
Unlike the Greedy algorithms which give a fixed guarantee (like 2x or 1.5x), a PTAS allows us to choose how close we want to be to the perfect solution. We can pick an error parameter $\epsilon$ (epsilon). The algorithm guarantees a solution within $(1+\epsilon)$ of the optimal. For a rough answer fast, choose a large $\epsilon$ whereas for a very precise answer, choose a tiny $\epsilon$. Moreover, the algorithm combines three techniques: Binary Search, Rounding, and Dynamic Programming.


Part A: The Geometric Rounding Logic<br>
Goal: Prove that rounding doesn't change the answer by more than a factor of $(1+\epsilon)$.

We define a set of interval buckets based on powers of $(1+\epsilon)$:
$[1, 1+\epsilon), [(1+\epsilon), (1+\epsilon)^2), \dots$

If a job has a real size $P_{real}$ that falls into the interval $[X, X(1+\epsilon))$, we simplify it by rounding it down to $X$. Rounded Size: $P_{rounded} = X$ Real Size: $P_{real} < X(1+\epsilon)$
Therefore, the relationship between the real and rounded size is:

$$P_{real} < P_{rounded} \cdot (1+\epsilon)$$

Conclusion: If we find a valid schedule using the rounded sizes that fits within time $T$, converting those blocks back to their real sizes will expand the total time by at most that same factor:

$$\text{Real Time} \le T \cdot (1+\epsilon)$$



Part B: The Small Jobs Logic

Goal: Prove that adding the "small" jobs (which we ignored earlier) doesn't break the schedule too badly.

A "small" job is defined as any job with size $p < \epsilon \cdot T$. When we greedily add these small jobs to the schedule, two things can happen: (i) It fits: The job fits into an existing gap below time $T$. (No increase in makespan). (ii) It overflows: The job forces a machine to go above time $T$. If it overflows, the machine was previously at some height $H \le T$, and we added a small job $p$. The new height is $H + p$. Since $p < \epsilon \cdot T$, the new height is:

$$\text{New Height} < T + \epsilon \cdot T = T(1+\epsilon)$$

Conclusion: Handling small jobs increases the makespan by at most a factor of $(1+\epsilon)$.



Part C: The Final Algebra

Now we combine the error from the Algorithm (Rounding & Small Jobs) with the error from the Binary Search. The Binary Search finds a target $T$ that is within $(1+\epsilon)$ of the optimal $T^*$. The Rounding/Small Job logic produces a schedule within $(1+\epsilon)$ of that target $T$.

$$ALG \le T \cdot (1+\epsilon) \cdot (1+\epsilon)$$

$$ALG \le T \cdot (1+\epsilon)^2$$

Expanding the Square: Using standard algebra: $(1+\epsilon)^2 = 1 + 2\epsilon + \epsilon^2$. Since $\epsilon$ is a small number (between 0 and 1), its square $\epsilon^2$ is even smaller (e.g., if $\epsilon = 0.1$, then $\epsilon^2 = 0.01$). In approximation theory, we often simplify $\epsilon^2 \le \epsilon$ to get a clean upper bound:

$$1 + 2\epsilon + \epsilon^2 \le 1 + 2\epsilon + \epsilon = 1 + 3\epsilon$$

Final Result:

$$ALG \le (1+3\epsilon) \cdot T^*$$

For any error parameter $0 < \epsilon \le 1$, the algorithm produces a schedule with makespan at most:

$$ALG \le (1+\epsilon)^2 \cdot T^* \approx (1+3\epsilon) \cdot T^*$$

**Time & Space Complexity:**

- **Time:** $O\big(n^{2k} \cdot \lceil \log_2(1/\epsilon) \rceil\big)$  
    - Binary search runs for $\lceil \log_2(1/\epsilon) \rceil$ iterations.
    - Each iteration for dynamic programming resolves in $O(n^{2k})$ time.
    - Here $k = \lceil \log_{1+\epsilon}(1/\epsilon) \rceil$ is determined by the rounding parameter $\epsilon$; where $\epsilon$ is fixed, $k$ is a constant.

- **Space:** $O(n^{k})$
    - The DP state uses a $k$-dimensional table with roughly $n^{k}$ entries and $O(n)$ overhead for job lists. So the dominant space is $O(n^{k})$.
