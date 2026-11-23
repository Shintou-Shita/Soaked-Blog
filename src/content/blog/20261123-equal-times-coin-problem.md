---
title: 'Equal Times Coin Problem'
description: 'If a coin is tossed repeatedly, when the number of times it lands heads up is equal to the number of times it lands tails up for the first time, what is the expected number of tosses?'
pubDate: '2026 NOV 23'
---

小红书上刷到一道题，尝试解答。

不断投掷硬币，首次正面向上和反面向上次数相等时投掷次数的期望是多少？

设首次正面向上和反面向上次数相等时，投掷次数为 $X$，那么我们要求的就是 $E[X]$.

$$
    E[X] = \sum_{i=1}^{\infty} \Pr[X = 2i]\times 2i
$$

那么 $\Pr[X = 2i]$ 是多少呢？不妨将其转化为从 $(0,0)$ 走到 $(i, i)$ 的一条路径，这条路径由若干四相邻整点连接的折线构成，要求路径只能向右走或向上走，且不能碰到 $y=x$ 这条直线。

不难发现，如果从 $(1, 0)$ 走到 $(i, i-1)$ 的遵守如上要求的路径为 Catalan 数 $C_{i-1}=\frac{1}{i}\binom{2i-2}{i-1}$. 根据对称性，从 $(0, 0)$ 只能走到 $(0, 1)$ 或 $(1, 0)$，$(i, i-1)$ 和 $(i-1, i)$ 只能走到 $(i,i)$，从而：

$$
    \Pr[X = 2i] = \dfrac{2\binom{2i-2}{i-1}}{i2^{2i}}
$$

从而，

$$
    E[X] = \sum_{i=1}^{\infty} \dfrac{2\binom{2i-2}{i-1}}{i2^{2i}}\times 2i = \sum_{i=1}^{\infty} \dfrac{(2i-2)!}{(i-1)!^22^{2i-2}} = \sum_{i=0}^{\infty} \dfrac{(2i)!}{i!^22^{2i}}
$$

接下来就是求解上面这个级数了，我们不妨记生成函数 $s = s(x) = \sum_{i=0}^\infty\dfrac{(2i)!}{i!^2}x^i$，那么 $E[X]=s(1/4)$.

经过凑项，不难发现 $s' + 2s = 4s + 4xs'$，即 $\dfrac{s'}{s}=\dfrac{2}{1-4x}$，两边对 $x$ 积分得到：$\ln s = \int \dfrac{2}{1-4x}\mathrm{d}x = -\ln \sqrt{1-4x} + C_1$，即 $s = \dfrac{C_2}{\sqrt{1-4x}}$.

这一积分过程可表述为如下：

$$
    \begin{aligned}
        \int \dfrac{\mathrm{d}x}{1-4x}
        &\xlongequal{x:=u^2/4}\dfrac{1}{2}\int \dfrac{u\mathrm{d}u}{1-u^2}\\
        &\xlongequal{u:=\sin v}\dfrac{1}{2}\int \dfrac{\sin v}{\cos v}\mathrm{d}v\\
        &=-\dfrac{1}{2}\ln|\cos v|\\
        &=-\dfrac{1}{2}\ln\sqrt{1-u^2}\\
        &=-\dfrac{1}{2}\ln\sqrt{1-4x}
    \end{aligned}
$$

常数 $C_2$ 由初值 $s(0) = 1$ 决定，从而 $C_2=1$，进而 $s = s(x) = \dfrac{1}{\sqrt{1-4x}}$. 那么 $s(1/4) = \infty$.

这暗示了 $E[X]$ 发散到无穷大，那么这是对的吗？我们需要使用分析学工具。

考虑使用 Stirling 公式 $n!\sim \sqrt{2\pi n}(n/e)^n$ 得到 $(2i)!/i!^2\sim \dfrac{4^n}{\sqrt{\pi n}}$，从而：

$$
    E[X]=\sum_{i=0}^\infty\dfrac{(2i)!}{i!^22^{2i}}\approx 1 + \sum_{i=1}^\infty \dfrac{1}{\sqrt{\pi i}}
$$

显然，对 $\sum i^{-1/2}$ 求和因 $-1/2 > -1$ 而发散，从而我们可以说 $E[X]\to \infty$.

从而，这个问题的答案就是抛掷硬币期望次数为无穷大。





