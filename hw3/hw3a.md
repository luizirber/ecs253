---
title: ECS253 - Homework 3a
author:
 - name: Luiz Carlos Irber Jr
   affiliation: UC Davis
   email: lcirberjr@ucdavis.edu
date: "2016-05-12"
...


<!--
1. Write a 2-3 sentences with the goal of the project.
2. Outline your plan of attack, and give preliminary results.
3. Itemize the stumbling blocks you foresee that will keep you from achieving the
desired outcome.
4. Each team member must turn in their own assignment. The outlines can be
the same, but come up with your own ideas about stumbling blocks. Also, the
more people on your team, the more we expect you to have accomplished by
now.
-->

# Project goal

The goal is to explore and characterize genome assembly graphs,
focusing on how structural features emerge as more data is collected.
Genome assembly graphs are constructed from reads,
random substrings from the underlying genome,
and in the de Bruijn graph version it depends heavily on $k$,
the size of the sliding window over reads chosen for the graph construction.

# Plan of attack and preliminary results

### Graph representation for a bacterial genome

On homework 2 I explored the de Bruijn graph (dBg) for a bacterial genome,
showing that even small $k$-mers are computationally challenging for
traditional libraries (networkx and graph-tool).
For $5$-mers the dBg is a complete graph,
with all $512$ possible node having degree $8$ (possible nodes = $4^{k} / 2$).
For $13$-mers the dBg is sparser,
with ~3.8 million nodes and 6 million edges,
and each node having an average degree of $3.15$.
Since there are ~33 million possible nodes,
around $11\%$ are present in this graph.

If we track the abundance of each $k$-mer
(which can be interpreted as node weight or can be used to calculate edge weights)
we can see that the $k$-mer abundance graph is close to a power law with $\alpha=1.25$.

This bacterial genome was previously assembled,
so there are only 'true' $k$-mers present in the graph.
If the reads are used instead,
the graph structure (and abundance distribution) change significantly,
and the abundance doesn't follow a power law anymore.

### Graph structure and linearization

As $k$ increases,
more nodes start to have degree 2,
and long runs of degree-2 connected nodes can be merged into one single node
(with weight $k+n-1$,
where $n$ is the number of nodes merged).
These nodes represent long linear paths through the graph,
and usually emerge due to the sliding window nature of the input.

Simple genomes (no repeats, no sequencing errors) can be "linearized" down to a single chromosome,
but for any reasonable complex genome it is not possible to completely linearize the graph.
This linearization process is a basic step in a assembler,
a software that generates the output used in all the other (meta-)genomic studies.
These programs also have heuristics to solve graph structural features to further linearize it.

### Model representation

There are many studies on the optimal $k$ for a given genome,
but we would like to understand better how the graph structure varies with limited information available.
Can we determine when the graph structure reaches a sort of steady state,
or detect when there is a transition from a linear graph to a complete graph
(and other interesting configuration in between?).
Our first idea was to create a generative model reaching similar configurations,
but this muddles the lines between model and data,
hypothesis and result.
An statistical approach,
using available genomes to infer structures,
might be more appropriate.

# Stumbling blocks

- How should we parameterize the model?
- How to generate random biologically-relevant genomes to test the model?
- How to use the information already available
(be it in assembled genomes or raw reads) to check for common graph structural features?
- How to train models on available genomes to extract parameters?

[image5]: img/ecoli_5.png
![Graph for _e.coli_, k=5.
  Node size and color represent the abundance of each 5-mer in the original dataset][image5]

[image21]: img/ecoli_21.png
![Compressed (linearized) graph for _e.coli_, k=21.
  Node size and color represent how many nodes were merged into one][image21]

[image31]: img/ecoli_31.png
![Compressed (linearized) graph for _e.coli_, k=31.
  Node size and color represent how many nodes were merged into one][image31]

