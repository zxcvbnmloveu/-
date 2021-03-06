# `VAE-变分自编码器`

一般的自编码器做的是将输入x映射到一个隐含变量z，再由z恢复出x'.
这是z一般为为高层次低维特征。

<img src = 'https://pic2.zhimg.com/80/v2-92a9061e7079089b75c37650943c6f25_1440w.jpg' width = '600'/>

一般自编码器的问题是，上图的映射是单值映射。事实上，我们更希望某一类图片对应的z的特征都是处于一定范围的，即概率分布。VAE就能做到这点。

VAE实现的方法就是对z施加约束。使之接近(0,1)高斯分布。`为啥对齐01高斯分布就能达到这点我真的还没搞懂`

#### 具体实现
z的分布是q,目标是p，两者都服从高斯分布。
<img src = 'https://www.zhihu.com/equation?tex=q%28z%29%5Csim+N%28%5Cmu%2C%5Csigma%29%2C%5Cspace%5Cspace%5Cspace%5Cspace+p%28z%29%5Csim+N%280%2C1%29' width = '300'/>

计算用

<img src = 'https://pic1.zhimg.com/80/v2-448f00abae7d6e91237e380999b8fdc0_1440w.jpg' width = '600'/>

这里公式只有两个变量（图有点错了），一个均值，一个方差。这两个变量在代码里都是由两个全连接层估计(为啥可以这样我也不知道，妈的），这里估计的是log_sigma,有一点不同。
```python3 
self._enc_mu = torch.nn.Linear(100, 8)
self._enc_log_sigma = torch.nn.Linear(100, 8)
```

然后就优化上面的公式了，即
```python3
0.5 * torch.mean(mean_sq + stddev_sq - torch.log(stddev_sq) - 1)
```

代码很好懂。
[代码地址](https://github.com/ethanluoyc/pytorch-vae/blob/master/vae.py)

两篇比较好的博客
[(1)](http://kvfrans.com/variational-autoencoders-explained/)
[(2)](https://zhuanlan.zhihu.com/p/64485020)

但是都没有回答到我上面的问题~

十分钟后回来，好像理解了，谢谢另一位大佬的文章[(3)](https://zhuanlan.zhihu.com/p/34998569).

对于第一个问题， 这边之所以要拟合(0,1)高斯分布，是因为我们的到一个**输入一个01高斯噪声**生成图片的生成模型（decoder），可以借鉴一些gan的输出。

现在的问题是怎么训练一个映射（encoder）使得输入（图片）去映射到一个01高斯分布空间里。高斯分布由两个参数，一个均值，一个方差，然后就可以组合成一个付出高斯分布的z，上面的z有问题。注意代码里拟合拟合 logσ^2 是因为 σ^2 总是非负的，需要加激活函数处理，而拟合 logσ^2 不需要加激活函数，因为它可正可负。

网上的教程都是一大堆数学推导，终于想到一个自洽的理论。

