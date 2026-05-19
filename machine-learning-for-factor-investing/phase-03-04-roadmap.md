# Phase 3 & 4 — 从预测到组合，再到高级主题

启动时间：2026-05-15  
覆盖范围：

- Phase 3：Chapter 10–12，From predictions to portfolios
- Phase 4：Chapter 13–16，Further important topics

## 为什么现在直接进入阶段 3/4

前面第 3 章已经建立了因子投资语境：特征、因子、异常、暴露、alpha、横截面收益。  
阶段 3/4 的重点是把这些东西变成真正可检验的投资流程。

一句话：

> 阶段 3 解决“模型预测怎么变成组合并经受回测”；阶段 4 解决“模型为什么可信、何时失效、还能怎么扩展”。

---

## Phase 3：From predictions to portfolios

### Chapter 10 — Validating and tuning

核心问题：

- 预测模型怎么评价？
- 回归指标和分类指标分别看什么？
- 为什么金融里验证集/测试集不能乱切？
- 过拟合是什么？
- 超参数搜索为什么也会过拟合？

关键词：

- regression metrics
- classification metrics
- bias-variance tradeoff
- overfitting
- validation
- grid search
- Bayesian optimization
- backtest validation

学习目标：

> 知道一个模型在训练集表现好不代表策略有价值；理解时间序列/金融回测中验证协议的重要性。

### Chapter 11 — Ensemble models

核心问题：

- 为什么多个模型组合可能优于单模型？
- 线性 ensemble 和 stacked ensemble 有什么区别？
- 如何避免 ensemble 只是把过拟合叠得更复杂？
- 模型之间相关性为什么重要？

关键词：

- linear ensemble
- stacked ensemble
- two-stage training
- model correlation
- shrinkage

学习目标：

> 把 ensemble 理解成“预测信号组合”，而不是简单堆模型。

### Chapter 12 — Portfolio backtesting

核心问题：

- 预测信号如何转成组合权重？
- 回测协议怎么设？
- 夏普、回撤、换手、交易成本怎么看？
- 前视偏差和回测过拟合怎么避免？
- 为什么预测好不等于组合好？

关键词：

- signal to weights
- rebalancing
- performance metrics
- turnover
- transaction costs
- forward-looking bias
- backtest overfitting
- non-stationarity

学习目标：

> 这是全书最重要实战章。目标是能独立审查一个 ML factor strategy 的回测是否可信。

---

## Phase 4：Further important topics

### Chapter 13 — Interpretability

核心问题：

- 模型为什么给出这个预测？
- 全局解释和局部解释有什么区别？
- variable importance、PDP、LIME、Shapley 分别回答什么？
- 金融里解释性为什么不只是“好看”，而是风控和研究必需品？

学习目标：

> 能从模型输出倒推经济含义，判断模型是否学到了合理结构。

### Chapter 14 — Causality and non-stationarity

核心问题：

- 相关性和因果有什么区别？
- Granger causality 能说明什么，不能说明什么？
- 市场环境变化时，模型为什么失效？
- online learning / transfer learning 适合解决什么问题？

学习目标：

> 建立“金融关系会变”的意识，避免把历史规律当作固定定律。

### Chapter 15 — Unsupervised learning

核心问题：

- 特征高度相关时怎么办？
- PCA 和 autoencoder 的共同点是什么？
- 聚类和 nearest neighbors 在因子投资里能做什么？

学习目标：

> 把无监督学习理解成发现结构、降维、构造表征，而不是直接预测收益。

### Chapter 16 — Reinforcement learning

核心问题：

- 强化学习和监督学习的差别是什么？
- Q-learning、SARSA、policy gradient 在交易/组合里如何映射？
- 为什么维度灾难在金融 RL 里尤其严重？

学习目标：

> 理解 RL 的思想和限制，不把它当成万能交易机器人。

---

## 推荐学习顺序

### 标准顺序

1. Ch.10：验证与调参
2. Ch.12：组合回测
3. Ch.11：集成模型
4. Ch.13：解释性
5. Ch.14：因果与非平稳
6. Ch.15：无监督学习
7. Ch.16：强化学习

我建议把 Ch.12 提前到 Ch.11 前后都可以。原因是：对投资而言，最终评判不是模型分数，而是组合回测。

### 实战主线

```text
预测指标是否靠谱
    ↓
验证/调参是否有泄漏
    ↓
预测信号如何转权重
    ↓
回测是否包含成本、换手、风控
    ↓
解释模型为什么有效
    ↓
检查非平稳和因果风险
    ↓
再考虑 ensemble / PCA / autoencoder / RL 等扩展
```

## 阶段 3/4 的读书原则

1. **先验证，再优化**：不可信的验证协议下，调参越多越危险。
2. **先组合，再模型崇拜**：预测误差小不等于赚钱。
3. **先成本，再 Sharpe**：不看换手和成本的回测基本不完整。
4. **先解释，再上线**：黑盒预测在金融里容易隐藏风格暴露和样本偏差。
5. **先非平稳，再长期外推**：市场规律会变，模型也会过期。

## 当前材料

- `chapter-10-extract.txt`
- `chapter-11-extract.txt`
- `chapter-12-extract.txt`
- `chapter-13-16-outline-extract.txt`
- `session-03-chapter-10.md`
- `phase-03-04-roadmap.md`
