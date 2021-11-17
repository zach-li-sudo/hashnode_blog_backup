## Heat Map Time Series Modelling and Prediction using PyTorch

When planning shortest paths for multiple robots in a large warehouse, we can make use of the prediction of future heat maps to detect possible congestion, which our smart robots can avoid before getting to the spot. This article is main about a heat map prediction module I designed for the warehouse project.

Here is a demo of this LSTM predictor (hosted on [my GitHub repo](https://github.com/zach-li-sudo/mapPred)):

<img src="https://github.com/zach-li-sudo/mapPred/raw/master/tensor_pt/mygif.gif">

Now let's see how to do it!

(1) Generate Map Sequence Data
   We suppose that the heat value of each box of our heat map is generated as a sine curve with noises:

   $$
   h_i=10A\sin(0.01t) + \vert B\vert + 0.1A*\text{random()}
   $$

   Then we add these heat values to a list of `nx.Graph()` to generate a heat map sequence.

```python
def heatmap_sequence_generator(t, params, grid_size):
    A = params[:, 0]
    B = params[:, 1]

    y = 10*A.dot(np.sin(0.1*t))
        + np.abs(B) + 0.1*A*random()

    unweighted_grid_graph = nx.grid_2d_graph(
                            grid_size[0],
                            grid_size[1]
                            )
    unweighted_grid_graph.pos = dict((n,n) for n in unweighted_grid_graph.nodes())
    dynamic_graph = nx.Graph()

    i = 0
    for edge in unweighted_grid_graph.edges():
        heat = y[i].item()
        i += 1
        dynamic_graph.add_edge(edge[0],
                                edge[1],
                                weight=heat)

    return dynamic_graph

l = 200
params = np.random.random(size=(nums, 2))
list_of_graphs = [heatmap_sequence_generator(i, params, grid_size) for i in range(l)]
```

(2) Plug the heat map sequence into a `GraphPredictor` instance

```python
from gp import GraphPredictor
from time import time

gp = GraphPredictor(list_of_graphs)
```

Before training the lstm network, we first need to convert the sequence of `nx.Graph()` into `torch.tensor()` and split them into data batches.

```python
t = time()
gp.convert_graph_list_to_tensor_batch()
print("converting time:\t", time() - t)
```

Then train the lstm net!

```python
t = time()
gp.train(my_epoch=40)
print("training time:\t", time() - t)
```

(3) Fitting, prediction, and error plots 


   Find the plots:
  
![Screen Shot 2021-11-17 at 4.14.56 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1637136930256/qCKM69ZMr.png)

  

(4) Map prediction visualization 
    During the training and predicting process, heat map data is save as pytorch tensor file to `tensor_pt/results_tensors.pt`. Executing `mapPred_visualization.py` script creates an animation file to visualize our results.
   <img src="https://github.com/zach-li-sudo/mapPred/raw/master/tensor_pt/mygif.gif">
