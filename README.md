## pytorch-light-GCN

*still testing*

This is our Pytorch implementation for the paper:

>Xiangnan He, Kuan Deng ,Xiang Wang, Yan Li, Yongdong Zhang, Meng Wang(2020). LightGCN: Simplifying and Powering Graph Convolution Network for Recommendation, [Paper in arXiv](https://arxiv.org/abs/2002.02126).

Author: Prof. Xiangnan He (staff.ustc.edu.cn/~hexn/)

(Also see Tensorflow [implementation](https://github.com/kuandeng/LightGCN))

## Introduction

In this work, we aim to simplify the design of GCN to make it more concise and appropriate for recommendation. We propose a new model named LightGCN,including only the most essential component in GCN—neighborhood aggregation—for collaborative filtering



## Enviroment Requirement

`pip install -r requirements.txt`



## Dataset

We provide three processed datasets: Gowalla, Yelp2018 and Amazon-book and one small dataset LastFM.

see more in `dataloader.py`

## An example to run a 3-layer LightGCN

run LightGCN on **Gowalla** dataset:

* command

` python main.py --decay=1e-4 --lr=0.001 --layer=3 --dataset="gowalla" --topks=[20] --recdim=64`

* log output

```shell
...
======================
EPOCH[5/1000]
BPR[sample time][16.2=15.84+0.42]
[saved][[BPR[aver loss1.128e-01]]
[0;30;43m[TEST][0m
{'precision': array([0.03315359]), 'recall': array([0.10711388]), 'ndcg': array([0.08940792])}
[TOTAL TIME] 35.9975962638855
...
======================
EPOCH[116/1000]
BPR[sample time][16.9=16.60+0.45]
[saved][[BPR[aver loss2.056e-02]]
[TOTAL TIME] 30.99874997138977
...
```

*NOTE*:

1. Even though we offer the code to split sparse matrix for matrix multiplication, we strongly suggest you don't enable it since it will extremely slow down the training speed.
2. If you feel the test process is slow, try to increase the ` testbatch` and enable `multicore`(Windows system may encounter problems with `multicore` enabled)
3. Use `tensorboard` option, it's good.
4. Since we fix the seed of `numpy` and `torch` in the beginning, if you run the command as we do above, you should have the exact output log despite the running time (check your output of *epoch 5* and *epoch 116*).


## notes:

code structure is below.

```shell
code
├── parse.py
├── Procedure.py
├── dataloader.py
├── main.py
├── model.py
├── utils.py
└── world.py
```

if you want to run lightGCN on your own dataset, you should go to `dataloader.py`, and implement a dataloader.

## Results
*tensorflow* version results:
![](imgs/tf.jpg)

*pytorch* version results

gowalla:

|             | Recall in paper | Recall in `torch` |  ndcg in paper    | ndcg in `torch` |
| ----------- | ---------------------------- | ----------------- | ---- | ---- |
| **layer=1** | 0.1726                       | 0.1692            |      |      |
| **layer=2** | 0.1786                       | 0.1783            |      |      |
| **layer=3** | 0.1809                       | 0.1826            |      |      |
| **layer=4** | 0.1817                       | 0.1826            |      |      |



