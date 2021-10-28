# Data Science Credit Prediction, Investing: Project Overview
- Create virtualization to have a better understanding of the data of the banking system to lend people money.
- Engineered features from the original variable to create a better model.
- They are optimized Linear Regression and Logistic Regression by using the library in R to reach the perfect investment strategy.

## Code and Resources Used
**Program**: Rstudio
**Packages**: dplyr, ggplot2, caTools, ROCR, corrplot, rpart,randomForest, and more.

## Data Cleaning
After scraping the data and basic check of the data frame. I needed to clean the data so that I could use it, and the following changes are below:

- Change some variables into factor type
- Removed rows without data
- Made new columns which used to calculate the profit if we invest in
- More fill NA or Remove depends on the data

**Some example** of removing outliers

Handle wiith which int.rate > 10 and fico > 1500 which is outliers
![Outliers](https://github.com/northpr/LendingClub/blob/main/images/Screen%20Shot%202564-10-28%20at%2011.15.00.png?raw=true)
```R
ggplot(df) +
  aes(x = fico, y = int.rate) +
  geom_point(shape = "circle", size = 1.5, colour = "#228B22")
```

```R
df = df[-which(df$fico > 1500),]
df = df[-which(df$int.rate> 5),]
df = df[-which(df$revol.util > 500),]
df = df[-which(df$revol.bal > 75000),]
```

## Data Virtualization
To understand the model that we want to predict better, I have analysed for better understanding, clean and more tasks. So I will give some small tasks here.

We could know that ‘int.rate’,‘revol.util’, and ‘credit.policy’ has correlation with ‘fico’ by correlation plot
![Correlation Plot](https://github.com/northpr/LendingClub/blob/main/images/Screen%20Shot%202564-10-28%20at%2001.02.50.png)
```R
corrplot(cor_matrix,method='number')
```

Get to know the relavance between int.rate and fico
![Scatter Plot](https://github.com/northpr/LendingClub/blob/main/images/Screen%20Shot%202564-10-28%20at%2001.02.58.png?raw=true)
```R
ggplot(df) +
  aes(x = fico, y = int.rate) +
  geom_point(shape = "circle", size = 1.5, colour = "#228B22")
```

The number of purpose to make a rent
![Box Plot](https://github.com/northpr/LendingClub/blob/main/images/Screen%20Shot%202564-10-28%20at%2001.03.05.png?raw=true)
```R
bar<- ggplot(df,aes(factor(purpose)))+geom_bar(aes(fill=factor(not.fully.paid)),position='dodge')
bar+theme(axis.text.x =element_text(angle = 90,size = 10,vjust = 0.5))+theme_bw()
```

Study the purpose and rate of the fico by using box plot to seperate the purpose 1-by-1
![Box Plot](https://github.com/northpr/LendingClub/blob/main/images/Screen%20Shot%202564-10-28%20at%2010.50.56.png?raw=true)
```R
boxplot(fico~purpose,data=df,col='orange')
```

## Model Building

First, transform all the data types that are supposed to be and turn the categorical variables into dummy variables.

I created many models and evaluated them using RMSE because it is suitable to calculate how good the model is.

My models:
- Linear Model - to predict the fico score.
- Logistic Regression - to predict the customer will pay the loan from their history or not.
- Decision Tree - to predict the fico score and make it easier to explain by using the decision tree.
- Random Forest - the best result of all models here.
- Clustering - Group the type of customers for further analysis.

Some examples of my model.

To check the best threshold in this model to decide who will be paid or not paid by using ROCR library
![ROCR prediction](https://github.com/northpr/LendingClub/blob/main/images/Screen%20Shot%202564-10-28%20at%2010.56.14.png?raw=true)
```R
ROCRpred = prediction (LogRegPredict, df_test$not.fully.paid)
ROCRperf = performance (ROCRpred, "tpr", "fpr")
plot (ROCRperf, colorize = TRUE, print.cutoffs.at = seq (0, 1, by = 0.01), text.adj = c(-0.2, 1.7))
abline(v=0.25)
```

Using decision tree to classify between the people who will paid or not paid
![Decision Tree](https://github.com/northpr/LendingClub/blob/main/images/Screen%20Shot%202564-10-28%20at%2010.58.34.png?raw=true)
```R
set.seed(99)
split = sample.split (df$not.fully.paid, SplitRatio = 0.70)
df_train = subset(df, split == TRUE)
df_test = subset(df, split == FALSE)
nfpDCTM = rpart(not.fully.paid~.,data=df_train, method = 'class',cp=0.002)
prp(nfpDCTM)
```

Scale and build cluster
![Clustering](https://github.com/northpr/LendingClub/blob/main/images/Screen%20Shot%202564-10-28%20at%2010.55.59.png?raw=true)
```R
df_cluster = df_cluster[c(1,2,6,7,9)]
df_cluster = na.omit(df_cluster)
df_scale = scale(df_cluster)
km.out = kmeans(df_scale, 3 ,nstart=20)
```

Plot the cluster
```R
plot(df_cluster, col = km.out$cluster)
```

## Model performance
The random forest model is the best model for linear regression, but it depends on what you want to do with the data. For example, if you're going to do further marketing to make people invest, it might not be the best to understand.

The decision tree might be the best one to train the accountant to release a loan to customers. Lastly, Logistic regression seems to be the one that can tune more and make meaningful things later!

## Final
Thanks for viewing. For further information, please check .html file or check the R markdown file.
