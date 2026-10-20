# Project Plan

> Version: v1 (20 October 2026)

---

## 1. Research Background
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

## 6. Methods: models, assumptions and checks
| Model | Used for | Key assumption | How it will be checked / what if it fails |
|---|---|---|---|
| Ordered logit | Y1, explanation | Proportional odds (parallel lines): each predictor has the same effect at every step (never → sometimes → often → always) | Brant test. If a few predictors violate it, use a partial proportional odds model that relaxes the assumption for those predictors. Simpler fallback: binary logistic regression (reads at least sometimes vs never). |
| Poisson GLM | Y2, explanation | Mean = variance | Dispersion statistic (Pearson χ²/df). If clearly > 1 (overdispersion), use negative binomial; if clearly < 1 (underdispersion), use binomial GLM. |
| Binomial GLM | Y2, explanation | Y2 can be treated as the number of "yes" answers out of 8 possible purposes, which respects the upper limit of 8; the 8 purposes are assumed independent with the same probability for each person | The 8 purposes are likely to be related (e.g. work and writing). Check dispersion; if violated, use quasi-binomial to give more conservative standard errors. |
| Regularised logistic regression | Y1, prediction (statistical benchmark for RQ3) | Effects are linear on the log-odds scale and additive; no interactions unless added | Regularisation (L2 penalty) reduces overfitting with many dummy variables. Penalty strength tuned by cross-validation. |
| Random forest / XGBoost | Y1 and Y2, prediction | No linearity assumption, but prone to overfitting | Train/test split (80/20, stratified for Y1); 5-fold cross-validation on the training set only to tune parameters such as tree depth and number of trees; final performance reported once on the test set. |
| Permutation importance / SHAP | Interpreting ML models | Importance is shared between correlated predictors | Interpret together with the regression results; group related variables where needed. |
| k-means | Segmenting users by digital habits | Variables on comparable scales; clusters roughly spherical and of similar size | Standardise variables. Choose k using the elbow method, silhouette scores and whether clusters can be clearly interpreted. Report low silhouette scores honestly. |
| Three-stage comparison (RQ4) | Contribution of each predictor block | Blocks may overlap, so the block added first may take credit for shared information | Run both orders (demographics → digital habits → AI attitudes, and demographics → AI attitudes → digital habits) and compare the gains in test performance. |

### Model selection rules (decided before seeing results)
- **Y2: Poisson vs binomial vs negative binomial.** Fit Poisson first and check the
  dispersion statistic. If it is clearly below 1, use binomial; if clearly above 1,
  use negative binomial. Compare candidate models with AIC (lower is better). The
  binomial model is also preferred in principle because Y2 cannot exceed 8.
- **Random forest vs XGBoost.** Both will be trained and compared on the same
  cross-validation metric. If one is clearly better, it will be used as the main ML
  model; if they perform similarly, the simpler model (random forest) will be preferred
  and both results reported.
- **Permutation importance and SHAP.** These are complementary rather than alternatives:
  permutation importance gives an overall ranking of predictors; SHAP shows the direction
  of each predictor's contribution and how it varies across individuals. Permutation
  importance will be done first; SHAP if time allows.

## 7. Evaluation
- **Data split:** 80% training / 20% test, stratified by Y1 category so that both sets
  have the same class proportions. Fixed random seed (42) for reproducibility. The test
  set is used only once, for the final comparison.
- **Cross-validation:** 5-fold cross-validation on the training set only, used to tune
  model parameters (e.g. regularisation strength, tree depth).
- **Y1 metrics:** Accuracy alone is not enough. First, the largest category ("sometimes",
  about 36% of the Y1 sample) gives a high accuracy even for a model that always predicts
  it. Second, Y1 is ordinal, so predicting "often" when the answer is "always" is a smaller
  error than predicting "never", but accuracy treats them the same. The main metric will
  therefore be quadratic weighted kappa (QWK), which penalises larger ordinal errors more
  and corrects for chance agreement. Accuracy and macro F1 will be reported as secondary
  metrics.
- **Y2 metrics:** Mean absolute error (MAE), interpreted as the average number of purposes
  by which the prediction is wrong. RMSE as a secondary metric.
- **Baselines:** Y1: always predict the most frequent category. Y2: always predict the
  training-set mean. A model is only useful if it clearly beats these baselines.
- **Preventing leakage:** Split the data before any preprocessing; fit scaling and
  encoding on the training set only, inside a scikit-learn pipeline; tune parameters with
  cross-validation, never on the test set; exclude any variable used to construct the
  outcome (e.g. the A3 items when predicting Y2).

## 8. Expected Limitations
1. **Association, not causation.** The data is cross-sectional, so the analysis can only
   show associations and predictions. Some predictors may also be influenced by the
   outcome (e.g. using AI and reading AI summaries may reinforce each other).
2. **Use, not reliance.** A6 measures how often people read AI summaries, not whether they
   trust them or act on them, so the project measures AI use rather than reliance on AI.
3. **Self-reported data.** Respondents may not recognise AI summaries as AI-generated or
   may misremember how often they read them. For example, about 41% of respondents who had
   never heard of AI still reported reading AI summaries at least sometimes.
4. **Survey mode effects.** Online and face-to-face respondents answer A6 very differently
   (19% vs 43% never read or never seen). Controlling for P_METHOD reduces but cannot fully
   separate differences in who was interviewed from differences caused by the interview
   mode itself.
5. **Smaller samples and limited generalisability.** Because of the modular design, Y1 is
   based on 3,515 respondents (3,178 in the ordinal model), and Y2 only on 2,061 AI users,
   so Y2 findings cannot be generalised to non-users.
6. **Coarse measures.** Age and income are only available in bands, Y1 has four levels, and
   Y2 counts how many purposes AI is used for, not how often or how intensively.
7. **A snapshot of a fast-changing technology.** The data reflects 2025; patterns of AI use
   may change quickly as the technology and its availability develop.

## 9. Deliverables and Timeline
| Deliverable | Target date |
|---|---|
| Plan v1 (sections 1–8) | 20 Oct |
| Python basics + data cleaning | 1 Nov |
| EDA | 7 Nov |
| Regression | 15 Nov |
| Machine learning + three-stage comparison | 29 Nov |
| Clustering | 5 Dec |
| Report | 9 Dec |
| Demo | 13 Dec |

## References

- Bick, A., Blandin, A. and Deming, D. J. (2024) *The Rapid Adoption of Generative AI*. NBER Working Paper No. 32966. Cambridge, MA: National Bureau of Economic Research.
- Humlum, A. and Vestergaard, E. (2025) 'The unequal adoption of ChatGPT exacerbates existing inequalities among workers', *Proceedings of the National Academy of Sciences*, 122(1), e2414972121. https://doi.org/10.1073/pnas.2414972121
- Kelly, S., Kaye, S. A. and Oviedo-Trespalacios, O. (2023) 'What factors contribute to the acceptance of artificial intelligence? A systematic review', *Telematics and Informatics*, 77, 101925. https://doi.org/10.1016/j.tele.2022.101925
