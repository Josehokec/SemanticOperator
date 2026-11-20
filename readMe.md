# 语义算子研究



# 1. SemBench [VLDB 19(8)]

benchmark的文章,主要探讨了5种语义算子(sem_filter, sem_join, sem_map, sem_rank, sem_classify),使用了5个场景的数据集,涉及了文本、图像、语音三种数据模态.[论文链接](./papers/VLDB'26_SemBench- A Benchmark for Semantic Query Processing Engines.pdf)

作者对比的时候把Palimpzest的Abacus优化器关了,不理解为什么.

filter, rank, classify目前都可以交给JEV处理了.没必要大模型了.



# 2. SemBench [VLDB 19(11)]

分类、总结、关键字提取、排序

sem_filter, sem_proj, sem_join, sem_order, sem_agg
