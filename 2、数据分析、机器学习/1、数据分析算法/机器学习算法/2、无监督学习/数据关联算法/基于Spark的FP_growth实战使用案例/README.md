### 基于spark的FP_growth的使用案例：

使用条件：

__1、安装java环境，配置环境变量__

__2、安装spark，配置环境变量__

__3、安装scala，配置环境变量__

__4、安装hadoop环境，配置环境变量__


__案例二的参考链接：__

https://spark.apache.org/docs/2.2.0/ml-frequent-pattern-mining.html

### Spark MLlib关联算法参数介绍
对于FPGrowth类，使用它的训练函数train主要需要输入三个参数：数据项集data，支持度阈值minSupport和数据并行运行时的数据分块数numPartitions。对于支持度阈值minSupport，它的取值大小影响最后的频繁项集的集合大小，支持度阈值越大，则最后的频繁项集数目越少，默认值0.3。而数据并行运行时的数据分块数numPartitions主要在分布式环境的时候有用，如果你是单机Spark，则可以忽略这个参数。


