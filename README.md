# NER-Deep-Learning
Using BiLSTM-CRF model for Chinese NER

## How To
简单介绍如何使用这个project来实现自己的NER模型。

### 环境要求
- python >= 3.5
- tensorflow == 1.4.0

### 训练数据准备
训练数据需要是已标注的数据，标注的形式为IOB，参考/data/xueyou/fashion/data/category.ner.ac.train.txt。

如果能有人工标注数据最好了，如果人工标注数据少，也支持使用预训练的embedding。这个需要提供预训练好的embedding文件，参考data/wiki_100.utf8，在config中配置pretrained_embedding_file

具体来说就是对于一个句子，标记出它里面的实体，比如人名，然后存储为每一行为一个字+类型，句子之间需要有空行分割。比如：

```
罗 B-PER
学 I-PER
优 I-PER
是 O
艾 B-ORG
耕 I-ORG
科 I-ORG
技 I-ORG
公 I-ORG
司 I-ORG
的 O
员 O
工 O
。 O

我 O
不 O
知 O
道 O
。 O
```

> 需要注意的是，如果句子中本身含有空格，需要替换为其他字符。

### 训练
训练参考bin/train_category.py，大部分情况下，你需要修改:
- DATA_DIR: 你的训练数据目录
- checkpoint_dir: 你的模型存储目录
- train,test,dev： 你的训练，测试，评估的文件

如果你需要修改模型参数：
- 可以参考一下config，修改模型的参数

### Inference
参考bin/inference.py，基本上你只需要将checkpoint_dir修改为你的模型目录即可

### Export
如果你需要导出模型作为serving使用，请参考bin/export.py，需要提供模型路径和导出路径，导出路径需要有版本号，如/tmp/export/0

## Char + Word Segment
This model is almost the same as [ChineseNER](https://github.com/zjy-ucas/ChineseNER), I implement it to learn the CRF model in tensorflow. I borrowed a lot data processing and evaluation code from [ChineseNER](https://github.com/zjy-ucas/ChineseNER), thanks to the author for sharing the code and the training data.

I used the config setting same as ChineseNER and got the best f1 scores of dev and test set are:
- dev: 89.35
- test: 88.53, PER = 90~91

I tried the original ChineseNER code, and got:
- dev: 90.65
- test: 91.02, PER = 93.89

## Char + Word Segment + More training data
I add training data from People's Daily 98(about 30K sentences) and Boson NLP(about 10K sentences). 

