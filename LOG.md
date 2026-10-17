# 项目日志 / Research Log

> 每次做完写 5 分钟。写真实发生的事，包括卡住、走错路和改主意，这些正是日志最有价值的部分。
> 面试被问"你遇到过什么困难"时，答案就在这里。

## 模板（复制使用）
```
## YYYY-MM-DD（用时：x 小时）
- 做了什么：
- 遇到的问题：
- 怎么解决的 / 还没解决：
- 发现的局限：
- 想到的优化：
- 下一步：
```

---

<!-- ⚠️ 下面两条是根据前两天实际发生的事整理的草稿，请用你自己的话重写，删掉不符合的部分 -->

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
  - Update the 10/8 entry: the two points I have now verified myself (spaces in blank
    cells, N = 3,515) should be marked as "verified myself on 10/17".

## 2026-10-07
- 做了什么：上完 Business Analytics for AI 课后和老师聊了项目想法（AI 使用 + 课上的回归、聚类），老师同意以后 office hour 讨论。确定用 Ofcom Adults' Media Literacy Tracker 2025 的公开数据，替换原计划的 Olist 电商项目。
- 遇到的问题：最初想研究"AI reliance（依赖）"，但问卷里的 A6 只问"多常读 AI 摘要"，读了不等于依赖。
- 怎么解决的：把研究对象收窄为"AI 摘要阅读频率（Y1）"和"AI 使用广度（Y2）"，reliance 只作为背景概念。
- 发现的局限：横截面数据，只能说相关和预测，不能说因果。
- 下一步：下载数据和 codebook，确认变量是否齐全。

## 2026-10-08
- 做了什么：下载 CSV（7,533 × 566）和 SPSS codebook，做数据审计。
- 遇到的问题：
  1. 空值存成了空格 `' '`，直接读入时整列变成文字，需要加 `na_values=' '`。
  2. AI 题只在 Module 1（3,768 人），A6 有效 N = 3,515，A3 有效 N = 2,061，不能说用了 7,533 人。
  3. face-to-face 和 online 受访者的 A6 分布差别很大（"从不 / 没见过"占比：44% vs 19%）。
  4. 收入有 21% 选了"不知道 / 不愿说"。
- 怎么解决的：调查方式 P_METHOD 作为控制变量；收入的"不知道"保留为单独一类；没听说过 AI 的人在 A4/A5 上单独设一类（routing missing，不是普通缺失）。
- 发现的局限：现成的 BREADTH 变量只有 4 档太粗，改用 Q13A–M 自己加总（0–13）。
- 其他：看过一次 Claude 跑的参考运行，用来确认项目可行、提前看到终点；正式分析由我自己从头做。
- 下一步：写项目规划书，建 GitHub 仓库。
