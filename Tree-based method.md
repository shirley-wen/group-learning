* 决策树
  * 回归树
  * 分类树
* Bagging
* 随机森林
* Boosting
## 决策树
### 回归树
![tree1](https://github.com/shirley-wen/group-learning/blob/master/图片/1.png)
![tree2](https://github.com/shirley-wen/group-learning/blob/master/图片/2.png)
#### process of building a regression tree
1. We divide the predictor space—that is, the set of possible values for X1, X2, . . . , Xp—into J distinct and non-overlapping regions,
R1, R2, . . . , RJ.
2. For every observation that falls into the region Rj, we make the same prediction, which is simply the mean of the response values for thetraining observations in Rj.
#### 如何分区
![tree3](https://github.com/shirley-wen/group-learning/blob/master/图片/3.png)
