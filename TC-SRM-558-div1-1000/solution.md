## Surrounding Game

作者：周子鑫

关键词：最小割

## 题目简述



一个人在一个$$n * m$$棋盘上玩游戏，想要占领一个格子有两个方法：

* 在这个格子放一个棋子。
* 这个格子周围(四联通)的格子中**都有棋子**。

在$$(i, j)$$中放棋子需要花费$$cost_{i, j}$$，占领$$(i, j)$$能获得$$benefit_{i,j}$$。求一种放置棋子的方法，使得总收益(收益 - 花费)最大。

$$2 \le n, m \le 20$$



## 分析＆题外话

这题看起来有点眼熟有没有.. 有点像文理分科那个题目。其实这个题的想法和Ctsc2015的Gender也有点像。有个学长和我说过，一般这种看起来限制十分强，而且限制也没什么明显方向性，数据范围不大不小的题目都是网络流模型（雾）

假设先不考虑具体的建模，直观感受一下，这个题肯定是把棋盘黑白染色一下，中间xjb连一些代表花费和收益的边，跑一个最小割之类的东西。

这题其实比较套路的，要验证你建出来的图是否正确也很容易，只要看看当你选择保留某条收益边的时候，会要割掉哪些边。假如需要割边的情况和题目要求一致，这个图就建对了。

以这个题为例的话就是，保留$$benefit_{i, j}​$$时候，需要满足$$cost_{i, j}​$$被割或与$$(i, j)​$$相邻的$$cost​$$边都被割了，并且其他不相关的边没有任何影响。

再来说个题外话，我最开始把题目看成了`这个格子周围(四联通)的格子中有棋子` 而不是 `这个格子周围(四联通)的格子中都有棋子`，想了半天不会做，不知道有没有大哥会做这个题 ![heihei](heihei.png)



## 算法简述



首先把图黑白染色，就构成了一个二分图。然后把每个点都拆成两个，左边的点拆成$$X_1和X_2$$，$$S$$到$$X_1$$连一条容量为$$cost$$的边，$$X_1$$到$$X_2$$连一条容量为$$benefit$$的边。右边的点如法炮制，拆成$$Y_1$$,$$Y_2$$ , 之间连一条$$benefit$$的边，$$Y_1$$到$T$连$$cost$$的边。

既然已经把两边的边都连好了，那么就可以来连中间的边了，对于每一对相邻的点，我们在$$(X_1, Y_2)$$和$$(X_2, Y_1)$$之间都连上一条$$\infty$$的边。这样来跑个最小割，用总收益减去最小割就是答案了。

那么这个构图为什么是对的呢。考虑保留每一个收益的话需要割掉哪些边，发现这个条件正好和题目要求的条件相同：

* 把这个格子的花费边割掉
* 把这个格子相邻的格子的花费边割掉



是不是灰常好理解呀  ![chigua](chigua.png)

时间复杂度：$$O(Maxflow(n^2, n ^ 2))$$