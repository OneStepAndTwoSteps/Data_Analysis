
# 从文本中提取特征信息    CountVectorizer类和TfidfVectorizer类 
 
`CountVectorizer和TfidfVectorizer方法的不同:`

`CountVectorizer` 和 `TfidfVectorizer` 是 `文本特征提取` 的两种方法。两者的主要区别在于，CountVectorizer仅仅通过计算词语词频，没有考虑该词语是否有代表性。而TfidfVectorizer可以更加精准的表征一个词语对某个话题的代表性。

 
        
### sklearn中的fit，transform，fit_transform 在文本提取特征中各自的作用。  

首先，计算机是不能从文本字符串中发现规律。只有将字符串编码为计算机可以理解的数字，计算机才有可能发现文本中的规律。

对文本编码，就是让词语与数字对应起来，建立基于给定文本的词典。（fit方法 ）

再根据词典对所有的文本数据进行转码。（transform方法）

scikit库的CountVectorize类就是这种思路。

     from sklearn.feature_extraction.text import CountVectorizer

     cv = CountVectorizer()

`使用fit方法，CountVectorizer()类的会从corpus语料中学习到所有词语，进而构建出text词典。`

    text=["Hey hey hey lets go get lunch today :)",
           "Did you go home?",
           "Hey!!! I need a favor"]
    
    text相当于三篇文章

 `fit学会语料中的所有词语，构建词典`
 
    cv.fit(text)
    CountVectorizer(analyzer='word', binary=False, decode_error='strict',
            dtype=<class 'numpy.int64'>, encoding='utf-8', input='content',
            lowercase=True, max_df=1.0, max_features=None, min_df=1,
            ngram_range=(1, 1), preprocessor=None, stop_words=None,
            strip_accents=None, token_pattern='(?u)\\b\\w\\w+\\b',
            tokenizer=None, vocabulary=None)
            
  `这里我们查看下“词典”，也就是特征集(11个特征词)`
  
    cv.get_feature_names()
    
   Out:
   
      ['did',
       'favor',
       'get',
       'go',
       'hey',
       'home',
       'lets',
       'lunch',
       'need',
       'today',
       'you']
       
  `注意feature_name的返回结果，我们可以发现这几条规律：`
    
    一、所有的单词都是小写

    二、单词长度小于两个字母的，会被剔除掉,如果我们想要保留长度为1的词 可以使用如cv = CountVectorizer(token_pattern='(?u)\\b\\w+\\b' )

    三、标点符号会剔除掉

    四、不重复

    五、这个特征集是有顺序的     
       
  文档-词频矩阵（document-term matrix），英文简写为dtm
   
    dtm=cv.transform(texts)
    print(dtm)
    
  Out:
  
      (0, 2)	1
      (0, 3)	1
      (0, 4)	3
      (0, 6)	1
      (0, 7)	1
      (0, 9)	1
      (1, 0)	1
      (1, 3)	1
      (1, 5)	1
      (1, 10)	1
      (2, 1)	1
      (2, 4)	1
      (2, 8)	1
    
  用pandas库以行列形式展示-  在 jupyer notebook中展示的没有加print：
  
    pd.DataFrame(cv_fit.toarray(),columns=cv.get_feature_names())
  
对应输出的pandas图片，和上面的out(输出)结合来看，就是第0行第3个数为1次，第0行第4个数为1次......
![Image_text](https://raw.githubusercontent.com/OneStepAndTwoSteps/data_mining_analysis/master/static/sklearn%E6%96%87%E6%9C%AC%E6%8F%90%E5%8F%96%E7%89%B9%E5%BE%81%E5%80%BC/1.jpg)

同时在我们pandas显示出来的图片中每一行代表一个文章，每一列代表一个特征，在第0行的hey特征下面的数字为3，表示hey在该文章里面出现了3次。


## `案例：CountVectorizer 提取文本信息`

`代码：`

    # CountVectorizer仅仅通过计算词语词频，没有考虑该词语是否有代表性
    from sklearn.feature_extraction.text import CountVectorizer
    import pandas as pd

    # 定义文本，这里假设有4篇文章
    texts=["Hey hey hey lets go get lunch today :)",
              "Did you go home?",
              "Hey!!! I need a favor"]

    # 实例化
    cv = CountVectorizer()
    cv_fit=cv.fit_transform(texts)

    print('cv_fit:\n',cv_fit)
    print('\ncv:\n\n',cv)
    print('\ncv_fit_transform:\n\n',cv_fit_transform)
    print('\nget_feature_names:\n\n',cv.get_feature_names())
    print('\ntoarray：\n',cv_fit.toarray())

`out:`

    cv_fit:
      (0, 9)	1
      (0, 7)	1
      (0, 2)	1
      (0, 3)	1
      (0, 6)	1
      (0, 4)	3
      (1, 5)	1
      (1, 10)	1
      (1, 0)	1
      (1, 3)	1
      (2, 1)	1
      (2, 8)	1
      (2, 4)	1

    cv:

    CountVectorizer()

    cv_fit_transform:

      (0, 9)	1
      (0, 7)	1
      (0, 2)	1
      (0, 3)	1
      (0, 6)	1
      (0, 4)	3
      (1, 5)	1
      (1, 10)	1
      (1, 0)	1
      (1, 3)	1
      (2, 1)	1
      (2, 8)	1
      (2, 4)	1

    get_feature_names:

    ['did', 'favor', 'get', 'go', 'hey', 'home', 'lets', 'lunch', 'need', 'today', 'you']

    toarray：
    [[0 0 1 1 3 0 1 1 0 1 0]
    [1 0 0 1 0 1 0 0 0 0 1]
    [0 1 0 0 1 0 0 0 1 0 0]]

## `案例：TfidfVectorizer 提取文本信息`


`代码：`

    from sklearn.feature_extraction.text import TfidfVectorizer

    tv = TfidfVectorizer(use_idf=True, smooth_idf=True, norm=None)

    texts=["dog cat fish","dog cat cat","fish bird", 'bird']


    tv_fit = tv.fit_transform(texts)

    # 查看一下构建的词汇表
    display(tv.get_feature_names())

    # 查看输入文本列表的VSM矩阵
    display(pd.DataFrame(tv_fit.toarray(),columns=tv.get_feature_names()))

`out:`

<div align=center><img src="./static/1.jpg"/></div>





## `注意: `
   此时我们已经构建完成了我们的词频矩阵， `如果我们还想加入新的文档此时我们需要注意了。`
   
   `举个例子：`
         
         new_document = ['Hello girl lets go get a drink tonight']
         new_dtm = cv.transform(new_document)
         print(new_dtm.toarray())
         pd.DataFrame(new_dtm.toarray(), columns=cv.get_feature_names())
         
   `Out:` new_dtm.toarray()的输出
   
         [[0 0 1 1 0 0 1 0 0 0 0]]
         
显示如下图：
![Image_text](https://raw.githubusercontent.com/OneStepAndTwoSteps/data_mining_analysis/master/static/sklearn%E6%96%87%E6%9C%AC%E6%8F%90%E5%8F%96%E7%89%B9%E5%BE%81%E5%80%BC/2.jpg)

`小结：`

即使new_document含有8个单词，但是在上面的dataframe表中只有3个特征词被有效编码，Hello,girl,drink和tonight词未被表征。 `这是因为我们初识的text语料所构建的词典并未含有这些词。但是对文本进行特征表征时，使用的确实text所生产的词典。`

我们机器学习所用的数据，一般被分成训练集和测试集。训练集是为了让机器学习数据的规律（拟合模型），测试集是为了验证规律的有效性。训练集本质上代表的是过去及现在已知的数据，测试集本质上代表的是未来的未知数据（现在不存在的数据），我们是用已知的数据预测未来。

`所以我们只能让fit方法操作于训练集，构建基于过去或已知数据的特征集。`

                  
                            
