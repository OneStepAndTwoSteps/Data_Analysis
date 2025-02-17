# `Unet`

## `网络介绍：`

在 UNet 模型中，编码器（Encoder）是一系列卷积层和池化层的堆叠，用于提取图像的高级特征表示。解码器（Decoder）是一系列卷积层和上采样层的堆叠，用于将编码器中提取的特征图映射回原始图像大小。在 UNet 中，编码器和解码器之间通过跳跃连接（skip connections）建立了密切的联系，使得解码器可以直接访问编码器中提取的低级别特征，这有助于解决在高分辨率图像中分割物体时可能出现的细节丢失问题。

在 UNet 的实现中，编码器和解码器中的每个层都会输出一些特征图，这些特征图分别捕捉了不同层次的图像特征，具有不同的分辨率和语义信息。为了在解码器中利用编码器的低级别特征，UNet 使用了跳跃连接，即将编码器中的特征图与解码器中对应的特征图进行拼接，形成一个更为丰富的特征表示，用于进一步提高分割的精度。这种结构类似于 U 形，因此得名 UNet。拼接操作通常是在特征图的通道维度上进行的。在训练过程中，通过反向传播算法，模型可以学习到如何利用跳跃连接将编码器中的低级别特征与解码器中的高级别特征相结合，从而更好地捕捉图像的语义信息。




