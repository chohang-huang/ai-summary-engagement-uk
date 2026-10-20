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

## 3. Data and Sample
| Item | Details |
|---|---|
| Data | Ofcom Adults' Media Literacy Tracker 2025, respondent-level CSV, with SPSS codebook |
| Total sample | 7,533 UK adults (online 5,449 / face-to-face CAPI 2,084) |
| AI module | The AI questions (A1–A11) are only in Module 1, which was given to 3,768 respondents |
| Y1 sample | A6 was asked only of Module 1 respondents who use the internet: N = 3,515. Respondents who answered "never seen these summaries / don't know what they are" (A6 = 5, n = 337) are not on the frequency scale, so they are excluded from the ordinal model (N = 3,178) and described separately. |
| Y2 sample | A3 (AI uses) applies only to AI users (SUMAI = 1): N = 2,061. Non-users are coded 0 rather than left blank, so the sample must be filtered on SUMAI before building Y2. |
| Weights | The weighting variable WT will be used for descriptive statistics, so that percentages represent the UK adult population. Models will be estimated unweighted, because the variables used to build the weights (e.g. age, gender) are included as predictors; weighted models will be run as a sensitivity check. |

## 4. Variables
### Dependent variables
| | Source | Coding | Type |
|---|---|---|---|
| Y1 | A6 (how often the respondent reads AI summaries in search results) | Reverse-coded so that higher = more reading: 0 = never, 1 = sometimes, 2 = often, 3 = always. A6 = 5 ("never seen / don't know") is excluded from the model and described separately. | Ordinal |
| Y2 | A3A–A3H (eight specific purposes of AI use) | Number of purposes selected (0–8), among AI users only (SUMAI = 1). Net/summary codes (e.g. A3K "work or education", which combines A3A and A3B), "other" and "don't know" are not counted, to avoid double counting. | Count |

### Independent variables (three blocks)
| Block | Variables | Treatment |
|---|---|---|
| A. Demographics | Age (S3), gender (S4), education (E5), working status (S9), socio-economic group (SEG), household income (E9), nation/region (REGION1), urban/rural (URBAN) | Categorical (dummy-coded). Small categories will be merged. Income "don't know / prefer not to say" (E9 = 8) is kept as a separate category. |
| B. Digital habits | Number of online activities (0–13, summed from Q13A–Q13M), number of devices used (Q10), time spent online per week (S1), online confidence (Q12A–Q12D), number of social media platforms used (Q16), trying new websites/apps (A7) | Own 0–13 activity count used instead of BREADTH, because BREADTH groups 9–13 activities into one band and hides clear differences. "Don't know" answers kept as a separate category where needed. |
| C. AI attitudes and experience | Trust in AI-written news (A4), confidence in recognising AI content (A5), AI awareness and use (SUMAI, Y1 models only) | Respondents who have never heard of AI were not asked A4/A5; they get a separate category "unaware of AI / not asked", distinct from "don't know". SUMAI is not used in Y2 models because all Y2 respondents are AI users. |

Note: SUMAI and Y1 may influence each other (using AI may lead to reading AI summaries, and vice versa), so its association with Y1 cannot be interpreted causally.

### Control variable
Survey mode (P_METHOD: online vs face-to-face CAPI) is included in all models. In the data, 43% of face-to-face respondents never read or have never seen AI summaries, compared with 19% of online respondents. Face-to-face respondents may also differ from online respondents in other characteristics, so without this control the models could wrongly attribute mode differences to other variables.

## 5. Hypotheses
These hypotheses were written before running any models. Some were formed after seeing
descriptive patterns during the data checks (e.g. cross-tabulations of A6 by survey mode,
income and AI awareness).

| ID | Hypothesis | Direction | Reasoning |
|---|---|---|---|
| H1 (RQ1) | People who do more types of online activity read AI summaries more often. | + | The more people use the internet, the more often AI summaries appear automatically in their searches, so they are more likely to notice and read them. |
| H2 (RQ1) | Trust in AI-written content (A4) is only weakly associated with how often people read AI summaries. | ~0 (weak) | AI summaries appear automatically, so both trusting and distrusting users may read them often. The difference may be in how they read them: distrustful users may read more critically, but this cannot be measured with this data. |
| H3 (RQ2) | Among AI users, younger people (especially those aged 16–24) use AI for a wider range of purposes. | − (age) | Younger people have grown up with digital technology and are more likely to try AI for study, work and everyday tasks. |
| H4 (RQ4) | Adding digital habits improves prediction more than adding attitudes towards AI. | Digital habits > AI attitudes | Following H1 and H2: digital habits are expected to be strongly associated with AI use, while trust is expected to be only weakly associated. |
| H5 (RQ3) | Machine-learning models predict both outcomes more accurately than traditional statistical models. | ML > regression | Online behaviour is complex, and factors may combine with each other (for example, age may matter more for people who use the internet less), which flexible models can capture better. |
| H6 (RQ1) | The association between age and reading AI summaries becomes clearly weaker once digital habits are added to the model. | Age effect shrinks after controlling for digital habits | Older people may read AI summaries less mainly because they go online less and do fewer things online, not because of age itself. If the age effect remains strong after adding digital habits, other reasons (e.g. distrust of new technology) may also play a role. |

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
