# 项目规划书 / Project Plan

> 在**跑任何模型之前**写完 v1，之后有改动就在第 10 节记录，不要直接覆盖旧内容。
> 💭 标记的地方是需要你自己思考后填写的。
> 版本：v1（2026-10-__）

---

## 1. 研究背景
Generative AI is now entering people's everyday information activities in two different ways. The first is passive exposure: when people use search engines such as Google, AI-generated summaries may appear automatically at the top of the results, so users may encounter AI content even without choosing to use AI. The second is active use: people deliberately open tools such as ChatGPT, Gemini or Copilot and use them for tasks such as work, study, writing, finding information or getting recommendations.

Previous research suggests that trust in and attitudes towards AI are associated with whether people use AI (Kelly, Kaye and Oviedo-Trespalacios, 2023). At the same time, generative AI use varies considerably across groups, for example by age, education and gender (Bick, Blandin and Deming, 2024; Humlum and Vestergaard, 2025). Moreover, whether through passive exposure or active use, people often turn to AI for several practical reasons at once, such as saving time or working more efficiently, so trust, personal characteristics and everyday digital habits are likely to overlap and interact. However, the relative importance of these factors in people's actual AI use remains unclear.

This project therefore asks: among UK adults, which factors are most strongly associated with how often people read AI search summaries, and how broadly they use generative AI? Is AI use more closely associated with attitudes towards AI, or with everyday digital habits? Answering these questions helps explain how unevenly AI-generated information reaches different groups, with implications for digital literacy education and AI product design.

## 2. Research Questions
- **RQ1:** Which factors are most strongly associated with how often UK adults read
  the AI-generated summaries that appear automatically in search results?
- **RQ2:** Among AI users, which factors are most strongly associated with the breadth
  of generative AI use (the number of different purposes AI is used for)?
- **RQ3:** Do machine-learning models (random forest, XGBoost) predict these two
  outcomes more accurately out of sample than traditional statistical models?
- **RQ4:** Beyond demographic characteristics, how much additional predictive power do
  everyday digital habits and attitudes towards AI each provide?

## 3. 数据与样本
| 项目 | 内容 |
|---|---|
| 数据 | Ofcom Adults' Media Literacy Tracker 2025，respondent-level CSV |
| 总样本 | 7,533（face-to-face 2,084 / online 5,449） |
| AI 模块 | Module 1，N = 3,768 |
| Y1 样本 | 💭 谁被问了 A6？排除"没见过"后剩多少人？为什么要排除？ |
| Y2 样本 | 💭 谁被问了 A3？ |
| 权重 | WT：描述统计用；建模 💭 用不用？为什么？ |

## 4. 变量定义
### 因变量
| | 变量 | 编码方式 | 类型 |
|---|---|---|---|
| Y1 | A6 | 💭 | 💭 |
| Y2 | A3A–A3H | 💭 哪些选项计入？Other / Don't know 怎么处理？ | 💭 |

### 自变量（三组）
| 组 | 变量 | 处理方式 |
|---|---|---|
| A 人口特征 | 💭 | 💭 |
| B 数字行为 | 💭 | 💭 |
| C AI 态度 | 💭 | 💭 "没听说过 AI"的人怎么处理？ |

### 控制变量
💭 P_METHOD 为什么必须放进模型？

## 5. 假设（**在看结果之前写，写你真实的猜测**）
| ID | 假设 | 方向 | 理由 |
|---|---|---|---|
| H1 | 💭 | +/− | 💭 |
| H2 | 💭 | | |
| H3 | 💭 | | |
| H4 | 💭（关于 RQ4：哪一组会增加最多预测力？） | | |

## 6. 方法：每个模型 + 它的假设 + 怎么检验
| 模型 | 用于 | 关键假设 | 怎么检验假设 / 假设不成立怎么办 |
|---|---|---|---|
| Ordered logit | Y1 解释 | Proportional odds：每个 X 对每一级的作用相同 | 💭 |
| Poisson GLM | Y2 解释 | 方差 = 均值 | 💭 怎么判断？如果不满足呢？ |
| Binomial GLM | Y2 解释 | 💭 为什么 Y2 可以看成"8 次试验中成功几次"？ | 💭 |
| Regularised logistic | Y1 预测 | 💭 | — |
| Random forest / XGBoost | Y1、Y2 预测 | 不假设线性，但可能过拟合 | 💭 怎么防止过拟合？ |
| k-means | 用户分群 | 簇大致呈球形、大小相近；变量需标准化 | 💭 k 怎么选？ |

## 7. 评估方式
- 数据划分：💭（train/test 比例？cross-validation 几折？）
- Y1 指标：💭（提示：为什么只看 accuracy 不够？有序类别适合什么指标？）
- Y2 指标：💭
- Baseline：💭（模型至少要比什么好？）
- 防止 leakage：💭

## 8. 预计局限
1. 💭
2. 💭
3. 💭

## 9. 交付物与时间线
| 交付物 | 截止 |
|---|---|
| 规划书 v1 | 10/12 |
| 清洗 + EDA | 10/18 |
| 回归 | 10/25 |
| ML + 三阶段比较 | 11/7 |
| 聚类 | 11/14 |
| 报告 | 11/28 |
| Demo | 12/9 |

## 10. 变更记录
| 日期 | 改了什么 | 为什么 |
|---|---|---|
| | | |

## 11. 想问老师的问题（office hour）
1. 这个项目能否作为课程 final project？
2. 二手公开数据需要 ethics 审批吗？
3. 💭 你自己最没把握的一个方法问题

## References

- Bick, A., Blandin, A. and Deming, D. J. (2024) *The Rapid Adoption of Generative AI*. NBER Working Paper No. 32966. Cambridge, MA: National Bureau of Economic Research.
- Humlum, A. and Vestergaard, E. (2025) 'The unequal adoption of ChatGPT exacerbates existing inequalities among workers', *Proceedings of the National Academy of Sciences*, 122(1), e2414972121. https://doi.org/10.1073/pnas.2414972121
- Kelly, S., Kaye, S. A. and Oviedo-Trespalacios, O. (2023) 'What factors contribute to the acceptance of artificial intelligence? A systematic review', *Telematics and Informatics*, 77, 101925. https://doi.org/10.1016/j.tele.2022.101925
