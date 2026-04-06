#### KNN

```
//分类
from sklearn.model_selection import train_test_split
import mglearn  //学习用第三方库
X, y = mglearn.datasets.make_forge()  //返回一个二分类且特征列数为2的数据集
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=0)  //训练数据和测试数据乱序
from sklearn.neighbors import KNeighborsClassifier
clf = KNeighborsClassifier(n_neighbors=3)
clf.fit(X_train, y_train)  //拟合
print("Test set predicition: {}".format(clf.predict(X_test)))  //预测
print("Test set accuracy: {:.2f}".format(clf.score(X_test, y_test)))  //评估
//回归
from sklearn.model_selection import train_test_split
from sklearn.neighbors import KNeighborsRegressor
import mglearn
X, y = mglearn.datasets.make_wave(n_samples=40)  //此处X只有单特征
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=0)
reg = KNeighborsRegressor(n_neighbors=3)  //K = 3
reg.fit(X_train, y_train)
predictions = reg.predict(X_test)
print(f"Test set predicition: {[f'{x:.3f}' for x in predictions ]}")  //这种写法可让列表中元素保留三位小数
print("Test set accuracy: {:.2f}".format(reg.score(X_test, y_test)))  //对于回归问题score方法返回R^2分数(决定系数)
//分析回归模型
from sklearn.model_selection import train_test_split
from sklearn.neighbors import KNeighborsRegressor
import mglearn
import matplotlib.pyplot as plt
import numpy as np
X, y = mglearn.datasets.make_wave(n_samples=40)
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=0)
fig, axes = plt.subplots(1, 3, figsize=(15, 4))
line = np.linspace(-3, 3, 1000).reshape(-1, 1)
for n_neighbors, ax in zip([1, 3, 9], axes):
    reg = KNeighborsRegressor(n_neighbors = n_neighbors)
    reg.fit(X_train, y_train)
    ax.plot(line, reg.predict(line))
    ax.plot(X_train, y_train, '*', color="red")
    ax.plot(X_test, y_test, '^', color="yellow")
    //省略title xlabel ylabel legend
```

#### 线性模型

###### 线性回归

```
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split
import mglearn
X, y = mglearn.datasets.load_extended_boston()
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=0)
lr = LinearRegression().fit(X_train, y_train)
print("lr.coef_:{}".format(lr.coef_))  //lc.coef_存w 即斜率或权重
print("lr.intercept_:{}".format(lr.intercept_))  //lc.intercept_存截距
print("Training set score: {:.2f}".format(lr.score(X_train, y_train)))
print("Test set score: {:.2f}".format(lr.score(X_test, y_test)))
```

###### 岭回归

```
from sklearn.linear_model import Ridge
from sklearn.model_selection import train_test_split
import mglearn
X, y = mglearn.datasets.load_extended_boston()
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=0)
ridge = Ridge(alpha=...).fit(X_train, y_train)  //增大或减小alpha参数以增大或减小系统受到的约束
print("Training set score: {:.2f}".format(ridge.score(X_train, y_train)))
print("Test set score: {:.2f}".format(ridge.score(X_test, y_test)))
```

###### Lasso

```
from sklearn.linear_model import Lasso
from sklearn.model_selection import train_test_split
import mglearn
import numpy as np
X, y = mglearn.datasets.load_extended_boston()
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=0)
lasso = Lasso(alpha=...).fit(X_train, y_train)  //增大或减小alpha参数以增大或减小系统受到的约束 减低alpha时记得增大max_iter(运行迭代的最大次数)
print("Training set score: {:.2f}".format(lasso.score(X_train, y_train)))
print("Test set score: {:.2f}".format(lasso.score(X_test, y_test)))
print("Number of features used: {}".format(np.sum(lasso.coef_!=0)))  //Lasso对线性回归的L1正则化过程可能会让模型忽略一些特征使其权重为0
```

###### ElasticNet (弹性网络) sklearn库提供可同时使用L1正则化和L2正则化

```
from sklearn.linear_model import ElasticNet
from sklearn.model_selection import train_test_split
import mglearn
import numpy as np
X, y = mglearn.datasets.load_extended_boston()
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=0)
en = ElasticNet(alpha=..., l1_ratio=0.5).fit(X_train, y_train)  //增大或减小alpha参数以增大或减小系统受到的约束  l1_ratio=0.5表示L1和L2占比各一半
print("Training set score: {:.2f}".format(en.score(X_train, y_train)))
print("Test set score: {:.2f}".format(en.score(X_test, y_test)))
print("Number of features used: {}".format(np.sum(en.coef_!=0)))  //Lasso对线性回归的L1正则化过程可能会让模型忽略一些特征使其权重为0
```

###### LogisticRegresssion(分类算法)

```
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
cancer = load_breast_cancer()
X_train, X_test, y_train, y_test = train_test_split(cancer.data, cancer.target, stratify=cancer.target, random_state=42)  //stratify用于确保训练集和测试集中的良性和恶性数据的比例和原数据集的target比例一致 避免出现训练集或测试集内数据出现全是恶性或全是良性的情况
logreg = LogisticRegression(C=100, max_iter=10000, l1_ratio=1).fit(X_train, y_train)  //默认L2正则化 C越大系数约束越弱 max_iter设置最大迭代次数 l1_ratio=1可设置为L1正则化(需要修改solver求解器 默认lbfgs仅支持L2)
print("Training set score: {:.2f}".format(logreg.score(X_train, y_train)))
print("Test set score: {:.2f}".format(logreg.score(X_test, y_test)))
import matplotlib.pyplot as plt
plt.plot(logreg.coef_.T, "o")  //此行代码可查看各系数点的值分布 转置是因为logreg.coef_的形状是1行n列即[[..., ...]]而不是[[...], [...], ...] 不转置plot方法会以为x轴只有一个0值
```

###### LinearSVM(分类算法)

```
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.svm import LinearSVC
cancer = load_breast_cancer()
X_train, X_test, y_train, y_test = train_test_split(cancer.data, cancer.target, stratify=cancer.target, random_state=42)  //stratify用于确保训练集和测试集中的良性和恶性数据的比例和原数据集的target比例一致 避免出现训练集或测试集内数据出现全是恶性或全是良性的情况
svc = LinearSVC(C=100, max_iter=10000, penalty="l1").fit(X_train, y_train)  //默认L2正则化 C越大系数约束越弱 max_iter设置最大迭代次数 penalty="l1"设置L1正则化(solver为liblinear可支持L1正则化)
print("Training set score: {:.2f}".format(svc.score(X_train, y_train)))
print("Test set score: {:.2f}".format(svc.score(X_test, y_test)))
```

**注** 除了二分类 线性模型也可支持多分类

**注** scikit-learn中所有模型的fit方法返回的都是self

#### 朴素贝叶斯分类器

```
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.naive_bayes import GaussianNB  //包含GaussianNB BernoulliNB(仅特征值为二分类时) MultinomialNB(仅特征值为计数数据即非负整数时) 此处GaussianNB为例
cancer = load_breast_cancer()
X_train, X_test, y_train, y_test = train_test_split(cancer.data, cancer.target, stratify=cancer.target, random_state=42) 
gnb = GaussianNB().fit(X_train, y_train)
print("Training set score: {:.2f}".format(gnb.score(X_train, y_train)))
print("Test set score: {:.2f}".format(gnb.score(X_test, y_test)))
```

#### 决策树

######  DecisionTreeClassifier 分类算法

```
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier
cancer = load_breast_cancer()
X_train, X_test, y_train, y_test = train_test_split(cancer.data, cancer.target, stratify=cancer.target, random_state=42) 
tree = DecisionTreeClassifier(max_depth=4, random_state=0).fit(X_train, y_train)  //random_state用于在内部解决平局问题 max_depth用于限制树深度不包括根节点(常用预剪枝策略之一，若不进行预剪枝会导致树一直展开最终过拟合，训练集精准度到达100%)
print("Training set score: {:.2f}".format(tree.score(X_train, y_train)))
print("Test set score: {:.2f}".format(tree.score(X_test, y_test)))
//分析决策树 树可视化
from sklearn.tree import export_graphviz
export_graphviz(tree, out_file="tree.dot", class_names=["malignant", "benign"], feature_names=cancer.feature_names, impurity=False, filled=True)  //生成.dot文件将树可视化
import graphviz
with open("tree.dot") as f:  //打开.dot文件
	dot_graph = f.read()
graphviz.Source(dot_graph)
print(tree.feature_importances_)  //输出特征重要性
```

###### DecisionTreeRegression 回归算法

```
//略
//树模型做回归任务外推能力为零
```

#### 决策树集成

###### 随机森林

```
from sklearn.ensemble import RandomForestClassifier
from sklearn.datasets import make_moons
from sklearn.model_selection import train_test_split
X, y = make_moons(n_samples=100, noise=0.25, random_state=3)  //n_samples为样本数 noise为添加到数据中的高斯噪声标准差 random_state为随机数生成器的种子
X_train, X_test, y_train, y_test = train_test_split(X, y, stratify=y, random_state=42)
forest = RandomForestClassifier(n_estimators=5, random_state=2)  //决策树数量为5 越大越易过拟合 可设置参数max_features调节单棵树的特征选择越大树与树之间越相似
forest.fit(X_train, y_train)
print(forest.score(X_train, y_train))
print(forest.score(X_test, y_test))
```

###### 梯度提升回归树(梯度提升机)

```
```

