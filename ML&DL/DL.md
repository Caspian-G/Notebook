#### MLP

基于sklearn

```
//多层感知机 普通前馈神经网络
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.neural_network import MLPClassifier
cancer = load_breast_cancer()
X_train, X_test, y_train, y_test = train_test_split(cancer.data, cancer.target, stratify=cancer.target, random_state=0)
mlp = MLPClassifier(max_iter=200, solver="lbfgs", random_state=0, hidden_layer_sizes=[100], alpha=0.0001, activation="relu")  //此处参数都是默认值，hidden_layer_sizes可设置为[10, 10]表示两层隐层每层10个隐单元
mlp.fit(X_train, y_train)
print("{:.3f}".format(mlp.score(X_train, y_train)))
print("{:.3f}".format(mlp.score(X_test, y_test)))
//输出 0.967 0.937
mlp = MLPClassifier(random_state=0, max_iter=500)
mean_on_training = X_train.mean(axis=0)
std_on_training = X_train.std(axis=0)
X_train_scaled = (X_train - mean_on_training)/std_on_training
X_test_scaled = (X_test - mean_on_training)/std_on_training
mlp.fit(X_train_scaled, y_train)
print("{:.3f}".format(mlp.score(X_train_scaled, y_train)))
print("{:.3f}".format(mlp.score(X_test_scaled, y_test)))
//数据集缩放后可显著提高精度 0.998 0.958

//分析输入层与第一层隐层之间的权值
import matplotlib.pyplot as plt
plt.figure(figsize=(20, 5))
plt.imshow(mlp.coefs_[0], interpolation="none", cmap="viridis")
plt.yticks(range(30), cancer.feature_names)
plt.xlabel("Cloums in weight matrix")
plt.ylabel("Input feature")
plt.colorbar()
plt.savefig('mlp_weight_matrix.png', dpi=300, bbox_inches='tight')  //dpi分辨率(300为标准打印质量 600高质量打印) bbox_inches="tight"去除多余白边 
```

![mlp_weight_matrix](D:\Notebook\Assets\Photoes\mlp_weight_matrix.png)

**优化算法选择** "adam" 默认 需保证数据集均值为0方差为1                              			"lbfgs" 拟牛顿法变种 训练时长更长                                              			"sgd" 随机梯度下降法 参数更多更灵活
