# 项目日志 / Research Log

---

## 2026-10-07
- **What I did:** After the Business Analytics for AI lecture, I talked to the lecturer
  about my project idea (AI use, applying the regression and clustering methods from the
  course). He agreed to discuss it further in office hours. I decided to use the public
  Ofcom Adults' Media Literacy Tracker 2025 data.
- **Problems I ran into:** At first I wanted to study "AI reliance", but question A6 only
  asks how often people read AI summaries, and reading a summary is not the same as
  relying on it.
- **How I solved it:** Narrowed the research focus to how often people read AI summaries
  (Y1) and the breadth of AI use (Y2), and kept "reliance" only as a background concept.
- **Limitations found:** The data is cross-sectional, so I can only talk about
  associations and prediction, not causation.
- **Next step:** Download the data and the codebook, and check whether all the
  variables I need are available.

## 2026-10-17 (time: ~3 h)
- **What I did:** Learned how to read the raw Ofcom data and match it to the codebook.
  1. Opened the CSV in a spreadsheet program (WPS). Row 1 holds the variable names
     (question codes, e.g. A6, E9); each row below is one respondent (7,533 in total);
     each column is one question; each cell is a numeric answer code, not the answer text.
  2. Used the Name Box to jump to a column: A6 is in column LC, E9 in column UR.
  3. Opened the codebook (one sheet, two parts):
     - Top part, "Variable Information": searched (Cmd+F) for "A6" to find the question
       wording in the Label column (around row 317).
     - Bottom part, "Variable Values" (from around row 571): searched "A6" again to find
       what each code means (around row 1302), e.g. A6 = 5 means "never seen these AI
       summaries / don't know what these are".
  4. Practised on two variables: A6 (how often people read AI search summaries) and
     E9 (household income band before tax).
  5. To compare columns that are far apart in the CSV (MODULEA in column C, INTUSED in
     column AB, A6 in column LC), I copied the three whole columns into a new worksheet
     side by side (A = MODULEA, B = INTUSED, C = A6). This let me read each respondent's
     values in one row without scrolling.
  6. Many respondents have no A6 answer. By comparing MODULEA, INTUSED and A6 side by side,
     I found that A6 is only answered when MODULEA = 1 (the respondent was assigned
     Module 1, which contains all the AI questions A1–A11) AND INTUSED = 1 (the respondent
     uses the internet). Otherwise the question was skipped by design (routing missing),
     not left unanswered. Using COUNTIF/COUNTIFS: Module 1 = 3,768; Module 1 and internet
     user = 3,515 (out of 7,533). So the A6 analysis sample is N = 3,515, not 7,533.
  7. Compared the two survey modes. In the codebook, P_METHOD is 1 = CATI (telephone),
     2 = Web (online), 3 = CAPI (face-to-face interview); only 2 and 3 appear in the data.
     I built a cross-tabulation of survey mode × A6 answer (2 rows × 5 columns) with
     COUNTIFS, checked that the total was 3,515, and converted the counts into row
     percentages:
     - Online (n = 2,729): "never read" + "never seen" = 19.1%
     - Face-to-face (n = 786): "never read" + "never seen" = 43.3%
     - "Always read": 19.3% online vs 9.7% face-to-face
  8. Noticed that the filters build on each other. The cross-tab COUNTIFS only used two
     conditions (P_METHOD and A6), yet the total was exactly 3,515. This is because the
     condition "A6 = 1–5" already excludes respondents who were not in Module 1 or do not
     use the internet (their A6 cell is a space), so it implicitly includes
     MODULEA = 1 and INTUSED = 1.

- **Spreadsheet functions and skills I learned:**
  - `=LEN(cell)`: counts the characters in a cell. Used it to prove that "empty" cells
    contain one space.
  - `=COUNTIF(range, value)`: counts how many cells in a range equal a value,
    e.g. `=COUNTIF(A2:A7534,1)` → 3,768.
  - `=COUNTIFS(range1, value1, range2, value2)`: counts rows that meet several conditions
    at the same time; conditions come in pairs ("which column, what value"),
    e.g. `=COUNTIFS(A2:A7534,1,B2:B7534,1)` → 3,515.
  - The "value" part can point to a cell, e.g. `=COUNTIFS($D:$D,$F2,$C:$C,G$1)` reads the
    values in F2 and G1, so one formula can be copied to build a whole cross-tab.
  - When a formula is copied, cell references move with it: copying right changes only
    the letter (column); copying down changes only the number (row).
  - `$` locks whatever comes right after it: `$F2` locks column F, `G$1` locks row 1,
    `$D:$D` locks the whole column. Rule of thumb: when copying right, check whether the
    letter has a `$`; when copying down, check whether the number has a `$`.
  - `=SUM(start:end)`: adds up a range, e.g. `=SUM(G2:K2)`; `G2:K3` means the whole block
    from the top-left to the bottom-right cell.
  - Division uses `/`, e.g. `=G2/$L2` gives the share within each group; then
    Format Cells → Percentage to display it as %.
  - `D:D` means the whole column D.

- **Problems I ran into:**
  1. At first I did not know how the data file and the codebook relate to each other,
     or what the rows, columns and numbers meant.
  2. Cells that look empty are not empty. In a cell outside the data area, =LEN(LC2)
     returned 1, so the "blank" cell contains one space character. The software also
     warned: "There are empty strings in the content! May cause calculation errors."
     (My first attempt returned 0 because a copied formula had shifted to the wrong cell.)
  3. Some codes are not on the same scale as the others: A6 = 5 ("never seen") is not
     "less often than never read" (4), and E9 = 8 ("don't know / prefer not to say") is
     not "higher than income band 7". Many respondents in the first rows had E9 = 8.
  4. Online and face-to-face respondents answered A6 very differently (see item 7).
  5. Spreadsheet mistakes along the way:
     - Copy-pasting a formula in WPS did not work at first: I had selected the whole
       block before copying, so I pasted empty cells. Fixed by copying only one cell,
       then selecting the target range and pasting.
     - My first SUM of the cross-tab gave 3,178, which was 337 short: the range missed
       column K (198 + 139). Changing it to G2:K3 gave 3,515.
     - My first percentage formula was `G$2/$L2`. Locking the row of the numerator meant
       it could not be copied down, so I had to write each row separately. Later I
       understood that one formula, `=G2/$L2`, is enough.

- **How I solved it / not yet solved:**
  - Did not click "Clear empty strings". Rule: never edit the raw data. Plan to treat
    spaces as missing values when loading the data in Python (not done yet).
  - Decided to treat A6 = 5 as a separate group rather than dropping it or merging it
    with "never read", so that no information is lost.
  - Two possible reasons for the survey-mode difference: (a) the two modes reach
    different people (e.g. face-to-face respondents may be older or less active online);
    (b) a mode effect, where the same person may answer differently when shown a printed
    example by an interviewer rather than seeing it on screen. This data cannot fully
    separate the two, so P_METHOD will be included as a control variable in the models.
  - How to handle E9 = 8: not decided yet.

- **Next step:**
  - Task 4: count how many respondents answered E9 = 8 ("don't know / prefer not to say")
    and decide how to handle it.
  - Possibly build a survey mode × age (S3) cross-tab with the same method to check
    explanation (a).

## 2026-10-18 (time: ~3 h)
- **What I did:**
  1. **Task 4 – income "don't know / prefer not to say" (E9 = 8).**
     - Copied column UR (E9) into my practice sheet (column P).
     - Counted E9 = 8 with COUNTIFS (three conditions: MODULEA = 1, INTUSED = 1, E9 = 8):
       727 of the 3,515 respondents in the A6 sample (20.7%).
     - Built a 2 × 5 cross-tab of income DK vs other × A6. For the "other" row I used
       =G2+G3-G10 (all respondents minus the DK group) instead of writing a new COUNTIFS.
       Total check: 727 + 2,788 = 3,515.
     - "Never read" + "never seen": 37.4% in the income-DK group vs 21.2% among others.
  2. **Task 5 – routing missing in A4 / A5.**
     - Looked up A1, A2, SUMAI, A4 and A5 in the codebook. SUMAI is not a question but a
       summary variable built from A1 and A2: 1 = aware and ever used AI, 2 = aware but
       never used, 3 = unaware of AI.
     - Copied SUMAI, A4 and A5 into my practice sheet and counted with COUNTIFS, using
       ">=1" to mean "has an answer". In the A6 sample: SUMAI = 3: 241; A4 answered: 3,274;
       241 + 3,274 = 3,515. Among SUMAI = 3, nobody answered A4 or A5.
     - So A4 and A5 are skipped for respondents who have never heard of AI (routing
       missing). This is different from A4 = 4 ("don't know"), which means the person was
       asked, is aware of AI, but is unsure.
     - SUMAI × A6: "never read" + "never seen" = 9.6% (used AI),
       43.1% (aware, never used), 58.9% (unaware). Yet about 41% of the unaware group
       still read AI summaries at least sometimes. Many people read AI summaries without
       recognising them as AI, which fits the "passive exposure" idea in the plan.
  3. **Task 6 – the coarse BREADTH variable.**
     - In the codebook, BREADTH groups internet use into 4 bands based on 13 online
       activities (Q13A–Q13M, each 0 = no, 1 = yes): 1 = none, 2 = narrow (1–4),
       3 = medium (5–8), 4 = broad (9–13).
     - Copied Q13A–Q13M (columns DP–EB, 13 columns) and BREADTH (EE) into my practice
       sheet. Did not include Q13N ("any of these") and Q13O ("none of these"), which are
       summaries, not activities.
     - Built my own activity count (0–13) with =SUM(AD2:AP2) and filled it down all rows
       by double-clicking the fill handle.
     - Within the A6 sample, compared two groups that BREADTH both codes as "broad":
       9 activities: 57 of 371 never read / never seen (15.4%);
       13 activities: 7 of 136 (5.1%) – about three times lower.
     - Across the full 0–13 range, the share of non-readers falls
       fairly steadily from 83% (0 activities) to 5% (13 activities).

- **New spreadsheet skills:**
  - Text conditions in COUNTIFS: ">=1" to count cells that have an answer, ">=4" to
    count A6 = 4 and 5 together.
  - Using existing totals to get a complement row (=G2+G3-G10) instead of a new formula.
  - Selecting several adjacent columns with Shift + click, and filling a formula down
    thousands of rows by double-clicking the fill handle.
  - Searching for a variable name with Cmd+F and "match entire cell contents", so that
    searching "A4" does not also find A4A, QA4, etc.

- **Problems I ran into:**
1. My first COUNTIFS with ">=1" failed because I used single quotes ('>=1'). Text
     conditions need straight double quotes (">=1"), typed with an English keyboard.

- **Decisions:**
  - Income DK and AI-unaware respondents are not random subsets: both groups are much
    less likely to read AI summaries. Dropping them would shrink the sample and make the
    remaining respondents look more engaged than they really are (biased results).
  - Keep E9 = 8 as a separate income category (like A6 = 5), instead of dropping it or
    treating 8 as "higher than band 7".
  - Do not drop the 241 AI-unaware respondents. Give A4 and A5 an extra category
    "unaware of AI / not asked", separate from A4 = 4 "don't know".
  - Use my own 0–13 activity count instead of BREADTH, because BREADTH puts people with
    9 and 13 activities in the same band and hides a clear difference.

- **Next step:**
  - All six data checks are done.
  - Continue PLAN.md section 2 (research questions).

## 2026-10-20 (time: ~4 h)
- **What I did:** Completed PLAN.md v1 (sections 2–8).
  1. **Research questions**
     - RQ1: Which factors are most strongly associated with how often UK adults read the
       AI-generated summaries that appear automatically in search results?
     - RQ2: Among AI users, which factors are most strongly associated with the breadth of
       AI use (number of purposes)?
     - RQ3: Do machine-learning models predict more accurately out of sample than
       traditional statistical models?
     - RQ4: Beyond demographics, how much additional predictive power do digital habits and
       attitudes towards AI each provide?
  2. **Data and sample:** Y1 sample N = 3,515; after excluding the 337 respondents who have
     never seen AI summaries, N = 3,178. Y2 sample: 2,061 AI users. Weights are used for
     descriptive statistics; models are unweighted, with a weighted run as a robustness check.
  3. **Variables:** Y1 reverse-coded as 0 = never to 3 = always (ordinal); Y2 = number of
     the 8 purposes in A3A–A3H (count). Predictors grouped into demographics, digital habits
     and AI attitudes; survey mode (P_METHOD) as a control variable.
  4. **Hypotheses**
     - H1: People who do more types of online activity read AI summaries more often.
     - H2: Trust in AI is only weakly associated with how often people read AI summaries.
     - H3: Among AI users, younger people (especially 16–24) use AI for more purposes.
     - H4: Adding digital habits improves prediction more than adding attitudes towards AI.
     - H5: Machine-learning models predict more accurately than traditional statistical models.
     - H6: The association between age and reading AI summaries becomes clearly weaker once
       digital habits are added.
  5. **Methods, evaluation, limitations:** Set out each model, its assumptions and how they
     will be checked, with model selection rules fixed before seeing results. Evaluation:
     80/20 split + 5-fold cross-validation; QWK for Y1, MAE for Y2, compared with baselines.
     Listed seven expected limitations.

- **Concepts I learned:**
  - The variable type decides the model: ordinal → ordered logit; count → Poisson / binomial.
  - Proportional odds (parallel lines) assumption, dispersion, overfitting, regularisation,
    train/test split, cross-validation, random seeds.
  - How k-means works, why variables need standardising, and how to choose k.
  - Why accuracy alone is not enough; data leakage.
  - Control variables and reverse causality.

- **Problems I ran into:**
  1. For non-AI users, A3 is coded 0 rather than left blank, so the sample must be filtered
     on SUMAI = 1 before building Y2.
  2. The assumption that face-to-face respondents are older did not hold when checked
     against the data (75+: 8% vs 5%), so I removed "age" from that explanation in
     section 4. Lesson: every claim should be checked against the data.
  3. The figure in section 8 (about 41% of respondents who have never heard of AI still read
     AI summaries) still needs to be reproduced in Python.

- **Next step:**
  - Learn pandas in Google Colab: load the CSV and redo the Excel checks with
    `value_counts()` and `pd.crosstab()`, including reproducing the 41% figure.
  - Then start data cleaning: select the samples and handle missing values and special codes
    (A6 = 5, E9 = 8, and respondents not asked A4/A5) as set out in the plan.

## 2026-10-27 (time: ~? h)
- **What I did:**
  1. **Loaded the data in Python.** Uploaded the CSV to Google Drive and opened it in
     Google Colab with pandas (notebook `01_load_data.ipynb`): 7,533 rows × 566 columns.
  2. **Handled blank cells.** Blank cells are stored as a space (" "), so `value_counts()`
     counted 4,018 "space" answers in A6. Re-read the file with `na_values=' '` so they are
     treated as missing (NaN).
  3. **Reproduced the Excel checks in Python:**
     - A6: 3,515 valid answers + 4,018 missing = 7,533.
     - MODULEA = 1: 3,768. A6 sample (MODULEA = 1 and INTUSED = 1): N = 3,515, with no
       missing A6 values.
  4. **Reproduced the cross-tabs with `pd.crosstab()`** (row percentages):
     - Survey mode × A6, "never read" + "never seen": 19.1% online vs 43.3% face-to-face.
     - SUMAI × A6, read at least sometimes (A6 = 1–3): 90.4% (used AI) vs 41.1% (unaware
       of AI).
     - Income DK (E9 = 8) × A6, "never read" + "never seen": 37.4% (DK) vs 21.2% (others).
     - All figures match the Excel results.

- **Concepts I learned:**
  - CSV, DataFrame (a table) and Series (one column); `df.shape`, `df.head()`.
  - `=` stores a value; `==` asks "is it equal?" and returns True/False.
  - `value_counts()`, `isna().sum()`, chaining methods with `.`.
  - Filtering rows with conditions: `df[(cond1) & (cond2)]`; brackets are required.
  - `pd.crosstab()` with `normalize='index'` gives row percentages.
  - `table[col]` selects a column; `table.loc[row]` selects a row. Adding two columns
    matches values by row label.
  - Using a condition (e.g. `E9 == 8`) directly as the rows of a cross-tab.

- **Problems I ran into:**
  1. Values in A6 now show as 1.0, 2.0 instead of 1, 2. A column containing NaN is stored
     as decimals (float); the values themselves are unchanged.
  2. Adding two rounded percentages gave 0.192 instead of 0.191. Rounding should be done
     after adding, not before.

- **Next step:**
  - Start data cleaning: recode A6 = 5, E9 = 8 and the A4/A5 "unaware /

## 2026-10-30 (time: 2 h, started on the evening of 27 Oct)
- **What I did:**
  1. Started data cleaning in a new notebook `02_cleaning.ipynb`. Re-loaded the data and
     filtered the A6 sample (MODULEA = 1 and INTUSED = 1, N = 3,515) using `.copy()`, so
     the original data stays unchanged.
  2. **Y1:** reverse-coded A6 into 0 = never read, 1 = sometimes, 2 = often, 3 = always.
     A6 = 5 ("never seen") was set to missing, so the Y1 sample is N = 3,178.
  3. **seen_summary:** 1 = has seen AI summaries (3,178), 0 = never seen (337).
  4. **income_dk:** 1 = income "don't know / prefer not to say" (E9 = 8, n = 727),
     0 = others (2,788).
  5. **A4_c / A5_c:** respondents unaware of AI (SUMAI = 3) were not asked A4 or A5.
     Filled these blanks with 0 = "unaware of AI / not asked". Checked with cross-tabs
     that 0 appears only for SUMAI = 3 (n = 241). The original A4/A5 columns are kept.
  6. Saved the cleaned sample as `sample_clean_v1.csv` (3,515 × 571) and read it back to
     check the shape.

- **Concepts I learned:**
  - Variables, `numpy` / `np.nan`, and why `.copy()` is needed before adding columns.
  - `.map()` with a dictionary: values not in the dictionary become NaN.
  - `!=`, `.astype(int)` to turn True/False into 1/0.
  - `.fillna()` fills only missing cells; `dropna=False` shows NaN in `value_counts()`.
  - `.sort_index()` sorts by row labels, so it belongs after `value_counts()`.
  - `.to_csv(..., index=False)` saves a table without the row labels.

- **Problems I ran into:**
  1. My first version of A4_c only flagged SUMAI = 3 (0/1), which lost the original A4
     answers. Fixed by using `A4.fillna(0)` so the answers 1–4 are kept.
  2. Adding `.sort_index()` when creating a column had no effect, because it sorts by
     respondent row number. It is only useful on the `value_counts()` output.

- **Next step:**
  - Data cleaning part 2: build Y2 (number of AI uses, A3A–A3H, AI users only), the
    0–13 online activity count (Q13A–Q13M), and select the three groups of predictors.

## 2026-10-31 (time: 3 h)
- **What I did:**
  1. Continued `02_cleaning.ipynb`.
  2. **Y2:** summed A3A–A3H (eight AI uses) for each respondent. Non-users (SUMAI = 2 or 3,
     n = 1,455) are coded 0 in A3 rather than blank, so I set their Y2 to missing.
     Y2 is defined for 2,060 AI users: 0 (36), 1 (555), 2 (484), 3 (395), 4 (249),
     5 (163), 6 (108), 7 (41), 8 (29).
  3. The 36 AI users with Y2 = 0 selected only "other" (A3I, n = 19) or "don't know"
     (A3J, n = 17). Kept them with Y2 = 0, since Y2 counts the eight listed uses only.
  4. **activity_count:** summed Q13A–Q13M (0–13, no missing). A cross-tab with BREADTH
     matches its bands exactly (0 / 1–4 / 5–8 / 9–13), confirming the calculation.
     BREADTH's top band combines 1,239 respondents with 9–13 activities.
  5. Saved `sample_clean_v2.csv`.

- **Concepts I learned:**
  - Selecting several columns with a list: `df[list_of_columns]`.
  - `.sum(axis=1)` adds across columns (one value per respondent); `axis=0` adds down
    each column.
  - `|` means "or", `&` means "and"; `!= 1` and `.isin([2, 3])` give the same result here.
  - `.loc[rows, column] = value`: rows can be selected with a True/False condition.
  - `value_counts()` summarises a column; it does not change the column itself.

- **Problems I ran into:**
  1. Using `&` for "SUMAI = 2 and SUMAI = 3" returned no one, because one person cannot
     have both values. Changed to `|`.
  2. Stored a condition with `.value_counts()` on the end, which turns it into a count
     table instead of one True/False per person, so it cannot be used to select rows.
  3. Wrote `.loc(...)` with round brackets; `.loc` needs square brackets.

- **Next step:**
  - Review the three predictor blocks in PLAN section 4 (demographics, digital habits,
    AI attitudes): check codes such as "don't know", merge small categories, and save
    the final cleaned dataset.
