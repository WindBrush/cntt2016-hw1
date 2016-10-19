# TheDivisionGame
作者:陆宇暄

关键词:博弈论 sg函数

## 题目简述
按如下方式定义一个游戏：
给定$$l$$,$$r$$($$l \leq r$$),定义**可重**数集$$S=\{x \in Z|l \leq x \leq r\}$$.定义一次操作为选中数集中的某个数$$num$$然后把它变成一个不等于他自己的因数.两个玩家轮流操作,没有合法操作(即$$S$$中所有数都是$$1$$)的玩家失败.
题目给定两个数$$L$$和$$R$$,问有多少种$$L \leq l \leq r \leq R$$满足$$l$$和$$r$$定义的游戏先手必胜.

保证$$1 \leq L \leq R \leq 10^9$$,$$R-L \leq 10^6$$.

## 分析
观察该游戏,发现数集中的每个数都是独立的nim问题,所以考虑计算每个数的sg函数.

###引理：这个游戏的任意一个数的sg值都是它的**可重复**质因数数量.

####例：$$12=2 \times 2 \times 3$$ <---> $$sg(12)=1+1+1=3$$

####证明：
由于这个数为1时先手必败,所以$$sg(1)=0$$.
当该引理对所有有$$0 \leq x \leq n$$个质因数的数$$x$$成立时:
考虑有$$n+1$$个质因数的数$$y=p_1*p_2*p_3*...*p_n*p_{n+1}$$必然有质因数数量为$$0$$到$$n$$的因子,且一定不会有质因数数量为$$n+1$$的因子,根据sg函数的定义,$$sg(y)=n+1$$.
所以该引理对所有有$$0 \leq x \leq n+1$$个质因数的数$$x$$成立
得证!

## 算法
既然不是很好统计先手必胜的情况,那就统计后手必胜即sg函数$$xor$$和为$$0$$的情况数.

假设我们已经求出的$$L \sim R$$中的所有数的sg值,由于$$xor$$运算的自反性,所以可以直接用一个计数数组统计出答案.

    long long ans=0;num[0]=1;int sg=0;
    for (int i=l;i<=r;i++)
    {
    	sg^=fac[i-l];
    	ans+=num[sg];
    	num[sg]++;
    }

所以现在的问题就是如何求出$$L \sim R$$中的所有数的质因数个数.这是一个经典(划掉)问题.一个简单的方法就是用$$\sqrt{10^9}$$以内的质数筛一遍就好了.

时间复杂度:   $$O( \sqrt{10^9} + 10^6 \times log(10^6))$$