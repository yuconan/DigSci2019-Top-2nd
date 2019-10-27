# DigSci2019 科学数据挖掘大赛  Final Top-2
###  作者 : DOTA  
#### 分数( MAP@3 ):0.53733 (Rank2) 

### 知乎：https://zhuanlan.zhihu.com/p/88257675

**方法** :   
>在解决本问题时，我借鉴了推荐算法的思想，将问题拆解了两部分——召回和排序。在召回阶段，使用了两种方式，其一是利用Wrod2Vec和TFIDF方法，将描述段落利用Word2Vec得到每个词的词向量，同时对句子中的词使用TF-IDF为权重进行加权得到Sentence Embedding，同时为了得到更好的效果，这里做了一个改进，即使用Smooth Inverse Frequency代替TFIDF作为每个词的权重；其二是利用TFIDF得到Sentence Embedding。两种方法各自计算余弦相似度得到3篇论文，去重后召回集中每个段落有3-6篇不等的召回论文。  
在排序阶段，我们利用BERT对描述段落Description和论文文本PaperText组成句子对（Description，PaperText）进行编码，在输出层经过Dense和Softmax层后得到概率值后排序。
>
**模型** : Word2Vec,TF-IDF,BERT  
**测试环境** : Ubuntu18,CPU32核,内存64G,两块显卡RTX2080Ti  

**模型说明** :
>从任务描述中我们可以看到，该任务需要对描述段落匹配三篇最相关的论文。单从形式上可以理解为这是一个“完形填空”任务。但相较于在本文的相应位置上填上相应的词语不同的是，这里需要填充的是一个Sentence，也就是论文的题目。但是如果你按照这个思路去寻求解决方案，你会发现在这个量级的文本数据上，一般算力是满足不了的。
既然如此，那我们不如换一个思路来思考这个问题，“对描述段落匹配三篇最相关的论文”，其实最简单的实现方式是计算描述段落和论文库里所有论文的相似度，找出最相似的即可。但这同样会存在一个问题，通过对数据进行探查你会发现“An efficient implementation based on BERT [1] and graph neural network (GNN) [2] is introduced.”这一描述段落，同时引用了两篇文章，那么在计算相似度时，到底哪个位置该是哪篇文章呢？

**代码说明** :  
**1、RecallPart**：两种方法各自计算余弦相似度得到3篇论文，去重后召回集中每个段落有3-6篇不等的召回论文；  
**1.1 ProSolution1** : 利用TF-IDF计算相似度召回3篇论文；   
**1.2 ProSolution2** : 利用DOTA-EmbeddingVector计算相似度召回3篇论文  
**2、SortPart**：利用BERT利用Encoder描述段落和候选论文，计算相似度；

**任务背景**  
科学研究已经成为现代社会创新的主要动力。大量科研数据的积累也让我们可以理解和预测科研发展，并能用来指导未来的研究。论文是人类最前沿知识的媒介，因此如果可以理解论文中的数据，可以极大地扩充计算机理解知识的能力和范围。  
在论文中，作者经常会引用其他论文，并对被引论文做出对应描述。如果我们可以自动地理解、识别描述对应的被引论文，不仅可以加深对科研脉络的理解，还能在科研知识图谱、科研自动问答系统和自动摘要系统等领域有所进步。  

**任务描述**  
本次比赛将提供一个论文库（约含20万篇论文），同时提供对论文的描述段落，来自论文中对同类研究的介绍。参赛选手需要为描述段落匹配三篇最相关的论文。

例子：

描述段落：

An efficient implementation based on BERT [1] and graph neural network (GNN) [2] is introduced.

相关论文：

[1] BERT: Pre-training of deep bidirectional transformers for language understanding.

[2] Relational inductive biases, deep learning, and graph networks.

