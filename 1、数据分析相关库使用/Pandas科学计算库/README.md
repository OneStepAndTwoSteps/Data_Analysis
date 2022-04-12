
# 📖pandas用法梳理

## pandas介绍：
  在数据分析工作中，Pandas 的使用频率是很高的，一方面是因为 Pandas 提供的基础数据结构 DataFrame 与 json 的契合度很高，转换起来就很方便。
  另一方面，如果我们日常的数据清理工作不是很复杂的话，你通常用几句 Pandas 代码就可以对数据进行规整。

  Pandas 可以说是基于 NumPy 构建的含有更高级数据结构和分析能力的工具包。在 NumPy 中数据结构是围绕 ndarray 展开的，那么在 Pandas 中的核心数据结构是什么呢？

  下面主要给你讲下Series 和 DataFrame 这两个核心数据结构，他们分别代表着一维的序列和二维的表结构。基于这两种数据结构，Pandas 可以对数据进行导入、清洗、处理、统计和输出。

## pandas.set_option 选项设置
  `可以设置pandas的属性，比如打印出来数据时显示多少列，显示多宽等等，可以一次性设置多个格式如下`
   
  设置pandas保留小数位数为三位
  
    pd.set_option('display.float_format', lambda x: '%.3f' % x)

    pd.options.display.float_format = '{:,.3f}'.format
  
  默认显示列数，行数,当设置为None时，表示显示所有内容
    
    pd.set_option('display.max_columns',5, 'display.max_rows', 100)
  
  设置pandas显示所有数据内容(不隐藏数据)：

  显示所有列 (列中有省略的不省略)

    pd.set_option('display.max_columns', None)
    
  `解决如下问题：`

      11 12 13 ... 19 20
      21 22 23 ... 29 30
      31 32 33 ... 39 40
      41 42 43 ... 49 50

  显示所有行 (行中有省略的不省略)
  
    pd.set_option('display.max_rows', None)

  `解决如下问题：`

      1 2 3
      4 5 6
      7 8 9
      ...
      100 101 102    


## `数据结构Series 和 Dataframe`

### Series 
  `Series 是个定长的字典序列` 。说是定长是因为在存储的时候，相当于两个 ndarray，这也是和字典结构最大的不同。因为在字典的结构里，元素的个数是不固定的。
  
  `Series 的两个基本属性` 有两个基本属性：index 和 values。在 Series 结构中，index 默认是 0,1,2,……递增的整数序列，当然我们也可以自己来指定索引，比如 index=[‘a’, ‘b’, ‘c’, ‘d’]。
  
  `例子：`
  
    import pandas as pd
    from pandas import Series, DataFrame
    x1 = Series([1,2,3,4])
    x2 = Series(data=[1,2,3,4], index=['a', 'b', 'c', 'd'])
    print x1
    print x2

  
  `运行结果：`
  
    0    1
    1    2
    2    3
    3    4
    dtype: int64
    a    1
    b    2
    c    3
    d    4
    dtype: int64
    
这个例子中，x1 中的 index 采用的是默认值，x2 中 index 进行了指定。我们也可以采用字典的方式来创建 Series，比如：

`例子：`

    d = {'a':1, 'b':2, 'c':3, 'd':4}
    x3 = Series(d)
    print x3 

`运行结果：`
  
    a    1
    b    2
    c    3
    d    4
    dtype: int64

### reshape函数 

用于调整数据的shape

`如：`

    #生成16个自然数，以2行8列的形式显示
    np.arange(16).reshape(2,8) 
  
  out : 
    
    array([[ 0,  1,  2,  3,  4,  5,  6,  7],
          [ 8,  9, 10, 11, 12, 13, 14, 15]])

`特殊用法：`

  `df.reshape(-1,1)` 将其转换成1列的格式，行的话为-1，表示将整个数据转为一列，其中行数不限制。

  example:

    np.arange(16).reshape(-1,1)
  
  out : 
    
    array([[ 0],
           [ 1],
           [ 2],
           [ 3],
           [ 4],
           [ 5],
           [ 6],
           [ 7],
           [ 8],
           [ 9],
           [10],
           [11],
           [12],
           [13],
           [14],
           [15]])
  

  `reshape(1,-1)` 转为 1 行格式，列不限

  example:

    np.arange(16).reshape(1,-1) 

  out : 
    
    array([[ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15]])


## `关于文件的读取操作：`

* https://www.jianshu.com/p/7764b6591cf5

* https://www.cnblogs.com/datablog/p/6127000.html

### `pandas read_csv()`

* `read_csv参数介绍：`gairuo.com/p/pandas-read-csv






* `a） 选择要导入的行数`

  可以使用参数nrows指定要导入的行数。例如：

      # 将只读取前10000行（包括标题）
      train = pd.read_csv('../input/train.csv', nrows=10000, dtype=dtypes)
      train.head()


* `b） 简单行跳转（带或不带标题）`

  还可以指定要跳过的行数（skiprows），例如，如果您希望在前500万行之后有100万行：

      
      train = pd.read_csv('../input/train.csv', skiprows=5000000, nrows=1000000, header = None, dtype=dtypes)
      train.head()

      
      train = pd.read_csv('../input/train.csv', skiprows=range(1, 5000000), nrows=1000000, dtype=dtypes)
      train.head()

* `c） 挑选要跳过的行（列出你不想要的东西）`

  因为“skiprows”可以接收要跳过的行的列表，所以可以创建一个要输入的随机行的列表。一、 你可以随意抽取你的数据！
  
  `1.1、`输出数据行数 Linux
      
      %%time
      import subprocess
      #from https://stackoverflow.com/questions/845058/how-to-get-line-count-cheaply-in-python , Olafur's answer
      def file_len(fname):
          # wc方法可以用于查看行数 
          p = subprocess.Popen(['wc', '-l', fname], stdout=subprocess.PIPE, 
                                                  stderr=subprocess.PIPE)
          result, err = p.communicate()
          if p.returncode != 0:
            raise IOError(err)
          return int(result.strip().split()[0])

      lines = file_len('../input/train.csv')
      print('Number of lines in "train.csv" is:', lines)

  输出结果：性能较高

      Number of lines in "train.csv" is: 184903891
      CPU times: user 2.55 ms, sys: 8.21 ms, total: 10.8 ms
      Wall time: 8.25 s


  `1.2、`输出数据行数

      %%time
      num_lines = sum(1 for line in open('myfile.txt'))

  输出结果：性能较差

      CPU times: user 28.3 s, sys: 1.88 s, total: 30.1 s
      Wall time: 30.1 s

  假设您想从整个数据集中随机抽取100万行数据。这意味着您需要一个从1到184903891的行-1-1000000个随机数的列表。
  
  `2、`生成要跳过的行列表

      #generate list of lines to skip
      skiplines = np.random.choice(np.arange(1, lines), size=lines-1-1000000, replace=False)

      #sort the list
      skiplines=np.sort(skiplines)

  `3、`检查生成的名单

      #check our list
      print('lines to skip:', len(skiplines))
      print('remaining lines in sample:', lines-len(skiplines), '(remember that it includes the heading!)')

      ################### 健全性检查 ###################
      # 通过检查每个连续行之间的差异来查找未跳过的行，如果diff不等于-1表示不连续，则出现跳行的现象
      # 前10万行中有多少会被导入csv？
      diff = skiplines[1:100000]-skiplines[2:100001]
      remain = sum(diff!=-1)
      print('Ratio of lines from first 100000 lines:',  '{0:.5f}'.format(remain/100000) ) 
      print('Ratio imported from all lines:', '{0:.5f}'.format((lines-len(skiplines))/lines) )

  输出案例：

      lines to skip: 183903890
      remaining lines in sample: 1000001 (remember that it includes the heading!)
      Ratio of lines from first 100000 lines: 0.00560
      Ratio imported from all lines: 0.00541


  `4、`按照 skiplines 筛选 -- 如果跳过的数据量很大，使用 skiprows 非常耗时

      train = pd.read_csv('../input/train.csv', skiprows=skiplines, dtype=dtypes)
      train.head()

### `文件的存储格式`

`feather 格式`

在参加各种机器学习比赛的时候，有时候要读取几百M甚至几个G 的表格数据，为了使读取速度加快，使用一种新的方法，把.csv格式格式的文件转存为.feather格式，再用read_feather读取，速度可以大大提升。

* `将Dataframe格式的数据以feather格式储存：`

      train_data.to_feather("train.feather")

* `读取 feather 文件：`

      # load the same dataframe next time directly, without reading the csv file again!
      train_df_new = pd.read_feather('train.feather')

### `Using Dask`

    import dask.dataframe as dd

使用 Dask 及其 dataframe 构造，您必须像在 pandas 中一样设置数据框，而不是将数据加载到 pandas 中，这种方法将数据框作为数据文件的“指针”，并且不会加载任何内容，直到你专门告诉它这样做。

`相当于只是加载了 dataframe 的结构框`

`读取：`

    %%time

    # dask's read_csv takes no time at all!
    ddf = dd.read_csv(TRAIN_PATH, usecols=cols, dtype=traintypes)

`耗时：`

    CPU times: user 148 ms, sys: 12 ms, total: 160 ms
    Wall time: 159 ms

`查看信息：`

    # no info?
    ddf.info()

`输出结果：`

    <class 'dask.dataframe.core.DataFrame'>
    Columns: 7 entries, fare_amount to passenger_count
    dtypes: object(1), float32(5), uint8(1)


对于 ddf 数据你可以使用 .head 或者 .describe 等方法，但是像 .describe 方法他计算的时间是高于 read_csv 读取的数据的。





### `np.vstack() 和 np.hstack()`

* `np.vstack` : 按垂直方向（`行顺序`）堆叠数组构成一个新的数组

* `np.hstack` : 按水平方向（`列顺序`）堆叠数组构成一个新的数组

  [np.vstack() 和 np.hstack() 案例说明](https://www.jianshu.com/p/2469e0e2a1cf)


### `np.stack()`

numpy 和 tensorflow 都有 stack() 函数，该函数主要是用来提升维度。


<div align='center'><img height='350' width='480' src='./static/1_2.jpg'> </div>


`axis分別等于0，1，2时：`

* 当axis=0时表示最外层的中括号，三个轴则最外层有三个中括号。axis=0则表示将向内一层的内容看做一个整体，比如示例中arrays[0]的值，axis=1在在axis=0的基础上再取更内一层的内容，层层向内解包，最后进入axis=2，则直接取得元素的值，如arrays[0][0][0]

`综上有：`

* `np.stack(arrays, axis=0)` 则表示取出每个二维数组（最外层有两个中括号）相应的索引对应的数组进行堆叠，这里np.stack(arrays, axis=0)则表示arrays[0], arrays[1], arrays[2]进行堆叠，所以结果与原始数组一样。

* `np.stack(arrays, axis=1)` 则表示arrays[0][0], arrays[1][0]和arrays[2][0]进行堆叠，然后是arrays[0][1]，arrays[1][1]与arrays[2][1]进行堆叠。

* `np.stack(arrays, axis=2)` 则表示arrays[0][0][0]，arrays[1][0][0]和arrays[2][0][0]进行堆叠，然后是arrays[0][0][1]，arrays[1][0][1]与arrays[2][0][1]进行堆叠，接着为arrays[0][0][2]，arrays[1][0][2]与arrays[2][0][2]进行堆叠......


`切割后重新组合：`

* 哪里切割组合后 axis 指定哪里：如下所示：

<div align='center'><img height='350' width='520' src='./static/2.jpg'> </div>




`注意：`

    np.stack((reR,reG,reB,reA),2) 等同于 np.stack((reR,reG,reB,reA),axis=2)


* [np.stack() 案例说明](https://blog.csdn.net/u011501388/article/details/81057578)



### 重新设置索引 reset_index()

生成数据：

    import pandas as pd
    import numpy as np
    df = pd.DataFrame(np.arange(20).reshape(5,4),index=[1,3,4,6,8])
    print(df)


输出：

        0   1   2   3
    1   0   1   2   3
    3   4   5   6   7
    4   8   9  10  11
    6  12  13  14  15
    8  16  17  18  19

重新生成索引，`保留原索引为数据列`：

    print(df.reset_index())

输出：

      index   0   1   2   3
    0      1   0   1   2   3
    1      3   4   5   6   7
    2      4   8   9  10  11
    3      6  12  13  14  15
    4      8  16  17  18  19




重新生成索引，`丢弃原索引`：

    print(df.reset_index(drop=True))

输出：

        0   1   2   3
    0   0   1   2   3
    1   4   5   6   7
    2   8   9  10  11
    3  12  13  14  15
    4  16  17  18  19

### DataFrame 类型数据结构类似数据库表。

  它包括了行索引和列索引，我们可以将 DataFrame 看成是由相同索引的 Series 组成的字典类型。
  
  我们虚构一个考试的场景，想要输出几位英雄的考试成绩：
    
    import pandas as pd
    from pandas import Series, DataFrame
    data = {'Chinese': [66, 95, 93, 90,80],'English': [65, 85, 92, 88, 90],'Math': [30, 98, 96, 77, 90]}
    df1= DataFrame(data)
    df2 = DataFrame(data, index=['ZhangFei', 'GuanYu', 'ZhaoYun', 'HuangZhong', 'DianWei'], columns=['English', 'Math', 'Chinese'])
    print df1
    print df2

  在后面的案例中，我一般会用 df, df1, df2 这些作为 DataFrame 数据类型的变量名，我们以例子中的 df2 为例，
  列索引是 [‘English’, ‘Math’, ‘Chinese’]，行索引是 [‘ZhangFei’, ‘GuanYu’, ‘ZhaoYun’, ‘HuangZhong’, ‘DianWei’]，所以 df2 的输出是：
  
  
                  English  Math  Chinese
    ZhangFei         65    30       66
    GuanYu           85    98       95
    ZhaoYun          92    96       93
    HuangZhong       88    77       90
    DianWei          90    90       80

在了解了 Series 和 DataFrame 这两个数据结构后，我们就从数据处理的流程角度，来看下他们的使用方法。

Pandas 允许直接从 xlsx，csv 等文件中导入数据，也可以输出到 xlsx, csv 等文件，非常方便。
  
    import pandas as pd
    from pandas import Series, DataFrame
    score = DataFrame(pd.read_excel('data.xlsx'))
    score.to_excel('data1.xlsx')
    print score
  
需要说明的是，在运行的过程可能会存在缺少 xlrd 和 openpyxl 包的情况，到时候如果缺少了，可以在命令行模式下使用“pip install”命令来进行安装。


### Dataframe 和 Series 的坑

*   `_场景：在一段df数据中我们进行取值`

    *   `数据：`
  
            train_data.head()

        out：

              Survived	Pclass	Sex	Age	Fare	Embarked	Title	Isalone
            0 	0	      3	      1	  1	  0   	2	        2	    0
            1	  1	      1	      0	  2	  0	    0	        3	    0
            2	  1	      3	      0	  1	  0   	2	        1	    1
            3	  1	      1	      0	  2	  0	    2	        3	    0
            4	  0	      3	      1	  2	  0	    2	        2	    1


*   `双括号和单括号之间的区别：`

    *   `双括号取出的 Survived 是 DataFrame 格式`

            type(train_data[['Survived']].head())

            pandas.core.frame.DataFrame

    *   `单括号取出的 Survived 是 Series 格式`

            type(train_data['Survived'].head())
            pandas.core.series.Series

        取出来为 Series 格式，但是你没有发觉，此后如果想赋值一个新的列，那么无法成功，因为Series中只能有一列。

*   `为列进行赋值后，数据全为NaN：`

    *   那是因为df赋值时是按照索引意义对应进行赋值，如果两个df之间的索引对不上，那么数据全为NaN


            # data 和 carshare 两个 df 的 索引 不同，其赋值给 data 之后，data的 car_hours字段为NaN
            data['car_hours'] = carshare['car_hours'][0:len(data['centroid_lon'])]

            # 可以使用 .values 方法，让其不按照 index 一一对应。
            data['car_hours'] = carshare['car_hours'][0:len(data['centroid_lon'])].values


*   `df中已存在index或者columns如果需要改名需要使用rename，不可以直接进行替换：`

    *   `数据展示`

            data = pd.DataFrame(geoCoord).T
            data

        out：

                    0	      1
            甘肃	103.73	36.03
            青海	101.74	36.56
            四川	104.06	30.67
            河北	114.48	38.03
            云南	102.73	25.04

        我们可以看到原先的列中就存在列名，其列名为 0 和 1。现在我们想要对其进行改名


    *   `错误做法：`

            data = pd.DataFrame(geoCoord).T
            data.columns={'centroid_lon','centroid_lat'}
            data

        error-out:

                  centroid_lat	centroid_lon
            甘肃	  103.73	        36.03
            青海	  101.74	        36.56
            四川	  104.06	        30.67
            河北	  114.48	        38.03

        这里我们发现，即使我们在定义列名的时候先指定定义 centroid_lon 再 定义 centroid_lat 也是无用的，他会优先将字符串顺序高的列替换原先的 0 列，优先级低的替换 1 列。

    *   `正确做法：`

            data = pd.DataFrame(geoCoord).T
            data.rename(columns={0:'centroid_lon',1:'centroid_lat'},inplace=True)
            data

        right-out:


                  centroid_lon	centroid_lat
            甘肃	  103.73	        36.03
            青海	  101.74	        36.56
            四川	  104.06	        30.67
            河北	  114.48	        38.03



### pandas 数据类型

<!-- Pandas `dtype` 映射： -->

<!-- <div align=center><img width="700" height="430" src="./static/pandas/3.png"/></div> -->


### 将 object 类型转为 category 分类类型

`1、`使用 .astype 方法进行转换。

例如：

    train['groupId'] = train['groupId'].astype('category')

当有很多特征中不同的数据太多时，使用单热编码相当于就是计算自杀。 我们可以将把它们变成类别代码。 这样我们仍然可以从随机森林算法中的组和匹配之间的相关性中受益。

因为使用的是随机森林所以特征之间的大小对模型没有什么影响所以可以采用分类标签。`注意：还是针对于对距离没有影响的算法才推荐使用。`

`2、`使用字典进行多特征转换：

    types = {
            'ip'            : 'uint32',
            'app'           : 'uint16',
            'device'        : 'uint16',
            'os'            : 'uint16',
            'channel'       : 'uint16',
            'is_attributed' : 'uint8',
            }

    train = pd.read_csv('../input/train_sample.csv', dtype=dtypes)




### 数据清洗
  数据清洗是数据准备过程中必不可少的环节，Pandas 也为我们提供了数据清洗的工具，在后面数据清洗的章节中会给你做详细的介绍，这里简单介绍下 Pandas 在数据清洗中的使用方法。
  
  还是以上面这些英雄人物的数据为例。
  
    data = {'Chinese': [66, 95, 93, 90,80],'English': [65, 85, 92, 88, 90],'Math': [30, 98, 96, 77, 90]}
    df2 = DataFrame(data, index=['ZhangFei', 'GuanYu', 'ZhaoYun', 'HuangZhong', 'DianWei'], columns=['English', 'Math', 'Chinese'])

  在数据清洗过程中，一般都会遇到以下这几种情况，下面我来简单介绍一下。
  
  `1. 删除 DataFrame 中的不必要的列或行：`
  
  Pandas 提供了一个便捷的方法 drop() 函数来删除我们不想要的列或行。比如我们想把“语文”这列删掉。
    
    df2 = df2.drop(columns=['Chinese'])

  想把“张飞”这行删掉。
  
    df2 = df2.drop(index=['ZhangFei'])
  
  刪除不需要的列名也可以用delete()，删除train_data.columns的第一列，注意delete不能对df数据类型使用。

    coeff_df = pd.DataFrame(train_data.columns.delete(0))


  `2. 重命名列名 columns，让列表名更容易识别：`
  
  如果你想对 DataFrame 中的 columns 进行重命名，可以直接使用 rename(columns=new_names, inplace=True) 函数，比如我把列名 Chinese 改成 YuWen，English 改成 YingYu。
  
    # inplace：刷选过缺失值得新数据是存为副本还是直接在原数据上进行修改。
    df2.rename(columns={'Chinese': 'YuWen', 'English': 'Yingyu'}, inplace = True)

  或者使用：

    # 直接覆盖式重命名
    df.columns = ['Chinese','English']  

  批量修改列名：

    # 将列名中包含 `id-` 的列进行修改，修改后重新赋值给 columns
    df_test.columns = df_test.columns.map(lambda x: x.split('-')[0]+ '_'+ x.split('-')[1] if 'id-' in x else x)

    或者：

    df_test.rename(lambda x: x.split('-')[0]+ '_'+ x.split('-')[1] if 'id-' in x else x, inplace=True)



  `3. 去重复的值：`

  数据采集可能存在重复的行，这时只要使用 drop_duplicates() 就会自动把重复的行去掉。
  
    df = df.drop_duplicates() # 去除重复行

  去除某几列重复的行数据

    data.drop_duplicates(subset=['A','B'],keep='first',inplace=True)


    subset： 列名，可选，默认为None

    keep： {‘first’, ‘last’, False}, 默认值 ‘first’
             first： 保留第一次出现的重复行，删除后面的重复行。
             last： 删除重复项，除了最后一次出现。
             False： 删除所有重复项。

    inplace：布尔值，是否在原数据中进行修改



  `4. 格式问题：`
  
  这是个比较常用的操作，因为很多时候数据格式不规范，我们可以使用 astype 函数来规范数据格式，比如我们把 Chinese 字段的值改成 str 类型，或者 int64 可以这么写：
  
    df2['Chinese'].astype('str') 
    df2['Chinese'].astype(np.int64) 

  
### 数据间的空格

  有时候我们先把格式转成了 str 类型，是为了方便对数据进行操作，这时想要删除数据间的空格，我们就可以使用 strip 函数：
  
    # 删除左右两边空格
    df2['Chinese']=df2['Chinese'].map(str.strip)
    # 删除左边空格
    df2['Chinese']=df2['Chinese'].map(str.lstrip)
    # 删除右边空格
    df2['Chinese']=df2['Chinese'].map(str.rstrip)

  如果数据里有某个特殊的符号，我们想要删除怎么办？同样可以使用 strip 函数，比如 Chinese 字段里有美元符号，我们想把这个删掉，可以这么写：

    df2['Chinese']=df2['Chinese'].str.strip('$')


`大小写转换：`
  
  大小写是个比较常见的操作，比如人名、城市名等的统一都可能用到大小写的转换，在 Python 里直接使用 upper(), lower(), title() 函数，方法如下：
  
    # 全部大写
    df2.columns = df2.columns.str.upper()
    # 全部小写
    df2.columns = df2.columns.str.lower()
    # 首字母大写
    df2.columns = df2.columns.str.title()

  
`查找缺失值：`

  数据量大的情况下，有些字段存在空值 NaN 的可能，这时就需要使用 Pandas 中的 `isnull` 函数进行查找。比如，我们输入一个数据表如下：
    
    姓名     语文     英语     数学
    
    张飞     66       65        
    
    关羽     95       85       98   
  
    赵云     95       92       96   
    
    黄忠     90       88       77   
  
    典韦     80       90       90   
  
  
  
  如果我们想看下哪个地方存在空值 NaN，可以针对数据表 df进行 `df.isnull()` :结果如下

        姓名      语文     英语     数学
     0  False    False    False    True   
     1  False    False    False    False   
     2  False    False    False    False   
     3  False    False    False    False   
     4  False    False    False    False   

  
  如果我想知道哪列存在空值，可以使用 `df.isnull().any()` ，结果如下：
  
    姓名     False
    语文     False
    英语     False
    数学     True
  
  `直接查看哪个列存在空值：`

    train.isnull().any()[train.isnull().any().values==True]

  out：

    overview    True
    runtime     True
    tagline     True
    dtype: bool

  `直接查看哪个行存在空值：`

    train[train.isnull().values==True]


  
  `使用 apply 函数对数据进行清洗：`
  
    apply 函数是 Pandas 中自由度非常高的函数，使用频率也非常高。
    比如我们想对 name 列的数值都进行大写转化可以用：
  
      df['name'] = df['name'].apply(str.upper)
      
    我们也可以定义个函数，在 apply 中进行使用。比如定义 double_df 函数是将原来的数值 *2 进行返回。然后对 df1 中的“语文”列的数值进行 *2 处理，可以写成：
  
      def double_df(x):
           return 2*x
      df1[u'语文'] = df1[u'语文'].apply(double_df)

    我们也可以定义更复杂的函数，比如对于 DataFrame，我们新增两列，其中’new1’列是“语文”和“英语”成绩之和的 m 倍，'new2’列是“语文”和“英语”成绩之和的 n 倍，我们可以这样写：
      
        def plus(df,n,m):
          df['new1'] = (df[u'语文']+df[u'英语']) * m
          df['new2'] = (df[u'语文']+df[u'英语']) * n
          return df
        df1 = df1.apply(plus,axis=1,args=(2,3,))

    其中 axis=1 代表按照列为轴进行操作，axis=0 代表按照行为轴进行操作，args 是传递的两个参数，即 n=2, m=3，在 plus 函数中使用到了 n 和 m，从而生成新的 df。
  
  `使用 apply 函数对数据进行清洗 (进阶) ：`
    
    # 将 train['production_companies'] 中的数据作为 x 传给apply函数，在apply中做处理
    # (取得的 production_companies 特征中的所有数据 [该数据是一个字典] 然后做if else判断)
    
    train['production_companies'].apply(lambda x: [i['name'] for i in x] if x != {} else []).values


`自定义函数apply`
 
      def search_hundredth(train_content):
          hundredth=train_content.loc[99]
          return hundredth

      search_func=train_content.apply(search_hundredth)
      print(search_func)

      1. 不同于 transform 只允许在 Series 上进行一次转换， apply对整个DataFrame 作用

      2.apply隐式地将group 上所有的列作为自定义函数

`自定义函数 transfrom`

  apply()里面可以跟自定义的函数，包括简单的求和函数以及复杂的特征间的差值函数等（注：apply不能直接使用agg()方法 / transform()中的python内置函数，例如sum、max、min、'count‘等方法）

  transform() 里面不能跟自定义的特征交互函数，因为transform是真针对每一元素（即每一列特征操作）进行计算，也就是说在使用 transform() 方法时，需要记得三点：

  1、`它只能对每一列进行计算`，所以在groupby()之后，.transform()之前是要指定要操作的列，这点也与apply有很大的不同。

  2、由于是只能对每一列计算，所以方法的通用性相比apply()就局限了很多，例如只能求列的最大/最小/均值/方差/分箱等操作

  3、transform还有什么用呢?最简单的情况是试图将函数的结果分配回原始的dataframe。也就是说返回的shape是 (len(df)，1)。注：如果与groupby()方法联合使用，需要对值进行去重。


`transform 和 apply的相同之处：`

  都能针对dataframe完成特征的计算，并且常常与groupby()方法一起使用。



  ### 数据统计
  
  在数据清洗后，我们就要对数据进行统计了。Pandas 和 NumPy 一样，都有常用的统计函数，如果遇到空值 NaN，会自动排除。
  常用的统计函数包括：
  以下函数可以指定计算的axis为行还是列，默认为行(就是某一列中的每一行特征) axis=0。注意如果计算所在某一列/行中存在字符，则该列/行将不进行计算

    count()     统计个数，空值NaN不计算
    describe()  一次性输出多个统计指标，包括：count,mean,std,min,max 等
    min()       最小值
    max()       最大值
    sum()       总和
    mean()      平均值
    median()    中位数
    var()       方差
    std()       标准差
    argmin()    统计最小值的索引位置
    argmax()    统计最大值的索引位置
    idxmin()    统计最小值的索引值
    idxmax()    统计最大值的索引值
    
    
  表格中有一个 describe() 函数，统计函数千千万，describe() 函数最简便。它是个统计大礼包，可以快速让我们对数据有个全面的了解。下面我直接使用 df1.describe() 输出结果为：
    
  分析泰坦尼克数据为例：

*   train_df.describe() # 默认只计算数值类型

    out:

              PassengerId	  Survived	 Pclass	       Age	       SibSp	     Parch	    Fare
        count	891.000000	 891.000000	891.000000	714.000000	891.000000	891.000000	891.000000
        mean	446.000000	 0.383838	  2.308642	  29.699118	  0.523008  	0.381594	  32.204208
        std	  257.353842	  0.486592	0.836071  	14.526497	  1.102743	  0.806057	  49.693429
        min	  1.000000	    0.000000	1.000000  	0.420000	  0.000000	  0.000000	  0.000000
        25%	  223.500000	  0.000000	2.000000	  20.125000	  0.000000	  0.000000	  7.910400
        50%	  446.000000	  0.000000	3.000000	  28.000000	  0.000000	  0.000000	  14.454200
        75%	  668.500000	  1.000000	3.000000	  38.000000	  1.000000	  0.000000	  31.000000
        max	  891.000000	  1.000000	3.000000	  80.000000	  8.000000	  6.000000	  512.329200

*   train_df.describe(include=['O']) # 只计算类型为str的数据
    
    out:
    
              Name	                           Sex	Ticket	Cabin	       Embarked
        count	  891         	                 891	891	     204	          889
        unique	891	                           2	  681	     147	          3
        top	Panula, Master. Juha Niilo	       male	1601	C23 C25 C27	      S
        freq    1                              577	7	         4	          644

*   删除不想保留的字段，并且将其转置(换一种方式看数据)

        如：train.describe(include=np.number).drop('count').T  # 删除 count 函数计算出的字段，并且转置，让计算特征作为列

    out:

                      mean	 std   min	 25%	  50%	  75%	  max
        assists	      0.234	0.589	0.000	0.000	0.000	0.000	22.000
        boosts	      1.107	1.716	0.000	0.000	0.000	2.000	33.000
        damageDealt	  130.633	169.887	0.000	0.000	84.240	186.000	6,616.000
        DBNOs	        0.658	1.146	0.000	0.000	0.000	1.000	53.000


### 一、数据表合并 merge
    
   有时候我们需要将多个渠道源的多个数据表进行合并，一个 DataFrame 相当于一个数据库的数据表，那么多个 DataFrame 数据表的合并就相当于多个数据库的表合并。
    
   比如我要创建两个 DataFrame：
    
      df1 = DataFrame({'name':['ZhangFei', 'GuanYu', 'a', 'b', 'c'], 'data1':range(5)})
      df2 = DataFrame({'name':['ZhangFei', 'GuanYu', 'A', 'B', 'C'], 'data2':range(5)})

   `1. 基于指定列进行连接`
      
   比如我们可以基于 name 这列进行连接。
    
        df3 = pd.merge(df1, df2, on='name')

   `运行结果:` 
   
   ![Image text](./static/1.png)
    
    
   `2. inner 内连接`
        
   inner 内链接是 merge 合并的默认情况，inner 内连接其实也就是键的交集，在这里 df1, df2 相同的键是 name，所以是基于 name 字段做的连接：
     
        df3 = pd.merge(df1, df2, how='inner')
   `运行结果:` 
   
   ![Image text](./static/2-1.png)
    
   `3. left 左连接`
    
   左连接是以第一个 DataFrame 为主进行的连接，第二个 DataFrame 作为补充。
        
          df3 = pd.merge(df1, df2, how='left')
   `运行结果:`
   
  ![Image text](./static/3.png)

   `4. right 右连接`
        
   右连接是以第二个 DataFrame 为主进行的连接，第一个 DataFrame 作为补充。
      
        df3 = pd.merge(df1, df2, how='right')
   `运行结果:`
   
  ![Image text](./static/4.png)

   `5. outer 外连接`

   外连接相当于求两个 DataFrame 的并集。
        
        df3 = pd.merge(df1, df2, how='outer'，on= '指定列进行对应合并')

   `运行结果:`
   
  ![Image text](./static/5.png)
  

### 一、数据表合并 `join`

    join(self, other, on=None, how='left', lsuffix='', rsuffix='',sort=False):

  其中参数的意义与 `merge` 方法基本相同,只是 `join` 方法 `默认` 为 `左外连接how=left`,参数 `on` 指定列进行对应合并。

  * 1.默认按索引合并，可以合并相同或相似的索引，不管他们有没有重叠列。

  * 2.可以连接多个DataFrame

  * 3.可以连接除索引外的其他列

  * 4.连接方式用参数how控制

  * 5.当两个df数据的列名相同时，需要指定 lsuffix='', rsuffix='' 区分相同列名的列，如下

    例子：

        df1.join(df2, lsuffix='_l', rsuffix='_r',on='shop_id') # 列名重复的时候需要指定lsuffix, rsuffix参数

    效果图：

      <div align=center><img width="550" height="250" src="./static/join.jpg"/></div>



### `align 对齐`

    df.align(df2,join='',axis=)


使用指定的每个轴索引的连接方法，将轴上的两个对象对齐



数据准备：

    df1 = pd.DataFrame([[1,2,3,4], [6,7,8,9]], columns=['D', 'B', 'E', 'A'], index=[1,2])
    df2 = pd.DataFrame([[10,20,30,40], [60,70,80,90], [600,700,800,900]], columns=['A', 'B', 'C', 'D'], index=[2,3,4])

    print(df1)

      D  B  E  A
    1  1  2  3  4
    2  6  7  8  9
    print(df2)

        A    B    C    D
    2   10   20   30   40
    3   60   70   80   90
    4  600  700  800  900

案例：

    # 常用
    a1, a2 = df1.align(df2, join='inner', axis=1)
    print(a1)
    print(a2)

输出：

      D  B  A
    1  1  2  4
    2  6  7  9

        D    B    A
    2   40   20   10
    3   90   70   60
    4  900  700  600

`小结：`可以看到当 `join='inner'` ，`axis=1` 时就能筛选出两个数据中共同列的部分，可以用于匹配 `训练数据` 和 `测试数据` 的共同特征,如下：


    # 先拿出目标列
    train_labels = app_train['TARGET']

    # Align the training and testing data, keep only columns present in both dataframes 
    # 选出两个数据中共同拥有的列，不共有的列过滤出去
    app_train, app_test = app_train.align(app_test, join = 'inner', axis = 1)

    # Add the target back in
    app_train['TARGET'] = train_labels

    print('Training Features shape: ', app_train.shape)
    print('Testing Features shape: ', app_test.shape)




### `df.pct_change()`

`语法：`

    DataFrame.pct_change(periods=1, fill_method=‘pad’, limit=None, freq=None, **kwargs)

* 表示当前元素与先前元素的相差百分比，当指定periods=n,表示当前元素与先前n 个元素的相差百分比。

`案例：`

    df = pd.DataFrame({
      'FR': [4.0405, 4.0963, 4.3149],
        'GR': [1.7246, 1.7482, 1.8519],
        'IT': [804.74, 810.01, 860.13]},
        index=['1980-01-01', '1980-02-01', '1980-03-01'])
    print(df)
    print(df.pct_change())
    print(df.pct_change(axis='columns'))#可以指定按照行还是列进行计算的

`输出：`

                    FR      GR      IT
    1980-01-01  4.0405  1.7246  804.74
    1980-02-01  4.0963  1.7482  810.01
    1980-03-01  4.3149  1.8519  860.13
                      FR        GR        IT
    1980-01-01       NaN       NaN       NaN
    1980-02-01  0.013810  0.013684  0.006549
    1980-03-01  0.053365  0.059318  0.061876
                FR        GR          IT
    1980-01-01 NaN -0.573172  465.624145
    1980-02-01 NaN -0.573225  462.339435
    1980-03-01 NaN -0.570813  463.458124
  
    --> a = 0.013810 = (4.0963 - 4.0405)/4.0405
    --> 4.0405 * (1+a) = 4.0963




 ### 如何用 SQL 方式打开 Pandas
    
 Pandas 的 DataFrame 数据类型可以让我们像处理数据表一样进行操作，比如数据表的增删改查，都可以用 Pandas 工具来完成。
 不过也会有很多人记不住这些 Pandas 的命令，相比之下还是用 SQL 语句更熟练，用 SQL 对数据表进行操作是最方便的，它的语句描述形式更接近我们的自然语言。
    
 事实上，在 Python 里可以直接使用 SQL 语句来操作 Pandas。
    
 `这里给你介绍个工具：pandasql。`
    
 pandasql 中的主要函数是 sqldf，它接收两个参数：一个 SQL 查询语句，还有一组环境变量 globals() 或 locals()。这样我们就可以在 Python 里，直接用 SQL 语句中对 DataFrame 进行操作，举个例子：import pandas as pd
     
  `例子：`
    
      from pandas import DataFrame
      from pandasql import sqldf, load_meat, load_births
      df1 = DataFrame({'name':['ZhangFei', 'GuanYu', 'a', 'b', 'c'], 'data1':range(5)})
      pysqldf = lambda sql: sqldf(sql, globals())
      sql = "select * from df1 where name ='ZhangFei'"
      print pysqldf(sql)

    
  `运行结果：`
  
      data1      name
      0      0  ZhangFei

  上面这个例子中，我们是对“name='ZhangFei”“的行进行了输出。当然你会看到我们用到了 lambda，lambda 在 python 中算是使用频率很高的，那 lambda 是用来做什么的呢？
  它实际上是用来定义一个匿名函数的，具体的使用形式为：
  
      lambda argument_list: expression

  
  
  这里 argument_list 是参数列表，expression 是关于参数的表达式，会根据 expression 表达式计算结果进行输出返回。
  
  在上面的代码中，我们定义了：
  
    pysqldf = lambda sql: sqldf(sql, globals())

  在这个例子里，输入的参数是 sql，返回的结果是 sqldf 对 sql 的运行结果，当然 sqldf 中也输入了 globals 全局参数，因为在 sql 中有对全局参数 df1 的使用。
  
  ### 读取文件里的内容
  以csv的格式读取文件里的内容
    
    train_content=pd.read_csv("train.csv")
  
  以xls的格式读取文件里的内容 (excel文件读取方式)
    
    train_content=pd.read_excel("train.xls")
    
  显示pd_content的前面三行(不包括列名字)  
     
     print(train_content.head(3)


### `pd.crosstab 交叉表`

`原数据：`

<div align=center><img width="350" height="270" src="./static/crosstab1.jpg"/></div>

`交叉操作: `

    pd.crosstab(df.Nationality, df.Handedness)

`展示：`

<div align=center><img width="200" height="170" src="./static/crosstab2.jpg"/></div>


`更多：`

* https://www.cnblogs.com/rachelross/p/10468589.html

### `df.pivot_table 函数`

* https://www.cnblogs.com/onemorepoint/p/8425300.html

* https://www.cnblogs.com/Yanjy-OnlyOne/p/11195621.html

`pivot_table` 有四个最重要的参数 `index`、`values`、`columns`、`aggfunc`

* `index`：`index` 代表索引，每个 `pivot_table` 必须拥有一个 `index` 。
  
* `Values`：`Values` 可以对需要的计算数据进行筛选
  
* `Aggfunc`：`aggfunc` 参数可以设置我们对数据聚合时进行的函数操作。

  当我们未设置 `aggfunc` 时，它默认 `aggfunc='mean'` 计算均值，可以设置多个 如：
            `[aggfunc=[np.sum,np.mean]]` 此时会显示 `np.sum` 和 `np.mean` 统计出来的数据。
            
* `Columns`：Columns类似Index可以设置列层次字段，它不是一个必要参数，作为一种分割数据的可选方式。

案例：

    #以 Pclass(船舱)为索引 查看不同船舱人员的平均存活率Survived。
    train_survived=train_content.pivot_table(index="Pclass",values="Survived")
    
    # 查看不同船舱的收费均值是多少
    train_age_fare=train_content.pivot_table(index="Pclass",values=["Age","Fare"])
    
    # 查看不同船舱人员的的人均年龄
    train_survived=train_content.pivot_table(index="Pclass",values="Age")

### `droplevel() 删除列标题`

    Series.droplevel(self, level, axis=0)
`droplevel()` 方法用于返回已删除请求的索引/列级别的DataFrame

案例：

    sales_by_item_id = sale_train.pivot_table(index=['item_id'],values=['item_cnt_day'], 
                                            columns='date_block_num', aggfunc=np.sum, fill_value=0).reset_index()

    sales_by_item_id.head()


<div align=center><img width="950" height="170" src="./static/pivot_table.jpg"/></div>


droplevel 处理：可以用于去掉级别的名称或位置索引

    sales_by_item_id.columns = sales_by_item_id.columns.droplevel().map(str)
    sales_by_item_id.head()

<div align=center><img width="950" height="170" src="./static/droplevel.jpg"/></div>


常用的处理：


    sales_by_item_id = sale_train.pivot_table(index=['item_id'],values=['item_cnt_day'], 
                                          columns='date_block_num', aggfunc=np.sum, fill_value=0).reset_index()
    sales_by_item_id.columns = sales_by_item_id.columns.droplevel().map(str)
    sales_by_item_id = sales_by_item_id.reset_index(drop=True).rename_axis(None, axis=1)
    sales_by_item_id.columns.values[0] = 'item_id'

    sales_by_item_id.head()

<div align=center><img width="950" height="170" src="./static/droplevel2.jpg"/></div>


### `df.pivot 函数`

    DataFrame.pivot(self, index=None, columns=None, values=None) → ’DataFrame’

介绍：

* index：重塑的新表的索引名称是什么。

* columns：重塑的新表的列名称是什么，一般来说就是被统计列的分组。

* values就是生成新列的值应该是多少，如果没有，则会对data_df剩下未统计的列进行重新排列放到columns的上层。


数据展示：

<div align=center><img width="950" height="170" src="./static/3.jpg"/></div>

    train_group = train_flattened.groupby(["month", "weekday"])['Visits'].mean().reset_index()
    train_group = train_group.pivot('weekday','month','Visits')
    train_group.sort_index(inplace=True)


结果展示：

<div align=center><img width="450" height="250" src="./static/4.jpg"/></div>





### icol和col 取范围
    
  iloc和loc的区别是 iloc只能跟整数，而loc可以跟数字
   
     print(train_content.iloc[83,3])     #找的是除title以外的第84行，因为数组默认是从0开始向上增长的
     print(train_content.iloc[82:83,3:5]) #去尾的83不包括 5不包括
     print(train_content.iloc[82:84,3:6]) #去尾的83不包括 5不包括

     print(train_content.loc[83,"Age"])
     print(train_content.loc[82:83,"Name":"Age"])   #还可以跟范围



### 查找 df 数据列中 列数据等于 x 长度的值

    # 找到在 genres 列中列长度等于7的数据
    train[train['genres'].str.len() == 7]


### 将Pandas中的DataFrame类型转换成Numpy中array类型的三种方法
  dataframe 转列表  
      
      
1、使用DataFrame中的values方法

    df.values
  
2、使用DataFrame中的as_matrix()方法

    df.as_matrix()

3、使用Numpy中的array方法

    np.array(df)

### pandas.mode() 

`返回出现频率最高的值 默认 axis=0，即每一特征中出现最高的值 默认忽略NA值，如果想将NA值计算进去可以使用 dropna=False`

`注意：如果存在频率相同的值会返回两个值`

`数据:`

    >>> df = pd.DataFrame([('bird', 2, 2),
    ...                    ('mammal', 4, np.nan),
    ...                    ('arthropod', 8, 0),
    ...                    ('bird', 2, np.nan)],
    ...                   index=('falcon', 'horse', 'spider', 'ostrich'),
    ...                   columns=('species', 'legs', 'wings'))
    >>> df
              species  legs  wings
    falcon        bird     2    2.0
    horse       mammal     4    NaN
    spider   arthropod     8    0.0
    ostrich       bird     2    NaN  

`例子1`

    >>> df.mode()
      species  legs  wings
    0    bird   2.0    0.0
    1     NaN   NaN    2.0

`例子2`

    >>> df.mode(dropna=False)
      species  legs  wings
    0    bird     2    NaN


### 删除缺失值：df.dropna()

    df = pd.DataFrame({"name": ['Alfred', 'Batman', 'Catwoman'],
                      "toy": [np.nan, 'Batmobile', 'Bullwhip'],
                      "born": [pd.NaT, pd.Timestamp("1940-04-25"),
                               pd.NaT]})

    >>> df

          name        toy       born
    0    Alfred        NaN        NaT
    1    Batman  Batmobile 1940-04-25
    2  Catwoman   Bullwhip        NaT

`删除至少缺少一个元素的行`

    df.dropna()

       name        toy       born
    1  Batman  Batmobile 1940-04-25


`删除至少缺少一个元素的列。`

    df.dropna(axis='columns')

          name
    0    Alfred
    1    Batman
    2  Catwoman

`删除缺少所有元素的行。`

    >>> df.dropna(how='all')

          name        toy       born
    0    Alfred        NaN        NaT
    1    Batman  Batmobile 1940-04-25
    2  Catwoman   Bullwhip        NaT

`仅保留至少包含2个非NA值的行。`

    >>> df.dropna(thresh=2)

          name        toy       born
    1    Batman  Batmobile 1940-04-25
    2  Catwoman   Bullwhip        NaT

`删除某列中缺失值：`

    df['toy'].dropna()

    1    Batmobile
    2     Bullwhip
    Name: toy, dtype: object


### 删除包含某个值的行

比如现在有如下数据：

      用户标识     姓名     ...    状态    下班打卡地址
    0   23927     小明     ...    缺卡      NaN
    1   20218     小王     ...    正常      NaN
    2   12193     小李     ...    缺卡      NaN
    3    7320     小黑     ...    早退      NaN
    4   10322     小绿     ...    早退      NaN
    5   42379     小红     ...    正常      NaN


现在我们想要过滤掉(删除掉)缺卡状态的数据：

    # 通过~取反，选取`状态` 中不包含 `缺卡` 的行
    data = data[~data['状态.1'].isin(['缺卡'])]
    print(data)

最终结果：

      用户标识     姓名     ...    状态    下班打卡地址
    1   20218     小王     ...    正常      NaN
    3    7320     小黑     ...    早退      NaN
    4   10322     小绿     ...    早退      NaN
    5   42379     小红     ...    正常      NaN


### 修改 dataframe 中的数据，保留其中部分内容 

只保留 qiandao['姓名'] 名字中的汉字，其他字符去除。

    def filter_name(names):
      data_lists = []
      df_name = pd.DataFrame({'new_姓名':data_lists})
      
      for index,name in zip(names.index,names):
          name = re.sub(u'[^\u4e00-\u9fff]+','',name,flags=re.U)
          df_name.loc[index,'new_姓名'] = name
      return df_name

    filter_name(qiandao['姓名'])

`[^\u4e00-\u9fff]+` 用于匹配汉字。

### pandas.DataFrame.fillna 用指定的方法填充NA/NaN

`DataFrame.fillna（value = None，method = None，axis = None，inplace = False，limit = None，downcast = None，** kwargs ）`
        
  value ： 标量，字典，系列或DataFrame用于填充孔的值（例如0），或者用于指定每个索引（对于Series）或列（对于DataFrame）使用哪个值的Dict /Series / DataFrame。（不会填写dict / Series / DataFrame中的值）。该值不能是列表。
                 
  method :  {'backfill'，'bfill'，'pad'，'ffill'，None}，默认无   用于填充重新索引的填充孔的方法系列填充/填充
              
  axis : {0或'索引'，1或'列'}
  
  例子：
  
      >>> df = pd.DataFrame([[np.nan, 2, np.nan, 0],
      ...                    [3, 4, np.nan, 1],
      ...                    [np.nan, np.nan, np.nan, 5],
      ...                    [np.nan, 3, np.nan, 4]],
      ...                    columns=list('ABCD'))
      >>> df
           A    B   C  D
      0  NaN  2.0 NaN  0
      1  3.0  4.0 NaN  1
      2  NaN  NaN NaN  5
      3  NaN  3.0 NaN  4

   `用0替换所有NaN元素`
   
      >>> df.fillna(0)
          A   B   C   D
      0   0.0 2.0 0.0 0
      2   0.0 0.0 0.0 5
      3   0.0 3.0 0.0 4
      1   3.0 4.0 0.0 1

   `我们还可以向前或向后传播非空值。`

    >>> df.fillna(method='ffill')
        A   B   C   D
    0   NaN 2.0 NaN 0
    1   3.0 4.0 NaN 1
    2   3.0 4.0 NaN 5
    3   3.0 3.0 NaN 4

   `将“A”，“B”，“C”和“D”列中的所有NaN元素分别替换为0,1,2和3。`

    >>> values = {'A': 0, 'B': 1, 'C': 2, 'D': 3}
    >>> df.fillna(value=values)
        A   B   C   D
    0   0.0 2.0 2.0 0
    1   3.0 4.0 2.0 1
    2   0.0 1.0 2.0 5
    3   0.0 3.0 2.0 4
    
  `只替换第一个NaN元素。`

    >>> df.fillna(value=values, limit=1)
        A   B   C   D
    0   0.0 2.0 2.0 0
    1   3.0 4.0 NaN 1
    2   NaN 1.0 NaN 5
    3   NaN 3.0 NaN 4
    
    
### pandas.DataFrame.groupby   
  
  DataFrame.groupby(by=None, axis=0, level=None, as_index=True, sort=True, group_keys=True, squeeze=False, observed=False, **kwargs)[source]

*   `参数介绍：`
    
      as_index:为True时会将第一列数据设置为index，为False时则会新建一个数字索引。默认为True

        import pandas as pd

        df = pd.DataFrame(data={'books':['bk1','bk1','bk1','bk2','bk2','bk3'], 'price': [12,12,12,15,15,17]})

        df

        out：
            books	price
          0  bk1	  12
          1	 bk1	  12
          2	 bk1	  12
          3	 bk2	  15
          4	 bk2	  15
          5	 bk3	  17

        a=df.groupby('books', as_index=True).sum()
        print(a.index)
        a

        out：

          Index(['bk1', 'bk2', 'bk3'], dtype='object', name='books')

                price
          books	
          bk1	    36
          bk2	    30
          bk3	    17

        df.groupby('books', as_index=False).sum() 
        
        out： 

            books	price
          0	bk1	  36
          1	bk2	  30
          2	bk3	  17
    
    *   `使用 as_index 时的注意事项：`

            1、df.groupby('books', as_index=False).agg('sum') 

            和

            2、 df.groupby('books', as_index=False).agg(['sum']) 

            的结果是不一样的。

            对于代码2，as_index=False 的效果不会被体现。也就是结果仍然会使用分组数据作为index。
            
            *** 但是在最后加上reset_index()能达到和上面代码相同的效果，因为会重新设置index ***

    `groupby操作涉及拆分对象，应用函数和组合结果的某种组合。这可用于对这些组上的大量数据和计算操作进行分组。`
    
    `使用groupby进行切片之后，我们如果进行操作其实是在那个切片(split)中进行的操作，计算完成之后返回合并结果(Combine) 如图：`


<div align=center><img width="650" height="300" src="./static/2.png"/></div>

*   `groupby函数 例子1`

        import pandas as pd
        import numpy as np

        dict_obj = {'key1' : ['a', 'b', 'a', 'b', 
                              'a', 'b', 'a', 'a'],
                    'key2' : ['one', 'one', 'two', 'three',
                              'two', 'two', 'one', 'three'],
                    'data1': np.random.randn(8),
                    'data2': np.random.randn(8)}
        df_obj = pd.DataFrame(dict_obj)
        print(df_obj)

    `out：`

        key1   key2     data1     data2
        0    a    one -0.109110  0.528666
        1    b    one -0.746051  1.994562
        2    a    two  2.685447  1.672294
        3    b  three  0.546663 -0.970285
        4    a    two -0.859890 -0.964093
        5    b    two -0.347244  0.146132
        6    a    one  0.254899  0.830872
        7    a  three -0.958547 -2.016811


*   `dataframe根据key1进行分组`                          
                                                                                     
       grouped1=df_obj.groupby('key1')

        [x for x in grouped1]


        [('a',   key1   key2     data1     data2
          0       a    one -0.109110  0.528666
          2       a    two  2.685447  1.672294
          4       a    two -0.859890 -0.964093
          6       a    one  0.254899  0.830872
          7       a  three -0.958547 -2.016811), ('b',   key1   key2     data1     data2
          1       b    one -0.746051  1.994562
          3       b  three  0.546663 -0.970285
          5       b    two -0.347244  0.146132)]

*   `使用groupby指定字段内容进行运算`

    `如：`

        # 按照 key1 进行分组从上面的数据中我们可以发现分为a,b两组
        grouped = df.groupby(df['key1'])
        grouped.mean()

    `out：`

        # 这里使用 df['key1'] 做了分组键，即按 a 和 b 进行分组。下例中没有显示 key2 列，是因为其值不是数字类型，被 mean() 方法自动忽视了
            data1	data2
        key1		
        a	0.202560	0.010185
        b	-0.182211	0.390136
        
    `如：` 

        # 以key1进行分组，将data2字段中的内容进行求和
        grouped1=df_obj.groupby(['key1'])['data2'].sum()
        grouped1

    `out：`

        key1
        a    0.050927
        b    1.170409
        Name: data2, dtype: float64

    `如：` 注意使用groupby进行多列的组合时，顺序会影响我们的分组效果 如groupby(['key1','key2']) 和groupby(['key2','key1'])展示出来的效果就会不同

        # 以key1，和key2 进行分组，将data2字段中的内容进行求和
        grouped1=df_obj.groupby(['key1','key2'])['data2'].sum()
        grouped1

    `out：`

        key1  key2 
        a     one      1.359537
              three   -2.016811
              two      0.708200
        b     one      1.994562
              three   -0.970285
              two      0.146132


    `如：`

        # 以Pclass进行分组，将字段中'Pclass','Survived'的内容进行求和
        print(train_data.groupby(['Pclass'])['Pclass','Survived'].mean())
        
                  Pclass    Survived
        Pclass                  
        1          1.0      0.629630
        2          2.0      0.472826
        3          3.0      0.242363
      
        print(train_data.groupby(['Pclass'])['Pclass'，'Survived','Age'].mean())

                  Pclass    Survived        Age
        Pclass                             
        1          1.0      0.629630      37.048118
        2          2.0      0.472826      29.866958
        3          3.0      0.242363      26.403259


*   `分层索引`

        我们可以使用level参数对不同级别的层次索引进行分组：

        >>> arrays = [['Falcon', 'Falcon', 'Parrot', 'Parrot'],
        ...           ['Capitve', 'Wild', 'Capitve', 'Wild']]
        >>> index = pd.MultiIndex.from_arrays(arrays, names=('Animal', 'Type'))
        >>> df = pd.DataFrame({'Max Speed' : [390., 350., 30., 20.]},
        ...                    index=index)
        >>> df
                        Max Speed
        Animal Type
        Falcon Capitve      390.0
              Wild         350.0
        Parrot Capitve       30.0
              Wild          20.0

        >>> df.groupby(level=0).mean()
                Max Speed
        Animal
        Falcon      370.0
        Parrot       25.0

        >>> df.groupby(level=1).mean()
                Max Speed
        Type
        Capitve      210.0
        Wild         185.0

*   `groupby 函数的两个方法 .size() .count()`

    可以使用 GroupBy 对象（不论是 DataFrameGroupBy 还是 SeriesGroupBy）的 .size() 方法查看分组大小：

    `.size 如:`

        grouped.size()

    `out：`

        key1
        a       3
        b       2

    `.count 如：`

        grouped.count()

    `out：`

            key2	data1	data2
        key1			
        a	  5	    5	    5
        b	  3	    3	    3

    `.size 和 .count的区别：`
    
    `1、size计数时包含NaN值，而count不包含NaN值`

    `2、使用 .size 的数据 和 使用 .count() 的数据 产生的输出结果数据的shape不一样`

            count会保留所有特征 同时特征中的值是结算结果(所有值相同 类型：DataFrame)，但是size不会，size之后保留一列特征(就是计算结果 类型：Series)

    *   `例子`

            train.groupby(['matchType','matchId']).size().shape

            train.groupby(['matchType','matchId']).count().shape
      
        `out`

            (47965,)

            (47965, 27)

*   `groupby.first()`

    首先计算每组中的值(如果某一个分组下有多个不同的值，取第一个值)

    `例子：`

        train.groupby('matchId')['matchType'].value_counts()

    `out`

        matchId         matchType       
        0000a43bce5eec  squad-fpp            95
        0000eb01ea6cdd  squad-fpp            98
        0002912fe5ed71  solo                 95
        0003b92987589e  duo                 100
        0006eb8c17708d  duo-fpp              93
        00077604e50a63  squad-fpp            98


    `例子：`

    首先使用 groupby('matchId') ,将 matchId 作为索引，后面接 matchType。
    
    `first` 的作用在于，因为 某一个 matchId 下面可能有多个 matchType。

    `first()` 只取第一个 matchType的值 。

        train.groupby('matchId')['matchType'].first().value_counts()

    `out`

        squad-fpp           18576
        duo-fpp             10620
        squad                6658
        solo-fpp             5679
        duo                  3356
        solo                 2297
        normal-squad-fpp      358
        normal-duo-fpp        158
        normal-solo-fpp        96
        crashfpp               73
        flaretpp               29
        normal-solo            23
        normal-squad           16
        normal-duo             12
        flarefpp                9
        crashtpp                5
        Name: matchType, dtype: int64


*   `pandas.groupby 进阶 groupby.apply().groupby`

    这里注意了，groupby 的后面是不能直接跟 .groupby 的，第一个 groupby 需要经过函数运算，才可以后面接 .groupby

    `例子`

        match = train.groupby(['matchType','matchId']).size().to_frame('players in match')

        # groupby 里面定义的特征是分组特征，函数计算的是最后一个分组特征的值。
        group = train.groupby(['matchType','matchId','groupId']).size().to_frame('players in group')

        # 基于上面分组之后的数据，再进行groupby，就是计算在上面基础的情况下 matchId的计算量，groupId的计算量。
        pd.concat([match.groupby('matchType').describe()[toTapleList(['players in match'],['min','mean','max'])], 
                  group.groupby('matchType').describe()[toTapleList(['players in group'],['min','mean','max'])]], axis=1)

    `解释：`

        解释 groupby.groupby的计算方法：
        
        以 group 为例，第一次使用 groupby 将 matchType , matchId , groupId 作为分组条件,
        使用size作为计算方法，计算出 在相同 matchType 、 matchId ，条件下 不同 groupId出现的次数。
        
        *此时注意*：我们的数据其实就只有一行，即size计算出来的那一行，上述代码中将其命名为 'players in group'。

        紧接着，我们使用第二个 groupby ，将 matchType 作为分组条件，
        然后使用 .describe 方法，其实他做的就是计算 'players in group' 这一行数据，
        因为经过分组计算之后，我们仅剩的数据特征就是 'players in group' 。 

        所以说两个 groupby 第一个先进行计算，后面的groupby是通过指定分组条件的基础上计算 第一个 groupby 分组计算后 得到的数据。

    `例子2`

          desc2 = train.groupby(['matchType','matchId','groupId']).count().groupby(['matchType','matchId']).size()\
          .to_frame('groups in match')

    `解释2：`

        首先 我们想得到每个 groupId 在每个 matchId 下出现的次数
        
        使用 train.groupby(['matchType','matchId','groupId']).count().groupby(....)  
        而不是直接使用 train.groupby(['matchType','matchId']).size()的目的在于，
        在相同的 matchId 下可能存在 相同的 groupId ，所以我们两个groupby连接。

        第一个 groupby 用于，帮助我们发现每个 不同的groupId 在 matchId 下出现的次数(通过 count 计算)
        第二个 groupby 就是指定分组，帮助我们对之前groupby计算出来的数据进行统计。

        所以说直接使用 train.groupby(['matchType','matchId']).size() 
        我们统计的是 matchId 下不同类别出现的次数，可能包含相同的 groupId。



### `agg 和 aggregate`

* `agg` 是 `aggregate` 的别名：

  代码，下面两行代码做的事情相同，`agg` 中可以使用字典的形式传参：
  
    都是通过：`ProductName` 进行 `groupby` 然后对 `MachineIdentifier` 计算 `count`

      train.groupby(['ProductName']).aggregate({'MachineIdentifier':'count'})

      train.groupby(['ProductName'])['MachineIdentifier'].agg('count')


*   `pandas.groupby 进阶 groupby.agg()`

    使用 groupby + agg 对某个分组下的数据做统计

    `需求：` 现在我有一组某鞋子的历史购买记录，现在想要筛选出某一个时间段下，花费的最高价格是多少。

    `例子：`

          购买人	码数	        购买时间	         成交价格

        0		xx	40⅔码	2020-01-18 19:09:06.812429	3379.0
        1		xx	40⅔码	2020-01-18 19:07:06.812429	3609.0
        3   xx	40⅔码	2020-01-18 05:10:06.812429	3739.0
            ...       ...           ...             ...
        6551	xx	40⅔码	2020-01-09 00:00:00.000000	3829.0
        6595	xx	40⅔码	2020-01-08 00:00:00.000000	5999.0
        6607	xx	40⅔码	2020-01-08 00:00:00.000000	3899.0
        6609	xx	40⅔码	2020-01-08 00:00:00.000000	3899.0

    `代码：`

        # 1、表示按照 '购买时间' 这一列来做聚合，'成交价格' 这一列来做统计。
        d1.groupby(['购买时间'],as_index=False)['成交价格'].agg('max') 

        # 上面这句代码的结果等同于这句代码
        d2 = d2.pivot_table(index="购买时间",values=["成交价格"],aggfunc='max')

    `效果：`

              购买时间	                max
        0	2020-01-18 03:10:06.812429	3539.0
        1	2020-01-18 05:10:06.812429	3739.0
        2	2020-01-18 08:10:06.812429	3539.0
        3	2020-01-18 09:10:06.812429	3539.0
        4	2020-01-18 10:10:06.812429	3609.0

    `**注意事项**`

    `1、` 对于agg函数来说，函数中的参数多个和单个是有差别的，如果函数中的参数是 `单个` 比如 agg('max') ,那么最后统计出来的那列数据的列名就是原来的列名，比如为 "购买价格"，但是如果函数中的参数为 `一个列表` ,比如 agg(['max']) ,那么最后统计出来的那列数据的列名就是 'max' 。

        * d1.groupby(['购买时间'])['成交价格'].agg('max') 

        
                                    成交价格
        购买时间	
        2020-01-08 00:00:00.000000	5999.0
        2020-01-09 00:00:00.000000	3829.0

        * d1.groupby(['购买时间'])['成交价格'].agg(['max']) 

                                    max
        购买时间	
        2020-01-08 00:00:00.000000	5999.0
        2020-01-09 00:00:00.000000	3829.0

    `2、` 同时对于agg(['max']), as_index 的效果会失效，也就是说即使你指定了 as_index=False，那么结果仍然会将分组列作为索引。当然可以通过 `reset_index` 的方法进行解决。

        d1.groupby(['购买时间'])['成交价格'].agg(['max']).reset_index() 

### pd.to_frame

pd.to_frame 将 Series 转化为 dataframe

*   `例子`

        x2 = pd.Series(data=[1,2,3,4], index=['a', 'b', 'c', 'd'])
        display(x2)
        print('***********')
        x2.to_frame('hehe')
        x2
    `out`

        a    1
        b    2
        c    3
        d    4
        dtype: int64

        ***********

            hehe
        a	  1
        b	  2
        c	  3
        d	  4
    

*   `例子`

        train.groupby(['matchType','matchId']).count().groupby(['matchType','matchId']).size().shape
      
        group = train.groupby(['matchType','matchId','groupId']).count().groupby(['matchType','matchId']).size().to_frame('groups in match')
        print(group)

        group.shape
    `out`

        (47965,)

                              groups in match
        matchType	  matchId	
        duo	        0003b92987589e	  47
                    0006eb8c17708d	  44
                    00086c74bb4efc	  48
        (47965,1)

    我们可以通过 shape 查看他到底是不是 series，只有 series 才可以进行 to_farme.


### pd.concat 合并 DataFrame

*   `主要说明一下 axis=0 和 axis=1 时的区别：`

        axis = 1，表示按行合并 （往右接）
        axis = 0，表示按列合并 （往下接）


    `使用方法：`

        pd.concat([df1,df2],axis=num)



* 需要注意的是：

      比如进行添加新样本（往下合并），合并时concat 会根据 df1,和df2 的列名进行匹配合并（总之确保df1和df2的列名一样），如果列名不匹配他会新增这些列名进行合并，也就会出现很多NAN。


    `例子：`

        pd.concat([desc1,desc2],axis=1) 按行合并，就是向右延伸

    `out：`

        	      numGroups	                 maxPlace	            groups in match
              min	mean	max	            min	mean	max	         min	mean	max
        matchType									
        duo	  1.0	45.812482	52.0	    3.0	47.608919	52.0	    1.0	45.348777	52.0
        solo	1.0	91.115157	100.0	    1.0	93.908771	100.0	    1.0	85.669426	100.0
        squad	2.0	27.039389	37.0	    2.0	27.982982	37.0	    2.0	26.834984	37.0

    `例子：`

        pd.concat([desc1,desc2],axis=0) 按列合并，就是向下延伸(默认axis=0)

    `out：`

              groups in match	              maxPlace	        numGroups
                  max	mean	min	        max	mean	min	    max	mean	min
        matchType									
        duo	      NaN	NaN	NaN	52.0	    47.608919	3.0	      52.0	45.812482	1.0
        solo	    NaN	NaN	NaN	100.0	    93.908771	1.0	      100.0	91.115157	1.0
        squad	    NaN	NaN	NaN	37.0	    27.982982	2.0	      37.0	27.039389	2.0

        duo	      52.0	45.348777	1.0	  NaN	NaN	NaN	        NaN	NaN	NaN
        solo	    100.0	85.669426	1.0	  NaN	NaN	NaN	        NaN	NaN	NaN
        squad	    37.0	26.834984	2.0	  NaN	NaN	NaN	        NaN	NaN	NaN

### pd.cut 为数据分段：

  量化将连续数映射成离散数。我们可以把离散化的数字看作是代表强度度量的容器的有序的序列。使用cut将连续数据进行切分量化成固定宽度的数据范围。

  需要将数据值分段并排序到箱中时使用cut。此函数对于从连续变量转换为分类变量也很有用。例如，cut可以将年龄转换为年龄范围组。支持分箱到相同数量的箱柜或预先指定的箱柜阵列。

  pandas.cut（x，bins，right = True，labels = None，retbins = False，precision = 3，include_lowest = False，duplicates ='raise' ）

`参数介绍：`
    
    x: 数据
    
    bins：进行划分的一维数组
      
      当 bins 为 整数时,将x划分为多少个等间距的区间

      当 bins 为 序列时,将x划分在指定的序列中，若不在该序列中，则是NaN

    right：是否包含右端点 bool型
      
      如果为 True：包含(右闭合区间) 
      
      如果为 False：不包含 (右开区间)

    labels : 是否用标记来代替返回的bins

    precision: 精度

    include_lowest:是否包含左端点

`例子：`

*   当bins为整数时：将数据分为三份等间距的区间

      pd.cut(np.array([1, 7, 5, 4, 6, 3]), 3)

          [(0.994, 3.0], (5.0, 7.0], (3.0, 5.0], (3.0, 5.0], (5.0, 7.0], (0.994, 3.0]]  # 输出的数据区间划分
          Categories (3, interval[float64]): [(0.994, 3.0] < (3.0, 5.0] < (5.0, 7.0]]   # 3份等间距区间

*   当bins为以序列时，可以看到1不属于我们2-4区间，也不属于4-6区间，也不属于6-8区间，所以输出的内容为NAN

        pd.cut(np.array([1, 7, 5, 4, 6, 3]), [2,4,6,8]) 

          [NaN, (6.0, 8.0], (4.0, 6.0], (2.0, 4.0], (4.0, 6.0], (2.0, 4.0]]
          Categories (3, interval[int64]): [(2, 4] < (4, 6] < (6, 8]]

*   给不同区间赋值

        pd.cut(np.array([1, 7, 5, 4, 6, 3]), [2,4,6,8],labels=['2到4区间','4到6区间','6到8区间']) 
        
          [NaN, 6到8区间, 4到6区间, 2到4区间, 4到6区间, 2到4区间]
          Categories (3, object): [2到4区间 < 4到6区间 < 6到8区间]


### pd.qcut 为数据分段：

上面我们将的是用pd.cut来个连续数据分段，但是有时候不好使呀固定宽度来进行分组，我们可以使用qcut，表示使用分数位进行切分数据

*   `按分位数分箱计数 定义数据：`

        large_counts = [
            296, 8286, 64011, 80, 3, 725, 867, 2215, 7689, 11495, 91897, 44, 28, 7971,
            926, 122, 22222
        ]


*   `按分位数分箱计数 例子1：`

        ### Continue example Example 2-3 with large_counts
        import pandas as pd
        ### Map the counts to quartiles
        pd.qcut(large_counts, 4, labels=False) # 按四分位进行切分

        Out:
          array([1, 2, 3, 0, 0, 1, 1, 2, 2, 3, 3, 0, 0, 2, 1, 0, 3], dtype=int64)

*   `按分位数分箱计数 例子2：`

        ### Compute the quantiles themselves
        large_counts_series = pd.Series(large_counts)
        large_counts_series.quantile([0.25, 0.5, 0.75]) # 这里指定三分位进行划分。

        Out:
          0.25     122.0
          0.50     926.0
          0.75    8286.0
          dtype: float64

  ### pandas按若干个列的组合条件筛选数据

    #取年龄等于26，并且存活的数据的数量
    print(train_data[(train_data['Age']==29) & (train_data['Survived']==1)].count())

  `注意：`有的时候过滤筛选出来的数据不能直接进行赋值，需要使用loc定位到相应位置才能赋值成功。

  ### pandas一次显示指定的多个标签 
    #使用 [[ ]] 两层嵌套括号 比如 
    print(train_data[['Survived','Age']])
    print("%s "%(new_user_data[new_user_data['节']==section][['名字','知识点']].values))

  ### 筛选df中符合的index数据

    # 筛选出 correlations 的index 中包含 ['ps_ind_10_bin','ps_ind_11_bin','ps_ind_12_bin','ps_ind_13_bin'] 的内容
    correlations.loc[correlations.index.isin(['ps_ind_10_bin','ps_ind_11_bin','ps_ind_12_bin','ps_ind_13_bin'])]
  
  ### 筛选出特定字符串的列

    [col for col in train.columns if 'str' in col]


    train.iloc[:,-1].str.contains('etc')

  ### 过滤出包含某个字符串的列信息 df.filter()

    #过滤出包含 matchType 字符串的所有列
    matchType_encoding = train.filter(regex='matchType')

  ### pandas.Series.map
    
    根据输入的对应关系映射系列的值。

    用于将系列中的每个值替换为另一个值，该值可以从函数，a dict或a 派生Series。
  
  `例子：`
  
      >>> s = pd.Series(['cat', 'dog', np.nan, 'rabbit'])
      >>> s
      0      cat
      1      dog
      2      NaN
      3   rabbit
      dtype: object
  
  map接受a dict或a Series。除非dict具有默认值（例如），否则将dict转换为未找到的NaN值defaultdict：
  
      >>> s.map({'cat': 'kitten', 'dog': 'puppy'})
      0   kitten
      1    puppy
      2      NaN
      3      NaN
      dtype: object
      
  `它还接受一个功能：`

    >>> s.map('I am a {}'.format)
    0       I am a cat
    1       I am a dog
    2       I am a nan
    3    I am a rabbit
    dtype: object
  
  为避免将函数应用于缺失值（并将其保留为 NaN），na_action='ignore'可以使用：

    >>> s.map('I am a {}'.format, na_action='ignore')
    0     I am a cat
    1     I am a dog
    2            NaN
    3  I am a rabbit
    dtype: object

  ### pandas to_dict

  `例子：`
    
    train_feature=vec.fit_transform(data.to_dict(orient='record'))

    orient参数不同会有不一样的效果 

    https://blog.csdn.net/m0_37804518/article/details/78444110


    
  ### python dataframe 获得 列名columns 和行名称 index
   
    dfname._stat_axis.values.tolist()   ==  dfname.index.values.tolist()      # 行名称
     
    
    dfname.columns.values.tolist()    # 列名称

  ### python dataframe 当读取csv文件，但csv数据中无列名时进行赋值列名

  `错误示范：`

    data=pd.read_csv('filepath')
    # 为数据增加一行列名
    column=['user_id','名字','知识点','节','课程','course_id']
    data.columns=column

  `正确示范：`

    data=pd.read_csv('filepath'，header=None)
    # 为数据增加一行列名
    column=['user_id','名字','知识点','节','课程','course_id']
    data.columns=column

  不加 header=None ，在进行read_csv之后，默认会将第一行数据设置为列名，此时如果直接进行 data.columns=column 那么第一行数据被替换成我们指定的列名，这样我们的数据就变少了，如果加上了header=None,那么读取之后的数据是没有列名的(第一行数据还是数据，不会成为列名)，此时我们进行命名列名就不会替换掉我们的数据。

### pd.melt

#### pandas.melt 使用参数：

     pandas.melt(frame, id_vars=None, value_vars=None, var_name=None, value_name='value', col_level=None)

#### 参数解释：

* frame:要处理的数据集。

* id_vars:不需要被转换的列名。

* value_vars:需要转换的列名，不指定则剩下的列全部都要转换。

* var_name 和 value_name 是自定义设置对应的列名。

* col_level :如果列是 MultiIndex，则使用此级别。

#### 案例：

代码：

    d = {'广州': ['100','110','120'], '北京': [200,220,280],'上海':[110,111,133],'商品':['水','可乐','牛奶']}
    df = pd.DataFrame(data=d)
    df

输出：

      广州	北京	上海	商品
    0	100	  200	  110	  水
    1	110	  220	  111	  可乐
    2	120	  280	  133	  牛奶

代码：

    pd.melt(df,id_vars=['商品'],var_name='城市',value_name='销量')

输出：

      商品	城市	销量
    0	水	  广州	100
    1	可乐	广州	110
    2	牛奶	广州	120
    3	水	  北京	200
    4	可乐	北京	220
    5	牛奶	北京	280
    6	水	  上海	110
    7	可乐	上海	111
    8	牛奶	上海	133


### 构造DataFrame

   `例子：` 如果想要构造这样的dataframe：

   <div align=center><img src="./static/1.jpg"/></div> 

      import pandas as pd

      list1=[[1,2,3],[4,5,6],[7,8,9]]

      dict={'id':[1,2,3],'item':[[5,6,7,8],[1,2,3],[4,5,6]]}

      data=pd.DataFrame(data=dict,columns=['id','item'])

      print(data)


  ### pandas 错误警告：

    在使用pd.read_csv()时 出现了如下的错误

      data=pd.read_csv(file_path)

      pandas.errors.ParserError: Error tokenizing data. C error: Expected 20 fields in line 4, saw 21

  `解决方案1：` 确保特征列(第一行)数据最长，包含下面所有的特征

      row1: column1,column2
      row2: column1,column2,column3,column4
      
      第一行是作为我们的特征名称，必须要大于等于第二行

  `解决方案2：` 加上分隔符 sep 

      data=pd.read_csv(file_path,sep='\t')

  `解决方案3：` 忽略错误的行

      data=pd.read_csv(file_path,error_bad_lines=False)


  ### 数据的偏度和峰度

  `df.skew()  偏度`

    Definition:是描述数据分布形态的统计量，其描述的是某总体取值分布的对称性，简单来说就是数据的不对称程度。
    偏度是三阶中心距计算出来的。
    （1）Skewness = 0 ，分布形态与正态分布偏度相同。
    （2）Skewness > 0 ，正偏差数值较大，为正偏或右偏。长尾巴拖在右边，数据右端有较多的极端值。
    （3）Skewness < 0 ，负偏差数值较大，为负偏或左偏。长尾巴拖在左边，数据左端有较多的极端值。
    （4）数值的绝对值越大，表明数据分布越不对称，偏斜程度大。


  `df.kurt()  峰度`

    Definition:偏度是描述某变量所有取值分布形态陡缓程度的统计量，简单来说就是数据分布顶的尖锐程度。
    峰度是四阶标准矩计算出来的。
    （1）Kurtosis=0 与正态分布的陡缓程度相同。
    （2）Kurtosis>0 比正态分布的高峰更加陡峭——尖顶峰
    （3）Kurtosis<0 比正态分布的高峰来得平缓——平顶峰

  ### pandas 计算矩阵关系系数

  使用.corr() 可以用于计算矩阵关系系数，可以使用得到的关系系数绘制热力图
    
  `举例：`

    现在我们有一些数据，我们要计算这些特征之间的关系
    
    corrmat = train_data.drop('Id',axis=1).corr()   # train_data 是read_cav()读取进来的数据
    corrmat

  `out`

    	            MSSubClass	LotFrontage	  LotArea	 OverallQual  OverallCond

    MSSubClass	  1.000000	  -0.386347	  -0.139781	  0.032628	  -0.059316	
    LotFrontage	  -0.386347 	1.000000	  0.426095	  0.251646	  -0.059213	
    LotArea	      -0.139781	  0.426095	  1.000000	  0.105806	  -0.005636	
    OverallQual	  0.032628	  0.251646  	0.105806	  1.000000	  -0.091932	
    OverallCond   -0.059316	  -0.059213	  -0.005636 	-0.091932	  1.000000	

  `绘制热力图`

    fig,ax=plt.subplots(figsize=(20,16))
    sns.heatmap(corrmat,annot=True,fmt ='.1')
    plt.show()


### df.to_sql df写到数据库：

  将df数据写入数据库

`案例：`

    import MySQLdb
    import pandas as pd
    from sqlalchemy import create_engine

    host = '127.0.0.1'
    port = 3306
    db = 'db_name'
    user = 'db_user'
    password = 'db_user_password'
    
    # ?charset=utf8 表示使用utf8字符集
    engine = create_engine(str(r"mysql+mysqldb://%s:" + '%s' + "@%s/%s?charset=utf8") % (user, password, host, db))

    try:
        # 将数据写入test1表，如果表存在就进行替换，如果数据库的编码格式为latin，并且数据中存在中文，会报乱码。
        Associate_dknowledge.to_sql('test1',con=engine,if_exists='replace',index=False)
    except Exception as e:
        print(e)

`df.to_sql暂时还不支持我们设置主键`

    如果想要设置主键：加上 标记为 <---- 的两句

    import MySQLdb
    import pandas as pd
    from sqlalchemy import create_engine

    host = '127.0.0.1'
    port = 3306
    db = 'db_name'
    user = 'db_user'
    password = 'db_user_password'
    
    # ?charset=utf8 表示使用utf8字符集
    engine = create_engine(str(r"mysql+mysqldb://%s:" + '%s' + "@%s/%s?charset=utf8") % (user, password, host, db))
    conn = engine.connect()     <-------

    try:
        # 将数据写入test1表，如果表存在就进行替换，如果数据库的编码格式为latin，并且数据中存在中文，会报乱码。
        Associate_dknowledge.to_sql('test1',con=engine,if_exists='replace',index=True)
        conn.execute('alter table `{}` add primary key(`index`)'.format(file_name))       <-------

    except Exception as e:
        print(e)


`df.to_sql if_exists='append' 时防止主键冲突`
  
  为了防止主键重复，于是我们修改原先的数据中的index，让其接着数据库中最大index往下排序

    import MySQLdb
    import pandas as pd
    from sqlalchemy import create_engine

    host = '127.0.0.1'
    port = 3306
    db = 'db_name'
    user = 'db_user'
    password = 'db_user_password'

    engine = create_engine(str(r"mysql+mysqldb://%s:" + '%s' + "@%s/%s?charset=utf8") % (user, password, host, db))
    conn = engine.connect()
    Session = sessionmaker(bind=engine)

    def df_to_sql(test1,data,flag): 
        session = Session(bind=conn)
        # 通过flag表示是否更新表，如果更新就替换之前的表，否则追加
        print('flag: ',flag)
        # 如果是更新表，则覆盖原有表
        if flag==True:
            method='replace'
            data.to_sql(test1,con=engine,if_exists=method,index=True)
        # 如果是追加数据，则追加
        else:
            method='append'
            try:
                result=session.execute('SELECT COUNT(*) FROM `test1`') 
                """ 为了防止主键重复，于是我们修改原先的数据中的index，让其接着数据库中最大index往下排序"""
                data_len=len(data)
                sql_data_len=result.fetchall()[0][0]
                data.index=np.arange(sql_data_len, sql_data_len+data_len)
                print(data)
                data.to_sql(test1,con=engine,if_exists=method,index=True)
                session.commit()
                session.close()
            except Exception as e:
                print(e)


    def set_primary_key(course_name):
      try:
          conn.execute('alter table test1 add primary key(`index`)') 
      except Exception as e:
          print(e)

#### 注意：

这里我们最好使用session会话，这样我们进行一次修改之后就会提交一次事务，否则可能造成事务未提交的情况，这个时候如果我们还留有事务，但是此时如果我们要进行表的替换replace，那么因为之前存在事务，但是我们要尝试去删表，就会造成metadata lock，就会阻塞，所以我们采用session，及时进行事务的提交。



### 数据库数据转为df：

    import pandas as pd
    import pymysql
    import sqlalchemy
    from sqlalchemy import create_engine

    host = '127.0.0.1'
    port = 3306
    db = 'db_name'
    user = 'db_user'
    password = 'db_user_password'

    # 1. 用sqlalchemy构建数据库链接engine
    engine = create_engine(str(r"mysql+mysqldb://%s:" + '%s' + "@%s/%s?charset=utf8") % (user, password, host, db))
    # sql 命令
    sql_cmd = "SELECT * FROM table"
    df = pd.read_sql(sql=sql_cmd, con=engine)

    # 2. 用DBAPI构建数据库链接engine
    con = pymysql.connect(host=localhost, user=username, password=password, database=dbname, charset='utf8', use_unicode=True)
    df = pd.read_sql(sql_cmd, con)

  


  ### pandas的拼接循环：

  举个例子：

    现在有两个df数据 train_df 和 test_df 

    现在 combine=[train_df,test_df]

    for dataset in combine:
        dataset['Title']=dataset.Name.str.extract('([A-Za-z]+)\.',expand=False)

  这里的 for dataset in combine 其实只是循环了两次，第一次是train_data 第二次是test_data数据，其实上就是操作修改我们train_data和test_data，之后输出train_df 我们会发现其多了一个特征 ‘Title’ 。

### df.copy()

  `复制此对象的索引和数据。`

  当deep=True（默认）时，将使用调用对象的数据和索引的副本创建新对象。对副本的数据或索引的修改不会反映在原始对象中（请参阅下面的注释）。

  何时deep=False，将创建一个新对象而不复制调用对象的数据或索引（仅复制对数据和索引的引用）。对原始数据的任何更改都将反映在浅层副本中（反之亦然）。

  `例子：`

    a=train_data['Survived']
    b=train_data['Survived']
    print(a.tail())
    print(b.tail())
    
    a:                             b:

    886    0                      886    0
    887    1                      887    1
    888    0                      888    0
    889    1                      889    1
    890    0                      890    0
    Name: Survived, dtype: int64  Name: Survived, dtype: int64
    
`现在我们没有进行deepcopy，我们修改b的数据，a的数据也会发生修改，所以我们如果要保持a和b数据独立，我们必须deepcopy`

    b['haha']=1           
    b.tail(1)

    haha    1
    Name: Survived, dtype: int64
    
    a.tail(1)

    haha    1
    Name: Survived, dtype: int64
      
`如果要使用deepcopy，使用df.copy(deep=True)即可`


### df 追加数据

`例子：`

    df = df.append(df2)
    


### df.quantile
####  分位数解释

`四分位数`

`概念：` 把给定的乱序数值由小到大排列并分成四等份，处于三个分割点位置的数值就是四分位数。

`第1四分位数 (Q1)`， 又称“较小四分位数”，等于该样本中所有数值由小到大排列后第25%的数字。

`第2四分位数 (Q2)`，又称“中位数”，等于该样本中所有数值由小到大排列后第50%的数字。

`第3四分位数 (Q3)`，又称“较大四分位数”，等于该样本中所有数值由小到大排列后第75%的数字。

`四分位距`（InterQuartile Range, IQR）= 第3四分位数与第1四分位数的差距

*   `确定p分位数位置的两种方法`

    position = (n+1)*p    - 箱型图采用的计算方法

    position = 1 + (n-1)*p  - python中计算分位数位置的方案

    其中 n 表示序列中包含的项数。也就是有多少个数。

在python中计算分位数位置的方案采用 `position=1+(n-1)*p`

`案例1`

    import pandas as pd
    import numpy as np
    df = pd.DataFrame(np.array([[1, 1], [2, 10], [3, 100], [4, 100]]), columns=['a', 'b'])
    print("数据原始格式：")
    print(df)
    print("计算p=0.1时，a列和b列的分位数")
    print(df.quantile(.1))

`data:`

    序号	a	   b
    0	    1	  2
    1	    2	  10
    2	    3	  100
    3	    4	  100

`程序计算结果：`

    计算p=0.1时，a列和b列的分位数
    a 1.3
    b 3.7
    Name: 0.1, dtype: float64

`手算计算结果：`

    计算a列
    pos = 1 + (4 - 1)*0.1 = 1.3
    fraction = 0.3
    ret = 1 + (2 - 1) * 0.3 = 1.3
    计算b列
    pos = 1.3
    ret = 2 + (10 - 2)* 0.3 = 4.4

`解释：`

1、首先根据公式 <position=1+(n-1)*p> ，因为a列有 4个项数，所以我们可以得到 pos = 1 + (4 - 1)*0.1 = 1.3 。

2、然后 fraction 是pos的分数位，即0.3。

3、接着 我们将数据进行排序，因为我们这里就是已经排序完成的数据了，就忽略这一步。

4、因为 pos为 1.3，项数位置在 第一项 和 第二项 之间 ( 1< 1.3 <2 )

5、所以那 i 就是 2 ，j 就是10，也就是b列表中的第一项 2 和第二个项 10 。

6、然后我们就可以返回 .1分位数的值，也就是 i + (j-i) * fraction = ret = 2 + (10 - 2)* 0.3 = 4.4 ，也就是前 10% 的数据。

`案例2`

    import pandas as pd
    import numpy as np
    dt = pd.Series(np.array([6, 47, 49, 15, 42, 41, 7, 39, 43, 40, 36])
    print("数据格式：")
    print(dt)
    print('Q1:', df.quantile(.25))
    print('Q2:', df.quantile(.5))
    print('Q3:', df.quantile(.75))

`计算结果`

    Q1: 25.5
    Q2: 40.0
    Q3: 42.5

  ## 总结：
  
   和 NumPy 一样，Pandas 有两个非常重要的数据结构：Series 和 DataFrame。使用 Pandas 可以直接从 csv 或 xlsx 等文件中导入数据，以及最终输出到 excel 表中。
   
   Pandas 包与 NumPy 工具库配合使用可以发挥巨大的威力，正是有了 Pandas 工具，Python 做数据挖掘才具有优势。
  
  
### df.memory_usage  

Pandas dataframe.memory_usage()函数返回每列的内存使用情况（以字节为单位）。内存使用情况可以选择包括索引和对象dtype元素的贡献。默认情况下，此值显示在DataFrame.info中。


-   `参数：`
  
      index：指定是否在返回的Series中包括DataFrame索引的内存使用情况。如果index = True（默认），则索引的内存使用量将是输出中的第一项。

      deep：如果为True，则通过查询对象dtype进行系统级内存消耗来深入检查数据，并将其包含在返回值中。

-   `返回：`
  
      一个系列，其索引是原始列名，其值是每列的内存使用量 `（以字节为单位）`

-   `代码示例：`

        >>> df.memory_usage()
        Index           128
        int64         40000
        float64       40000
        complex128    80000
        object        40000
        bool           5000
        dtype: int64

# pandas 对时间序列的处理

https://www.kaggle.com/kk0105/everything-you-can-do-with-a-time-series

## `df.dt` 

* `df.dt.dayofweek : 返回星期几` 

  `筛选：周六和周日: 下面代码中出现的 date 为列名`

      train_flattened['weekend'] = ((train_flattened.date.dt.dayofweek) // 5 == 1).astype(float) 
      # 能够整除5，就是 周六 + 周日

  or

      # 筛选出 dayofweek = 5 的和 dayofweek = 6 的数据，也就是挑选周六 和 周日 
      train_flattened['weekend'] = ((train_flattened.date.dt.dayofweek) == 5 | 6 ).astype(float) 


  `注意`：对于 `dayofweek` 来说 `0` 为 `星期一`

* `dt.year `

      train_flattened['year']=train_flattened.date.dt.year 


* `dt.month `

      train_flattened['month']=train_flattened.date.dt.month 

* `dt.day `

      train_flattened['day']=train_flattened['date'].dt.day 

      pd.to_datetime(train['click_time']).dt.day

* `dt.hour`

      pd.to_datetime(train['click_time']).dt.hour # 对于特征数据类型不为时间类型的
      # 14
      # 15

* `dt.round` 取整

      train['new_click_time'].dt.round('H')   # 只适用于数据类型为 datatime 的，.round(H) 用于小时向下取整
      
      # 2017-11-06 15:00:00
      # 2017-11-06 16:00:00



## `筛选某个时间段的数据：如 20130101 ~ 20130131 `

    import datetime

    s_date = datetime.datetime.strptime('20130101', '%Y%m%d').date()
    e_date = datetime.datetime.strptime('20130131', '%Y%m%d').date()
    
    # 使用 to_datetime 转成的时间数据类型为 Timestamp,而使用datetime.date转成的是 datetime.date类型，无法直接比较，所以这里进行转化
    s_date = pd.to_datetime(s_date)
    e_date = pd.to_datetime(e_date)

    
    train['date'] = pd.to_datetime(train['date'],format='%Y-%m-%d') 
    train[(train['date'] >= s_date) & (train['date'] <= e_date)]

<div align=center><img width="550" height="270" src="./static/datetime筛选结果展示.png"/></div>

## `resample` 时间重采样

resample 可以对时间进行重采样：

`数据展示：`

    index = pd.date_range('1/1/2000', periods=9, freq='D')
    df = pd.DataFrame(range(9), index=index,columns=['count'])
    df


                count
    2000-01-01	  0
    2000-01-02	  1
    2000-01-03	  2
    2000-01-04	  3
    2000-01-05	  4
    2000-01-06	  5
    2000-01-07	  6
    2000-01-08  	7
    2000-01-09	  8

`重采样：`

按照 `resample` 指定的时间，比如说 `3D (3天)` ，划分之后对数据 `count` 做加法，有点 `groupby` 的味道

    series.resample('3D').sum()

    2000-01-01     3
    2000-01-04    12
    2000-01-07    21
    Freq: 3D, dtype: int64







