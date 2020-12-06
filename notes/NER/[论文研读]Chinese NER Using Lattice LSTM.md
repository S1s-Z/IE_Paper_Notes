# [论文研读]Chinese NER Using Lattice LSTM

## 一、导读

本文提出了一种基于格结构的LSTM模型（Lattice LSTM），该模型对输入字符序列以及与词典相匹配的所有潜在单词进行编码。对比传统的字符级别的方法，该模型不存在分词错误，同时模型可以从一个句子中选择最相关的字符和单词，以获得更好的NER结果。对比word-based LSTM 和 character-based LSTM，取得了最好的效果。



### 1.1 发展现状

NER是IE中的一项基本任务，传统上，该任务作为一个序列标记问题来解决，其中实体边界和类别标记是联合预测的。使用LSTM-CRF模型将字符信息集成到单词表示中，实现了英文NER的当前水平SOTA水平。

而中文NER，特别地，命名实体的边界也是词的边界，因此在进行序列标注之前，先进行分词是一种比较直观的中文NER处理方法。

然而，由于命名实体（NEs）是未登录词（OOV）的重要来源，而实体边界分割不正确会导致NER的错误，所以从segmentation到NER这一解决思路会存在错误传播（error propagation）的问题。

因此人们往往使用character-based的方法，而非word-based的方法。

但character-based的方法没有充分利用显式的单词和单词序列信息，而这可能是有用的。



## 二、方法

### 2.1 解决方法

使用一种格LSTM表示句子中的词汇，从而将潜在的单词信息集成到基于character-based的LSTM-CRF中。通过将一个句子与一个词汇库匹配来构造一个单词字符格。

例如："长江"，“长江大桥”，“大桥”就消除了人名“江大桥”的可能性（消除了上下文中潜在的相关命名实体的歧义）。

<img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201202210747261.png" alt="image-20201202210747261" style="zoom:50%;" />

在一个格中有指数级的单词字符路径，可以利用格LSTM结构来自动控制从句子开头到结尾的信息流。

<img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201202210802676.png" alt="image-20201202210802676" style="zoom:50%;" />



### 2.2 模型解释

采用LSTM-CRF作为主要的网格结构。

​             <img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201202212057515.png" alt="image-20201202212057515" style="zoom: 33%;" /><img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201202212119712.png" alt="image-20201202212119712" style="zoom:33%;" /><img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201202212140134.png" alt="image-20201202212140134" style="zoom:33%;" />

传统的LSTM可以数学表示如下：

<img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201203135315089.png" alt="image-20201203135315089" style="zoom:50%;" />

而现在$c_j^c$考虑了词典的subsequences，$w_{b,e}^d$的信息，每个子序列$w_{b,e}^d$被表示为：
$$
x_{b,e}^w=e^w(w_{b,e}^d)
$$
$e^w$提供词的embedding，现在的Lattice LSTM表示如下：

<img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201206213707675.png" alt="image-20201206213707675" style="zoom: 50%;" />

$i_{b,e}^w$和$f_{b,e}^w$表示输入门和遗忘门，因为labeling过程发生在char-level，所以对于word-level来说不存在输出门。

之后对于$c_j^c$我们需要把所有的以$j$结尾的$c_{b,e}^w$连接起来，即$c_{b,e}^w$with$b\in \{b'|w_{b',e}^d\in D\}$，同时我们使用一个额外的门来控制每个subsequence cell$c_{b,e}^w$对$c_{b,e}^c$的作用：

<img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201206215324436.png" alt="image-20201206215324436" style="zoom: 50%;" />

Lattice LSTM引入了一个word cell结构，对于当前的字符，融合以该字符结束的所有word信息，如对于「市」融合了「南京市」的信息；「桥」融合了「长江大桥」和「大桥」的信息。对于每一个字符，Lattice LSTM采取注意力机制去融合个数可变的word cell单元，其主要的数学形式化表达为：

<img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201203135154270.png" alt="image-20201203135154270" style="zoom:33%;" />

<img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201206215416544.png" alt="image-20201206215416544" style="zoom:50%;" />

Decoding是接一个常见的CRF。



## 三、实验



<img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201206221713241.png" alt="image-20201206221713241" style="zoom:50%;" />

<img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201206221731705.png" alt="image-20201206221731705" style="zoom: 50%;" />



在OntoNotes development set下的结果：

<img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201206221434776.png" alt="image-20201206221434776" style="zoom:50%;" />

<img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201206222519326.png" alt="image-20201206222519326" style="zoom:50%;" />

<img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201206222530251.png" alt="image-20201206222530251" style="zoom:50%;" />

<img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201206223955947.png" alt="image-20201206223955947" style="zoom:50%;" />

<img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201206224004942.png" alt="image-20201206224004942" style="zoom:50%;" />

<img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201206224017262.png" alt="image-20201206224017262" style="zoom:50%;" />



## 四、小结

对于中文NER任务，相比于word-based和char-based LSTM-CRF，lattice LSTM完全独立于分词，由于在NER消歧上下文中选择词典单词的自由性，lattice LSTM在使用单词信息方面也更加有效。