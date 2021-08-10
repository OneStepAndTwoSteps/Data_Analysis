# SVD 

使用 `特征值` 和 `特征向量` 进行 `特征分解` 时，要求被分解的矩阵要是 `方阵`,所以存在着局限性，`SVD` 奇异值分解恰好能解决这样的问题。


## `SVD 的定义`

`SVD` 也是对矩阵进行分解，但是和特征分解不同，`SVD` 并不要求要分解的矩阵为方阵。假设我们的矩阵A是一个 `m×n` 的矩阵，那么我们定义矩阵 `A` 的 `SVD` 为：

<div align=center><img src="./static/1.jpg"/></div>

其中 `U` 为一个 `m×m` 的方阵，`V` 为一个 `n×n` 的方阵， `U` 和 `V` 都是 `酉矩阵` （ `U` 和 `V` 特征向量为 `标准正交基`，满足 `U^T = U^-1`）

`U` 又被称为 `左奇异矩阵`，`V` 又被称为 `右奇异矩阵`。

### `1、如何求解得到 V ?`

*   其实通过：

<div align=center><img src="./static/2.jpg"/></div>

*   我们可以发现 关于 `AAT` 的所有特征向量所张成的矩阵是 `n×n` 矩阵 `V` 。

*   #### `为什么呢？可以通过下面公式：`


    <div align=center><img src="./static/4.jpg"/></div>

*   可以发现：`V` 就是 `特征向量` ，`∑` 就是 `特征值`。


### `2、如何求解得到 U ?`

*   其实通过：

<div align=center><img src="./static/3.jpg"/></div>

*   我们可以求得 关于 `ATA` 的所有特征向量所张成的 `m×m` 矩阵 U 。

*   根据 `V` 中所示的公式，我们可以发现 关于 `ATA` 的所有特征向量所张成的矩阵是 `m×m` 矩阵 `U` 。


### `3、所以通过求解 ATA 和 AAT 的特征向量和特征值，我们可以得到 矩阵 U 和 V`

*   `对于 ATA` ：
    <div align=center><img src="./static/5.jpg"/></div>

*   `对于 AAT` ：
    <div align=center><img src="./static/6.jpg"/></div>

*   `特征值` 和 `特征向量` 求解的一种步骤：

    *   根据 `ATA` 或者 `AAT` 所成方阵行列式不为零来先求解出相关的特征值

    *   通过回代 `奇异值` 与 `特征向量` 乘积为 0 求得 `特征向量`
    
    *   如下所示

        <div align=center><img width="600" height="400" src="./static/14.jpg"/></div>

        <!-- <div align=center><img width="600" height="400" src="./static/7.jpg"/></div> -->

### `4、求解出 特征向量 后根据公式可以求解得到 Σ 中的特征值`

*   根据先前公式：

    <div align=center><img src="./static/4.jpg"/></div>

*   通过公式我们能发现 `ATA` 的特征值为 `Σ` 的平方，那么 `Σ` 就是 `ATA` 的特征值开方后的结果，如下所示：


    <div align=center><img src="./static/8.jpg"/></div>

此时我们就能求得 `Σ` 了。


## `SVD 数据压缩`

现在我们 已经求解出了 `U V Σ`：

这里要先介绍一个特性,对于 `奇异值分解` 或者 `特征分解` 来说有如下性质：

*   在 `奇异值矩阵` 中 `奇异值` 也是按照 `从大到小` 排列，而且 `奇异值` 的 `减少` 特别的 `快` 。

*   并且：`在很多情况下，前10%甚至1%的奇异值的和就占了全部的奇异值之和的99%以上的比例`。

那么 `SVD` 如何进行数据压缩呢？其实和 `PCA` 类似，通过保留 `奇异值`（特征值）较大的值来压缩数据,也就是通过设置 `Σ` 中对角线的 `σ` 来进行数据压缩，也就是通过保留前 `k` 位具有较大值的 `σ` 来实现数据的压缩：


### `1、特别的：对于奇异矩阵来说它还支持对数据的行或者列单独进行压缩：` 

*   左奇异矩阵 `U` 与 `Σ` 相乘：可以用于 `行` 数的压缩。

*   `Σ` 与 右奇异矩阵 `V` 相乘：可以用于 `列` 数即特征维度的压缩，也就是我们的PCA降维。

### `2、当然：其中也还是通过指定前 k 位较大的特征值来进行数据压缩的。`


<div align=center><img width="550" height="700" src="./static/12.jpg" ></div>

<!-- <div align=center><img width="550" height="700" src="./static/10.jpg" ></div> -->


<div align=center><img width="950" height="700"  src="./static/13.jpg"></div>

<!-- <div align=center><img width="550" height="700" src="./static/11.jpg" ></div> -->





