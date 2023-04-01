---
title: "Union Join and Subset Graph of Acyclic&nbsp;Hypergraphs"
description: Implementation of the algorithms in my paper "Computing the Union Join and Subset Graph of Acyclic Hypergraphs in Subquadratic Time". Work in progress.

layout: default

links:
  git: https://github.com/ALeitert/p2c-Union-Join-Graph
---


Goal of this project is to implement the algorithms in my paper [5].
It investigates the two problems of computing the union join graph as well as computing the subset graph for acyclic hypergraphs and their subclasses.

The *union join graph $G$* of a given acyclic hypergraph $H$ is the union of all its join trees.
That is, each vertex of $G$ represents a hyperedge of $H$ and two vertices of $G$ are adjacent if there exits a join tree $T$ for $H$ such that the corresponding hyperedges are adjacent in $T$.

The *subset graph* of a hypergraph $H$ is a directed graph $G$ where each vertex represents a hyperedge of $H$ and there is a directed edge from a vertex $u$ to a vertex $v$ if the hyperedge corresponding to $u$ is a subset of the hyperedge corresponding to $v$.


# Computing the Subset Graph

A core part of the algorithms in [5] is the computation of a subset graph.
Indeed, most algorithms to compute union join graphs use a subset graph algorithm.
The paper presents such algorithms for β-acyclic, γ-acyclic, and interval hypergraphs.
For α-acylic hypergraphs, however, we need a subset graph algorithm for general graphs.
We also need such an algorithm to test our implementation.

Pritchard [6] present algorithms in their paper which compute the subset graph for an arbitrary hypergraph.
They start with a simple, high-level algorithm and then modify it two times with the goal of improving the runtime.
Pritchard shows that the runtime with all these modifications is in $\mathcal{O} \bigl( N^2 \log \log N / \log^2 N \bigr)$.

Runtime tests clearly show that Pritchard's approach (even in its simplest form) is much faster than a naive implementation.
Using reduced sets then gives an additional improvement.


# Computing a Union Join Graph

To verify the correctnes of a computed union join graph, we use an approach which is based on [Kruskal's MST algorithm](https://en.wikipedia.org/wiki/Kruskal%27s_algorithm).
That approach was suggested in [1].
To do so, we first compute the weighted linegraph $L$ of a given hypergraph.
We then use a modification of Kruskal's algorithm to compute the union of all maximum spanning trees of $L$.

The algorithm is surprisingly fast.
Most-likely, due to the simplicity and efficiency of Kruskal's algorithm.


# "Parsing" Hypergraphs

For each class of hypergraphs, it is importatn that we determine a characteristig property, order, or structure of a given hypergraph.
That include a join tree for α- and β-acylic hypergraphs, a pruning sequence for γ-acyclic hypergraphs, and a join path for interval hypergraphs.

To compute a join tree, we use the algorithm from [7].

An algorithm for computing a pruning sequence is given in [2].
Their algorithm relies on the recognition of cographs.
To do that, we use an algorithm from [4].

To compute the join path of an interval hypergraph, we use the algorithm presented in [3].


# References

[[1]](https://arxiv.org/abs/1607.02911)
A. Berry, G. Simonet:
Computing the atom graph of a graph and the union join graph of a hypergraph.
*arXiv* 1607.02911, 2016.

[[2]](https://doi.org/10.1016/S0304-3975(00)00234-6)
G. Damiand, M. Habib, C. Paul:
A simple paradigm for graph recognition: application to cographs and distance hereditary graphs.
*Theoretical Computer Science* 263, 99-111, 2001.

[[3]](https://doi.org/10.1016/S0304-3975(97)00241-7)
M. Habib, R. McConnell, C. Paul, L. Viennot:
Lex-BFS and partition refinement, with applicationsto transitive orientation, interval graph recognition and consecutive ones testing.
*Theoretical Computer Science* 234, 59-84, 2000.

[[4]](https://doi.org/10.1016/j.dam.2004.01.011)
M. Habib, C. Paul:
A simple linear time algorithm for cograph recognition.
*Discrete Applied Mathematics* 145, 183-197, 2005.

[[5]](https://arxiv.org/abs/2104.06636)
A. Leitert:
Computing the Union Join and Subset Graph of Acyclic Hypergraphs in Subquadratic Time.
*arXiv* 2104.06636, 2021.

[[6]](https://link.springer.com/article/10.1007/PL00009272)
P. Pritchard:
A Fast Bit-Parallel Algorithm for Computing the Subset Partial Order.
*Algorithmica* 24, 76-86, 1999.

[[7]](https://epubs.siam.org/doi/abs/10.1137/0213035)
R.E. Tarjan, M. Yannakakis:
Simple Linear-Time Algorithms to Test Chordality of Graphs, Test Acyclicity of Hypergraphs, and Selectively Reduce Acyclic Hypergraphs.
*SIAM J. Comput.* 13 (3), 566–579, 1984.
