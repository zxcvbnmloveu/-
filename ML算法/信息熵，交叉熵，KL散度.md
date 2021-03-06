# `信息熵，交叉熵，KL散度`

#### 熵的解释

信息熵用来衡量一个信源（随机变量）所能产生的信息量。

首先是信息量，当概率P越小，x消息出现的概率就越小，(自信息量)越大。

<img src = 'https://bkimg.cdn.bcebos.com/formula/cbca30682ca90aa45280998809336a26.svg'/>

乘上发生的概率，就是信息熵。

   * 信息是非负的如果一件事物发生的概率是1（没有选择的自由度），信息量为0。
   * 如果两件事物的发生是独立的（联合概率），它们一起发生时我们获得的信息是两者单独发生的信息之和。
   * 信息的度量应该是概率的函数，函数最好是单调连续的。

所以定义为：

<img src = 'https://math.jianshu.com/math?formula=H(X)%20%3D%20E_%7BX%20%5Csim%20P%7D%5BI(X)%5D%20%3D%20-E_%7BX%20%5Csim%20P%7D%5Blog(P(X))%5D%20%3D-%20%5Csum_%7Bk%3D1%7D%5E%7BN%7Dp_k%5Clog_2(p_k)'/>

通俗的解释：信息熵衡量了系统的不确定性，而我们要消除这个不确定性，所要付出的【最小努力】

比如猜硬币，猜球 参考 ![猜球](https://www.zhihu.com/people/pwlin/answers)，自信息量就是猜中一个球的期望，信息熵就是概率乘以这个期望的和。链接可以看到，以这个期望代表的概率分布的加权和是最小的。

#### 交叉熵和KL散度

如上面提到，非真实分布q得到的平均编码长度H(p,q)（需要使用更多的载体存储信息）大于根据真实分布p得到的平均编码长度H(p)。根据Gibbs'inequality可知，H(p,q)>=H(p)。

相对熵值得就是H(p,q)，KL散度为 H(P,Q)-H(P)，其用来衡量两个取值为正的函数或概率分布之间的差异。

#### 证明p，q相等，H(p,q) 最小

Gibbs'inequality，H(p,q)>=H(p)， 当且仅当p=q。

![证明过程](https://baike.baidu.com/item/%E5%90%89%E5%B8%83%E6%96%AF%E4%B8%8D%E7%AD%89%E5%BC%8F/22780937)

多个样本（mini-batch）的平均交叉熵损失函数公式，M为样本数量：

<img src='https://math.jianshu.com/math?formula=Loss%20%3D%20-%5Cfrac%7B1%7D%7BM%7D%5Csum_%7Bi%3D1%7D%5E%7BM%7D%5Csum_%7Bk%3D1%7D%5E%7BN%7Dp_%7Bik%7D%5Clog_2(q_%7Bik%7D)'/>
