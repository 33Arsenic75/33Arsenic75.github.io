---
layout: ../../layouts/post.astro
title: "Finding Hamiltonian Path in Tournaments in O(nLogn)"
type: ["blog"]
pubDate: 2024-08-31
description: "Explore an efficient algorithm for finding Hamiltonian Paths in Tournament graphs with a time complexity of O(n log n). We first prove the existence of Hamiltonian Paths in Tournaments and along the way find an O(n^2) algorithm for same. We then discuss the key insights and algorithms to achieve an O(n log n) time complexity. At the end we draw some parallels with the optimization techniques used with some very standard algorithms problem."
author: "Abhinav Rajesh Shripad"
excerpt: "This post explores the efficient algorithms for finding Hamiltonian Paths in Tournaments. We discuss the theoretical background, key algorithms, and practical implications of achieving an O(n log n) time complexity."
image:
  src: "/images/hamiltonian-path-Tournament.png"
  alt: "Graph illustrating Hamiltonian Path in a Tournament"
tags: ["Algorithms", "Graph Theory", "Tournaments"]
link: "/posts/Hamiltonian Path in Tournaments"
---


# Definitions

## Tournament

In graph theory, a **Tournament** is a directed graph (digraph) where there is exactly one directed edge between every pair of vertices. In other words, for any two vertices **u** and **v**, either (**u** → **v**) or (**v** → **u**) is in the graph, **but not both**. Tournaments are used to model situations where each participant competes with every other participant exactly once, such as in round-robin Tournaments in sports. Throughout the post, we will refer to a Tournament with **n**  vertices and **n(n-1)/2** edges is known as a **complete Tournament graph**.


## Hamiltonian Path

A **Hamiltonian Path** in a graph is a path that visits each vertex exactly once. If such a path exists in a graph, the graph is called a **Hamiltonian graph**. Finding Hamiltonian Paths is a common problem in graph theory and has applications in optimization and scheduling such as the [**Travelling Salesman Problem(TSP)**](https://en.wikipedia.org/wiki/Travelling_salesman_problem).


#### NP-Completeness of Finding Hamiltonian Paths

Finding Hamiltonian Paths is classified as an NP-complete problem in **general graphs**. This classification means that no known polynomial-time algorithm can solve the problem for all instances.

#### Complexity of BFS and DFS in Tournaments

In contrast, for specific types of graphs, such as Tournaments the problem of finding Hamiltonian Paths can be approached more efficiently. Simple Breadth-First Search (BFS) and Depth-First Search (DFS) algorithms on a Tournament graph typically have a time complexity of O(V + E). For Tournaments, where the number of edges E is O(n^2), this results in a time complexity of O(n^2). Thus the algorithm discussed here is even faster for finding hamiltonian path than the standard BFS and DFS algorithms for Tournaments. We can also think of this as the algorithm doesn't even need to think/process about all the edges.



## Proving the Existence of a Hamiltonian Path in Tournaments

To prove the existence of a Hamiltonian Path in a Tournament graph, we use induction on the number of vertices.

### Recursive Proof

1. **Base Case:**
   - For a Tournament with a single vertex \(n = 1\), the path trivially exists as the vertex itself is a path.

2. **Recursive Step:**
   - Assume that for a Tournament with **k** vertices, there exists a Hamiltonian Path. We will prove that this property holds for a Tournament with **k+1** vertices.
   - Let **v_1**, **v_2**, ..., **v_k** be the hamiltonian path **P** of the Tournament graph **T** with **k** vertices (if not, then just rename the vertices). When we add another vertex **v** to the graph, we need to show that there exists a Hamiltonian Path in the new graph with **k+1** vertices.


5. **Inserting the New Vertex:**
     - **Case 1:**
       - If **v** has edges to **v_1**, then **v** can be placed at the beginning  **P**, forming a Hamiltonian Path. Similarly if **v** has edges from **v_k**, then **v** can be placed at the end of **P** to form a Hamiltonian Path.
     - **Case 2:**
       - This occurs if and only if **v** has an edge from **v_1** AND an edge to **v_k**. Think about this carefully and convince yourself that this is the only possibility. So how do we handle this possibility ? We can insert **v** between **v_i** and **v_i+1** for some **i** such that **v_i** has an edge to **v** and **v_i+1** has an edge from **v**. This insertion will not break the Hamiltonian Path **P**. But how do we guarantee that such an **i** exists ?  Since **v** has an edge from **v_1** and an edge to **v_k**, there must be a vertex **v_i** and  **v_i+1** such that the direction of edges from **v_i** and **v_i+1** toward **v** is different.Think about this **switching of direction of edge** very carefully and convince yourself it is true, as this will be the crux of our optimization. Thus we can insert **v** between **v_i** and **v_i+1** to form a Hamiltonian Path.

6. **Conclusion:**
   - By inserting **v** into **P** appropriately, we ensure that the new graph **T** with **k+1** vertices also contains a Hamiltonian Path. This completes the induction step.

Thus, by induction, we have proved that every Tournament graph with **n** vertices contains a Hamiltonian Path.

## Naive O(n^2)  Algorithm for Finding a Hamiltonian Path

In Tournament graphs, finding a Hamiltonian Path can be done efficiently using a naive O(n^2)  approach.
The naive algorithm for finding a Hamiltonian Path in a Tournament graph operates as follows:

1. **Base Case:**
   - For a Tournament with a single vertex (\( n = 1 \)), the path trivially exists as the vertex itself is a Hamiltonian Path.

2. **Find Hamiltonian Path for \( n-1 \) Vertices:**
   - For  n > 1 , start by finding a Hamiltonian Path in the subgraph with n-1 vertices. This is done recursively.

3. **Insert the \( n \)th Vertex:**
   - Once a Hamiltonian Path for  n-1  vertices is identified, try inserting the \( n \)th vertex at every possible position in the graph, ie between every 2 consecutive vertices and before and after the first and last vertex. Check if inserting the vertex maintains the path's property of visiting all vertices exactly once. If a valid insertion is found, return the Hamiltonian Path.

### Time Complexity
 - Finding a Hamiltonian Path in n vertices is T(n).
 - Inserting and checking the  n th vertex involves O(n) additional operations.

 The recurrence relation for the time complexity T(n) of this naive algorithm is given by:

 <div style="text-align: center;">
      T(n) = T(n-1) + O(n)
 </div>

 By solving this recurrence relation, we find that the total time complexity of the naive algorithm is O(n^2).

# Side Quest
**Before we delve into the optimized algorithm, let's us solve a different problem, which has nothing to do with graphs. And use the learnings from this problem to optimize our algorithm for finding Hamiltonian Paths in Tournaments.**
### Problem Statement
Consider an array of **n** distinct integers. We need to find a local minima in the array. A local minima is defined as an element in the array which is less than its neighbors.

Since this obviously has an O(n) solution, we will not be discussing the solution here. But we will discuss O(log(n)) solution for this problem.

### Solution


To find a local minima in O(log n) time, we can use a binary search-based approach. Here’s a step-by-step explanation:

**Binary Search Approach:**

   - **Initial Setup:**
     - Start with two pointers, low and  high , which initially point to the beginning and end of the array, respectively.

   - **Find Middle Element:**
     - Calculate the middle index  mid = (low + high) / 2 .

   - **Check Local Minima Conditions:**
     - **If the middle element is a local minima:**
       - The element at index  mid  is less than its neighbors (if they exist). Return this element as the local minima.
     - **If the middle element is not a local minima:**
       - Compare the middle element with its neighbors:
         - **If the left neighbor is smaller:** This implies there is a local minima in the left half of the array. Update  high  to  mid - 1 .
         - **If the right neighbor is smaller:** This implies there is a local minima in the right half of the array. Update  low  to  mid + 1 .

   - **Repeat:**
     - Continue the process until  low  exceeds  high . At each step, the array is halved, leading to a logarithmic number of comparisons.


### Take Away
We check for some condition at the middle element and based on that information we decide whether to search in the left half or right half. This is the key idea we will use to optimize our algorithm for finding Hamiltonian Paths in Tournaments.

So whenever we are applying this idea,we must ensure that a valid solution always exists in the subspace we are going to search, based on the information we have at the middle element. Make sure you understand this idea of guaranteeing a valid solution in the subspace we are going to search, from the information we gather by checking at the mid point.

# Optimization

So recall the algorithm we discussed earlier for finding Hamiltonian Paths in Tournaments. Before we optimize the algorithm, we must ask ourselves, what part of the algorithm can be improved ?? One idea might be to solve the problem for 2 graphs with n/2 vertices and some how join these 2 solutions to get the solution for n vertices. We encourage the reader to think why this idea is not fruitful (or if it is please write to me on my email).

Another optimization one might think of is, how to quickly find the 2 vertices **v_i** and **v_i+1** between which we insert the latest verted **v**. If the subcase 1 follows, where **v** is inserted either at the front or back of the path, then it is already optimized as O(1). So we only need to think how to optimize the subcase 2, where **v** is inserted between 2 vertices.

### Idea

Since it is already subcase 2, we know that **v_1** has an edge towards **v** and **v** has an edge towards **v_2**. Let i=⌊n/2​⌋ and check if **v_i** and **v_i+1** is feasible. If it is, we are done, if not we focus our search on one of the halves (**v_1** and **v_i**) or (**v_i+1** and **v_n**). The halve we choose is the half, were both the end points have edges to **v** going in different directions. One such half will always exist. This is the key idea, by which we are guaranteed that a solution exists in the half we choose, because of the information we have at the middle element. Readers might see the similarity between this idea and the idea we discussed in the side quest.

### Time Complexity
 - Finding a Hamiltonian Path in n vertices is T(n).
 - Inserting and checking the  n th vertex involves O(logn) additional operations.

 The recurrence relation for the time complexity T(n) of this naive algorithm is given by:

 <div style="text-align: center;">
      T(n) = T(n-1) + O(log(n))
 </div>

 By solving this recurrence relation, we find that the total time complexity of the naive algorithm is O(nlog(n)).


# Side Quest II

Read this only after you think you have understood the optimization we did for finding Hamiltonian Paths in Tournaments.
In this, we discuss about some of the questions I had about the optimization we did. The main question which arises is what is the data structure we will use  to store the ordering the path ? We can use a linked list, but then the time complexity of accesing the middle vertex will be O(n). If we use a vector/array, we can access the middle element in O(1), but then the time complexity of inserting the vertex will be O(n). So which data structure should we use ?? To solve this problem, a data structure similar to a linked list with additional bookkeeping is required. The full solution is beyond the scope of this article, but if you're interested in solving it, you can refer to [this solution](https://cis.temple.edu/~wu/teaching/Spring2022/adversary-supplement-2.pdf).
