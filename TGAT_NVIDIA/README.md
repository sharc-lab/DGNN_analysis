# TGAT Profiling Using NVIDIA Nsight Systems

This repository contains the code for Dynamic Graph Neural Networks on Hardware: Bottleneck Analysis, published in IISWC 2022.

## Data

The dataset used for TGAT is Wikipedia. It can be downloaded here: http://snap.stanford.edu/jodie/wikipedia.csv. 

## NVIDIA Nsight Systems download

Please download this profiling tool usig this link: https://developer.nvidia.com/nsight-systems

## Running the experiments


#### Preprocess the data
We use the dense `npy` format to save the features in binary format. If edge features or nodes features are absent, it will be replaced by a vector of zeros. 
```{bash}
python process.py 
```

### Requirements

* python >= 3.7

* Dependency

```{bash}
matplotlib==3.5.2
numpy==1.21.2
pandas==1.4.2
PyYAML==6.0
scikit_learn==1.1.2
scipy==1.8.1
torch==1.10.1
torchvision==0.11.2
tqdm==4.64.1
```

### Command and configurations

#### Sample commend

* To do the profiling on GPU
```{bash}
# TGAT profiling on wikipedia data
nsys profile -w true -t cuda,nvtx,osrt,cudnn,cublas -s cpu --capture-range=cudaProfilerApi --capture-range-end=stop --cudabacktrace=true -x true --cuda-memory-usage=true python -u learn_edge.py -d wikipedia --bs 200 --uniform  --n_degree 20 --agg_method attn --attn_mode prod --gpu 0 --n_head 2 --prefix hello_world

#### General flags

```{txt}
optional arguments:
  -h, --help            show this help message and exit
  -d DATA, --data DATA  data sources to use, try wikipedia or reddit
  --bs BS               batch_size
  --prefix PREFIX       prefix to name the checkpoints
  --n_degree N_DEGREE   number of neighbors to sample
  --n_head N_HEAD       number of heads used in attention layer
  --n_epoch N_EPOCH     number of epochs
  --n_layer N_LAYER     number of network layers
  --lr LR               learning rate
  --drop_out DROP_OUT   dropout probability
  --gpu GPU             idx for the gpu to use
  --node_dim NODE_DIM   Dimentions of the node embedding
  --time_dim TIME_DIM   Dimentions of the time embedding
  --agg_method {attn,lstm,mean}
                        local aggregation method
  --attn_mode {prod,map}
                        use dot product attention or mapping based
  --time {time,pos,empty}
                        how to use time information
  --uniform             take uniform sampling from temporal neighbors
  --use_cuda            choose to use GPU or CPU for profiling
  --start_profiling     whether to start profiling
  --full_dataset        whether to profile on the whole dataset/choose the number of batches for profiling
  --dataset_type        choose which part of the dataset for profiling(train/val/test)
  --n_batch             the number of batches for profiling if not using the whole dataset.
```



