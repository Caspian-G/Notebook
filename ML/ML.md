#### KNN

```
from sklearn.model_selection import train_test_split
import mglearn  //学习用第三方库
X, y = mglearn.datasets.make_forge()  //返回一个二分类且特征列数为2的数据集
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=0)
from sklearn.neighbors import KNeighborsClassifier
clf = KNeighborsClassifier(n_neighbors=3)
clf.fit(X_train, y_train)  //拟合
print("Test set predicition: {}".format(clf.predict(X_test)))  //预测
print("Test set accuracy: {:.2f}".format(clf.score(X_test, y_test)))  //评估
```

