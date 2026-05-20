# Time-Series Foundation Models — Paper Pack & Reading Plan

更新日期：2026-05-20

本目录收集时间序列深度预测、Transformer 长序列预测、LLM/基座模型方向的核心论文。目标不是按论文年份机械阅读，而是沿着一条问题链理解：

```text
统计/深度全局预测模型
  -> Transformer 如何处理长时间序列
  -> patching / decomposition / frequency 等时间序列归纳偏置
  -> LLM/foundation model 如何迁移到 forecasting
  -> zero-shot / probabilistic / multivariate forecasting 是否真的成立
```

> 注：引用量会随时间变化，不同来源（Semantic Scholar、OpenAlex、Google Scholar）口径也不同。前一版检索中 TimesFM、Moirai 有较高引用量参考；Chronos、TimeGPT 等因 API 限流待复核。阅读优先级不只看引用量，也看它在技术路线中的位置。

---

## 0. 已下载论文清单

### A. Time-series foundation model 主线

| 文件 | 论文 | 年份 | 定位 |
|---|---|---:|---|
| `2024-timesfm-decoder-only-foundation-model-for-time-series-forecasting.pdf` | A decoder-only foundation model for time-series forecasting / TimesFM | 2023/2024 | 连续 patch + decoder-only Transformer + MSE point forecast |
| `2024-chronos-learning-the-language-of-time-series.pdf` | Chronos: Learning the Language of Time Series | 2024 | 把时间序列量化成 token，用 LM 范式做 forecasting |
| `2023-lag-llama-towards-foundation-models-for-probabilistic-time-series-forecasting.pdf` | Lag-Llama | 2023 | LLaMA 风格 decoder + lag features + probabilistic forecasting |
| `2024-moirai-unified-training-of-universal-time-series-forecasting-transformers.pdf` | Moirai: Unified Training of Universal Time Series Forecasting Transformers | 2024 | 统一多数据集、多频率、多变量/概率预测框架 |
| `2023-timegpt-1.pdf` | TimeGPT-1 | 2023 | 产品化/商业化 time-series foundation model 代表 |
| `2024-moment-family-of-open-time-series-foundation-models.pdf` | MOMENT: A Family of Open Time-series Foundation Models | 2024 | 开源 time-series foundation model 家族 |
| `2024-timer-generative-pretrained-transformers-are-large-time-series-models.pdf` | Timer: Generative Pre-trained Transformers Are Large Time Series Models | 2024 | 生成式预训练 Transformer 路线 |

### B. LLM 迁移/重编程路线

| 文件 | 论文 | 年份 | 定位 |
|---|---|---:|---|
| `2023-llmtime-large-language-models-are-zero-shot-time-series-forecasters.pdf` | LLMTime: Large Language Models Are Zero-Shot Time Series Forecasters | 2023 | 把数值序列文本化，直接利用 LLM zero-shot 预测 |
| `2023-time-llm-time-series-forecasting-by-reprogramming-large-language-models.pdf` | Time-LLM: Time Series Forecasting by Reprogramming Large Language Models | 2023 | 通过 reprogramming 让 LLM 适配时间序列 |
| `2023-gpt4ts-one-fits-all-power-general-time-series-analysis-by-pretrained-lm.pdf` | One Fits All / GPT4TS | 2023 | 用预训练语言模型做通用时间序列分析 |

### C. Transformer / patching / decomposition 基础

| 文件 | 论文 | 年份 | 定位 |
|---|---|---:|---|
| `2022-patchtst-a-time-series-is-worth-64-words.pdf` | PatchTST: A Time Series is Worth 64 Words | 2022/2023 | patching 思想关键论文；TimesFM 直接受其启发 |
| `2022-timesnet-temporal-2d-variation-modeling.pdf` | TimesNet | 2022/2023 | 把 1D 时间序列转为 2D temporal variation 表示 |
| `2021-informer-beyond-efficient-transformer-for-long-sequence-time-series-forecasting.pdf` | Informer | 2021 | 早期长序列 Transformer forecasting 代表 |
| `2021-autoformer-decomposition-transformers-with-auto-correlation.pdf` | Autoformer | 2021 | decomposition + auto-correlation 时间序列归纳偏置 |
| `2022-fedformer-frequency-enhanced-decomposed-transformer.pdf` | FEDformer | 2022 | frequency-domain + decomposition 长序列预测 |

### D. 深度预测基础模型

| 文件 | 论文 | 年份 | 定位 |
|---|---|---:|---|
| `2017-deepar-probabilistic-forecasting-with-autoregressive-recurrent-networks.pdf` | DeepAR | 2017 | global univariate probabilistic forecasting 基础 |
| `2019-nbeats-neural-basis-expansion-analysis-for-interpretable-time-series-forecasting.pdf` | N-BEATS | 2019 | 强神经预测基线；basis expansion + interpretability |
| `2019-temporal-fusion-transformers-for-interpretable-multi-horizon-forecasting.pdf` | Temporal Fusion Transformers / TFT | 2019 | 多变量、多 horizon、协变量和可解释性工业预测框架 |

---

## 1. 推荐阅读顺序

### Phase 1 — 先理解深度时间序列预测的基本范式

1. **DeepAR**
   - 问题：为什么可以跨很多条序列训练一个 global model？
   - 重点：probabilistic forecasting、autoregressive likelihood、scale handling。

2. **N-BEATS**
   - 问题：纯深度网络如何在 forecasting 上做强基线？
   - 重点：backcast/forecast blocks、basis expansion、trend/seasonality block。

3. **TFT**
   - 问题：工业多变量预测如何利用 known future covariates？
   - 重点：static covariates、known/observed covariates、multi-horizon、attention interpretability。

### Phase 2 — Transformer 长序列预测与时间序列归纳偏置

4. **Informer**
   - 问题：Transformer attention 太贵，长序列预测怎么做？
   - 重点：ProbSparse attention、long sequence forecasting benchmark。
   - 注意：历史意义大，但后续 benchmark 争议较多。

5. **Autoformer**
   - 问题：时间序列能否显式拆 trend/seasonal？
   - 重点：decomposition、auto-correlation。

6. **FEDformer**
   - 问题：频域信息能否提高长序列预测？
   - 重点：frequency enhanced blocks、decomposition。

7. **PatchTST**
   - 问题：为什么 patching 对时间序列有效？
   - 重点：patch token、channel independence、long horizon forecasting。
   - 这是读 TimesFM/Chronos 前最重要的桥。

8. **TimesNet**
   - 问题：如何把一维时间序列的多周期结构转成二维表示？
   - 重点：temporal 2D variation、period discovery、general time-series analysis。

### Phase 3 — TimesFM/Chronos/Lag-Llama/Moirai 主线

9. **TimesFM**
   - 问题：decoder-only Transformer 如何直接做 zero-shot point forecasting？
   - 重点：input patch、output patch、MSE、univariate foundation forecaster、XReg 外挂协变量。

10. **Chronos**
    - 问题：能否把时间序列当语言来建模？
    - 重点：scaling、quantization、tokenization、forecast samples from token distribution。
    - 和 TimesFM 对比：连续 patch/MSE vs 离散 token/LM loss。

11. **Lag-Llama**
    - 问题：LLaMA-style decoder 如何结合传统 lag 特征做概率预测？
    - 重点：lagged values、probabilistic outputs、zero-shot transfer。

12. **Moirai**
    - 问题：如何统一不同频率、不同变量数、不同 horizon？
    - 重点：universal forecasting、multi patch size、probabilistic forecasting、multivariate handling。

13. **TimeGPT**
    - 问题：商业化 time-series foundation model 如何定义 benchmark 和产品价值？
    - 重点：zero-shot、fine-tuning、API 产品形态、可复现性限制。

14. **MOMENT**
    - 问题：开源 time-series foundation model 家族如何覆盖 forecasting/classification/anomaly/imputation？
    - 重点：通用任务接口、预训练目标、开源生态。

15. **Timer**
    - 问题：generative pretraining 在时间序列上是否能 scale？
    - 重点：大规模预训练、生成式建模、和 LLM 类比。

### Phase 4 — LLM 迁移路线作为对照

16. **LLMTime**
    - 问题：直接把数字写成文本喂给 LLM，为什么也能预测？
    - 重点：prompting、serialization、zero-shot，但成本与稳定性存疑。

17. **GPT4TS / One Fits All**
    - 问题：预训练 LM 能不能作为通用时间序列 backbone？
    - 重点：冻结/微调、跨任务通用性。

18. **Time-LLM**
    - 问题：reprogramming LLM 比从头训练 time-series model 有什么优劣？
    - 重点：prototype / prompt-as-prefix / text prototypes，适配机制。

---

## 2. 对比阅读问题清单

读每篇时建议固定回答这些问题：

1. **输入是什么？** raw value、patch、token、lag features、covariates？
2. **输出是什么？** point forecast、quantile、distribution parameters、samples？
3. **loss 是什么？** MSE、MAE、cross-entropy、NLL、quantile loss？
4. **是否支持 zero-shot？** 还是必须 supervised training / fine-tuning？
5. **是否支持 multivariate？** 原生联合建模，还是 channel-independent / 外挂回归？
6. **是否支持 known future covariates？** 这对商业预测非常关键。
7. **如何处理尺度？** normalization / scaling / tokenization。
8. **预训练数据是什么？** 真实数据、合成数据、公开 benchmark、业务私有数据？
9. **和 naive baseline 比提升在哪里？** 对强季节性序列还是低 SNR 序列？
10. **对金融有什么意义？** 预测 level/vol/flow 可能有用；预测 return alpha 要谨慎。

---

## 3. 和金融量化连接时的警惕

这些模型很多 benchmark 来自：

```text
traffic / electricity / weather / web traffic / sales / energy / sensor
```

这些序列往往有：

```text
强自相关
强季节性
可见趋势
更高 SNR
更稳定生成机制
```

而金融收益序列通常是：

```text
低 SNR
弱自相关
非平稳
带反馈
可交易 alpha 会衰减
```

所以要区分：

```text
forecasting business time series
    ≠
forecasting tradable financial returns
```

更现实的金融用法可能是：

- 预测波动率、成交量、流动性、资金流，而不是直接预测收益；
- 作为特征抽取器，生成资产/市场状态 representation；
- 对强季节性或强自相关金融相关序列有用，例如成交量曲线、订单流强度、波动率聚集；
- 和经济因子、风险模型、regime 特征结合，而不是单独作为 alpha 机器。

---

## 4. 当前建议的下一篇

我们已经读过 TimesFM 的核心结构。下一篇最建议读：

```text
Chronos: Learning the Language of Time Series
```

原因：它和 TimesFM 的对比最清楚：

```text
TimesFM: continuous patch + MSE point forecast
Chronos: quantized value token + language-model-style forecasting
```

读完 Chronos 后，再回头读 PatchTST，会更清楚 patching 为什么是这一代 time-series foundation model 的关键桥梁。
