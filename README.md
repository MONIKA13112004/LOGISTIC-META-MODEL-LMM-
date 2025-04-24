ALGORITHM:
Step 1: Load and Preprocess Data
⦁	Read the dataset from the CSV file.
⦁	Separate features (X) and target (y).
⦁	Split the dataset into training (80%) and testing (20%) sets.
⦁	Ensure class stratification to maintain fraud-to-non-fraud ratio.
⦁	Handle class imbalance using class weights and scaling.

Step 2: Train Base Models (Level-0 Models)
⦁	Train Random Forest (RF)
⦁	Initialize RandomForestClassifier with:
⦁	n_estimators=100 (100 decision trees).
⦁	class_weight={0: 1, 1: 5} (higher weight to fraud cases).
⦁	Train the model on X_train, y_train using bootstrapped samples.
⦁	Each tree makes predictions independently.
⦁	Final prediction: Majority voting (classification).
⦁	Train XGBoost (XGB)
⦁	Initialize XGBClassifier with:
⦁	use_label_encoder=False (avoiding deprecated warnings).
⦁	eval_metric='logloss' (logarithmic loss for classification).
⦁	scale_pos_weight = (count of non-fraud cases / count of fraud cases).
⦁	Train using gradient boosting:
⦁	Model learns residual errors from previous iterations.
⦁	Assigns higher weights to misclassified fraud cases.
⦁	Final prediction: Aggregates weak models into a strong classifier.
⦁	Train Logistic Regression (LR)
⦁	Initialize LogisticRegression with:
⦁	class_weight={0:1, 1:5} (to address class imbalance).
⦁	max_iter=500 (higher iterations for convergence).
⦁	Train the model using:
⦁	Sigmoid activation function:

 

⦁	Uses Maximum Likelihood Estimation (MLE) to optimize weights.
⦁	Final prediction: Computes probability of fraud (0-1).

Step 3: Train Stacking Classifier (Level-1 Meta-Model)
⦁	Initialize StackingClassifier with:
⦁	Base models: Random Forest, XGBoost, Logistic Regression.
⦁	Final estimator (meta-model): Logistic Regression.
⦁	passthrough=True: Passes raw features + base model predictions to meta-model.
⦁	Train stacking model:
⦁	Each base model makes predictions on training data.
⦁	Their predictions are combined into a new dataset.
⦁	Meta-model learns to optimize final fraud detection.
⦁	Final prediction: Logistic Regression meta-model determines fraud probability.

Step 4: Make Predictions and Evaluate Performance
⦁	Make predictions on X_test:
⦁	Base models generate predictions.
⦁	Meta-model refines predictions.
⦁	Compute performance metrics:
⦁	Confusion Matrix: Count of TP, FP, TN, FN.
⦁	Precision, Recall, F1-score: Fraud detection efficiency.
⦁	ROC-AUC Score: Measures model discrimination power.
⦁	Visualize results:
⦁	Heatmap for confusion matrix.
⦁	ROC Curve for fraud detection performance.


