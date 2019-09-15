# GraphNAS

#### Overview
This directory contains code necessary to run the GraphNAS algorithm. Graph Neural Architecture Search method (GraphNAS 
for short) enables automatic design of the best graph neural architecture based on reinforcement learning. 
Specifically, GraphNAS first uses a recurrent network to generate variable-length strings that describe the 
architectures of graph neural networks and then trains the recurrent network with a policy gradient algorithm 
to maximize the expected accuracy of the generated architectures on a validation data set.

<p align="center">
<img src="./images/macro_search.png" width="500"  alt="A simple illustration of GraphNAS" align=center>
</p>
An illustration of GraphNAS. A recurrent network (Controller RNN) generates descriptions of graph neural architectures (Child model GNNs). Once an architecture m is generated by the controller, GraphNAS trains m on a given graph G and test m on a validate set D. The validation result R is taken as the reward of the recurrent network.



#### Requirements
Recent versions of PyTorch, numpy, scipy, sklearn, dgl, torch_geometric and networkx are required.
Ensure that at least PyTorch 1.0.0 is installed. Then run:
>  pip install -r requirements.txt

If you want to run in docker, you can run:
>  docker build -t graphnas -f DockerFile . \
>  docker run -it -v $(pwd):/GraphNAS graphnas python main.py --dataset cora

#### Running the code
##### Architecture evaluation
To evaluate our best architecture found in semi-supervised experiments by training from scratch, run
> python -m eval_scripts.semi.eval_found_gnn

To evaluate our best architecture found in semi-supervised experiments by training from scratch, run
> python -m eval_scripts.sup.eval_found_gnn
###### Results
Semi-supervised node classification w.r.t. accuracy

Model| Cora | Citeseer | Pubmed
|-|-|-|-| 
GCN    | 81.5$\pm$0.4 | 70.9$\pm$0.5   | 79.0$\pm$0.4  
SGC    |  81.0$\pm$0.0 |   71.9$\pm$0.1   |  78.9$\pm$0.0   
GAT    |  83.0$\pm$0.7  |  72.5$\pm$0.7   | 79.0$\pm$0.3    
LGCN    |  83.3$\pm$0.5  | 73.0$\pm$0.6    |  79.5$\pm$0.2   
DGCN    |  82.0$\pm$0.2  | 72.2$\pm$0.3    |  78.6$\pm$0.1   
ARMA    |  82.8$\pm$0.6  | 72.3$\pm$1.1    |  78.8$\pm$0.3   
APPNP   |  83.3$\pm$0.6  | 71.8$\pm$0.4    |  80.2$\pm$0.2   
simple-NAS |  81.4$\pm$0.6  |  71.7$\pm$0.6    |  79.5$\pm$0.5  
GraphNAS | \textbf{84.4$\pm$0.4}  | \textbf{73.5$\pm$0.3}    | \textbf{80.5$\pm$0.3}    
		
Supervised node classification w.r.t. accuracy	

Model| Cora | Citeseer | Pubmed  
|-|-|-|-| 
GCN    | 90.2$\pm$0.0  | 80.0$\pm$0.3   | 87.8$\pm$0.2  
SGC    | 88.8$\pm$0.0 |  80.6$\pm$0.0   |   86.5$\pm$0.1  
GAT    |  89.5$\pm$0.3  |  78.6$\pm$0.3    |  86.5$\pm$0.6   
LGCN    | 88.7$\pm$0.5  | 79.2$\pm$0.4     |  OOM    
DGCN    |  88.4$\pm$0.2  |  78.0$\pm$0.2    |  88.0$\pm$0.9    
ARMA    |  89.8$\pm$0.1  |  79.9$\pm$0.6    |  88.1$\pm$0.2    
APPNP    | 90.4$\pm$0.2  | 79.2$\pm$0.4     | 87.4$\pm$0.3    
random-NAS | 90.0$\pm$0.3   |  81.1$\pm$0.3    | 90.7$\pm$0.6    
simple-NAS | 90.1$\pm$0.3  |  79.6$\pm$0.5    |  88.5$\pm$0.2  
GraphNAS | \textbf{90.6$\pm$0.3}   |  \textbf{81.2$\pm$0.5}   | \textbf{91.2$\pm$0.3}     
	
Supervised learning on randomly split training data

Model| Cora | Citeseer | Pubmed  
|-|-|-|-| 
GCN    | 88.3$\pm$1.3 | 77.2$\pm$1.7   | 88.1$\pm$1.4  
SGC    | 88.2$\pm$1.4  |  77.4$\pm$1.8   |  85.8$\pm$1.2      
GAT    | 87.2$\pm$1.1   |   77.1$\pm$1.3   | 87.8$\pm$1.4    
LGCN    | 87.9$\pm$1.5   |   76.6$\pm$1.6   | OOM    
DGCN    | 87.8$\pm$1.3  |   74.4$\pm$1.7   |  88.4$\pm$1.2    
ARMA    | 88.2$\pm$1.0  |   76.7$\pm$1.5   |  88.7$\pm$1.0    
APPNP    | 87.5$\pm$1.4   |  77.3$\pm$1.6    |  88.2$\pm$1.1    
random-NAS |  88.5$\pm$1.0  |  76.5$\pm$1.3    | 90.3$\pm$0.8   
simple-NAS |  88.5$\pm$1.0  |   77.5$\pm$2.3   |  88.5$\pm$1.1    
GraphNAS | \textbf{88.9$\pm$1.2}  |  \textbf{77.6$\pm$1.5}    |  \textbf{91.1$\pm$1.0} 
       
##### Searching for new architectures
To carry out architecture search using search space described in Section 3.2, run
> python -m models.common.common_main --dataset Citeseer

To carry out architecture search using search space described in Section 3.4, run
> python -m models.micro_nas.micro_main --dataset Citeseer 



#### Acknowledgements
This repo is modified based on [DGL](https://github.com/dmlc/dgl) and [PYG](https://github.com/rusty1s/pytorch_geometric).
