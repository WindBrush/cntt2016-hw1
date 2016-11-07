# SparseFactorial

作者：马龙

关键词：数学 同余 中国剩余定理

## 题目简述

定义“稀疏阶乘函数”$$F(x)=\Pi_{k=0}^{\lfloor \sqrt{x-1} \rfloor} (x-k^2)$$，求在$$[lo, hi]$$中有多少个正整数$$x$$满足$$F(x)$$是$$m$$的倍数。

$$1 \leq lo \leq hi \leq 10^{12}, 2 \leq m \leq 10^6$$

## 分析

我们可以将题目中的要求转化为求$$[1, T]$$中满足$$F(x) \equiv 0 \pmod{m}$$的$$x$$个数。不难发现，$$F(x) \mod{m}$$仅与$$x \mod{m}$$和$$k$$的最大值有关，且假如$$F(x) \equiv 0 \mod{m}$$，那么就有$$F(x + m) \equiv 0 \mod{m}$$。因此，我们可以对所有的$$x$$按模$$m$$的余数进行分类，并计算出每一类$$x$$中满足条件的下界是多少，就能得到答案了。设这个下界为$$A_m[]$$。

此外，对于一组满足$$(x, y) = 1$$的$$x$$和$$y$$，不难利用$$A_x[]$$及$$A_y[]$$在$$O(xy)$$时间内构造出正确的$$A_{xy}[]$$。因此，现在的问题是，给定某个质数幂$$p^c$$，求出$$A_{p^c}[]$$的值。

## 算法一

考虑最直接的做法。对于每个$$[0, p^c)$$中的$$i$$，我们枚举$$k$$，计算出$$i-k^2$$中$$p$$的数量并累加，直到不少于$$c$$；$$A_{p^c}[i]$$就等于此时的$$k^2 + 1$$。由于$$k$$是$$O(\sqrt{x})$$的，总复杂度$$O(m \sqrt{hi})$$。

## 算法二

算法一的复杂度太高了，需要优化。注意到$$c$$的范围非常小，因此假如我们只枚举$$i-k^2$$中包含至少一个$$p$$的$$i$$和$$k$$，复杂度就会降低许多。因此，我们先枚举$$k$$，并维护一个$$cnt[i]$$表示到目前为止所有$$i-k^2$$中$$p$$的总个数；然后再枚举满足$$i \equiv k^2 \mod{p}$$的所有$$i$$，这样的$$i$$就只有$$p^{c-1}$$个了，单次求解的复杂度降至$$O(p^{c-1} \sqrt{hi})$$。

## 算法三

从算法二中，我们发现，每个$$k$$所影响的$$i$$以$$p$$为周期循环，$$k$$与$$k+p$$所影响的$$i$$是相同的；而每次每个受影响的$$i$$的$$cnt[i]$$至少会加一，因此有用的$$k$$只有$$p*c$$个。与算法二结合后，复杂度降至$$O(p^{c-1} * p * c) = O(p^c * c)$$，由于$$c=O(\log m)$$，总复杂度$$O(m \log m)$$，可以通过本题。