+++
author = "Marvin Tseng"
date = 2021-05-07T17:53:01Z
description = ""
draft = false
image = "https://images.unsplash.com/photo-1594482628061-3e6fa7f34d14?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MnwxMTc3M3wwfDF8c2VhcmNofDh8fHJpZGRsZXxlbnwwfHx8fDE2MTk5NTM3OTg&ixlib=rb-1.2.1&q=80&w=2000"
slug = "postfinance-challenge"
title = "🧑‍🏫 PostFinance Challenge"

+++


The other day I was scrolling through my LinkedIn newsfeed and saw an advertisement called [IT Challenge for Charity](https://itchallengeforfuture.postfinance.ch/). PostFinance set up a challenge for a charitable cause. They asked for the shortest path in the graph. In the beginning, I thought that this would be a simple task since I encounter optimization problems like these in almost every math course. So I accepted this challenge.

![Map](https://github.com/fardage/PostFinance-Challenge/raw/main/img/dependencyMap.png)

## Dijkstra's algorithm

For the shortest direct path, I implemented Dijkstra's algorithm in Java. I got the first result but when I submitted my solution (79.56) to the website, I was told that my result was wrong. So I suspected one has to visit all nodes to get the right result.

![Coordinates](https://github.com/fardage/PostFinance-Challenge/raw/main/img/coordinates.png)

## Travelling Salesman Problem

Words like "shortest Path" & "visiting all nodes" always reminds me of the Travelling Salesman Problem. Time is precious in times of university. To avoid having to implement an algorithm from scratch, I went looking for a Python library. Shortly, I came across [mlrose](https://mlrose.readthedocs.io/en/stable/). This module features various algorithms to solve optimisation problems. I picked the genetic algorithm to solve my TSP.

I ran into this issue, where their definition of a travelling salesman problem, no start and destination nodes are given. However, this problem can be easily circumvented by introducing a dummy edge between the start and destination with a path cost of zero.

mlrose unfortunately ran indefinitely and found no solution. The reason for it was that it is not possible to visit all nodes in this constellation without visiting one node twice. Another approach was needed.

## Floyd-Warshall

With the Floyd-Warshall algorithm, all best distances to all nodes are calculated and returned as an adjacency matrix. Using this matrix one can brute force all permutations and compute the best shortest path. To keep the number of permutations small I divided the graph into subgraphs.

```
Shortest Path - Visiting all

Subgraph 1: 35.03432834262672
Subgraph 2: 38.39545497883789
Subgraph 3: 49.310871849778685
Subgraph 4: 51.43259519936775

Best Sum: 174.17325037061107
```

![Graph](https://github.com/fardage/PostFinance-Challenge/raw/main/img/my_graph.png)

Sadly the submission site still would not accept my solution. So, I wrote some tests to verify my results and debug my code. But still, I could not find my mistake. After the challenge ended, I reached out to PostFinance and asked for the solution. They kindly replied and told me that the correct result would have been 79.56! So I was right all along with my first result, and their site likely had an issue with validating results.

- [Github](https://github.com/fardage/PostFinance-Challenge)