# [论文研读]BioBERT: a pre-trained biomedical language representation model for biomedical text mining

## 一、导读

### 1.1 动机

随着生物医学文档数量的快速增长，生物医学文本挖掘变得越来越重要。随着自然语言处理技术的进步，从生物医学文献中提取有价值的信息越来越受到研究者的青睐，深度学习促进了有效的生物医学文本挖掘模型的发展。然而，直接将自然语言处理的进展应用到生物医学文本挖掘中，由于单词分布从一般领域语料库向生物医学语料库的转移，往往会产生不令人满意的结果。在这篇文章中研究了最近引入的预训练语言模型BERT如何适用于生物医学语料库。



## 1.2 结果

引入BioBERT，这是一个在大规模生物医学语料库上预先训练的特定领域语言表示模型。当在生物医学语料库上进行预训练时，BioBERT在各种生物医学文本挖掘任务中的架构几乎相同，在很大程度上优于BERT和以前的最先进的模型。

结果表明，尤其是NER，RE，QA任务上，在生物医学语料库上预先训练BERT有助于理解复杂的生物医学文本。



## 二、方法

作者所述贡献主要如下：

- 是第一个基于生物医学语料库的特定领域BERT模型；
- 表明在生物医学语料库上对BERT进行预训练可以大大提高其性能；
- BioBERT模型在各种生物医学文本挖掘任务上实现了最先进的性能，同时只需要最小的体系结构修改。（Pre-training）
- 公开了代码（We make the pre-trained weights of BioBERT freely available at https://github.com/naver/biobert-pretrained, and the source code for fine-tuning BioBERT available at https://github.com/dmis-lab/biobert.）



### 2.1 Pre-training BioBERT

<img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201118200817642.png" alt="image-20201118200817642" style="zoom:80%;" />



<img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201118200927775.png" alt="image-20201118200927775" style="zoom:80%;" />



- 利用WordPiece解决OOV
- 使用区分大小写的英文，而非全部小写
- BERT的参数作为BioBERT的初始参数



### 2.2 Fine-tuning BioBERT

主要介绍了NER，RE，QA任务上如何进行微调，不再展开。



## 三、实验

### 3.1 NER

<img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201118204230729.png" alt="image-20201118204230729" style="zoom:67%;" />

### 3.2 RE

<img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201118204321721.png" alt="image-20201118204321721" style="zoom:67%;" />



### 3.3 QA

<img src="C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201118204340437.png" alt="image-20201118204340437" style="zoom:67%;" />



### 3.4 其他信息

![image-20201118212815542](C:\Users\sishu\AppData\Roaming\Typora\typora-user-images\image-20201118212815542.png)



## 四、小结

本文主要介绍了BioBERT，这是一个在大规模生物医学语料库上预先训练的特定领域语言表示模型，本身可能没什么新意，其主要是给出了在生物医疗语料上训练会得到更好的效果的结果，这也是符合直觉的，且开源了代码。