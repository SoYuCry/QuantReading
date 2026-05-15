# Machine Learning for Factor Investing — 带读计划

来源：`../machine-learning-for-factor-investing-python-version.pdf`  
PDF 信息：358 页；作者 Guillaume Coqueret、Tony Guida；Python version。

## 我们怎么读

目标不是逐页翻译，而是把书读成三类产出：

1. **概念地图**：因子投资、资产定价异常、机器学习建模、回测与可解释性之间的关系。
2. **实操清单**：每章哪些代码/实验值得复现，哪些只是了解即可。
3. **风险意识**：过拟合、前视偏差、数据噪声、非平稳性、交易成本、因果 vs 相关。

每次带读建议固定格式：

- 本次读哪几页/哪一节
- 3–5 个核心问题
- 关键概念解释
- 金融直觉 + 机器学习直觉
- 代码/实验任务
- 读后自测
- 下一次预告

## 推荐路线

### 阶段 0：建立地图

- Preface：本书边界、目标读者、先修知识
- Chapter 1：记号与数据
- Chapter 2：整体工作流与警告

目标：知道这本书不是泛泛讲 ML in finance，而是聚焦基于公司特征的 equity factor investing。

### 阶段 1：因子投资底座

- Chapter 3：Factor investing and asset pricing anomalies
- Chapter 4：Data preprocessing

目标：理解“特征 → 预期收益/组合权重”的研究范式，以及金融数据为什么难。

### 阶段 2：常用监督学习模型

- Chapter 5：惩罚回归 / sparse hedging
- Chapter 6：树模型、随机森林、boosting、XGBoost
- Chapter 7：神经网络
- Chapter 8：SVM
- Chapter 9：贝叶斯方法

目标：不是追模型名，而是问：这个模型怎样处理噪声、非线性、变量选择和过拟合？

### 阶段 3：从预测到组合

- Chapter 10：验证、调参、过拟合
- Chapter 11：集成模型
- Chapter 12：组合回测

目标：这是全书最应该精读的部分。预测准确不等于投资可用；需要权重构造、成本、换手、风险调整、外样本验证。

### 阶段 4：高级主题

- Chapter 13：模型解释性
- Chapter 14：因果与非平稳
- Chapter 15：无监督学习
- Chapter 16：强化学习

目标：理解高阶方法的适用边界，避免“模型越复杂越好”的误区。

## 建议节奏

- 快读版：8 次，每次 1–2 章，目标是建立地图。
- 标准版：16 次，每次 1 章，配套自测和代码复现。
- 实战版：20+ 次，读书 + 复现代码 + 改造为自己的因子回测实验。

## 当前进度

- [x] 提取 PDF 元数据与目录
- [x] 建立读书计划
- [ ] Session 01：Preface + Chapter 1 + Chapter 2
- [ ] Session 02：Chapter 3 因子投资与资产定价异常
- [ ] Session 03：Chapter 4 数据预处理
