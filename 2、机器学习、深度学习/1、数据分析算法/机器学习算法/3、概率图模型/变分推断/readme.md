# 变分推断 `variationl inferece `

## `一、变分推断的背景`

机器学习的统计中，分 `频率学派` 和 `贝叶斯学派`，从这两个角度来进行分析:

<div align=center><img src="./static/background.jpg"/></div>

### `1.1、贝叶斯中有两个重要的概念：`

* `贝叶斯 inference `

* `贝叶斯 决策 `


    `如图所示：`在整个贝叶斯框架中，我们最主要的是要求 `后验概率`。

所以问题就集中在如何求解后验。

### `1.2、求解后验的方式有：`

* `inference：`

    * `精确推断` (直接前通过公式得到后验)

    * `近似推断` (当维度很复杂的时候无法直接求解后验)

        * `确定性近似`：`variationl inferece  (VI) 变分推断 `

        * `随机近似`：`MCMC` 


## 资料：



* 机器学习-白板推导系列(十二)-变分推断（Variational Inference）笔记：https://zhuanlan.zhihu.com/p/345597656





















