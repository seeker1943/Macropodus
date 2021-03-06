<p align="center">
    <img src="macropodus_images/macropodus_logo.png" width="320"\>
</p>

# [Macropodus](https://github.com/yongzhuo/Macropodus)

[![PyPI](https://img.shields.io/pypi/v/Macropodus)](https://pypi.org/project/Macropodus/)
[![Build Status](https://travis-ci.com/yongzhuo/Macropodus.svg?branch=master)](https://travis-ci.com/yongzhuo/Macropodus)
[![PyPI_downloads](https://img.shields.io/pypi/dm/Macropodus)](https://pypi.org/project/Macropodus/)
[![Stars](https://img.shields.io/github/stars/yongzhuo/Macropodus?style=social)](https://github.com/yongzhuo/Macropodus/stargazers)
[![Forks](https://img.shields.io/github/forks/yongzhuo/Macropodus.svg?style=social)](https://github.com/yongzhuo/Macropodus/network/members)
[![Join the chat at https://gitter.im/yongzhuo/Macropodus](https://badges.gitter.im/yongzhuo/Macropodus.svg)](https://gitter.im/yongzhuo/Macropodus?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
>>> Macropodus是一个以Albert+BiLSTM+CRF网络结构为基础，用大规模中文语料训练的自然语言处理工具包。将提供中文分词、命名实体识别、关键词抽取、文本摘要、新词发现、文本相似度、计算器、数字转化等常见NLP功能。


## 目录

* [安装](#安装)
* [使用方式](#使用方式)
* [参考/引用](#参考/引用)
* [FAQ](#FAQ)


# 安装 
1. 通过PyPI安装(自带模型文件)：
	```
	pip install macropodus
	```
2. 使用镜像源，例如：   
	```
	pip install -i https://pypi.tuna.tsinghua.edu.cn/simple macropodus
	```


# 使用方式


## 快速使用
```python3
import macropodus

sen_calculate = "23 + 13 * (25+(-9-2-5-2*3-6/3-40*4/(2-3)/5+6*3))加根号144你算得几多"
sen_chi2num = "三千零七十八亿三千零十五万零三百一十二点一九九四"
sen_num2chi = 1994.1994
sent1 = "PageRank算法简介"
sent2 = "百度百科如是介绍他的思想:PageRank通过网络浩瀚的超链接关系来确定一个页面的等级。"
summary = "PageRank算法简介。" \
           "是上世纪90年代末提出的一种计算网页权重的算法! " \
           "当时，互联网技术突飞猛进，各种网页网站爆炸式增长。 " \
           "业界急需一种相对比较准确的网页重要性计算方法。 " \
           "是人们能够从海量互联网世界中找出自己需要的信息。 " \
           "百度百科如是介绍他的思想:PageRank通过网络浩瀚的超链接关系来确定一个页面的等级。 " \
           "Google把从A页面到B页面的链接解释为A页面给B页面投票。 " \
           "Google根据投票来源甚至来源的来源，即链接到A页面的页面。 " \
           "和投票目标的等级来决定新的等级。简单的说， " \
           "一个高等级的页面可以使其他低等级页面的等级提升。 " \
           "具体说来就是，PageRank有两个基本思想，也可以说是假设。 " \
           "即数量假设：一个网页被越多的其他页面链接，就越重）。 " \
           "质量假设：一个网页越是被高质量的网页链接，就越重要。 " \
           "总的来说就是一句话，从全局角度考虑，获取重要的信。 "

# 分词(词典最大概率分词DAG)
words = macropodus.cut(summary)
print(words)
# 新词发现
new_words = macropodus.find(summary)
print(new_words)
# 文本摘要
sum = macropodus.summarize(summary)
print(sum)
# 关键词抽取
keyword = macropodus.keyword(summary)
print(keyword)
# 文本相似度
sim = macropodus.sim(sent1, sent2)
print(sim)
# tookit
# 计算器
score_calcul = macropodus.calculate(sen_calculate)
print(score_calcul)
# 中文数字与阿拉伯数字相互转化
res_chi2num = macropodus.chi2num(sen_chi2num)
print(res_chi2num)
res_num2chi = macropodus.num2chi(sen_num2chi)
print(res_num2chi)

```


## 中文分词

各种分词方法
```python3
import macropodus

# 用户词典
macropodus.add_word(word="斗鱼科")
macropodus.add_word(word="鲈形目") # 不持久化, 当前有效
macropodus.save_add_words(word_freqs={"喜斗":32, "护卵":64, "护幼":132}) # 持久化保存到用户字典
sent = "斗鱼属，Macropodus (1801)，鲈形目斗鱼科的一属鱼类。本属鱼类通称斗鱼。因喜斗而得名。分布于亚洲东南部。中国有2种，即叉尾斗鱼，分布于长江及以南各省；叉尾斗鱼，分布于辽河到珠江流域。其喜栖居于小溪、河沟、池塘、稻田等缓流或静水中。雄鱼好斗，产卵期集草成巢，雄鱼口吐粘液泡沫，雌鱼产卵其中，卵浮性，受精卵在泡沫内孵化。雄鱼尚有护卵和护幼现象。"

# 分词
sents = macropodus.cut_bidirectional(sent)
print("cut_bidirectional: " + " ".join(sents))
sents = macropodus.cut_forward(sent)
print("cut_forward: " + " ".join(sents))
sents = macropodus.cut_reverse(sent)
print("cut_reverse: " + " ".join(sents))
sents = macropodus.cut_search(sent)
print("cut_search: " + " ".join(sents))
# DAG
sents = macropodus.cut_dag(sent)
print("cut_dag: " + " ".join(sents))

```


## 文本相似度

  文本相似度主要使用词向量, 余弦相似度 或 jaccard相似度
```python3
import macropodus

sent1="叉尾斗鱼是一种观赏性动物"
sent2="中国斗鱼生性好斗,适应性强,能在恶劣的环境中生存"
           
# 文本相似度(similarity)
sents = macropodus.sim(sent1, sent2, type_sim="total", type_encode="avg")
print(sents)
sents = macropodus.sim(sent1, sent2, type_sim="cosine", type_encode="single")
print(sents)

```


## 文本摘要

  文本摘要方法有text_pronouns, text_teaser, word_sign, textrank, lead3, mmr, lda, lsi, nmf
```python3
import macropodus

summary = "PageRank算法简介。" \
           "是上世纪90年代末提出的一种计算网页权重的算法! " \
           "当时，互联网技术突飞猛进，各种网页网站爆炸式增长。 " \
           "业界急需一种相对比较准确的网页重要性计算方法。 " \
           "是人们能够从海量互联网世界中找出自己需要的信息。 " \
           "百度百科如是介绍他的思想:PageRank通过网络浩瀚的超链接关系来确定一个页面的等级。 " \
           "Google把从A页面到B页面的链接解释为A页面给B页面投票。 " \
           "Google根据投票来源甚至来源的来源，即链接到A页面的页面。 " \
           "和投票目标的等级来决定新的等级。简单的说， " \
           "一个高等级的页面可以使其他低等级页面的等级提升。 " \
           "具体说来就是，PageRank有两个基本思想，也可以说是假设。 " \
           "即数量假设：一个网页被越多的其他页面链接，就越重）。 " \
           "质量假设：一个网页越是被高质量的网页链接，就越重要。 " \
           "总的来说就是一句话，从全局角度考虑，获取重要的信。 "
           
# 文本摘要(summarize, 默认接口)
sents = macropodus.summarize(summary)
print(sents)

# 文本摘要(summarization, 可定义方法, 提供9种文本摘要方法, 'lda', 'mmr', 'textrank', 'text_teaser')
sents = macropodus.summarization(text=summary, type_summarize="lda")
print(sents)

```


## 新词发现

  新词发现主要使用凝固度, 左熵, 右熵, 词频等方案, 综合考虑
```python3
import macropodus

summary = "PageRank算法简介。" \
           "是上世纪90年代末提出的一种计算网页权重的算法! " \
           "当时，互联网技术突飞猛进，各种网页网站爆炸式增长。 " \
           "业界急需一种相对比较准确的网页重要性计算方法。 " \
           "是人们能够从海量互联网世界中找出自己需要的信息。 " \
           "百度百科如是介绍他的思想:PageRank通过网络浩瀚的超链接关系来确定一个页面的等级。 " \
           "Google把从A页面到B页面的链接解释为A页面给B页面投票。 " \
           "Google根据投票来源甚至来源的来源，即链接到A页面的页面。 " \
           "和投票目标的等级来决定新的等级。简单的说， " \
           "一个高等级的页面可以使其他低等级页面的等级提升。 " \
           "具体说来就是，PageRank有两个基本思想，也可以说是假设。 " \
           "即数量假设：一个网页被越多的其他页面链接，就越重）。 " \
           "质量假设：一个网页越是被高质量的网页链接，就越重要。 " \
           "总的来说就是一句话，从全局角度考虑，获取重要的信。 "
           
# 新词发现(findword, 默认接口)
sents = macropodus.find(text=summary, freq_min=2, len_max=7, entropy_min=1.2, aggregation_min=0.5, use_avg=True)
print(sents)

```


## 关键词

  关键词抽取使用的是textrank, 边关系构建: 1. 字向量构建句向量; 2. 余弦相似度计算边得分
```python3
import macropodus

sent = "斗鱼属，Macropodus (1801)，鲈形目斗鱼科的一属鱼类。本属鱼类通称斗鱼。因喜斗而得名。分布于亚洲东南部。中国有2种，即叉尾斗鱼，分布于长江及以南各省；叉尾斗鱼，分布于辽河到珠江流域。其喜栖居于小溪、河沟、池塘、稻田等缓流或静水中。雄鱼好斗，产卵期集草成巢，雄鱼口吐粘液泡沫，雌鱼产卵其中，卵浮性，受精卵在泡沫内孵化。雄鱼尚有护卵和护幼现象。"
# 关键词(keyword)
sents = macropodus.keyword(sent)
print(sents)

```


## 常用小工具(tookit)

  工具包括科学计算器, 阿拉伯-中文数字转化
```python3
import macropodus

sen_calculate = "23 + 13 * (25+(-9-2-5-2*3-6/3-40*4/(2-3)/5+6*3))加根号144你算得几多"
sen_chi2num = "三千零七十八亿三千零十五万零三百一十二点一九九四"
sen_num2chi = 1994.1994
# tookit, 科学计算器
score_calcul = macropodus.calculate(sen_calculate)
print(score_calcul)
# tookit, 中文数字转阿拉伯
res_chi2num = macropodus.chi2num(sen_chi2num)
print(res_chi2num)
# tookit, 阿拉伯数字转中文
res_num2chi = macropodus.num2chi(sen_num2chi)
print(res_num2chi)

```

# 参考/引用
* textrank_gensim: [https://github.com/RaRe-Technologies/gensim](https://github.com/RaRe-Technologies/gensim)
* 最大概率(DAG-动态规划)词典分词: [https://github.com/fxsjy/jieba](https://github.com/fxsjy/jieba)
* CRF(-未解决): [https://github.com/BrikerMan/Kashgari](https://github.com/BrikerMan/Kashgari)

# FAQ
