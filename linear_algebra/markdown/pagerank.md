# pagerank

### 最有价值的特征向量

在谷歌搜索台湾大学，谷歌为什么会把台大的官网排在第一个，维基百科排在第二个？

谷歌依赖的是pagerank分数。

<img src="https://raw.githubusercontent.com/xuyouqian/picgo/master/20210625134852.png"/>

一些网页的pagerank分数：

<img src="https://raw.githubusercontent.com/xuyouqian/picgo/master/20210625135235.png"/>

<img src="https://raw.githubusercontent.com/xuyouqian/picgo/master/20210625135425.png"/>

- PageRank越高的网页越有可能出现在Google搜索结果的顶部。
- PageRank依靠web独特的民主性，将其庞大的链接结构作为单个页面价值的指标
- 谷歌将从a页到B页的链接解释为a页对B页的投票。

<img src="https://raw.githubusercontent.com/xuyouqian/picgo/master/20210625140351.png"/>

脸越大pagerank越高

黄色的脸被很多网页链接到，红色的脸被黄脸连接到，所以分数也很高

### pagerank 的计算

<img src="https://raw.githubusercontent.com/xuyouqian/picgo/master/20210625141408.png"/>

pagerank计算其实就是解方程

假设只有4个网页，他们的链接关系如上，方程也是如上，

第一个网页的分数就是第三个网页加上第四个网页分数的1/2,因为第三个网页只链接向1，所以它的全部分数都能传递给1，第四个网页链接向第一个和第三个网页，所以只能传递1/2给1……

<img src="https://raw.githubusercontent.com/xuyouqian/picgo/master/20210625142249.png"/>

如果一个方阵的每一列的和都是1，它有一个特征值一定是1

解是

<img src="https://raw.githubusercontent.com/xuyouqian/picgo/master/20210625143252.png"/>

还有一种情况是有的网页没有链接到外部，只进不出，那就不能保证她这一列的和都是1，不能保证特征值是1

这种情况暂不考虑

### pagerank排名只有一种可能吗？

特征值为1时对应的特征空间可能有多个相互独立的解，比如：

<img src="https://raw.githubusercontent.com/xuyouqian/picgo/master/20210625143556.png"/>

这个矩阵就能算出两种排名

<img src="https://raw.githubusercontent.com/xuyouqian/picgo/master/20210625143752.png"/>

出现这种情况其实是网络上有两个不互相关联的孤岛

### 怎样才能使解唯一

可以优化原来的矩阵A

<img src="https://raw.githubusercontent.com/xuyouqian/picgo/master/20210625144138.png"/>

M是被优化的矩阵

m是0-1之间的数字，比较接近0的值

S是一个矩阵，里面的数字全是1/n,比如只有三个网页，s = [[1/3,1/3,1/3],[1/3,1/3,1/3],[1/3,1/3,1/3]]

这样相当于有两种跳转模式，一种是按照原来的跳转方式，还有就是完全等可能的跳向其他网页包括自己，这样就不会存在孤岛。而且每一列的和还都是1.

### Power method求解pagerank

<img src="https://raw.githubusercontent.com/xuyouqian/picgo/master/20210625145638.png"/>

以最开始的四个网页为例

```python
import numpy as np

A = np.array([
    [0, 0, 1, 1 / 2],
    [1 / 3, 0, 0, 0],
    [1 / 3, 1 / 2, 0, 1 / 2],
    [1 / 3, 1 / 2, 0, 0]
])

x = np.array([1 / 4, 1 / 4, 1 / 4, 1 / 4])


def step_frward(x, A):
    return A.dot(x)

for i in range(50):
    x = step_frward(x,A)

print(x*31)
'''
[ 12.   4.   9.   6.]
其实10次就差不多了
'''
```

