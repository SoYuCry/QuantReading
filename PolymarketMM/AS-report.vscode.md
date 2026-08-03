下面我把 Avellaneda–Stoikov 这篇 **High-frequency trading in a limit order book** 拆成一套“从业务直觉到公式”的推导。正式发表版本是 2008 年 Quantitative Finance, 8(3), 217–224，作者是 Marco Avellaneda 和 Sasha Stoikov；我下面引用的公式来自可公开读取的论文版本。([IDEAS/RePEc][1])

---

## 1. 这篇论文到底想解决什么问题？

做市商同时挂买单和卖单：

$$
p^b=s-\delta^b,\qquad p^a=s+\delta^a
$$

其中：

$$
s=\text{当前 mid-price}
$$

$$
\delta^b=\text{bid 距离 mid 的深度}
$$

$$
\delta^a=\text{ask 距离 mid 的深度}
$$

做市商赚钱的来源看似简单：低买高卖，赚 spread。但核心风险是：

**你的买单和卖单不会同时成交。**

比如你先买到了很多，库存 $q>0$，但价格接下来下跌，你就亏。或者你先卖空很多，库存 $q<0$，价格上涨，你也亏。所以这篇论文的出发点不是“预测价格”，而是：

> 在不知道短期方向的情况下，如何根据当前库存，动态调整 bid/ask，使自己既能赚 spread，又不会积累过大的库存风险。

论文明确说它关注两类风险中的 **inventory risk**，而不是信息不对称风险；同时它把做市商的报价问题写成一个最大化期望效用的问题。([Cornell People][2])

---

## 2. 为什么 mid-price 用 Brownian motion？

论文先假设 mid-price 满足：

$$
dS_t=\sigma dW_t
$$

也就是没有 drift 的 Brownian motion。这里不是说真实价格一定是布朗运动，而是模型故意不加入方向预测：做市商没有 alpha 观点，只处理库存风险。论文也说明，这个连续时间模型隐含了交易者对 drift 或 autocorrelation 没有观点。([Cornell People][2])

这个假设的作用是：

$$
S_T=S_t+\sigma(W_T-W_t)
$$

所以：

$$
S_T\mid S_t=s \sim N(s,\sigma^2(T-t))
$$

如果你持有 $q$ 股，那么终端库存价值 $qS_T$ 的方差是：

$$
\operatorname{Var}(qS_T)=q^2\sigma^2(T-t)
$$

这就是库存风险的数学来源。

注意这个模型的核心不是价格预测，而是说：

> 库存越大，未来价格随机波动带来的 P&L 方差越大。

这就是后面所有 $q$、$\sigma^2$、$T-t$ 出现的原因。

---

## 3. 为什么用 exponential utility？

论文选择最大化：

$$
\mathbb{E}\left[-e^{-\gamma(X_T+q_TS_T)}\right]
$$

其中：

$$
\gamma>0
$$

是风险厌恶系数。

这叫 CARA utility，也就是 constant absolute risk aversion。它有两个好处：

第一，它能惩罚尾部亏损。财富越低，效用下降得越快。

第二，它在数学上非常方便。论文也指出，exponential utility 方便定义 reservation price / indifference price，并且这些价格不依赖初始财富。([Cornell People][2])

直觉上，$\gamma$ 越大，做市商越怕库存风险，于是报价会更保守、更倾向于把库存打回零。

---

## 4. 先看“不交易，只持仓”的 frozen inventory 问题

这是整篇论文最关键的第一步。假设做市商现在有：

$$
x=\text{现金}
$$

$$
q=\text{库存}
$$

$$
S_t=s
$$

在 $T$ 时刻财富是：

$$
x+qS_T
$$

价值函数：

$$
v(x,s,q,t)
=

\mathbb{E}_t\left[-e^{-\gamma(x+qS_T)}\right]
$$

因为：

$$
S_T=s+\sigma(W_T-W_t)
$$

令：

$$
\tau=T-t
$$

则：

$$
qS_T=qs+q\sigma(W_T-W_t)
$$

所以：

$$
v(x,s,q,t)
=

-\exp(-\gamma x)
\exp(-\gamma qs)
\mathbb{E}\left[\exp(-\gamma q\sigma(W_T-W_t))\right]
$$

而正态变量的 moment generating function 是：

$$
\mathbb{E}[e^{aZ}]=e^{\frac{1}{2}a^2\operatorname{Var}(Z)}
$$

所以：

$$
\mathbb{E}\left[\exp(-\gamma q\sigma(W_T-W_t))\right]
=

\exp\left(\frac{1}{2}\gamma^2q^2\sigma^2\tau\right)
$$

于是：

$$
v(x,s,q,t)
=

-\exp\left(
-\gamma x
-\gamma qs
+
\frac{1}{2}\gamma^2q^2\sigma^2(T-t)
\right)
$$

也可以写成：

$$
v(x,s,q,t)
=

-\exp\left[
-\gamma
\left(
x+qs-\frac{1}{2}\gamma q^2\sigma^2(T-t)
\right)
\right]
$$

论文的式子 2.3 本质就是这个结果。([Cornell People][2])

括号里的：

$$
x+qs-\frac{1}{2}\gamma q^2\sigma^2(T-t)
$$

可以理解成 certainty equivalent，也就是“扣掉风险惩罚后的等价财富”。

这里非常重要：

$$
\frac{1}{2}\gamma q^2\sigma^2(T-t)
$$

就是库存风险成本。

它说明：

库存风险成本和 $q^2$ 成正比，而不是和 $q$ 成正比。

所以库存越大，惩罚增长得越快。

---

## 5. reservation price 是怎么推出来的？

论文定义 reservation bid price $r^b$：做市商愿意为了多买一股最多支付多少钱，使得自己买前买后效用无差异。形式上：

$$
v(x-r^b,s,q+1,t)=v(x,s,q,t)
$$

论文也强调，reservation price 是交易者主观估值，不是市场一定会成交的价格。([Cornell People][2])

用刚才的 certainty equivalent 来推更直观。

当前等价财富：

$$
CE(q)=x+qs-\frac{1}{2}\gamma q^2\sigma^2\tau
$$

如果花 $r^b$ 买一股，现金变成 $x-r^b$，库存变成 $q+1$：

$$
CE(q+1)=x-r^b+(q+1)s-\frac{1}{2}\gamma(q+1)^2\sigma^2\tau
$$

无差异条件：

$$
CE(q+1)=CE(q)
$$

即：

$$
x-r^b+(q+1)s-\frac{1}{2}\gamma(q+1)^2\sigma^2\tau
=

x+qs-\frac{1}{2}\gamma q^2\sigma^2\tau
$$

消掉 $x$ 和 (qs)：

$$
-r^b+s-\frac{1}{2}\gamma[(q+1)^2-q^2]\sigma^2\tau=0
$$

因为：

$$
(q+1)^2-q^2=2q+1
$$

所以：

$$
r^b=s-\frac{1}{2}\gamma(2q+1)\sigma^2\tau
$$

也就是：

$$
r^b
=

s+(-1-2q)\frac{\gamma\sigma^2(T-t)}{2}
$$

这就是论文的式 2.7。([Cornell People][2])

同理，reservation ask price $r^a$ 是卖出一股后无差异：

$$
v(x+r^a,s,q-1,t)=v(x,s,q,t)
$$

也就是：

$$
x+r^a+(q-1)s-\frac{1}{2}\gamma(q-1)^2\sigma^2\tau
=

x+qs-\frac{1}{2}\gamma q^2\sigma^2\tau
$$

解得：

$$
r^a=s+\frac{1}{2}\gamma(1-2q)\sigma^2\tau
$$

即：

$$
r^a
=

s+(1-2q)\frac{\gamma\sigma^2(T-t)}{2}
$$

这对应论文式 2.6。([Cornell People][2])

然后论文取二者平均，得到 indifference price / reservation price：

$$
r(s,q,t)=\frac{r^a+r^b}{2}
$$

代入：

$$
r(s,q,t)
=

s-q\gamma\sigma^2(T-t)
$$

这就是 Avellaneda–Stoikov 最核心的库存偏移公式。论文也解释说，如果 $q>0$，也就是做市商持有多头库存，那么 reservation price 会低于 mid-price，表示他更想卖出库存；如果 $q<0$，reservation price 会高于 mid-price，表示他更想买回来。([Cornell People][2])

---

## 6. 这个 reservation price 的直觉是什么？

公式：

$$
r=s-q\gamma\sigma^2(T-t)
$$

可以这样看。

如果你没有库存：

$$
q=0
$$

那么：

$$
r=s
$$

你觉得 mid-price 就是 fair price。

如果你已经多头：

$$
q>0
$$

那么：

$$
r<s
$$

你会把自己的报价中心往下移。

这会产生两个效果：

$$
p^a \text{ 下降}
$$

你的卖价更容易成交，有利于减仓。

$$
p^b \text{ 下降}
$$

你的买价更不容易成交，避免继续买入。

如果你已经空头：

$$
q<0
$$

那么：

$$
r>s
$$

你把报价中心往上移：

$$
p^b \text{ 上升}
$$

更容易买回来。

$$
p^a \text{ 上升}
$$

更不容易继续卖空。

所以这个模型不是简单地“扩大 spread”，而是更重要地做了一件事：

> 根据库存，把整个 bid/ask 报价中心从 mid-price 移到 reservation price。

---

## 7. 现在加入 limit order：为什么成交用 Poisson intensity？

做市商挂的单不一定成交。离 mid-price 越近，越容易成交；离 mid-price 越远，越不容易成交。

论文设：

$$
\lambda^a(\delta^a)
=

\text{卖单 ask 被买方 market order 打到的强度}
$$

$$
\lambda^b(\delta^b)
=

\text{买单 bid 被卖方 market order 打到的强度}
$$

并且它们都是 $\delta$ 的递减函数。论文还从市场单规模分布和临时冲击函数出发，说明一种自然形式是：

$$
\lambda(\delta)=Ae^{-k\delta}
$$

也就是报价越深，成交强度指数衰减。([Cornell People][2])

这个假设的直觉是：

你把 ask 挂得越高，只有更大的买方 market order 才能扫到你。

你把 bid 挂得越低，只有更大的卖方 market order 才能打到你。

大单出现概率通常比小单低，所以成交概率随报价深度下降。

这里的参数含义是：

$$
A=\text{基础成交强度}
$$

$$
k=\text{成交强度随报价深度衰减的速度}
$$

(k) 越大，说明你稍微挂远一点，成交概率就掉得很快。

---

## 8. 现金和库存如何跳变？

当 ask 成交，做市商卖出 1 股：

$$
x\to x+s+\delta^a
$$

$$
q\to q-1
$$

当 bid 成交，做市商买入 1 股：

$$
x\to x-(s-\delta^b)=x-s+\delta^b
$$

$$
q\to q+1
$$

论文把现金过程写成：

$$
dX_t=p^a dN_t^a-p^b dN_t^b
$$

库存是：

$$
q_t=N_t^b-N_t^a
$$

其中 $N^a,N^b$ 是 Poisson 过程。([Cornell People][2])

这一步非常关键，因为它把“限价单是否成交”变成了随机跳跃问题。

价格 $S_t$ 是连续扩散。

成交 $N_t^a,N_t^b$ 是离散跳跃。

所以最后的 HJB 里会同时出现 diffusion term 和 jump term。

---

## 9. HJB 方程从哪里来？

做市商的目标函数是：

$$
u(s,x,q,t)
=

\max_{\delta^a,\delta^b}
\mathbb{E}_t
\left[
-e^{-\gamma(X_T+q_TS_T)}
\right]
$$

终端条件：

$$
u(s,x,q,T)
=

-e^{-\gamma(x+qs)}
$$

论文用 dynamic programming principle 得到 HJB。核心思想是：

> 现在的最优价值 = 在接下来一个很小时间 $dt$ 内选择最优报价，然后加上未来最优价值。

在 $dt$ 内可能发生三件事：

第一，mid-price Brownian motion 小幅波动，带来：

$$
u_t+\frac{1}{2}\sigma^2u_{ss}
$$

第二，bid 成交，概率约为：

$$
\lambda^b(\delta^b)dt
$$

状态变成：

$$
(s,\ x-s+\delta^b,\ q+1)
$$

所以价值跳变：

$$
u(s,x-s+\delta^b,q+1,t)-u(s,x,q,t)
$$

第三，ask 成交，概率约为：

$$
\lambda^a(\delta^a)dt
$$

状态变成：

$$
(s,\ x+s+\delta^a,\ q-1)
$$

所以价值跳变：

$$
u(s,x+s+\delta^a,q-1,t)-u(s,x,q,t)
$$

合在一起：

$$
0=
u_t+\frac{1}{2}\sigma^2u_{ss}
+
\max_{\delta^b}
\lambda^b(\delta^b)
\left[
u(s,x-s+\delta^b,q+1,t)-u(s,x,q,t)
\right]
+
\max_{\delta^a}
\lambda^a(\delta^a)
\left[
u(s,x+s+\delta^a,q-1,t)-u(s,x,q,t)
\right]
$$

这就是论文中的 HJB 方程。([Cornell People][2])

它为什么有效？

因为在这个模型里，未来只依赖当前状态：

$$
(s,x,q,t)
$$

而不依赖更早的历史。这叫 Markov structure。所以我们可以用一个 value function (u) 概括“从现在到终点的全部最优未来价值”。

---

## 10. 为什么可以用 ansatz 把 $x$ 消掉？

论文使用：

$$
u(s,x,q,t)
=

-\exp(-\gamma x)\exp(-\gamma\theta(s,q,t))
$$

这一步来自 exponential utility 的 CARA 性质。现金 $x$ 只是线性进入财富：

$$
X_T+q_TS_T
$$

所以多一块现金，会让效用整体乘上一个固定因子。也就是说，$x$ 可以从状态变量里分离出去。论文也指出，正是因为选择了 exponential utility，问题可以通过这个 ansatz 简化。([Cornell People][2])

这里的 $\theta(s,q,t)$ 可以粗略理解成：

> 除了当前现金 $x$ 之外，库存和未来交易机会带来的“等价价值”。

把 ansatz 代入 HJB，需要用：

$$
u_t=-\gamma u\theta_t
$$

$$
u_s=-\gamma u\theta_s
$$

$$
u_{ss}=u(\gamma^2\theta_s^2-\gamma\theta_{ss})
$$

于是 diffusion 部分变成：

$$
\theta_t+\frac{1}{2}\sigma^2\theta_{ss}
---------------------------------------

\frac{1}{2}\gamma\sigma^2\theta_s^2
$$

后面那个负项：

$$
-\frac{1}{2}\gamma\sigma^2\theta_s^2
$$

就是风险厌恶和价格波动共同带来的非线性项。

---

## 11. $r^b,r^a$ 为什么可以写成 $\theta$ 的差？

根据 ansatz：

$$
u(s,x,q,t)
=

-e^{-\gamma x}e^{-\gamma\theta(s,q,t)}
$$

reservation bid price 满足：

$$
u(s,x-r^b,q+1,t)=u(s,x,q,t)
$$

代入：

$$
-e^{-\gamma(x-r^b)}e^{-\gamma\theta(s,q+1,t)}
=

-e^{-\gamma x}e^{-\gamma\theta(s,q,t)}
$$

去掉负号并取 log：

$$
-\gamma x+\gamma r^b-\gamma\theta(s,q+1,t)
=

-\gamma x-\gamma\theta(s,q,t)
$$

所以：

$$
r^b(s,q,t)
=

\theta(s,q+1,t)-\theta(s,q,t)
$$

同理：

$$
r^a(s,q,t)
=

\theta(s,q,t)-\theta(s,q-1,t)
$$

这就是论文的式 3.4 和 3.5。([Cornell People][2])

直觉是：

> reservation price 就是多一股或少一股库存时，value function 的边际变化。

---

## 12. 最优 $\delta^b,\delta^a$ 怎么推出来？

看 bid 一侧。

bid 成交后：

$$
x\to x-s+\delta^b
$$

$$
q\to q+1
$$

在 $\theta$ 形式下，bid 的目标项变成：

$$
\frac{\lambda^b(\delta^b)}{\gamma}
\left[
1-e^{\gamma(s-\delta^b-r^b)}
\right]
$$

我们要对 $\delta^b$ 最大化它。

令：

$$
F^b(\delta)
=

\frac{\lambda(\delta)}{\gamma}
\left[
1-e^{\gamma(s-\delta-r^b)}
\right]
$$

求一阶条件。

记：

$$
E=\gamma(s-\delta-r^b)
$$

则：

$$
F^b(\delta)
=

\frac{\lambda(\delta)}{\gamma}(1-e^E)
$$

求导：

$$
(F^b)'(\delta)
=

\frac{\lambda'(\delta)}{\gamma}(1-e^E)
+
\lambda(\delta)e^E
$$

令其等于 0：

$$
\lambda'(\delta)(1-e^E)+\gamma\lambda(\delta)e^E=0
$$

整理：

$$
\lambda'(\delta)
+
e^E(\gamma\lambda(\delta)-\lambda'(\delta))=0
$$

所以：

$$
e^E
=

\frac{-\lambda'(\delta)}
{\gamma\lambda(\delta)-\lambda'(\delta)}
$$

等价于：

$$
e^E
=

\frac{1}
{1-\gamma\frac{\lambda(\delta)}{\lambda'(\delta)}}
$$

取 log：

$$
\gamma(s-\delta-r^b)
=

-\ln
\left(
1-\gamma\frac{\lambda(\delta)}{\lambda'(\delta)}
\right)
$$

于是：

$$
s-r^b
=

## \delta

\frac{1}{\gamma}
\ln
\left(
1-\gamma\frac{\lambda(\delta)}{\lambda'(\delta)}
\right)
$$

这就是论文的 bid 一侧隐式最优条件。ask 一侧同理：

$$
r^a-s
=

## \delta^a

\frac{1}{\gamma}
\ln
\left(
1-\gamma\frac{\lambda^a(\delta^a)}
{\partial_\delta\lambda^a(\delta^a)}
\right)
$$

论文把这两个式子写成式 3.6 和 3.7。([Cornell People][2])

这组一阶条件的经济含义是：

> 报价更远，单笔成交利润更高，但成交概率更低；最优报价就是边际利润和边际成交率损失之间的平衡点。

---

## 13. 为什么指数 intensity 会给出漂亮的 closed-form？

设：

$$
\lambda(\delta)=Ae^{-k\delta}
$$

则：

$$
\lambda'(\delta)=-kAe^{-k\delta}=-k\lambda(\delta)
$$

所以：

$$
\frac{\lambda(\delta)}{\lambda'(\delta)}
=

-\frac{1}{k}
$$

代入一阶条件：

$$
1-\gamma\frac{\lambda}{\lambda'}
=

1+\frac{\gamma}{k}
$$

定义：

$$
c=\frac{1}{\gamma}\ln\left(1+\frac{\gamma}{k}\right)
$$

那么：

$$
\delta^b=s-r^b+c
$$

$$
\delta^a=r^a-s+c
$$

这就是最优报价中“纯粹由成交概率 trade-off 决定”的那部分。

如果使用 frozen inventory 得到的 reservation bid/ask：

$$
r^b=s-\left(q+\frac12\right)\gamma\sigma^2(T-t)
$$

$$
r^a=s-\left(q-\frac12\right)\gamma\sigma^2(T-t)
$$

那么：

$$
\delta^b
=

\left(q+\frac12\right)\gamma\sigma^2(T-t)
+
\frac{1}{\gamma}\ln\left(1+\frac{\gamma}{k}\right)
$$

$$
\delta^a
=

\left(\frac12-q\right)\gamma\sigma^2(T-t)
+
\frac{1}{\gamma}\ln\left(1+\frac{\gamma}{k}\right)
$$

所以 bid/ask 总 spread 是：

$$
\delta^a+\delta^b
=

\gamma\sigma^2(T-t)
+
\frac{2}{\gamma}
\ln\left(1+\frac{\gamma}{k}\right)
$$

论文通过对库存变量 $q$ 做 asymptotic expansion，并在线性近似下得到同样形式的 indifference price 和 spread，也就是式 3.17 和 3.18。([Cornell People][2])

---

## 14. 最终报价公式是什么？

最常用的 Avellaneda–Stoikov 近似形式是：

$$
r=s-q\gamma\sigma^2(T-t)
$$

$$
\text{spread}
=

\gamma\sigma^2(T-t)
+
\frac{2}{\gamma}
\ln\left(1+\frac{\gamma}{k}\right)
$$

令：

$$
\Delta
=

\gamma\sigma^2(T-t)
+
\frac{2}{\gamma}
\ln\left(1+\frac{\gamma}{k}\right)
$$

则：

$$
p^a=r+\frac{\Delta}{2}
$$

$$
p^b=r-\frac{\Delta}{2}
$$

展开：

$$
p^a
=

s-q\gamma\sigma^2(T-t)
+
\frac12\gamma\sigma^2(T-t)
+
\frac{1}{\gamma}
\ln\left(1+\frac{\gamma}{k}\right)
$$

$$
p^b
=

## s-q\gamma\sigma^2(T-t)

## \frac12\gamma\sigma^2(T-t)

\frac{1}{\gamma}
\ln\left(1+\frac{\gamma}{k}\right)
$$

也就是：

$$
p^a
=

s+\left(\frac12-q\right)\gamma\sigma^2(T-t)
+
\frac{1}{\gamma}
\ln\left(1+\frac{\gamma}{k}\right)
$$

$$
p^b
=

## s-\left(q+\frac12\right)\gamma\sigma^2(T-t)

\frac{1}{\gamma}
\ln\left(1+\frac{\gamma}{k}\right)
$$

这两个式子就是实务里最常见的 AS quote。

---

## 15. 每个参数到底控制什么？

### $q$：库存

$$
r=s-q\gamma\sigma^2(T-t)
$$

如果 $q>0$，报价中心下移。

如果 $q<0$，报价中心上移。

所以 $q$ 控制 **quote skew**。

---

### $\gamma$：风险厌恶

$$
r=s-q\gamma\sigma^2(T-t)
$$

$$
\text{spread}
=

\gamma\sigma^2(T-t)
+
\frac{2}{\gamma}\ln\left(1+\frac{\gamma}{k}\right)
$$

$\gamma$ 越大，库存惩罚越强，报价中心对库存越敏感，spread 也更偏保守。论文的模拟也显示，当 $\gamma$ 很小、接近风险中性时，inventory strategy 和 symmetric strategy 的差异会变小；当风险厌恶更强时，库存和 P&L 方差会更低，但收益也更保守。([Cornell People][2])

---

### $\sigma$：价格波动率

$$
q\gamma\sigma^2(T-t)
$$

波动率越高，持有库存越危险，所以报价中心偏移更大，spread 中的库存风险部分也更大。

---

### $T-t$：剩余时间

$$
q\gamma\sigma^2(T-t)
$$

剩余时间越长，价格未来随机波动的方差越大，所以库存风险越大。

在这篇论文的有限期限设定里，越接近终点 $T$，库存风险项越小，因为终端财富按 $S_T$ 估值；论文也解释过，越接近 $T$，库存暴露的时间越短。

实盘里如果你必须用 market order 平仓，或者有交易成本，临近收盘时反而可能需要更激进地清仓；这是原模型和真实交易之间的一个重要差异。

---

### (k)：订单到达率对报价深度的敏感度

$$
\lambda(\delta)=Ae^{-k\delta}
$$

$$
c=\frac{1}{\gamma}\ln\left(1+\frac{\gamma}{k}\right)
$$

如果 (k) 很大，说明你报价稍微远一点，成交率就快速下降，所以最优报价不能离 mid 太远。

如果 (k) 很小，说明市场深度较好，报价远一点也可能成交，所以你可以要求更大的 spread。

---

### (A)：基础订单到达率

在最简 closed-form spread 里，(A) 被抵消了。这是因为一阶条件里只关心：

$$
\frac{\lambda}{\lambda'}
$$

对于指数强度：

$$
\frac{Ae^{-k\delta}}{-kAe^{-k\delta}}=-\frac1k
$$

所以 (A) 不影响最优报价深度。

但这不代表 (A) 不重要。它会影响实际成交次数、P&L 分布，以及更完整的 HJB 解。在简化 closed-form 里它只是没有进入最终报价公式。

---

## 16. 为什么这种数学方式“有效”？

它有效的原因不是因为它完美描述市场，而是因为它把做市问题拆成了两个非常合理的 trade-off。

第一个 trade-off 是 **库存风险**。

持有 $q$ 股，未来价格风险方差是：

$$
q^2\sigma^2(T-t)
$$

exponential utility 把这个风险变成：

$$
\frac12\gamma q^2\sigma^2(T-t)
$$

所以库存越大，风险惩罚越大。对 $q$ 求边际变化，就自然得到线性的 reservation price adjustment：

$$
-q\gamma\sigma^2(T-t)
$$

这就是为什么报价中心要随库存移动。

第二个 trade-off 是 **成交利润 vs 成交概率**。

报价越远：

$$
\delta \uparrow
$$

单次成交利润更大。

但成交率：

$$
\lambda(\delta)
$$

下降。

HJB 的一阶条件正是在做这个边际平衡：

$$
\text{多赚一点 spread 的好处}
=

\text{少成交的机会成本}
$$

指数 intensity 让这个平衡点变成一个 log 项：

$$
\frac{1}{\gamma}\ln\left(1+\frac{\gamma}{k}\right)
$$

所以最终报价由两部分组成：

$$
\text{库存风险项}
+
\text{市场成交概率项}
$$

也就是：

$$
\text{spread}
=

\underbrace{\gamma\sigma^2(T-t)}*{\text{库存风险补偿}}
+
\underbrace{\frac{2}{\gamma}\ln\left(1+\frac{\gamma}{k}\right)}*{\text{成交概率/流动性补偿}}
$$

---

## 17. 用一句话概括 AS 模型

Avellaneda–Stoikov 模型可以压缩成一句话：

> 先根据库存算出自己的主观 fair price，再根据成交概率决定 bid/ask 离这个 fair price 多远。

也就是：

$$
\boxed{
r=s-q\gamma\sigma^2(T-t)
}
$$

$$
\boxed{
p^a=r+\frac12\left[
\gamma\sigma^2(T-t)
+
\frac{2}{\gamma}\ln\left(1+\frac{\gamma}{k}\right)
\right]
}
$$

$$
\boxed{
p^b=r-\frac12\left[
\gamma\sigma^2(T-t)
+
\frac{2}{\gamma}\ln\left(1+\frac{\gamma}{k}\right)
\right]
}
$$

---

## 18. 它和 naive symmetric quoting 的区别

naive strategy 是：

$$
p^a=s+\frac{\Delta}{2}
$$

$$
p^b=s-\frac{\Delta}{2}
$$

永远围绕 mid-price 对称报价。

AS strategy 是：

$$
p^a=r+\frac{\Delta}{2}
$$

$$
p^b=r-\frac{\Delta}{2}
$$

围绕 reservation price 报价。

区别只有一个：

$$
s \quad \text{vs} \quad r=s-q\gamma\sigma^2(T-t)
$$

但这个区别非常重要。

因为它让库存自然均值回归：多了就倾向于卖，空了就倾向于买。

论文的模拟比较了 inventory strategy 和围绕 mid-price 的 symmetric strategy，结果显示 inventory strategy 的利润方差和最终库存方差显著更低，不过有时平均收益会更低。([Cornell People][2])

---

## 19. 这篇论文的局限

你读这篇时一定要记住：它是 foundational model，不是可以直接裸跑的完整实盘系统。

主要局限有：

第一，mid-price 没有 drift，也没有 alpha。

真实高频做市通常会用 order book imbalance、microprice、短期 alpha、queue dynamics 等信息。

第二，订单到达被建模为 Poisson。

真实订单流会有 clustering、自激性、Hawkes 特征，并不是独立 Poisson。

第三，没有显式处理 queue position。

你挂在 best bid 第一位和最后一位，成交概率完全不同。

第四，没有 adverse selection。

如果你的 ask 被打，可能不是随机成交，而是因为买方知道价格要涨。

第五，论文假设 limit orders 可以连续、无成本更新。论文中确实作了这类简化设定；实盘中有 latency、tick size、手续费、撤单限制、排队优先级等问题。([Cornell People][2])

所以 AS 更像一个“库存控制骨架”：

$$
\text{quote center}=\text{fair price}-\text{inventory penalty}
$$

$$
\text{spread}=\text{risk compensation}+\text{liquidity compensation}
$$

实盘会在这个骨架上加 alpha、queue model、fees、tick rounding、position limits、latency model 和 kill switch。

---

## 20. 最后用一个极简直觉收尾

你可以把 AS 模型想成一个自动调节器。

当你库存为零：

$$
r=s
$$

正常围绕 mid 做市。

当你买太多：

$$
q>0
$$

$$
r<s
$$

你把 bid/ask 一起往下压，让自己更容易卖掉库存，不容易继续买入。

当你卖空太多：

$$
q<0
$$

$$
r>s
$$

你把 bid/ask 一起往上抬，让自己更容易买回来，不容易继续卖空。

spread 的宽度则由：

$$
\gamma,\sigma,T-t,k
$$

决定：风险越大，spread 越宽；市场成交率下降越快，报价越不能挂太远。

所以这篇论文的核心不是复杂公式，而是非常清楚的两步：

$$
\boxed{
\text{先管库存风险，再管成交概率}
}
$$

[1]: https://ideas.repec.org/a/taf/quantf/v8y2008i3p217-224.html "High-frequency trading in a limit order book"
[2]: https://people.orie.cornell.edu/sfs33/LimitOrderBook.pdf "LimitOrderBookRevised.dvi"
