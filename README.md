# text-semantics
解决文本语义相似的核心是让模型理解什么是语义。本文先介绍embedding之前有哪些文本相似方法，embedding之后文本相似的发展，进一步尝试对语义下定义，最后给出一个好的语义相似模型，应该具有哪些最基本能力。
## embedding前的文本相似
1 编辑距离

2 simhash

3 bm25

4 杰卡德相似系数

5 tf-idf

还有很多其他算法，这些算法有各自的优点，但同时都有共同的不足。如：句子一：序列标注；句子二：字符分类。这两句话是很相似的，用以上算法很难解。
## embedding后的文本相似
### 1 word2vec

说embedding，word2vec是绕不过去的，其建模的内在是基于一个假设：一个词的embedding是由其上下文决定，即词频共现。把词的embedding加起来或连起来做相似。


### 2 Skip-Thought https://arxiv.org/pdf/1506.06726v1.pdf

通过预测一句话的上一句和下一句建模，可以理解一句话的语义是由其上下文共同决定。

![image](https://user-images.githubusercontent.com/39753454/142753747-dc4aeb42-5e6d-4570-a88c-6991e06b1054.png)


### 3 Quick Thought https://arxiv.org/abs/1803.02893

其表示语义的方式较 Skip-Thought有所精简，但还是从上下文入手。

![image](https://user-images.githubusercontent.com/39753454/142754036-c51b1b63-1948-4de5-a9e4-1d7664ba2245.png)

### 4 infersent https://www.aclweb.org/anthology/D17-1070.pdf

infersent 用于蕴含推理，也可以从中看到，语义应该具有对细节的反应。

![image](https://user-images.githubusercontent.com/39753454/142754232-7d9d8e67-21bf-46d0-a113-48ddd7609660.png)


### 5 metrics learning https://arxiv.org/pdf/1604.01325.pdf

以triplet loss为代表，也为何为语义添了精彩一笔。

![image](https://user-images.githubusercontent.com/39753454/142754499-1e77a75e-f7b3-4ef4-b410-a1f6cca55e8a.png)


### 6 预训练 + 多任务

现在在大规模预料上预训练，然后结合多任务去表示语义，研究的成果很多（bert簇，GPT簇）。


### 7 预训练 + 多模态 + 多任务

Oscar: Object-Semantics Aligned Pre-training for Vision-Language Tasks

![image](https://user-images.githubusercontent.com/39753454/142755110-5e3566d4-af4c-4988-b66c-b551507d9044.png)


## 什么是语义

到底什么是语义？语言学没有明确定义。以图搜图，从全局embding到局部特征，再到局部特征和全局结合；文本搜索，从w2v，到大规模预训练、多任务结合；多摸搜索，从全局embding，到局部特征，语义对齐，再到预训练多任务。从这些任务（还有其他任务）的发展历程中，我们可以试着给语义下一个定义。
语义：如果一个模型会做一些事情，那么即理解了语义。比如模型会做阅读理解，会做完形填空，模型会做蕴含推理，模型会对比相似，模型会做语义对齐等等

## 好的语义学习模型应该具备哪些能力


| 能力    | 例子 | 说明 |
| --------|:-----: |:---:   |
| 细节感知能力| s1:她找了他20年，她爱他 s2:她找了他20年，她恨他| 模型应该对这两句话有区分性|
| 相似分级| | 模型应该对语义相似分级，而不是只有相似和不相似|
| 同义不同字符| s1:序列标注 s2:字符分类| s1和s2很相似，模型应该很好解决这类问题。即倒排索引能召回的，模型能召回，倒排索引召不回的模型也能召回|
| 同字符不同义| s1:我爱吃螃蟹 s2:螃蟹爱吃我| s1和s2相似度不高，模型应该很好解决这类问题。|
| 低代价| | 人工标注代价大，上千万，上亿的文本标注，很贵|
| 持续进步| | 模型应该持续的从不同文章中学到新知识|
