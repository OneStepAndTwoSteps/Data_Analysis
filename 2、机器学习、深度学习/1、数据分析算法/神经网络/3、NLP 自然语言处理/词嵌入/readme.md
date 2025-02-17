# `词嵌入`


## `词嵌入 embedding：`

### `1、词嵌入介绍：`

* `词嵌入（Word embedding）`是 `自然语言处理（NLP）`中语言模型与表征学习技术的统称。概念上而言，`它是指把一个维数为所有词的数量的高维空间嵌入到一个维数低得多的连续向量空间中，每个单词或词组被映射为实数域上的向量`。

* 计算机无法直接处理文字，所以 `NLP` 第一步就是将文字转为数字表示，最常用的转换方式就是 `onehot` 向量，比如一共有五类，那么属于第二类的话，它的编码就是 `(0, 1, 0, 0, 0)` ，对于分类问题，这样当然特别简明。

    但是在自然语言处理中，因为单词的数目过多，这样做就行不通了，比如有 `10000` 个不同的词，那么使用 `one-hot` 这样的方式来定义，矩阵就非常稀疏，每个单词都是 `10000` 维的向量，其中只有一位是 `1` ，其余都是 `0` ，这会导致无法充分学习到数据中的信息。除此之外，也不能体现单词的词性，因为每一个单词都是 `one-hot` ，虽然有些单词在语义上会更加接近，但是 `one-hot` 没办法体现这个特点，每个特征正交，所以必须使用另外一种方式定义每一个单词，这就引出了词嵌入。

* 通过 `embedding` 可以解决：
  
   * `维度灾难（降维）`。
   
   * `无法保留词序信息（滑动窗口设置较大时，可以保留多个词，此时包含词序信息）`。
   
   * `存在语义鸿沟（词的相关性）` 。
   
   三大问题。  


### `2、词嵌入的过程称为查表的原因`

* 如下：



    <div align=center><img height =150 src="./static/查找表.jpg"/></div>


    为了有效地进行计算，这种稀疏状态下不会进行矩阵乘法计算，可以看到矩阵的计算的结果实际上是矩阵对应的向量中值为1的索引，上面的例子中，左边向量中取值为1的对应维度为3（下标从0开始），那么计算结果就是矩阵的第3行（下标从0开始）—— [10, 12, 19]，这样模型中的隐层权重矩阵便成了一个”查找表“（lookup table），进行矩阵计算时，直接去查输入向量中取值为1的维度下对应的那些权重值。隐层的输出就是每个输入单词的“嵌入词向量”。
