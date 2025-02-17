# `Dropout`

`Dropout` 是常用的一种正则化方法，`Dropout层` 是一种正则化层。全连接层参数量非常庞大（占 据了CNN模型参数量的80%～90%左右），发生过拟合问题的风险比较高，所以我们通常需要 一些正则化方法训练带有全连接层的CNN模型。在每次迭代训练时，将神经元以一定的概率值​ 暂时随机丢弃，即在当前迭代中不参与训练。

## `Dropout什么意思呢？`

因为我们的参数很多，在每次迭代训练的时候我们并不是希望一个神经元都是被激活的，我们曾经通过激活函数(如果激活值小于阈值则不会传递到下一层)，现在我们也可以通过dorpout来做这样的事情，他就是以一定的概率值随机丢弃一些神经元，单这些丢弃的神经元只是在这一次迭代中不参与训练，下一次训练我们还可以通过Dorpout在进行丢弃。

这样一来，我们这次训练的时候就不是所有的神经元都参与训练。

<div align=center><img width="600" height="300" src="./static/dropout.png"/></div>



以我们的图为例，我们可以看到隐藏层的神经元有6个，如果我们使用dropout，那么我们隐藏神经元的第二个和第三个和第五个神经元就不在参与训练，这样使得我们的模型变得简单，减少了模型的参数。降低复杂度。

同时随机的神经元进行组合，减少了神经元之间可能形成的共同依赖，`dropout` 的神经网络是由 `dropout` 之后的子模型组成的，因为我们在训练完之后，我们是需要所有的神经元参与的(我们在训练的时候训练很多子模型，但是最后我们要将训练好的子模型进行一个组合，因为我们是需要所有的神经元参与预测) `这样就有利于提高我们模型的泛化能力`。
