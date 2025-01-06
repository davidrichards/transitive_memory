# Big-O Notation

In order to see Big-O, build up to it:

- Get clear and ask questions.
- Write the pseudo code.
- Look at each step
  - What does it do?
  - How many times will this run?
- Build an outer loop, then inner loops, add the terms.
- Summarize the dominant term.

## Common Patterns

- Iterate over an array: O(n)
- Sorting algorithm: O(n log n)
- Binary search: O(log n)
- Nested loops: multiply complexity, O(n^2)
- Recursive splitting (divide and conquer): O(n log n)

## Concepts

- Time complexity: O(1), O(log n) O(n), O(n^2), O(2n)
- Space complexity: memory relative to input size
- Asymptotic: Big-O upper bound, Omega lower bound, Theta exact bound
- Growth rates:
  - Constant time: O(1)
  - Logarithmic time: O(log n)
  - Linearithmic time: O(n log n)
  - Quadratic time: O(n^2)
  - Exponential time: O(2^n)

## Common Algorithms

- Sorting
  - Quicksort: O(n log n), O(n^2) worst case
  - Mergesort: O(n log n)
  - Bubble Sort: O(n^2)
- Search
  - Linear search: O(n)
  - Binary search: O(log n)
- Graph Traversal
  - BFS: O(V + E)
  - DFS: O(V + E)

## Problem Classification

- P vs NP: solveable in polynomial time (O(n^k)) vs verifiable but not solvable in polynomial time.
- NP-Complete: when all problems are solveable in polynomial time.
- NP-Hard: at least as hard as the hardest NP problems.

## Summary

- Trade-Offs: Time vs Space, faster often requiring more space.
- Edge cases: large inputs, empty inputs, single-element inputs, highly skewed inputs.
- Real-world considerations: cache, parallelism, I/O, etc.
- Parallelism: does the algorithm benefit from parallelism?
- When to apply: for small, O(n^2) may work, for large O(n log n) may be required.

