# Naive Bayes Classifier - Mushroom Types

This repository contains an R-based machine learning lab focused on building a **Naive Bayes classifier** to classify mushrooms as either **edible** or **poisonous**.

The project demonstrates how to load and preprocess categorical data, split the data into training and testing sets, train a Naive Bayes model, generate predictions, evaluate results with a confusion matrix, and calculate overall model accuracy.

## Project Overview

The lab applies Naive Bayes classification to a mushroom dataset containing observations from the **Agaricus** and **Lepiota** family of gilled mushrooms.

The dataset contains:

- 8,123 mushroom observations
- 23 mushroom species or characteristics
- A target class that indicates whether each mushroom is edible or poisonous

The classification target is stored in the `classes` column:

- `e` = edible
- `p` = poisonous

The main objective is to determine whether a mushroom is edible or poisonous based on its observed characteristics.

## Main Objective

The goal of this project is to demonstrate a complete supervised machine learning classification workflow using the Naive Bayes algorithm.

The lab walks through the following steps:

1. Load the mushroom dataset
2. Inspect and summarize the data
3. Handle missing values
4. Create training and testing datasets
5. Train a Naive Bayes model
6. Generate predictions on unseen test data
7. Build a confusion matrix
8. Calculate model accuracy
9. Interpret overall model performance

## Technologies Used

This project uses:

- R
- `e1071` package

The `e1071` package provides the Naive Bayes classifier used in this lab.

```r
install.packages("e1071")
library(e1071)
```

## Repository Structure

```text
.
├── lab
└── README.md
```

The main lab code is contained in the `lab` file.

## Dataset

The lab expects a CSV file named:

```text
Mushroom.csv
```

The dataset should be saved in the working directory before running the lab.

The file is loaded using:

```r
mushroom <- read.csv('Mushroom.csv', stringsAsFactors = T, na.strings = NA)
```

## Data Preprocessing

The lab starts by loading and summarizing the dataset:

```r
summary(mushroom)
```

It then checks for incomplete records:

```r
nrow(mushroom[!complete.cases(mushroom),])
nrow(mushroom)
```

Because Naive Bayes relies on probability estimates, missing values can affect conditional probability calculations. The lab removes observations with missing values:

```r
mushroom = mushroom[complete.cases(mushroom),]
nrow(mushroom)
```

## Training and Testing Split

The project uses a 70/30 train-test split:

- 70% of the data is used for training
- 30% of the data is used for testing

The split is created using random sampling:

```r
sample_size <- floor(0.7 * nrow(mushroom))
training_index <- sample(nrow(mushroom), size = sample_size, replace = FALSE)

train <- mushroom[training_index,]
test <- mushroom[-training_index,]
```

## Model Training

The model is trained using the `naiveBayes()` function from the `e1071` package.

```r
mushroom.model <- naiveBayes(classes ~ . , data = train)
```

The formula `classes ~ .` means:

- Predict `classes`
- Use all other columns in the dataset as predictor variables

After training, the model object can be printed to inspect the conditional probabilities learned from the training data:

```r
mushroom.model
```

## Prediction

After fitting the model, the test dataset is passed into the model to generate predictions:

```r
mushroom.predict <- predict(mushroom.model, test, type = 'class')
```

The predicted classes are then combined with the actual classes:

```r
results <- data.frame(actual = test[,'classes'], predicted = mushroom.predict)
```

## Model Evaluation

The lab evaluates model performance using a confusion matrix:

```r
table(results)
```

The confusion matrix compares:

- Actual mushroom class
- Predicted mushroom class

This helps identify how many mushrooms were correctly and incorrectly classified as edible or poisonous.

The lab also calculates overall prediction accuracy:

```r
results$correct <- ifelse(results$actual == results$predicted, 1, 0)
sum(results$correct) / nrow(results)
```

The lab notes that the model performs well on this dataset, although the exact accuracy may vary because the training and testing split is randomly generated.

## Key Machine Learning Concepts Demonstrated

This project demonstrates several important machine learning and data science concepts:

- Supervised learning
- Classification
- Naive Bayes algorithm
- Conditional probability
- Prior probability
- Categorical data handling
- Missing value handling
- Training and testing split
- Prediction on unseen data
- Confusion matrix
- Accuracy calculation
- Model performance evaluation

## How to Run

1. Clone the repository:

```bash
git clone https://github.com/mickemora/Naive-Bayes-Classifier-Mushroom-Types.git
```

2. Navigate into the project directory:

```bash
cd Naive-Bayes-Classifier-Mushroom-Types
```

3. Open the project in RStudio or another R environment.

4. Install the required R package:

```r
install.packages("e1071")
```

5. Make sure the following dataset is available in the working directory:

```text
Mushroom.csv
```

6. Run the lab script.

## Example Workflow

```r
library(e1071)

mushroom <- read.csv('Mushroom.csv', stringsAsFactors = T, na.strings = NA)
mushroom <- mushroom[complete.cases(mushroom),]

sample_size <- floor(0.7 * nrow(mushroom))
training_index <- sample(nrow(mushroom), size = sample_size, replace = FALSE)

train <- mushroom[training_index,]
test <- mushroom[-training_index,]

mushroom.model <- naiveBayes(classes ~ . , data = train)
mushroom.predict <- predict(mushroom.model, test, type = 'class')

results <- data.frame(actual = test[,'classes'], predicted = mushroom.predict)
table(results)

results$correct <- ifelse(results$actual == results$predicted, 1, 0)
sum(results$correct) / nrow(results)
```

## Notes

This repository is intended as a learning lab rather than a production machine learning application.

It is useful for understanding how Naive Bayes can be applied to categorical classification problems and how model performance can be evaluated using test data.

## Potential Enhancements

Future improvements could include:

- Add the `Mushroom.csv` dataset or a link to the data source
- Add a random seed for reproducibility
- Add actual accuracy results from a completed run
- Add a cleaner confusion matrix visualization
- Add precision, recall, and F1-score
- Compare Naive Bayes with Decision Tree or Random Forest models
- Add explanatory notes on Bayes' theorem
- Convert the lab into an R Markdown notebook
- Add business use cases for probabilistic classification

## Business and Real-World Applications

Although this lab uses mushroom classification, the same modeling pattern applies to many real-world classification problems, such as:

- Spam detection
- Medical risk classification
- Warranty claim categorization
- Fraud detection triage
- Customer issue classification
- Product defect classification
- Document or text classification

Naive Bayes is especially useful when working with categorical features or when a simple, interpretable, probability-based baseline model is needed.

## Summary

This project applies the Naive Bayes algorithm to classify mushrooms as edible or poisonous based on their characteristics. It demonstrates a complete classification workflow in R: data loading, preprocessing, train-test splitting, model training, prediction, confusion matrix evaluation, and accuracy calculation.
