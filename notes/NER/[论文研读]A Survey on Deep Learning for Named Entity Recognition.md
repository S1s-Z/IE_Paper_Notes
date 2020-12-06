# [论文研读]A Survey on Deep Learning for Named Entity Recognition

## 一、导读

这篇文章是一篇关于NER的综述文章，文章比较新，主要介绍了DL在NER中的应用。



## 二、方法

### 2.1 什么是NER

命名实体是一个词或短语，它从一组具有相似属性的其他项目中清楚地识别出一个项目。NER是定位和分类文本中命名实体到预定义实体类别的过程。

<img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201117143222498.png" alt="image-20201117143222498" style="zoom:80%;" />

一般来说给定一串token$s=<w_1,w_2,...,w_N>$，NER的输出是一串元组$I_s，I_e,t$，$I_s$和$I_e$代表实体内容的起点和终点，$t$代表预先定义好的实体的分类。NER包括：粗略的NER;细致的NER。前者每个命名实体一种类型；后者关注更大的实体类型集合，其中一个实体可以被分配多个细粒度类型。



### 2.2 常见的数据集与工具

<img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201117144750687.png" alt="image-20201117144750687" style="zoom: 80%;" />

<img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201117144916145.png" alt="image-20201117144916145" style="zoom:80%;" />



### 2.3 NER评估指标

![image-20201117151914085](C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201117151914085.png)

![image-20201117151923160](C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201117151923160.png)



### 2.4 Distributed Representations for Input

- Word-level Representation：常见的embedding的方法有Word2Vec、GloVe、fastText、SENNA。值得一提的是Bio-NER，Bio-NER中的单词表示是在PubMed数据库上使用skip-gram训练的。

- Character-level Representation：字符级别的表示主要通过CNN和RNN得到

  <img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201117155256442.png" alt="image-20201117155256442" style="zoom:80%;" />

  可以自然地处理词汇外的信息；字符级表示对于利用明确的子单词级信息(如前缀和后缀)很有用

- Hybrid Representation：除了单词级和字符级表示，一些研究还将附加信息(例如地名索引、词汇相似性、语言依赖性和视觉特征)结合到单词的最终表示中，然后馈入上下文编码层。换句话说，基于DL的表示是以混合的方式与基于特征的方法相结合的。添加额外的信息可能会提高NER性能，但代价是损害这些系统的通用性。BERT也可以理解为一种Hybrid Representation。

  

### 2.5 Context Encoder Architectures

主要有：

- Convolutional Neural Networks

  <img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201117173309139.png" alt="image-20201117173309139" style="zoom: 80%;" />

- Recurrent Neural Networks

  <img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201117173324703.png" alt="image-20201117173324703" style="zoom:80%;" />

- Recursive Neural Networks

  <img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201117173337729.png" alt="image-20201117173337729" style="zoom:80%;" />

- Neural Language Models

  <img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201117173419467.png" alt="image-20201117173419467" style="zoom:80%;" />

  <img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201117173509536.png" alt="image-20201117173509536" style="zoom:80%;" />

- Deep Transformer

<img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201117173448057.png" alt="image-20201117173448057" style="zoom:80%;" />

### 2.6 Tag Decoder Architectures

标签解码器（Tag Decoder）是NER模型的最后阶段。它将依赖于上下文的表示作为输入，并产生对应于输入序列的标签序列。

<img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201117175012864.png" alt="image-20201117175012864" style="zoom:67%;" />

主要有：

- Multi-Layer Perceptron + Softmax
- CRF
- Recurrent Neural Networks
- Pointer Networks



<img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201117180452675.png" alt="image-20201117180452675" style="zoom:80%;" />



### 2.7 APPLIED DEEP LEARNING FOR NER

以下是一些有趣的为NER应用的DL技术：

- Deep Multi-task Learning for NER
- Deep Transfer Learning for NER
- Deep Active Learning for NER
- Deep Reinforcement Learning for NER
- Deep Adversarial Learning for NER
- Neural Attention for NER



## 三、实验

综述文章，无实验



## 四、小结

本文主要介绍了NER中主要的数据集和一些SOTA结果和方法论，可以作为了解新技术的索引。