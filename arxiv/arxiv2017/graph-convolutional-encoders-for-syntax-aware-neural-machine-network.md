# Graph Convolutional Encoders for Syntax-aware Neural Network Machine Translation

Joost Bastings, Ivan Titov, Wilker Aziz, Diego Marcheggiani, Khalil Sima'an

ILLC, University of Amsterdam

## Intro

以往的NMT Neural Machine Translation在语义表现上有缺陷，因为缺少能够从encoder中获取语义的简单有效方法。

前面已有的方法，或者不直接，或者在语义和翻译任务上有限制，这篇paper提出了给encoder一个能够提供丰富语义的方法，而且没有语义和翻译任务的限制。

attention-based NMT系统，将源句子表示成隐藏向量，然后通过这些向量翻译。而我们的任务就是能够将源句子中的每个单词周围邻居的语义信息考虑在内，并提供给encoder，从而提高翻译质量。既然单词是用向量表示，那么可以考虑使用依赖语义，依赖树，如图1所示，表示的是单词之间的关系。

使用graph-convolutional network产生单词的特征，也就是encoder。我们使用syntactic GCN，是构造与语义依赖树上的GCN。

## Background

### Neural Machine Translation

NMT的任务是给定一个翻译“对”，也就是两种平行的词库，来获取条件概率$$P(y|x)$$，NMT分为三个部分，encoder，decoder，和一个encoder转换到decoder的方式，比如attention mechanism。

#### Encoder

将源句子作为输入，输出的是包含语法含义的表示。有三种：recurrent，convolutional，和bag-of-words encoder

1. Recurrent：每一个时间点收到一个向量，更新因此状态。给定输入的序列，word embedding，RNN这么定义：$$RNN(x_{1:t}) = f(x_t, RNN(x_{1:t-1}))$$。f是一个非线性的函数。有时候为了考虑future，会经常使用bidirectional RNN，也就是同时训练两个方向：$$BIRNN(x_{1:T_x},t) =  RNN_F(X_{1:t}) + RNN_B(X_{T_x:t}) $$。
2. Convolutional：在输入的数据上使用了fixed-sized window，想比如RNN的特点是可以高速平行计算，而放弃了非本地的上下文。为了弥补这个上下文的损失，会stack好几层CNN。
3. Bag-of-Words：BoW encoder每一个单词都被表示成word embedding。为了给decoder位置信息，会加上position embedding(PE)。有不同的方法定义位置，这篇paper使用了absolute position。

#### Decoder

decoder可以通过condition on context vector，来产生输出的序列。context vector会随着时间更新。

### Graph Convolutional Networks

GCN是操作于图上的convolutional net，信息沿着图中的边传递。当stack几层GCN layer的时候，larger neighbourhoods的信息会被收集。

比如第一层的GCN，new node representation通过如下计算 $$h_v = \rho ( \sum_{u \in N(v)} W x_u + b )$$。

其中$$\rho$$是activation function。所以stacked的结果如下：

$$h_v^{j+1} = \rho ( \sum_{u \in N(v)} W^j h_u^j + b^j )$$

### Syntatic GCNs

今年年初有人放了篇paper，关于GCN在directed和labeled graph上的工作。可以将这个概念沿用到依赖树上。将无向边分解为两条有向边，$$h_v^{j+1} = \rho ( \sum_{u \in N(v)} W_{dir}^j h_u^j + b_{dir}^j )$$。

既然已经引入了方向，那么很自然的可以引入label。$$h_v^{j+1} = \rho ( \sum_{u \in N(v)} W_{lab}^j h_u^j + b_{lab}^j )$$。不过为了避免参数过多，只是对bias项使用了label数目。

## Graph Convolutional Encoders

尝试了三种模型，BoW+GCN, Convolutional+GCN, BiRNN+GCN。

1. BoW + GCN: 输入的隐藏层，是每一个单词的embedding与在句子中位置的和。
2. Convolutional + GCN: 用convolutional network学习word representation。但是与前面stacked CNN不同，我们是将把GCN放在一层CNN上。
3. BiRNN + GCN: 对源句子用BiRNN编码，将产生的hidden state作为GCN的输入。GCN还支持单词之间的“远程连接”。

# Appendix