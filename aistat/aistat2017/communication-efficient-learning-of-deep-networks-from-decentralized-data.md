# Communication-Efficient Learning of Deep Networks from Decentralized Data

H. Brendan McMahan, Eider Moore, Daniel Ramage, Seth Hampson, Blaise Aguera y Arcas

Google

# Intro

现在的算法都是在大量分布式的数据集上运算，这里提出了一个Federated Learning,一种去中心化的方法。

Federated Learning：有这么几个属性

1. 从实际世界的数据训练，相比于在数据中心的proxy data上训练，有更大的优势
2. 数据是隐私的，而且数据量很大
3. 对于监督式学习，标签可以通过用户的反应来推测

federated learning主要一点是考虑到了unbalanced和non-IID

# The FederatedAveraging Algorithm

SGD可以应用到federated learning的场景，但是需要大量的training epoch。

使用基于data center设定的large-batch sync SGD作为baseline，而且比async结果好。把这个baseline algorithm叫做FederatedSGD(FedSGD)。

简单的FedSGD是每一个worker都把基于local data的average gradient计算之后，传给central server，然后central server再次进行aggregate。因此可以有另外一种想法，就是每一个client多执行几次gradient计算。把这个方法叫做FederatedAveraging(FedAvg)。