# SweetFruits 
作者：罗哲正

关键词：Matrix-Tree定理 meet-in-the-middle 容斥原理
## 题目简述
有$$n$$个水果，其中某些是甜的而另外一些不是，每个甜的水果都有一个甜度$$sweetness[i]$$，现在你需要用$$n-1$$条边把它们连成一棵树，定义一个水果为真甜的当且仅当它是甜的且它与至少一个甜的水果直接相连，一棵树的甜度等于所有真甜的水果的甜度之和。
现在求有多少种连成树的方案使得树的甜度不超过$$maxSweetness$$。

$$1 \leq n \leq 40.$$

## 分解问题
直接处理原问题极为复杂尤其每个水果除了标号之外还有一个甜度，而甜度只有成为真甜的水果并加起来之后才有限制，几乎无法处理，考虑把问题进行分解。
考虑最后恰有$$k$$个水果是真甜的，那么我们需要做两件事：
1.从所有甜的水果中选出$$k$$个水果作为真甜的，要求他们甜度之和不超过$$maxSweetness$$的方案数$$cnt_k$$。
2.令$$S$$为甜的水果数目，求出组成的树的方案数$$tree_k$$使得$$S$$个甜的水果中恰有$$k$$个水果为真甜的。
为防止重复或者遗漏计数，我们规定第一部分选出的水果是顺序无关的，即选择集合，而第二部分求组成树的方案数的水果是带标号的。
于是：
$$
ans=\sum_{k=0}^n cnt_k tree_k
$$
##第一部分：meet-in-the-middle
第一部分其实是一个经典问题，由于$$n \leq 40$$，我们可以用$$meet-in-the- middle$$做到$$O(2^{\frac{n}{2}}n)$$的复杂度。
具体来说，我们对于前一半水果，暴力枚举哪些水果在集合里，对于后一半水果也暴力枚举哪些水果在集合里，并分别按照总甜度排序。然后枚举$$x,y$$表示在前一半里选了$$x$$个，后一半里选了$$y$$个，使用two-pointers算法求出总甜度之和不超过$$maxSweetness$$的方案数即可。时间复杂度是$$O(2^{\frac{n}{2}}n)$$的。
##第二部分：Matrix-Tree+容斥
我们已知恰有$$k$$个水果是真甜的，如何求组成的树的方案数呢？
由于水果是带标号的，不妨编号为$$1 \sim n$$，假设$$1 \sim k$$号水果是真甜的；$$k+1 \sim S$$号水果是甜的且非真甜的，不妨成为半甜的；$$S+1 \sim n$$号水果是不甜的。
我们可以使用Matrix-Tree定理求出真甜水果集合是编号为$$1 \sim k$$的水果的子集的方案数$$T_k$$，具体做法是让真甜的水果和任意水果连边，让半甜的水果只能和不甜的水果连边，不甜的水果可以和任意水果连边，并使用Matrix-Tree定理求出这个无向图的生成树数目。这样编号$$k+1 \sim S$$的水果一定是半甜的，而编号$$1 \sim k$$的水果可以是真甜的也可以是半甜的。
接着考虑求真甜水果集合恰为$$1 \sim k​$$的方案数，这个可以使用容斥原理，对于每个$$k​$$，枚举$$0 \leq i < k​$$，并从$$T_k​$$中减去恰有$$i​$$个水果是真甜的方案数，由于水果是有标号的，还需要乘上二项式系数。即：
$$
tree_k=T_k-\sum_{i=0}^{k-1} \binom{k}{i} tree_i
$$
时间复杂度$$O(n^4)$$。
##Matrix-Tree定理
令$$A$$为无向图$$G$$的邻接矩阵，即$$A_{i,j}$$为$$i,j$$之间的边数。
令$$D$$为无向图$$G$$的度数矩阵，即$$D_{i,i}$$为节点$$i$$的度数且其他元素皆为$$0$$。
令$$K=D-A$$为无向图的基尔霍夫矩阵，则无向图的生成树数目为$$K$$的任一代数余子式的值。
使用高斯消元求解行列式可以做到$$O(n^3)$$的复杂度。
详细解释及证明可以查看[Kirchhoff's theorem - Wikipedia](https://en.wikipedia.org/wiki/Kirchhoff%27s_theorem)
##总结
本题较为复杂，第一步的拆解十分重要，问题被拆分成两个部分之后，第一部分是经典问题可以套用meet-in-the-middle解决，第二部分的关键是看到在使用Matrix-Tree定理解决树的计数时，限制水果之间没有边要比限制至少有一条边容易的多，从而想到使用容斥原理将所统计方案的限制从恰好$$k$$个真甜的水果转化成至多$$k$$个真甜的水果，并使用Matrix-Tree定理解决。
总时间复杂度是$$O(2^{\frac{n}{2}}n+n^4)$$。