## Path Finding in Warehouses

## Implementation and Visualization of Breadth-First Search in Grid Maps

Takeaway techs:

- Breadth-First Search
- Python Visualization
- Python Networkx package

> Breadth-first search (BFS) is a very commonly used algorithm for graph and tree search. It starts from the tree root (start node in graph)   and explores all nodes at the present depth (neighbour nodes in graph) prior to moving on to the nodes at the next depth level. 
>
> ----*Wikipedia*

This algorithm appears not only in regular path finding, but also in many other types of map analysis. The article from [*Red Blob Games*](https://www.redblobgames.com/pathfinding/a-star/introduction.html#breadth-first-search) gives detailed explanation with impressive animations on how the BFS works in finding shortest path in grid maps. Here we avoid the repetition on the theory, but shows how the algorithm can be used in simple node-to-node path finding algorithm in our entire warehouse project. 

If we leave what behind the algorithm, it simply takes three things as inputs: a graph object (possibly with weights), start node and goal node, and it returns a shortest path and the corresponding cost. Here is the pseudo-code

```python
from SomeLibrary import Graph, BFS
class Graph(Grid2D):
  	class Meta:
      size = (5, 5)

graph = Graph()
start_node = (0, 0)
goal_node = (3, 4)
shortest_path, cost = BFS(graph, start_node, goal_node)
print(shorest_path, cost)
```






