# ML

#### ML库

```
Anaconda 用于大规模数据处理、预测分析和科学计算的python发行版 预装NumPy、SciPy、matplotlib、pandas、IPython、JupyterNotebook、scikit-learn
NumPy 多维数组
SciPy 函数集
matplotlib 科学绘图库
pandas 利用字典创建Data Frame
scikit-learn 包含数据集、ML函数库等
```

#### NumPy

```
核心是ndarray对象 底层计算使用c
每个数组都由两部分组成 RawData实际存储数值的连续内存块 Metadata描述如何解释这些数据的元数据(形状、数据类型、步长)
import numpy as np
//create
x = np.array([[1, 2, 3], [4, 5, 6]])  //所有元素类型必须相同
x = np.zeros((3, 3))  //全零矩阵
x = np.ones((3, 3))  //全一矩阵
x = np.eye(3)  //单位矩阵
x = np.arange(0, 10, 2)  //[0 2 4 6 8]
x = np.linspace(0, 1, 5)  //[0.0 0.25 0.5 0.75 1.0]
//index
x = x[1: 5]  //截取2到5个元素
x = x[1, 2]  //获取2行3列元素  非传统x[][]
x = x[[0, 2, 4]]  //花式索引 取索引为0 2 4的元素
x = x[x > 5]  //布尔索引 过滤出所有数值大于5的元素
//count
广播机制 "形状"大的数组与"形状"小的数组进行运算时 "形状"小的会进行拉伸
矩阵加法的逐运算直接 +
矩阵乘法的逐运算直接 *
矩阵点乘 @
x = np.array([[1, 2, 3], [4, 5, 6]]).T  //.T 转置
np.sum(x, axis = 1)  //按行求和
np.sum(x, axis = 0)  //按列求和
np.mean(x, axis = 1)  //按行求平均
np.mean(x, axis = 0)  //按列求平均
np.std(x)  //计算标准差 越大波动越大
//视图 and 副本
b = a.reshape(2, 1)  //a和b共享同一块内存数据 只是看数据的视角变了
c = a.copy()  //这会开辟新的内存空间
.reshape()可修改看待数据的方式(元数据)
a = np.arange(10)
a = a.reshape(2, 5)  //1行10列修改为2行5列
a = a.reshape(2, -1)  //参数为-1可自动推导
//修改内存布局
x = np.array([[1, 2, 3], [4, 5, 6]], order = "F")  //以Fortran风格在内存中存储多维数组
```

#### SciPy

```
科学计算库 包含大量高效数学算法
from scipy import optimize  //用于优化与最小化
from scipy import integrate  //数值积分与常微分方程求解
from scipy import linalg  //高级线性代数运算 可作为numpy扩展
from scipy import interpolate  //插值
from scipy import fftpack  //快速傅里叶变换(FFT)
from scipy import signal  //信号处理(滤波器设计、频谱分析)
from scipy import stats  //统计分布与统计检验
from scipy import sparse  //稀疏矩阵操作
from scipy import spatial  //空间数据结构与几何运算
from scipy import io  //数据输入输出

//optimize
//minimize_scalar()方法专用于处理一元函数问题
def func(x):
    return x**2 + 5*x + 6
result = optimize.minimize_scalar(func)
print(f"最小值点 x = {result.x:.4f}")
print(f"最小值 f(x) = {result.fun:.4f}")
print(f"是否成功: {result.success}")
//minimize()多元函数问题
def rosen(x):
    return (1 - x[0])**2 + 100 * (x[1] - x[0]**2)**2
x0 = np.array([0, 0])  //初始猜测影响结果收敛速度
result = optimize.minimize(rosen, x0, method='BFGS')  //使用BFGS算法求解 BFGS要求待求解函数足够平滑
print(f"最优解: x = {result.x[0]:.6f}, y = {result.x[1]:.6f}")
print(f"最小值: {result.fun:.6f}")
print(f"迭代次数: {result.nit}")
print(f"函数评估次数: {result.nfev}")
//...

//sparse
//lil_matrix 适合创建、动态添加和修改

//csr_matrix 计算效率高
eye = np.eye(3)
sparse_matrix = sparse.csr_matrix(eye)  //转成csr格式的稀疏矩阵

```

