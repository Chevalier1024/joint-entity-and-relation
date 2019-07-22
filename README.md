# joint-entity-and-relation
paper notes about joint extraction of entity and relation

# dataset
- [WebNLG](https://drive.google.com/open?id=1zISxYa-8ROe2Zv8iRc82jY9QsQrfY1Vj)
- [CoNLL04](http://cogcomp.org/Data/ER/conll04.corp)
- [DREC](https://github.com/bekou/multihead_joint_entity_relation_extraction/tree/master/data/DREC)
- [NYT](https://github.com/gswycf/Joint-Extraction-of-Entities-and-Relations-Based-on-a-Novel-Tagging-Scheme)

# 一、2014
	Incremental Joint Extraction of Entity Mentions and Relations. ACL (1) 2014:
	动机：
	方法：基于特征工程的结构化系统
	问题：需要设计复杂的特征工程，严重依赖于NLP工具，这可能会导致错误传播问题。

	Modeling Joint Entity and Relation Extraction with Table Representation. EMNLP 2014
	动机：
	方法：基于特征工程的结构化系统
	问题：需要设计复杂的特征工程，严重依赖于NLP工具，这可能会导致错误传播问题。

# 二、2016
	End-to-End Relation Extraction using LSTMs on Sequences and Tree 
	Structures. ACL (1) 2016
	动机：在关系抽取中词序信息和树结构信息是可以互补的。比如，在句子“This is …, one U.S. source said”中，词之间的依存信息不足以预测‘source’和‘U.S.’之间的‘ORG-AFF’关系。很多传统的基于特征工程的关系分类方法从序列和解析树中抽取特征。然而，之前基于RNN的模型仅关注于这些语言结构的一种。
	方法：因此，作者提出了一种新颖的端到端模型，基于词序信息和依存树结构信息来抽取实体间的关系。该模型使用了双向的LSTM和tree-LSTM结构，同时抽取实体及其关系。
	问题：本文论文题目是端到端的关系抽取，但利用了NER作为辅助任务帮助关系抽取，可以说是实体和关系联合抽取的开山之作，所以往后的论文都会拿出来吊打一波。预测实体时忽略了标签之间的长依赖关系， (Y. H. Suncong Zheng n.d.)利用LSTM解码器解决了标签的长依赖问题[9]；预测好实体后，两两配对后输入到关系分类模块中识别它们之间的关系，因为不存在关系的实体对也输入到关系分类模块中，造成信息冗余，增加了错误率， (Suncong Zheng 2017)提出了新颖的标注机制解决了信息冗余的问题[8]。
	代码：https://github.com/tticoin/LSTM-ER


# 三、2017
	Joint Extraction of Entities and Relations Based on a Novel Tagging Scheme. ACL (1) 2017
	动机： (Makoto Miwa 2016)提出的联合模型可以在一个模型中通过共享参数表示实体及关系，但他们分离地抽取实体和关系并且产生信息冗余[6]。
	方法：提出了一个新颖的标注机制，将联合抽取问题转换为序列标注问题。
	问题：生成标注序列后，将标签合并为实体关系三元组时采用就近组合的方法，存在一些问题；无法解决关系重叠问题。
	代码：https://github.com/zsctju/triplets-extraction
	https://github.com/gswycf/Joint-Extraction-of-Entities-and-Relations-Based-on-a-Novel-Tagging-Scheme

	CoType: Joint Extraction of Typed Entities and Relations with Knowledge Bases. WWW 2017:
	动机：
	方法：基于特征工程的结构化系统
	问题：需要设计复杂的特征工程，严重依赖于NLP工具，这可能会导致错误传播问题。

	Joint entity and relation extraction based on a hybrid neural network. Neurocomputing 2017
	动机： (Makoto Miwa 2016)提出了基于神经网络的端到端实体和关系抽取模型。然而，当检测实体时，他们使用一个NN结构来预测实体标签，忽略了实体标签之间的长距离依赖关系[6]。
	方法：输入句子通过公用的Embedding层和BiLSTM层，然后分别使用一个LSTM来进行NER和一个CNN来进行RC。
	问题：本文并不是完全意义上的统一，还是要先NER，然后将预测的实体两两配对再进行RC，这样会产生信息冗余，可以与2017ACL有一篇基于标注策略的联合抽取进行对比。本文对R(e_1,e_2)和对R(e_2,e_1)的分类有点模糊。

	Global Normalization of Convolutional Neural Networks for Joint Entity and Relation Classification. EMNLP 2017
	动机：pipeline方法忽略了两个子任务的相关性
	方法：采用不同的CNN分别建模实体和关系表示，利用线性链CRF建模了输出的实体和关系之间的依赖性。
	问题：从模型和代码中都可以知道，训练集和测试集首先利用候选的实体对将句子拆成不同部分，然后输入到模型中。但事实上，对于测试集，我们是不知道候选的实体对的。
	代码：https://github.com/heikeadel/global_normalization

	Going out on a limb: Joint Extraction of Entity Mentions and Relations without Dependency Trees. ACL 2017
	动机： (Makoto Miwa 2016)提出的模型主要取决于对依赖树的访问，将其限制为句子级别提取以及存在（良好）依赖解析器的语言[6]。而且，他们的模型不共同提取实体和关系; 他们首先提取所有实体，然后对句子中的所有实体对进行关系分类。
	方法：一边识别实体，一边抽取关系，具体来说是当前位置实体识别出来后，通过比较它与之前所有位置实体的相似度（attention机制），来识别出当前实体与其他实体的关系。
	问题：
	代码：https://github.com/Luka0612/JEAR


# 三、2018
	Adversarial training for multi-context joint entity and relation extraction. EMNLP 2018:
	动机： (Christian Szegedy 2014)观察到对一些模型的输入的有意的小规模扰动（比如对抗样本）可能导致不正确的决定（具有高置信度）[2]。 (Ian Goodfellow 2015)提出了对抗训练（用于图像识别）作为正则化方法，混合原始样本和对抗样本来增强模型的鲁棒性[5]。虽然AT已经被用于NLP任务（比如，文本分类），但据作者所知，AT是第一次用于两个相关任务的联合学习中。
	方法：在原始样本的Embedding中加入一个小的扰动值，得到对抗样本，然后混合原始样本和对抗样本一起训练模型。
	问题：该模型在进行关系分类时，只是简单地将两个单词的隐藏层表示相加得到表示这两个单词的关系向量，这种方法不能很好地表示两个单词的关系。
	代码： https://github.com/bekou/multihead_joint_entity_relation_extraction

	Joint entity recognition and relation extraction as a multi-head selection problem. Expert Syst.
	动机： [3][4][6][7]这些工作检查关系提取的实体对时，并不是直接对整个句子建模。这意味着不考虑同一句子中其他实体对的关系 - 这可能有助于确定特定对的关系类型。 (Arzoo Katiyar 2017)直接对整个句子建模，但是无法处理多关系问题[1]。
	方法：首先进行命名实体识别，然后对句中的每个单词都与句中的所有单词判断关系。
	问题：该模型在进行关系分类时，只是简单地将两个单词的隐藏层表示相加得到表示这两个单词的关系向量，这种方法不能很好地表示两个单词的关系。
	代码：https://github.com/bekou/multihead_joint_entity_relation_extraction

	Joint Extraction of Entities and Relations Based on a Novel Graph Scheme. IJCAI 2018
	动机：大多数联合方法通过参数共享分开抽取实体及其关系，而不是共同decoding。这会导致输出的实体和关系的信息未能完全利用，因为没有显示的特征来建模输出之间的依赖。 (F. W. Suncong Zheng 2017)是唯一的另外，设计了一个新颖的标注机制将联合抽取任务转换为序列标注问题[8]。但是该方法仅间接捕获输出结构对应关系，并且无法解决关系重叠问题。
	方法：作者通过设计一个有向图机制将联合抽取任务转换为一个有向图问题，使用基于转移的解析框架来解决。
	问题：关系重叠可以分为两类：第一类是一个实体和多个实体之间存在关系；第二类是同一实体对存在多个关系。从作者介绍的方法看，只解决了第一类关系重叠问题。
	代码：https://github.com/hitwsl/joint-entity-relation


# 引用
	[1]. Arzoo Katiyar, Claire Cardie. "Going out on a limb: Joint Extraction of Entity Mentions and Relations 	without Dependency Trees." ACL, 2017: 917-928.
	[2]. Christian Szegedy, Wojciech Zaremba, Ilya Sutskever, Joan Bruna, Dumitru Erhan, Ian J. Goodfellow, Rob Fergus. "Intriguing properties of neural networks." ICLR, 2014.
	[3]. Fei Li, Meishan Zhang, Guohong Fu, Donghong Ji. "A neural joint model for entity and relation extraction from biomedical text." BMC Bioinformatics 18, 2017: 198:1-198:11.
	[4]. Heike Adel, Hinrich Schütze. "Global normalization of convolutional neural networks for joint entity and relation classification." EMNLP, 2017: 1723-1729.
	[5]. Ian Goodfellow, Jonathon Shlens, and Christian Szegedy. "Explaining and harnessing adversarial examples." ICLR, 2015.
	[6]. Makoto Miwa,  Mohit Bansal. "End-to-End Relation Extraction using LSTMs on Sequences and Tree Structures." ACL, 2016.
	[7]. Pankaj Gupta, Hinrich Schütze, Bernt Andrassy. "Table filling multi-task recurrent neural network for joint entity and relation extraction." COLING, 2016: 2537-2547.
	[8]. Suncong Zheng, Feng Wang, Hongyun Bao, Yuexing Hao, Peng Zhou, Bo Xu. "Joint Extraction of Entities and Relations Based on a Novel Tagging Scheme." ACL (1), 2017: 1227-1236.
	[9]. Suncong Zheng, Yuexing Hao, Dongyuan Lu, Hongyun Bao, Jiaming Xu, Hongwei Hao, Bo Xu. "Joint entity and relation extraction based on a hybrid neural network." Neurocomputing 257, n.d.: 59-66 (2017).


