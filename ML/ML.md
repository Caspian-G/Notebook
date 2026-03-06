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
X, y = mglearn.datasets.load_extended_boston()
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=0)
lasso = Lasso(alpha=...).fit(X_train, y_train)  //增大或减小alpha参数以增大或减小系统受到的约束 减低alpha时记得增大max_iter(运行迭代的最大次数)
print("Training set score: {:.2f}".format(lasso.score(X_train, y_train)))
print("Test set score: {:.2f}".format(lasso.score(X_test, y_test)))
print("Number of features used: {}".format(np.sum(lasso.coef_!=0)))  //Lasso对线性回归的L1正则化过程可能会让模型忽略一些特征使其权重为0
```

