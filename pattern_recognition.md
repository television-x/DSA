# Competitive Programming Algorithm Cheat Sheet

## How to Use This Document

This is your **reference knowledge base** — the raw catalog of techniques your brain should be scanning through when you do pattern recognition. It's organized by *category* (what kind of problem structure it solves), and within each category, techniques are listed **roughly from cheapest/simplest to most expensive/complex**, with their typical time complexity.

Pair this with the Pattern-Recognition Coach prompt: when the coach asks "what candidates can you think of," this is the list you draw from. Over time you shouldn't need this file — it should live in your head. Until then, keep it open beside you.

**Complexity notation used below:** `n` = input/array size, `m` = number of operations/edges/queries, `k` = a secondary parameter (window size, bitmask size, etc.), `log` = base 2 unless stated.

---

## 1. Brute Force / Baseline

| Technique | Time Complexity | When It's Actually Fine |
|---|---|---|
| Full brute force (try everything) | O(2ⁿ), O(n!), O(nᵏ) | n ≤ ~20 (subsets), n ≤ ~10 (permutations) |
| Nested loops | O(n²) or O(n³) | n ≤ ~5,000 (n²), n ≤ ~500 (n³) |
| Linear scan | O(n) | Always cheap — use as your complexity floor/reference |

Always compute this first. It's your baseline and often reveals what "budget" you have to beat.

---

## 2. Searching & Sorting Fundamentals

| Technique | Time Complexity | Typical Signal |
|---|---|---|
| Linear Search | O(n) | Unsorted, single pass suffices |
| Binary Search (on array) | O(log n) | Sorted array, "find target/boundary" |
| Binary Search on Answer | O(log(range) × check_cost) | "Minimize the maximum" / "maximize the minimum" phrasing, monotonic feasibility |
| Sorting (comparison-based) | O(n log n) | Almost any problem benefiting from order |
| Counting Sort / Radix Sort | O(n + k) | Small bounded value range |
| Two Pointers | O(n) (after sort if needed) | Sorted array, pair/triplet sum, merging |
| Sliding Window | O(n) | Contiguous subarray/substring with a condition |

---

## 3. Prefix/Range Aggregation (Static Data)

| Technique | Build | Query | Update | Signal |
|---|---|---|---|---|
| Prefix Sum | O(n) | O(1) | — (static) | "Sum of subarray," no updates |
| Difference Array | O(n + m) total | O(n) read at end | O(1) per range update | "Add to range, read final array once," no interleaved queries |
| Sparse Table | O(n log n) | O(1) | — (static, idempotent ops like min/max/gcd) | Static array, many range-min/max/gcd queries |
| Mo's Algorithm | O((n+m)√n) | — | — | Offline range queries, no updates, complex aggregate (distinct counts etc.) |

---

## 4. Range Update + Interleaved Query (Dynamic Data)

| Technique | Update | Query | Signal |
|---|---|---|---|
| Fenwick Tree / BIT (point update, range sum) | O(log n) | O(log n) | Point updates, prefix/range sum queries |
| Fenwick Tree (range update, point query) | O(log n) | O(log n) | Range add, point read, interleaved with updates |
| Segment Tree (no lazy) | O(log n) | O(log n) | Point update, range query (sum/min/max) |
| Segment Tree with Lazy Propagation | O(log n) | O(log n) | Range update AND range query, interleaved |
| Persistent Segment Tree | O(log n) per version | O(log n) | Need to query *historical* versions of the array |

**Rule of thumb:** if queries and updates are interleaved (you need an answer *between* updates), reach for segment tree/Fenwick. If all updates happen first and you read once at the end, a difference array is enough and much simpler.

---

## 5. Dynamic Programming

| Technique | Time Complexity | Signal |
|---|---|---|
| 1D DP (Fibonacci-style, LIS naive) | O(n) to O(n²) | "Number of ways," "min/max cost to reach state i" |
| Knapsack DP (0/1, unbounded) | O(n × capacity) | Item selection under a weight/cost limit |
| LIS (naive) | O(n²) | Longest increasing subsequence |
| LIS (optimized, patience sorting + binary search) | O(n log n) | Same, but n is large |
| Interval DP | O(n³) (sometimes O(n²)) | "Merge intervals," "partition into segments," matrix chain multiplication, palindrome partitioning |
| Tree DP | O(n) | DP defined over a tree, "for each subtree..." |
| DP on Trees with Rerooting | O(n) | Need the DP answer *for every node as root* |
| Bitmask DP | O(2ⁿ × n) or O(2ⁿ × n²) | n ≤ ~20, "assign/visit each of n items," TSP-like |
| Digit DP | O(digits × states) | "Count numbers in [L, R] satisfying digit property" |
| DP with Convex Hull Trick / Divide & Conquer Optimization | O(n log n) | DP transition is O(n²) naively but has monotonicity structure |
| Matrix Exponentiation (linear recurrence) | O(k³ log n) | Huge n (10⁹+), fixed-size linear recurrence |

---

## 6. Graph Algorithms

| Technique | Time Complexity | Signal |
|---|---|---|
| BFS | O(n + m) | Shortest path, unweighted graph |
| DFS | O(n + m) | Connectivity, cycle detection, traversal, backtracking base |
| Dijkstra (with heap) | O((n + m) log n) | Shortest path, non-negative weights |
| Bellman-Ford | O(n × m) | Shortest path, negative weights allowed, detect negative cycle |
| Floyd-Warshall | O(n³) | All-pairs shortest path, small n (≤ ~500) |
| Topological Sort (Kahn's/DFS) | O(n + m) | DAG, ordering with dependencies |
| Union-Find / DSU | O(m α(n)) ≈ O(m) | Connectivity queries, Kruskal's MST, offline component merging |
| Kruskal's MST | O(m log m) | Minimum spanning tree |
| Prim's MST (with heap) | O(m log n) | Minimum spanning tree, dense graph variant |
| SCC (Tarjan's / Kosaraju's) | O(n + m) | Strongly connected components in directed graph |
| LCA (binary lifting) | O(n log n) build, O(log n) query | Tree ancestor queries |
| Euler Tour + Segment Tree | O(n log n) | Subtree queries/updates on a tree |
| Heavy-Light Decomposition | O(n log n) build, O(log² n) query | Path queries/updates on a tree |
| Max Flow (Dinic's) | O(n² × m) (general), faster on unit-cap graphs | "Maximum flow," "min cut," bipartite matching via flow |
| Bipartite Matching (Hopcroft-Karp) | O(m √n) | Explicit bipartite matching |

---

## 7. String Algorithms

| Technique | Time Complexity | Signal |
|---|---|---|
| Naive substring search | O(n × m) | Small inputs only |
| KMP (Knuth-Morris-Pratt) | O(n + m) | Pattern matching, prefix function |
| Z-function | O(n) | Pattern matching, string periodicity |
| Rabin-Karp | O(n + m) average | Multiple pattern matching, rolling hash |
| Trie | O(total length) build, O(length) query | Prefix queries, dictionary of words |
| Suffix Array (+ LCP array) | O(n log n) build | Substring queries, longest common substring |
| Suffix Automaton | O(n) build | Advanced substring counting/distinct substrings |
| Manacher's Algorithm | O(n) | Longest palindromic substring |

---

## 8. Combinatorics & Number Theory

| Technique | Time Complexity | Signal |
|---|---|---|
| Sieve of Eratosthenes | O(n log log n) | Primes up to n |
| GCD / Extended Euclidean | O(log(min(a,b))) | GCD, modular inverse |
| Fast Exponentiation | O(log n) | Large power mod, matrix power |
| Modular Inverse (Fermat's little theorem) | O(log p) | Division under modulo, p prime |
| Combinatorics (nCr with precomputed factorials) | O(n) precompute, O(1) query | "Number of ways to choose," binomial coefficients |
| Inclusion-Exclusion | O(2ᵏ) over k sets | "Count elements satisfying at least one of k conditions" |
| FFT / NTT | O(n log n) | Polynomial multiplication, convolution |

---

## 9. Greedy & Structural Tricks

| Technique | Time Complexity | Signal |
|---|---|---|
| Greedy with sorting | O(n log n) | Interval scheduling, exchange-argument-provable problems |
| Monotonic Stack | O(n) | "Next greater/smaller element," histogram problems |
| Monotonic Queue (deque) | O(n) | Sliding window min/max |
| Heap / Priority Queue based greedy | O(n log n) | "Always take the smallest/largest available," Huffman-style |
| Line Sweep | O(n log n) | Interval overlap, closest pair, geometry events |
| Coordinate Compression | O(n log n) | Large value range but few distinct values, used before Fenwick/segment tree |
| Meet in the Middle | O(2^(n/2)) | n ≤ ~40, split search space in half |
| Small-to-Large Merging (DSU on tree) | O(n log n) | Merging sets/counts across subtrees |

---

## 10. Game Theory & Probability (Less Common but Recognizable)

| Technique | Time Complexity | Signal |
|---|---|---|
| Game Theory (Grundy numbers / Nim) | O(n × states) | Two-player optimal-play games, "who wins" |
| Minimax with memoization | O(states) | Game trees with limited depth |
| Basic Probability / Expected Value DP | O(n × states) | "Expected number of moves/steps" |
| Randomization / Randomized Algorithms | Varies, often O(n) expected | Hashing collisions, randomized quickselect, Monte Carlo |

---

## Quick Decision Flow (Use With the Coach's Stage 1–4)

1. **What's n, m?** → gives your complexity budget (see table below).
2. **Read once at the end, or query between updates?** → difference array vs. segment tree/Fenwick.
3. **Order-independent updates (commutative ops)?** → strong hint toward prefix-sum/difference-array family.
4. **Graph-shaped?** → narrow to Section 6 based on weights (none/positive/negative) and what's being asked (path, connectivity, flow).
5. **Optimization with a monotonic feasibility check?** → Binary Search on Answer.
6. **Small n (≤ 20-40)?** → Bitmask DP or Meet in the Middle are back on the table even if "slow."
7. **String-specific?** → Section 7.
8. **"Number of ways" / counting?** → DP or Combinatorics.

### Complexity Budget Reference (rule of thumb, ~10⁸ ops/sec)

| n (or n, m) | Complexity that likely fits |
|---|---|
| ≤ 10-12 | O(n!), O(2ⁿ × n) |
| ≤ 20-22 | O(2ⁿ), Bitmask DP |
| ≤ 40 | O(2^(n/2)) — Meet in the Middle |
| ≤ 500 | O(n³) |
| ≤ 5,000 | O(n²) |
| ≤ 10⁵ - 10⁶ | O(n log n), O(n) |
| ≤ 10⁷ - 10⁸ | O(n), O(n log log n) |
| ≥ 10⁹ | O(log n), O(1), or math closed-form only |

---

## Your Personal Cheat-Sheet Extension

Leave this section for yourself — append one line per problem you solve, in the format:

```
[Problem name] — Signature: "..." — Technique: ... — Complexity: ...
```

This turns the static reference above into a living, personalized version built from problems *you've* actually cracked, which sticks in memory far better than the generic table.
