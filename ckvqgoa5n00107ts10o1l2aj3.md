## Guideline for Warehouse Robots Path Searching: A Predictive Method

Multiple robots in large warehouses need well-tailored path searching algorithms and conflict resolution mechanism to perform package pickup and delivery tasks. Here is an outline of the key points in my solution to this problem.


## Path Generating

#### Single robot path finding:
1. A star:  [A Star Heuristic](https://zach-sudo.hashnode.dev/a-heuristic-and-dijkstra-performance-comparison) 
2. Dijkstra:  [Dijkstra for Weighted Graphs](https://zach-sudo.hashnode.dev/dijkstras-algorithm-in-weighted-graphs) 
![dijkstra_astar.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1636358140404/AMBYZFUmN.png)


#### Multiple robots path searching:
1. Conflict resolution mechanism: local search
Drawback: only knows when happens, cannot avoid in advance
![conflict.001.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1636362444287/xTdUXvOCw.png)
2. Use future prediction: heat map prediction + time-series modelling

![pred.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1636363579064/kCCzV-KZL.png)


#### Predictive path searching
1. Path searching in dynamic heat map prediction
