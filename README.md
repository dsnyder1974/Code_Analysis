### Software Engineering Code Analysis

**David Snyder**

#### Executive summary
This project explores whether we can predict which parts of a codebase are more likely to contain defects based on code characteristics and development activity. The goal is to surface higher-risk areas early so teams can focus testing and code review where it matters most.

#### Rationale
Defects are expensive, not just in terms of rework, but also in customer impact and team velocity. If teams can better anticipate where issues are likely to occur, they can prioritize more effectively, reduce production incidents, and improve overall delivery quality.

#### Research Question
This project is trying to determine whether code characteristics and development activity can be used to predict which parts of a codebase are most likely to contain defects. The goal is to identify higher-risk areas early so teams can prioritize testing and code reviews, reduce production issues, and improve delivery quality.

#### Data Sources
My data source is the GHPR Dataset, a GitHub pull-request-based software defect prediction dataset (https://github.com/feiwww/GHPR_dataset).  The dataset identifies bug fixes from GitHub pull requests and provides learning instances that can be used to classify code changes as defective or non-defective.

#### Methodology
This project uses supervised classification models to predict whether code is likely to contain a defect. Logistic Regression, Decision Tree, Random Forest, and Gradient Boosting are compared using cross-validation and metrics such as accuracy, precision, recall, F1 score, ROC-AUC, and confusion matrices. The strongest models are then tuned with GridSearchCV and reviewed using feature importance to identify the code characteristics most associated with defects.

#### Results
Based on the analysis, tree-based models performed better than the simpler Logistic Regression baseline, suggesting that software defects are influenced by nonlinear relationships between code metrics. The Decision Tree produced the strongest default results, while Gradient Boosting also performed very well and showed promise after tuning. Overall, the findings indicate that code complexity and structural characteristics can help identify defect-prone areas, although additional development-activity features may further improve performance.

#### Next steps
Since the strongest models have already been tuned, the next steps include:
 - Choose the best model based on the test results and take a closer look at its confusion matrix and feature importance. 
 - Adjust the prediction threshold to improve recall or F1 score.
 - Include adding features like code churn, commit history, previous defects, and test coverage or testing the model on another software project.

#### Outline of project

 - [Link to notebook](https://github.com/dsnyder1974/Code_Analysis/blob/main/Code_Analysis.ipynb)
 - [Link to dataset](https://github.com/dsnyder1974/Code_Analysis/blob/main/data/baseline.csv)

