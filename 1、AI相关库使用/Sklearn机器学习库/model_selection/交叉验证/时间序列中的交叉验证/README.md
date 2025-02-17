## 交叉验证在时间序列数据中应用

时间序列数据的特点是 __时间 (autocorrelation(自相关性))之间是具有相关性的__。

然而，传统的交叉验证技术，例如 KFold 和 ShuffleSplit 假设样本是独立的且分布相同的，并且在时间序列数据上会导致训练和测试实例之间不合理的相关性（产生广义误差的估计较差）。 

因此，对 “future(未来)” 观测的时间序列数据模型的评估至少与用于训练模型的观测模型非常重要。为了达到这个目的，一个解决方案是由TimeSeriesSplit提供的。

### 时间序列分割

__TimeSeriesSplit__ 是k-fold的一个变体，它首先返回k折作为训练数据集，并且 (k+1) 折作为测试数据集。

请注意，与标准的交叉验证方法不同，连续的训练集是超越前者的超集。另外,它将所有的剩余数据添加到第一个训练分区，它总是用来训练模型。

这个类可以用来交叉验证以固定时间间隔观察到的时间序列数据样本。
