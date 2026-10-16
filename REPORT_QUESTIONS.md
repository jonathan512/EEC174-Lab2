# Lab 2: Written report questions

Answer the following questions in your written report. For questions about model performance, refer to results from your own notebook and report the relevant metrics.

## 1. Data and labels

1. **Prediction task:** What does one row of the dataset represent? What are the three classes predicted in this lab, and how are they constructed from the BSA and DTA annotations?
   - One row of data represents on breath from the patient. The three classes predicted in this lab are Non-PVA (no patient ventilator asynchrony), BSA (breath stacking asynchrony), and DTA (double trigger asynchrony). The labels are constructed by assigning class 0 when neither BSA or DTA are present, class 1 when only BSA, and class 2 when only DTA. 
2. **Ambiguous labels:** The notebook removes breaths annotated as both BSA and DTA. Why are these rows a problem for the three-class target? Describe one alternative to removing them.
   - They are removed since the notebook expects either class 0, 1 or 2, but both these rows together produce a label of 3 which is outside the defined classes.
3. **Patient identifiers:** Why are `patient` and `breathid` excluded from the model inputs? Does excluding them guarantee that the model will generalize to patients it has never seen? Explain.
   - Patient and breathid do not describe characteristics of the breathing, rather they are just used for identification. Excluding them however does not guarantee generalization since there could be other features that create patterns

## 2. Features and preprocessing

4. **Feature extraction:** Why does the lab extract features from flow and pressure waveforms before training a classifier? Identify two features that capture different aspects of a breath, and explain what each contributes.
   - The lab extract features from flow and pressure waveforms to produce usable numbers, such as breathing duration and air volume which provide a consistent set of inputs for learning patterns.
5. **Feature scaling:** Which scaling method did you use? Why should it be fitted on the training data and then applied to the validation data, rather than fitted on both datasets together?
   - I used standard scaler, which subtracts a feature's training mean and divides by its standard deviation. It should be fitted on the training data so that the validation data findings do not affect preprocessing.

## 3. Model performance

6. **Random forest:** In your own words, how does a random forest combine decision trees to make a prediction? Give one reason it is a reasonable choice for this dataset.

   - A random forest compiles the predictions from many decision trees, and chooses the most common/supported prediction. One reason it is a reasonable choice for this dataset is that it can pick up on nonlinear relationships between breath characteristics.

7. **Class-specific errors:** Examine your random forest's classification report. Which class has the lowest recall, and what kinds of mistakes does that indicate? Why might accuracy alone hide this problem?

   - Class 2 (DTA) had the lowest recall at 0.019, indicating that 98.1% of actually DTA breaths were missed and predicted as another class instead. Accuracy could hide this problem because DTA is responsible for a small part of the total validation breaths, covering only 104 of 1247 breaths, so the other 1143 could dominate the accuracy.

8. **Precision and recall:** For detecting BSA or DTA, what is the difference between a false positive and a false negative? Explain a situation in which you would prioritize recall over precision for one of these classes.

   - A false positive happens when a BSA or DTA is predicted for a breath that does not actually belong to that class, while a false negative occurs when a breath actually belongs to a class but the model failed to recognize it. Recall could be emphasized in a situation where missing a BSA or DTA event would be more costly than just performing an extra review.

9. **Model comparison:** Compare the random forest with the other supervised-learning model you implemented. Which has the higher F1-score? Does the same model perform best for every class? Support your answer with your results.

   - |         | random forest | kNN  |
     | ------- | ------------- | ---- |
     | non-PVA | 0.931         | 0.74 |
     | BSA     | 0.907         | 0.63 |
     | DTA     | 0.038         | 0.17 |

   - Random forest performed better than kNN for non-PVA and BSA, but kNN had a higher score for DTA. Therefore, the same model did not perform best for every class.

## 4. Model selection and validation

10. **Feature selection:** Compare performance using all available breath features with performance using selected features or the lab's expert-defined rules. Did the reduced feature set improve validation performance? Why might a feature that appears useful on the training set fail to improve validation performance?

    - Choosing to only use tve and ep_auc did not improve validation performance, with non-PVA and BSA seeing significant decreases in F1 score. A feature that appears useful may fail to improve validation performance because it could contain random noise or patterns only specific to the training data.

11. **Cross-validation:** What does stratification preserve in 5-fold cross-validation? Report your F1-score for each fold and its average. What would substantial differences between folds suggest?

    - Stratification preserves roughly the same proportion of each class in each fold. My F1 scores for each fold were:

    - | 1    | 0.850 |
      | ---- | ----- |
      | 2    | 0.822 |
      | 3    | 0.847 |
      | 4    | 0.857 |
      | 5    | 0.872 |

    - Average = 0.849

    - Substantial differences between the folds would suggest that the performance of a fold depends heavily on the data set that it receives.

12. **Patient-level validation:** If breaths from the same patient appear in both training and validation sets, how might that affect the reported performance? Describe a validation strategy for estimating performance on patients the model has not seen before.

    - If the same patient's breaths are seen in both training and validation sets, the model may interpret those patterns and report overly positive predictions for a new patient. One strategy for estimating performance on patients the model has not seen before could be to separate the data by patient, so that all breaths from one patient stays grouped together.
