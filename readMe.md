# 语义算子研究

近期数据库的语义算子的研究很火,我觉得这里面有很大探索空间,因此找了这方面一些文章去看看他们在做什么.

标题格式为: [编号].论文标题简写 [会议 年份 出版日期/出版编号] 


## 1. SemBench [VLDB 2026 19(8)]

benchmark的文章,主要探讨了5种语义算子(sem_filter, sem_join, sem_map, sem_rank, sem_classify),使用了5个场景的数据集,涉及了文本、图像、语音三种数据模态. [论文链接](./papers/VLDB'26_SemBench.pdf)

作者对比的时候把Palimpzest的Abacus优化器关了,不理解为什么.

filter, rank, classify目前都可以交给JEV处理了.没必要大模型了.



## 2. SEMA [VLDB 2026 19(11)]

非常推荐阅读的一篇文章.关注与传统database的相近的五种语义算子sem_filter, sem_proj, sem_join, sem_order, sem_agg.考虑到降低成本的一些操作. [论文链接](./papers/VLDB'26_Sema.pdf)

它的优化器的核心思想非常不错,确实需要谓词推断.在某些场景下性能收益会非常高的(论文例子说能降低41.8%数据处理量).不过它需要知道数值分布,且需要额外调大模型.另外,他是在rule-based优化之前调用的.



## 3. Accelerating Approximate Analytical Join Queries [SIGMOD2026  Jun.]

这篇文章在玩理论,没有很大的一些读的兴趣,而且对比的baselines都很naive.感觉问题定义的不是非常好(假设Oracle一定完全正确,而Embedding犯错概率相对比较大),读起来没有SEMA收益多.图一的例子用自链接没有排除自己跟自己的操作,另外,每一次join调用大模型开销很大,为什么不利用减枝策略去减少join操作呢? [论文链接](SIGMOD'26_Accelerating_Approximate_Analytical_Join_Queries.pdf)

一些例子感觉很离谱:
- 学术/内容查重率估(文本 Join):拿一篇新文章的每一句话,去和海量参考文献库里的每一句话做语义比对,统计 `COUNT(*)` 看有多少比例的句子属于洗稿/改写. **查重真这么做你看他一天能不能查完,举这个例子离谱的很,纯虚造场景.**
- 金融/商业竞品分析(文本 Join):两张上市公司表,没有共同的行业代码,想算生产相似产品的公司之间的平均股价差. **产品相似如何定义就压根没有标准,我可以说生土豆和薯片相似,因为薯片就是土豆炸出来的,也可以说不相似,土豆在超市里算果蔬类,薯片算零食.**

## 4. Cut Costs, Not Accuracy: LLM-Powered Data Processing with Guarantees [SIGMOD 2025 Dec.]

研究的是如何在大规模文本数据处理中,通过大小模型级联大幅降低调用昂贵大模型的成本,同时提供严格、可证明的统计质量保证?这个文章纯玩理论,可读兴致不高,不过核心思想可以借鉴. [论文链接](SIGMOD'26_Cut Costs.pdf)

在数据系统中使用顶尖大模型(Oracle)处理海量文本准确率高，但成本极高;使用廉价小模型(Proxy)成本低很多，但结果质量会下降。主流做法是首先采样,然后根据小模型输出的置信度分数（Proxy Scores，如 log-probabilities）设定一个级联阈值 $\rho$——置信度高于阈值的交由小模型直接处理，低于阈值的交由大模型兜底。不过采样过程忽视了任务目标和数据分布特征，导致阈值估计过于保守，省钱效果有限.

