### 使用真实数据

学习机器学习时，最好使用真实数据，而不是人工数据集。幸运的是，有上千个开源数据集 可以进行选择，涵盖多个领域。以下是一些可以查找的数据的地方：

*   __流行的开源数据仓库：__  
    
    *   [UC	Irvine	Machine	Learning Repository](https://link.jianshu.com/?t=http%3A%2F%2Farchive.ics.uci.edu%2Fml%2F)
    
    *   [Kaggle	datasets](https://link.jianshu.com/?t=https%3A%2F%2Fwww.kaggle.com%2Fdatasets)   
    
    *   [Amazon’s AWS datasets](https://link.jianshu.com/?t=http%3A%2F%2Faws.amazon.com%2Ffr%2Fdatasets%2F)


*   __准入口（提供开源数据列表）__     

    *   http://dataportals.org/ 

    *   http://opendatamonitor.eu/ 

    *   http://quandl.com/ 

*   __其它列出流行开源数据仓库的网页：__ 

    *   [Wikipedia’s list of Machine Learning datasets](https://link.jianshu.com/?t=https%3A%2F%2Fgoo.gl%2FSJHN2k)

    *   [Quora.com question](https://link.jianshu.com/?t=http%3A%2F%2Fgoo.gl%2FzDR78y)

    *   [Datasets subreddit](https://link.jianshu.com/?t=https%3A%2F%2Fwww.reddit.com%2Fr%2Fdatasets)



### 数据挖掘，机器学习，深度学习的区别是什么？ 

__数据挖掘__ 通常是从现有的数据中提取规律模式（pattern）以及使用算法模型（model）。核心目的是找到这些数据变量之间的关系，因此我们也会通过数据可视化对变量之间的关系进行呈现，用算法模型挖掘变量之间的关联关系。通常情况下，我们只能判断出来变量 A 和变量 B 是有关系的，但并不一定清楚这两者之间有什么具体关系。在我们谈论数据挖掘的时候，更强调的是从数据中挖掘价值。

__机器学习__ 是人工智能的一部分，它指的是通过训练数据和算法模型让机器具有一定的智能。一般是通过已有的数据来学习知识，并通过各种算法模型形成一定的处理能力，比如分类、聚类、预测、推荐能力等。这样当有新的数据进来时，就可以通过训练好的模型对这些数据进行预测，也就是通过机器的智能帮我们完成某些特定的任务。

__深度学习__ 属于机器学习的一种，它的目标同样是让机器具有智能，只是与传统的机器学习算法不同，它是通过神经网络来实现的。神经网络就好比是机器的大脑，刚开始就像一个婴儿一样，是一张白纸。但通过多次训练之后，“大脑”就可以逐渐具备某种能力。这个训练过程中，我们只需要告诉这个大脑输入数据是什么，以及对应的输出结果是什么即可。通过多次训练，“大脑”中的多层神经网络的参数就会自动优化，从而得到一个适应于训练数据的模型。

我们会发现传统的机器学习模型中，我们都会讲解模型的算法原理，比如 K-Means 的算法原理，KNN 的原理等。而到了神经网络，我们更关注的是网络结构，以及网络结构中每层神经元的传输机制。我们不需要告诉机器具体的特征规律是什么，只需把我们想要训练的数据和对应的结果告诉机器大脑即可。

举个例子，比如说我们想要假设一个函数来表示我们真实世界的一个具体的问题，这个时候我们传统的方法就是找到一种表达式，比如我们的线性回归，我们希望我们的线性回归表达式一个单变量的或者多变量的，也可能我们的模型很复杂无法写出表达式，但是我们有他的输入值和标签，这个时候我们把我们的数据放进神经网络中，神经网络会帮我们拟合出函数，如果拟合不出，函数也能在我们的数据集上进行工作，神经网络可以自行帮助我们进行模型的建立。神经网络的全连接层可以帮助我们进行特征提取处理，神经网络的全连接层是一种对输入数据直接做线性变换的线性计算层。它是神经网络中最常用的一种层，用于学习输出数据和输入数据之间的变换关系。全连接层可作为特征提取层使用，在学习特征的同时实现特征融合；也可作为最终的分类层使用，其输出神经元的值代表了每个输出类别的概率。

__深度学习会自己找到数据的特征规律！而传统机器学习往往需要专家（我们）来告诉机器采用什么样的模型算法，这就是深度学习与传统机器学习最大的区别。__

另外深度学习的神经网络结构通常比较深，一般都是 5 层以上，甚至也有 101 层或更多的层数。这些深度的神经网络可以让机器更好地自动捕获数据的特征。越深我们提取出来的特征的维度就越高，但是模型就越复杂。

