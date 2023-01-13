# k-최근접 이웃이란?
`k-최근접 이웃(k-NN)`은 가장 간단한 머신러닝 알고리즘으로 새로운 데이터에 대해 이와 가장 거리가 가까운 k개의 과거 데이터의 결과를 이용해 다수결로 분류하는 방법이다.

분류와 회귀에 모두 사용할 수 있다. 또한 새로운 데이터에 더 가까운 이웃일수록 더 먼 이웃보다 평균에 많이 기여하도록 가중치를 부여할 수 있다. 거리를 계산하는 방법은 대표적으로 3가지가 있다.

- 유클리디안 거리
- 민코우스키 거리
- 맨하탄 거리

<br>

k-NN 알고리즘을 사용하기 위해서는 `DMwR2` 패키지가 필요하다.

<br>

## Data Load
```r
> train <- read.csv('ch6-2_train.csv', header=TRUE)
> test <- read.csv('ch6-2_test.csv', header=TRUE)
```
<br>

## k-NN (k=3)
`DMwR2` 패키지 다운로드 및 설치
```r
> install.packages('DMwR2')
> library(DMwR2)
```

<br>

사용법은 `kNN(y ~ x, train, test, k)`으로 사용한다.
```r
knn <- kNN(breeds ~ ., train, test, k=3)
```

<br>

test 데이터에 예측값을 넣어준다.
breeds는 실제값, pred는 예측값이다.
```r
> test$pred <- knn
> head(test)
  wing_length tail_length comb_height breeds pred
1         258          67          32      a    a
2         260          64          34      a    a
3         251          63          31      a    a
4         248          63          30      a    a
5         254          62          32      a    a
6         230          64          33      a    a
```

앞서 진행한 나이브 베이즈와 k-NN 알고리즘의 차이점을 발견할 수 있다.

나이브 베이즈는 `predict()` 함수를 통해 예측값을 생성하였으나, k-NN은 사용하지 않았다. 

>Why?
k-NN 알고리즘은 거리 기반의 단순한 알고리즘이기 때문이다. 이러한 학습 현태를 게으른 학습이라고 한다.

<br>

### 정오분류표
k=3일 때 예측값이 pred의 값과 실제 값의 차이를 정오분류표를 그려서 확인해보자. `caret` 패키지의 `confusionMatrix()` 함수를 사용한다.
```r
> library(caret)
```
```r
> test$breeds <- as.factor(test$breeds)
> confusionMatrix(test$pred, test$breeds)
Confusion Matrix and Statistics

          Reference
Prediction  a  b  c
         a 19  1  0
         b  1 18  1
         c  0  1 19

Overall Statistics
                                         
               Accuracy : 0.9333         
                 95% CI : (0.838, 0.9815)
    No Information Rate : 0.3333         
    P-Value [Acc > NIR] : < 2.2e-16      
                                         
                  Kappa : 0.9            
                                         
 Mcnemar's Test P-Value : NA             

Statistics by Class:

                     Class: a Class: b Class: c
Sensitivity            0.9500   0.9000   0.9500
Specificity            0.9750   0.9500   0.9750
Pos Pred Value         0.9500   0.9000   0.9500
Neg Pred Value         0.9750   0.9500   0.9750
Prevalence             0.3333   0.3333   0.3333
Detection Rate         0.3167   0.3000   0.3167
Detection Prevalence   0.3333   0.3333   0.3333
Balanced Accuracy      0.9625   0.9250   0.9625
```
60개의 데이터 중 56개를 맞췄다.
- Accuracy : 0.9333
- Sensitivity : 0.9500 0.9000 0.9500
- Specificity : 0.9750 0.9500 0.9750
