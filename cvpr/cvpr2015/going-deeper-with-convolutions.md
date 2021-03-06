# Going Deeper with Convolutions

Christian Szegedy, etc

# Intro

基于ImageNet Large-Scale Visual Recognition Challenge (ILSVRC14)，提出了一个新的模型GoogLeNet。

# Motivation and High Level Considerations

为了是的模型性能提高，最常用的增加depth和width，但是会有两个问题：overfitting和computation resources。

一个解决方案是引入sparsity，并且fully-connected layer上使用sparse。有证明，如果一个dataset的概率分布由一个非常大，非常深的network表示，那么最优的topology可以由层层叠加的layer表示，叠加的方式可以通过分析前一层layer的activations的correlation statistics和输出高度相关的clustering neurons。

# Inception Architecture

Inception的主要目标是考虑一个optimal local sparse structure能够被估计，并且被dense components覆盖。

假设每一层每一个unit都对应image中的某一个区域，并且这些units被分配到了filter banks。

因为计算资源过于昂贵，所以引入另外一个概念：dimensionality reduction。这是受到了embedding的启发，将sparse的信息非常密集的存储起来。

# GoogLeNet

是在ILSVRC上使用的Inception模型的专门名称。具体参数在表1中。

# Appendix

这篇paper在15年发表，当时使用的还是DistBelief，这几年就已经更新到TensorFlow，MXNet，PyTorch。