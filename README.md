# finhacks2018
Fraud Detection using R

```{r}
library(randomForest)
```

```{r}
dataTrain <- read.csv('data_input/fraud_train.csv')
dataTest <- read.csv('data_input/fraud_test.csv')
```

```{r}
dataTrain$flag_transaksi_finansial <- NULL
dataTrain$status_transaksi <- NULL
dataTrain$bank_pemilik_kartu <- NULL

dataTest$flag_transaksi_finansial <- NULL
dataTest$status_transaksi <- NULL
dataTest$bank_pemilik_kartu <- NULL
```

```{r}
dataTrain <- dataTrain[complete.cases(dataTrain),]
dataTest <- dataTest[complete.cases(dataTest),]
```

```{r}
dataTrain$flag_transaksi_fraud <- factor(as.character(dataTrain$flag_transaksi_fraud))
dataTrain$flag_transaksi_fraud <- factor(as.character(dataTrain$flag_transaksi_fraud))
```

```{r}
set.seed(100)
modelFit <- randomForest(flag_transaksi_fraud ~., data = dataTrain)
prediction <- predict(modelFit, dataTest)
dataTest$prediction <- prediction
probability <- predict(modelFit, dataTest, type = "prob")
dataTest$probability <- ifelse(dataTest$prediction == 1, probability[,-1], probability[,-2])
```

```{r}
fraud <- dataTest[,-c(2:24)]
write.csv(fraud, 'prediction.csv')
```
