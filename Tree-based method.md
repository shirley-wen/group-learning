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
最小化RSS
![tree3](https://github.com/shirley-wen/group-learning/blob/master/图片/3.png)  
  考虑每种将特征空间划分成J个区域的计算量太大，因而使用一种自上而下的贪婪算法-recursive binary splitting，此时只关注每一步的最佳划分，不考虑全局最优，因而是贪婪的。  
  In order to perform recursive binary splitting, we first select the predictor Xj and the cutpoint s such that splitting the predictor space into the regions {X|Xj < s} and {X|Xj ≥ s} leads to the greatest possible reduction in RSS.   
  将一个总空间先分为2个空间，再对分割后的2个空间挑选更能减少RSS的那个空间进行二分，特征可以重复利用，直到达到你指定的分割J个区域后停止划分。  
#### 树剪枝
  划分越多的区域在训练集肯定效果会更好，但有过拟合风险，在测试集效果可能很差，因为这棵树过于复杂，所以需要进行剪枝，获得一棵更小的树。  
  如何剪枝？  
  先生成一颗大树T0，通过cross-validation error或验证集的RSS来决定剪多少。但对每一棵子树验证的代价太大，我们可以通过在cost function中增加树的复杂项考虑一个序列的子树。最小化  
  
  α是一个非负的参数，|T|是叶节点的个数，回归树的算法：
1. Use recursive binary splitting to grow a large tree on the training data, stopping only when each terminal node has fewer than some
minimum number of observations.
2. Apply cost complexity pruning to the large tree in order to obtain a sequence of best subtrees, as a function of α.
3. Use K-fold cross-validation to choose α. That is, divide the training observations into K folds. For each k = 1, . . . , K:
(a) Repeat Steps 1 and 2 on all but the kth fold of the training data.
(b) Evaluate the mean squared prediction error on the data in the left-out kth fold, as a function of α.
Average the results for each value of α, and pick α to minimize the average error.
4. Return the subtree from Step 2 that corresponds to the chosen value of α.
