# Software Defect Prediction

**Author:** David Snyder  
**Course:** UC Berkeley Executive Education – Machine Learning and Artificial Intelligence  
**Project:** Software Defect Prediction  
**Date:** August 2026

This project explores whether machine learning models can use static code metrics to identify software components that are more likely to contain defects. The goal is not to replace testing or code review, but to investigate whether defect-risk predictions could help engineering teams prioritize where to spend additional review and testing effort.

## Project Goals

The project focuses on three main questions:

1. Which machine learning models perform best for software defect prediction?
2. Which static code metrics contribute the most predictive value?
3. Do models trained on one software dataset generalize to other software projects?

## Modeling Approach

The analysis compares several classification models, including:

- Logistic Regression
- Decision Tree
- Random Forest
- Extra Trees
- Gradient Boosting
- Support Vector Machine
- TensorFlow Neural Network

The strongest traditional models were tuned using cross-validation and compared using:

- Accuracy
- Precision
- Recall
- F1 score
- ROC-AUC

Because missing a real defect can be more costly than reviewing an additional false positive, recall and false negatives are particularly important in this project.

## Model Selection

Gradient Boosting was selected for deeper analysis because it provided a strong overall balance of recall, F1 score, and ROC-AUC.

The tuned Gradient Boosting model produced approximately:

| Metric | Score |
|---|---:|
| Accuracy | 0.773 |
| Precision | 0.741 |
| Recall | 0.838 |
| F1 | 0.787 |
| ROC-AUC | 0.812 |

### Classification Threshold

The default classification threshold of `0.50` was also compared with a lower threshold of `0.40`.

Lowering the threshold:

- Increased recall from approximately `0.838` to `0.861`
- Reduced false negatives from `98` to `84`
- Identified `14` additional actual defects
- Increased false positives from `177` to `210`

The lower threshold may therefore be useful when catching additional defects is more important than minimizing false alarms.

## Feature Importance and Pruning

Feature importance from the tuned Gradient Boosting model was used to identify low-value predictors.

The original model contained **21 features**. Cross-validation showed that the model could be reduced substantially without sacrificing performance:

| Importance Threshold | Number of Features | Mean CV F1 |
|---|---:|---:|
| 0.000 | 21 | 0.8064 |
| 0.005 | 9 | 0.8073 |
| 0.010 | 6 | 0.8079 |

Using only six features slightly improved mean cross-validation F1 while greatly simplifying the model.

The six retained features were:

- `number_of_static_invocations`
- `lines_of_code`
- `depth_of_inheritance_tree`
- `coupling_between_objects`
- `math_operation_count`
- `assignment_count`

This suggests that much of the predictive signal in the original dataset is concentrated in a relatively small number of code size, complexity, coupling, and code-operation metrics.

## Cross-Project Validation

To test whether the model generalized beyond the original dataset, additional datasets from the PROMISE software defect collection were evaluated.

The external datasets were obtained from:

[SDP using DQN-based feature extraction](https://github.com/asqwq/SDP-using-DQN-based-feature-extraction)

Not all six selected features existed in the PROMISE datasets, so a separate shared feature set was created using six directly comparable metrics:

- Weighted methods per class
- Depth of inheritance tree
- Coupling between objects
- Response for class
- Lack of cohesion in methods
- Lines of code

A Gradient Boosting model was trained on the original dataset using only these six common features and then evaluated directly on the external datasets without retraining.

Separate models were also trained and tested within each PROMISE dataset as a control.

## External Validation Results

### ANT 1.7

The ANT dataset had a defect rate of approximately **22.3%**.

| Evaluation | ROC-AUC |
|---|---:|
| Original-trained model → ANT | 0.508 |
| ANT-trained model → ANT | 0.802 |

The transferred model performed close to random ranking, while the same feature set performed much better when the model was trained directly on ANT.

### Synapse 1.2

The Synapse dataset had a defect rate of approximately **33.6%**.

| Evaluation | ROC-AUC |
|---|---:|
| Original-trained model → Synapse | 0.555 |
| Synapse-trained model → Synapse | 0.780 |

Synapse showed a similar pattern: the shared static-code metrics contained useful predictive information, but the relationships learned from the original dataset did not transfer well to another project.

## Key Findings

The project produced several important findings:

- Gradient Boosting provided strong overall defect-prediction performance on the original dataset.
- Lowering the classification threshold can reduce missed defects at the cost of additional false positives.
- Feature pruning reduced the model from 21 predictors to only 6 without reducing cross-validation performance.
- Static code metrics appear useful for predicting defects within individual software projects.
- A model trained on one project does not necessarily generalize well to another project, even when the same metrics are available.
- Differences in feature distributions, class balance, architecture, development practices, and defect patterns may contribute to weak cross-project performance.

## Real-World Interpretation

Software defect prediction does not necessarily need to work as a universal model across every codebase to provide value.

A practical implementation could train a model using an organization's own historical code metrics and defect data. The resulting risk scores could help identify files or classes that deserve additional code review, automated testing, regression testing, or QA attention.

The results of this project suggest that defect-prediction models may be most effective when trained or adapted for the specific software environment in which they will be used.

## Project Structure

The project structure is:

```text
.
├── data/
│   ├── baseline.csv
│   ├── ant-1.7.csv
│   └── synapse-1.2.csv
├── Code_Analysis.ipynb
└── README.md
```

## External Links

 - [Link to notebook](https://github.com/dsnyder1974/Code_Analysis/blob/main/Code_Analysis.ipynb)
 - [Link to dataset](https://github.com/dsnyder1974/Code_Analysis/blob/main/data/baseline.csv)

