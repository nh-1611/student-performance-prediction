# Nonparametric student performance dependency analysis and predictive modeling using Machine Learning

Polish version: [README_PL.md](README_PL.md)

*Student Performance Factors*: https://www.kaggle.com/datasets/lainguyn123/student-performance-factors/data

<img width="1320" height="737" alt="image" src="https://github.com/user-attachments/assets/a138c8fa-3b07-4403-aaab-b22feed15e96" />
*The number of surveyed students on the dashboard (6,378) is lower than the total number of observations in the analysis (6,607) because missing data imputation was not applied to the dashboard dataset; instead, records with missing values were removed.*

## About the Project

This project focuses on a non-parametric and econometric analysis of the relationship between academic performance and potential influencing factors, alongside building a machine learning model to predict exam scores. The study was conducted on a sample of 6,607 observations. The analysis combines comprehensive data cleaning, exploratory data analysis of descriptive statistics, testing for normality and homogeneity of variance assumptions required for ANOVA, non-parametric Mann-Whitney U and Kruskal-Wallis tests with post-hoc testing, correlation analysis, and the construction and evaluation of predictive models.

---

## At a Glance

**Objective:** Identification of variables influencing exam scores and selection of the best machine learning model.

**Data:** Public Kaggle dataset, 6,607 observations. 235 missing values imputed using the MICE method.

**Approach:** Data preparation and cleaning → Exploratory data analysis → Assumption testing → Relationship analysis → Predictive modeling and evaluation → Power BI dashboard.

**Model:** Linear Regression (chosen among Linear Regression, Random Forest, and Gradient Boosting).

| Metric | Value |
| :---: | :---: |
| R^2 | 0.6952 |
| RMSE | 2.2804 |
| MAE | 0.5218 |

**Key Findings:**

- Gender, sleep hours, and school type do not affect academic performance.
- Relationships were detected between exam scores and motivation level, internet access, access to educational resources, and parental income.
- The presence of outlier scores negatively impacts model quality.
- Strongly linear nature of the sample.

**Repository Contents:**

- `student_performance.ipynb`: Data cleaning and preparation, exploratory data analysis, assumption verification, statistical tests, modeling, interpretation.
- `student_performance_dashboard_pl.pbix`: Analytical Power BI dashboard.
- `requirements.txt`: Required libraries.

**How to run:** Run `pip install -r requirements.txt`, then open the notebook: `student_performance.ipynb`.

---

## Variables

6,607 observations

The dataset contains information on attendance, parental involvement, student study habits, and other aspects potentially affecting school performance.

- `Hours_Studied` – number of hours spent studying weekly,
- `Attendance` – class attendance (%),
- `Parental_Involvement` – level of parental involvement in the student's education,
- `Access_to_Resources` – availability of educational resources,
- `Extracurricular_Activities` – participation in extracurricular activities,
- `Sleep_Hours` – average number of sleep hours,
- `Previous_Scores` – results from previous exams,
- `Motivation_Level` – level of motivation,
- `Internet_Access` – internet availability,
- `Tutoring_Sessions` – number of tutoring sessions per month,
- `Family_Income` – family income level,
- `Teacher_Quality` – quality of teachers,
- `School_Type` – type of school,
- `Peer_Influence` – peer influence on education,
- `Physical_Activity` – average weekly hours spent on physical activity,
- `Learning_Disabilities` – learning disabilities,
- `Parental_Education_Level` – highest education level of parents,
- `Distance_from_Home` – distance from home,
- `Gender` – gender,
- `Exam_Score` – final exam score.

---

## Project Architecture

1. Data preparation and cleaning (Python)
2. Exploratory Data Analysis (EDA) (Python)
3. Verification of normality and homogeneity of variance assumptions (Python)
4. Relationship analysis using non-parametric tests (Python)
5. Data preparation for ML models (Python)
6. Training and evaluation of Linear Regression, Random Forest, and Gradient Boosting models (Python)
7. Feature importance interpretation (Python)
8. Building an analytical dashboard (Power BI)
9. Conclusions, recommendations, limitations, and future steps

---

## Tools Used

- **Data Handling**: `pandas`, `numpy`
- **Data Imputation**: `sklearn.experimental` (`enable_iterative_imputer`), `sklearn.impute` (`IterativeImputer`)
- **Data Visualization**: `seaborn`, `matplotlib`, `pylab`
- **Statistics**: `scikit_posthocs`, `scipy` (`stats`), `statsmodels`
- **Machine Learning**: `sklearn.ensemble` (`GradientBoostingRegressor`, `RandomForestRegressor`), `sklearn.linear_model` (`LinearRegression`), `sklearn.metrics` (`mean_absolute_error`, `mean_squared_error`, `r2_score`), `sklearn.model_selection` (`RandomizedSearchCV`, `train_test_split`)
- **Dashboard:** Power BI Desktop

---

## Step 1: Data Preparation and Cleaning

### Duplicates

The dataset was checked for duplicates. No duplicate records were found.

### Missing Values

A total of 235 missing values were detected (78 for `Teacher_Quality`, 90 for `Parental_Education_Level`, 67 for `Distance_from_Home`). To impute them, the MICE (*Multiple Imputation by Chained Equations*) method was applied.

### Outliers

Boxplot analysis revealed the presence of outliers for 3 continuous variables (`Hours_Studied`, `Tutoring_Sessions`, `Exam_Score`). In the case of `Exam_Score`, many high exam scores were recorded as extreme, likely resulting from the dataset's characteristics. These observations were retained for further analysis.

---

## Step 2: Exploratory Data Analysis (EDA)

**Numerical Variables:**
> * **Final exam scores** among surveyed students range from 55 to 101 points. **The average student scores 67 points.** Most examinees **receive between 65 and 69 points**, and higher scores are achieved by only 25% of respondents.
> * **Students spend an average of 20 hours a week studying**, which equals nearly 3 hours a day.
> * **Attendance for the majority of respondents ranges from 70% to 90%**, with an average of 80%. The lowest is 60%.
> * Most students **sleep an average of 7 hours**, though extremely low cases (4 hours) and extremely high ones (10 hours) occur.
> * **The average score on previous exams was higher than on the final exam (75 vs 67).**
> * Most students attend **1 or 2 tutoring sessions per month**. A large number of respondents do not use tutoring at all.
> * Respondents spend an average of **3 hours a week on physical activity**.

**Categorical Variables:**
> * In the surveyed sample, half of the students report **moderate parental involvement in their studies (3,362, 50.9%).** High involvement occurs for 28.9% (1,908) of students, and low for only 20.2% (1,337).
> * **Half of the surveyed students have medium access to learning resources (3,319, 50.2%).** High access is declared by nearly 30% of all students (1,975), and **low access by nearly 20%. Low access to supporting study resources can be a problem and lead to lower grades; this observation cannot be ignored, especially since it affects as many as 1,313 students.**
> * Nearly **60% of all students attend extracurricular classes (3,938).**
> * **Only 20% of respondents declare a high level of motivation (1,319).** The student body is dominated by those with medium (3,351, 50.7%) and low (1,937, 29.3%) motivation levels. Attention should be paid to motivation levels and fostering self-discipline.
> * **Lack of internet access is not a major issue among the surveyed students.** Only 7.6% (499) report it. Nevertheless, its potential negative impact on school performance cannot be ignored, even among such a small group.
> * **Most students come from families with low (2,672, 40.4%) and medium (2,666, 40.4%) income.**
> * **11.1% of people (735) report a low level of teaching quality.** This signals a need for in-depth analysis and corrective action.
> * Among surveyed students, nearly 70% attend public schools (4,598). Private schools account for 30.4% (2,009).
> * **Peer influence on education is rated by most students as positive (2,638, 39.9%) or neutral (2,592, 39.2%)**, but a group rates it as negative (1,377, 20.8%), which is alarming.
> * **The group of students struggling with learning difficulties accounts for 10.5% of respondents (695).** These individuals require a tailored approach and pace of learning.
> * The largest group of students indicates that their parents have **secondary education (3,313, 50.1%).**
> * **10% of respondents describe the distance to home as large (658)**, which potentially negatively affects performance.
> * Among respondents, **males predominate (3,814, 57.7%).** Females account for 42.3% of the group (2,793).

---

## Step 3: Verification of Normality and Homogeneity of Variance Assumptions

### D'Agostino-Pearson Normality Test and Q-Q Plots

Due to the large sample size ($n = 6,607$), the D'Agostino-Pearson test based on skewness and kurtosis was used to verify normality, offering resilience to large sample sizes and high statistical power.

**The distribution is not close to normal**, primarily influenced by heavy tails (outliers) visible on Q-Q plots.

### Levene's Test for Homogeneity of Variance

The assumption of homogeneity of variance was met for all variables.

---

## Step 4: Relationship Analysis Using Non-Parametric Tests

The choice of non-parametric tests for relationship analysis was dictated by the failure to meet the normality assumption.

### Non-Parametric Mann-Whitney U Test

The **Mann-Whitney U test** for two groups showed significant differences for the variables `Extracurricular_Activities`, `Internet_Access`, and `Learning_Disabilities`. No statistical differences were shown for `Gender` and `School_Type`. The test results suggest an effect of extracurricular activities, internet access, and learning difficulties on exam scores, but median values oscillating around 67 for all groups and box-plot analysis indicate no major differences in score distributions across groups. With such a **large sample ($n = 6,607$), extreme sensitivity** to minor deviations must be kept in mind.

In the group characterized by **no learning difficulties**, **more outliers** can be observed compared to students with difficulties, suggesting that such individuals more frequently achieve higher scores. A similar situation occurs with **internet access**, where the **group declaring such access** also features a **higher number of extreme scores**. Nevertheless, central values remain similar.

### Non-Parametric Kruskal-Wallis Test and Dunn's Post-Hoc Test

The **Kruskal-Wallis test** showed significant differences between groups for all variables (`Teacher_Quality`, `Motivation_Level`, `Parental_Education_Level`, `Parental_Involvement`, `Access_to_Resources`, `Family_Income`, `Peer_Influence`, `Distance_from_Home`). As with the Mann-Whitney U test, the **large sample size ($n = 6,607$)** makes the test very sensitive. Median values for all groups are close to 67, and box-plots do not indicate major distribution differences. Still, it is worth analyzing the visible **outliers**.

> * The Kruskal-Wallis test shows statistical significance for the impact of **teaching quality (`Teacher_Quality`) on exam scores (`Exam_Score`)**. According to Dunn's *post-hoc* test with Bonferroni correction, all groups differ from each other. Box-plots show that distributions nevertheless remain similar. The main difference is a higher number of extreme scores in the `Medium` and `High` groups compared to the `Low` group. It can be inferred that **individuals in the lowest teaching quality group less frequently receive top scores compared to other groups**.
> * The test indicates that exam scores **vary by motivation level (`Motivation_Level`)**, and even that all groups differ significantly from one another. Nevertheless, analysis of medians and box-plots yields conclusions similar to teaching quality.
> * Similar conclusions come from in-depth analysis of tests and plots for `Parental_Education_Level`, `Parental_Involvement`, `Family_Income`, `Peer_Influence`.
> * Groups by resource access (`Access_to_Resources`) theoretically differ significantly regarding exam scores, but this proceeds analogously to previous variables. An interesting observation is the lower number of outliers for the `Low` group than for `Medium` and `High`, implying that **students with low resource access less frequently achieve extremely high grades than medium and high access groups.**
> * Regarding distance from home (`Distance_from_Home`), the most extreme values are observed for `Near` and `Moderate` groups, while the `Far` group has fewer. This suggests that **students living close to home more frequently receive top exam scores than those with a long distance.**

### Spearman's Correlation

**All analyzed significant relationships are positive.** Spearman's test indicated sleep hours (`Sleep_Hours`) as the only variable uncorrelated with exam score.

> * The highest correlation with exam scores is exhibited by attendance (`Attendance`, $\rho = 0.67$) and study hours (`Hours_Studied`, $\rho = 0.48$). These results are confirmed by relationship plots.
> * Moderate correlation with exam scores is shown by previous scores (`Previous_Scores`, $\rho = 0.19$) and the number of tutoring sessions (`Tutoring_Sessions`, $\rho = 0.16$).
> * Negligible correlation was observed between exam scores and physical activity (`Physical_Activity`, $\rho = 0.03$).

---

## Step 5: Data Preparation for ML Models

Before modeling, **insignificant variables** (`Sleep_Hours`, `Gender`, and `School_Type`) were excluded. Next, **categorical variables** intended for model construction were encoded. Data was **split into training and test sets** maintaining an 80/20 ratio.

---

## Step 6: Training and Evaluation of Linear Regression, Random Forest, and Gradient Boosting Models

| Metric | Linear Regression | Random Forest | Gradient Boosting |
| :--- | :---: | :---: | :---: |
| R^2 | 0.6952 | 0.5945 | 0.6565 |
| RMSE | 2.2804 | 2.6305 | 2.4209 |
| MAE | 0.5218 | 1.2119 | 0.8472 |

The best results across all criteria are achieved by the baseline model, i.e., **Linear Regression.** It achieves the **highest coefficient of determination** `R2` (0.6952 vs 0.5945 vs 0.6565) as well as the **lowest root mean squared error `RMSE`** (2.2804 vs 2.6305 vs 2.4209) and **lowest mean absolute error `MAE`** (0.5218 vs 1.2119 vs 0.8472).

> * The superiority of linear regression over decision tree-based models (Random Forest, Gradient Boosting) confirms that relationships between explanatory variables and exam scores are **strongly linear**. Complex non-linear algorithms tended to overfit on the training set here.
> * Visual plot analysis reveals a recurring pattern for all three models: **clear underestimation of scores in the top-right corner** (for students with extremely high scores above 80–90 points). This means none of the models fully captured the non-linear "ceiling" specific to top achievements.

Linear regression not only yields the lowest prediction errors but also offers **full coefficient interpretability**, making it an optimal business and research choice.

Additionally, despite hyperparameter tuning for the second-best model (Gradient Boosting), its **results remained inferior to linear regression** (`R2`: 0.6711 vs 0.6952, `RMSE`: 2.3690 vs 2.2804, `MAE`: 0.7625 vs 0.5218).

---

## Step 7: Feature Importance Interpretation

The greatest positive impact on exam scores comes from **positive peer influence on learning, internet access, and a short distance from home.**
> * Students who rate peer influence as positive achieve an average of 1.1 exam points more than those rating it as negative, *ceteris paribus*.
> * Individuals with internet access score an average of 0.9 points higher on exams than those without internet access, *ceteris paribus*.
> * Students with a short distance between school and home score an average of 0.9 points more than those who must travel a long distance, *ceteris paribus*.

The greatest negative impact on exam scores stems from **low access to educational resources, low parental involvement in studies, low-income family background, and low motivation.**
> * Individuals with low access to proper study materials score an average of 2 points lower than those with high resource access, *ceteris paribus*.
> * Students with low parental involvement achieve an average of 1.9 points less than those whose parents show high involvement, *ceteris paribus*.
> * Unmotivated students score an average of 1.1 points lower than motivated ones, *ceteris paribus*.

**The smallest impact on scores according to the linear regression model is held by previous scores, physical activity, and attendance**, despite its high correlation coefficient with the variable.

---

## Step 8: Conclusions, Recommendations, Limitations, and Future Steps

### Key Conclusions

> * **Higher academic performance** is largely driven by **internet access, positive peer influence, and short distance from home.** Access to educational resources and parental involvement also matter.
> * **Lower performance** is associated with **lack of motivation, low resource access, and lack of internet access.**
> * Gender, sleep hours, and school type do not affect academic performance.
> * Contrary to expectations favoring advanced algorithms, **classical linear regression achieved the best results**, confirming a strongly **linear nature** of relationships in the student sample.
> * With such a large sample ($n = 6,607$), **statistical tests showed extreme sensitivity**; statistically significant differences often did not translate into large differences in medians, which hovered around 67 points for most subgroups.

### Recommendations

> * **Equalizing opportunities and resources:** Educational institutions should prioritize supporting students with limited access to materials and internet, as these deficits correlate with the largest point drops (reaching 2 points).
> * **Supporting engagement and motivation:** Remedial and support programs should focus on activating parents and building student motivation for independent study.
> * **Monitoring the peer environment:** Attention should be paid to groups reporting negative peer influence and supporting students commuting long distances.

### Limitations

> * **Synthetic nature of data:** The analyzed dataset is synthetic, meaning generated relationships and data structures may diverge from the full complexity and natural noise present in real educational processes.
> * **Issue of extreme scores:** Predictive models exhibit a recurring tendency to underestimate values for students achieving extreme, outlier scores in the right tail of the distribution, making it harder to fully capture top results.
> * **Potential data leakage:** Missing value imputation using MICE was performed on the entire dataset before splitting into training and test sets, which may have slightly inflated optimistic model metrics.
> * **Statistical testing methodology:** With such a large sample ($n = 6,607$), variance and mean verification could rely on classical analysis of variance (ANOVA) due to the Central Limit Theorem. However, choosing non-parametric tests was a deliberate exploratory and demonstrative choice.
> * **Lack of advanced metrics and interpretability:** The current version omitted effect size calculations for statistical tests and advanced model explainability algorithms like SHAP (*SHapley Additive exPlanations*), relying primarily on classical regression coefficients and Spearman's correlation.

### Future Steps

> * **In-depth model validation:** Moving from simple construction and evaluation to rigorous validation procedures, such as $k$-fold cross-validation with data imputation strictly inside the training loop.
> * **Systematic feature selection:** Implementing advanced feature selection techniques to optimize model structure.
> * **Expanding interpretability:** Implementing SHAP analysis methods to assess both global and local impacts of individual variables on model predictions.
> * **Effect size analysis:** Supplementing non-parametric statistics with effect size measures to more precisely distinguish statistical significance driven by large sample size from real practical significance.
