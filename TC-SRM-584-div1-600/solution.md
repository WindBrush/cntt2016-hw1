#Excavations
作者:孙奕灿

关键词: DP,暴力,生成函数.

##问题简述
  有 $$N$$ 个建筑,第 $$i$$ 个建筑的深度是 $$depth_i$$ , 种类是 $$kind_i$$ , 你有一个探测器,只能尝试探测 $$K$$ 个建筑,且每个建筑只能尝试探测一次,深度不超过 $$D$$ 的建筑会被成功探测到,否则不会.

  你忘记了 $$D$$ 的值,但是你记得你成功探测到的建筑的种类集合 $$found$$.
  
  现在你需要计算有多少个 $$K$$ 元组使得存在一个 $$D$$ 满足探测这些建筑会符合你记得的探测结果.
  
##数据范围
 $$1 \leq N \leq 50$$
 
 $$1 \leq kind_i \leq 50$$
 
 $$1\leq |found| \leq N$$
 
##算法一
考虑一个 $$K$$ 元组合法的条件,用 $$mindep_u$$ 表示这个 $$K$$ 元组中第 $$u$$ 种建筑的最浅的深度.那么合法的条件就是:

$$\forall{u\in found}~~~{mindep_u} < \min_{w\notin found}{mindep_w}$$

于是可以枚举这个 $$K$$ 元组然后暴搜.

时间复杂度 $$O(\binom{n}{K} \times K)$$ .

##算法二
一个很直观的想法就是枚举 $$mindep$$ ,然后对于第 $$i$$ 种建筑,其深度超过 $$mindep_i$$ 的部分是可以随便选的,我们只需记录这一部分的数量然后用组合数计算答案就好了。

然后就可以 $$DP$$ 了。首先枚举没出现在 $$found$$ 中的建筑的深度最小值$$notmin$$,然后用 $$f_{i,j}$$ 表示前 $$i$$ 种有 $$j$$ 个建筑已经随便选了的方案数。把第$$i$$种建筑的深度从大到小排序,枚举第 $$i$$ 种建筑选了第 $$k$$ 大(当然它的深度须严格小于$$notmin$$)的建筑然后从 $$f_{i-1,j-(k-1)}$$ 转移过来就好了。最后只需使用组合数算答案就好了。

时间复杂度 $$O(N^4)$$. 

##算法三
我们依然在最外面枚举$$notmin$$,考虑上一个算法的转移过程.

设$$F_i(x) = \sum_{j\ge0}{f_{i,j}x^j}$$.

会发现算法一中转移时的$$k$$值是一个后缀,根据$$DP$$方程，有：

$$F_i(x)=F_{i-1}(x)\times (\sum_{j=a_i}^{b_i}{x^j})$$

其中$$a_i,b_i$$分别表示每次转移的区间.把后面的和式写成$$\frac{x^{a_i}-x^{b_i+1}}{1-x}$$的形式.那么最后的生成函数就是$$\frac{x^T\prod_{i=1}^{cnt}{(x^{a_i}-x^{b_i+1})}}{(1-x)^{cnt}}$$,其中$$cnt$$表示$$found$$集合的大小,$$T$$表示选择
$$notmin$$以后随便选的不属于$$found$$的建筑个数.

把上面的分子背包一下,下面的分母展开后是组合数.把两者卷积之后就可以得到结果了.

于是我们可以在$$O(N^2)$$的时间内解决一次枚举的计算,时间复杂度$$O(N^3)$$.

##算法四
再继续分析,发现分母是不变的,需要维护的只是分子.我们能不能一边枚举$$notmin$$,一边维护分子的多项式呢?答案是可以的.

把所有的建筑拎出来按深度从小到大排序,然后从前往后扫,如果扫到一个不在$$found$$中的建筑就算一下,否则我们会发现分子的多项式只有一个因子会改变,使用多项式乘除法来维护分子即可.因为每次乘除的多项式都是$$(x^l-x^r)$$的形式,所以可以$$O(N)$$完成一次乘除.

至于具体实现乘除,乘法是比较好做的,除法只需把
$$\frac{1}{x^l-x^r}$$展开成$$x^{-l}\sum_{j\ge0}{x^{(r-l)j}}$$
,发现乘这个多项式相当于做无限背包.

时间复杂度$$O(N^2)$$.

