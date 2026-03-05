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

//np.random
np.random.seed(n)
np.random.rand()  //[0, 1) 随机浮点数
np.random.rand(2)  //array([..., ...]) 一行二列数组 元素[0, 1)
np.random.rand(2, 2)  //array([[..., ...], [..., ...]]) 二行二列数组 元素[0, 1)
np.random.randn()  //同上按照参数返回一个或一组服从标准正态分布的随机样本值 注 标准正态分布以0为均值1为标准差
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
//coo_matrix 坐标格式 适合数据交换
dense = np.array([[1, 0], [0, 2], [3, 0]])
sparse_matrix = sparse.coo_matrix(dense)  //直接稠密矩阵转coo
row = np.array([0, 0, 1, 2, 2, 3])
col = np.array([0, 2, 1, 1, 2, 3])
data = np.array([1, 2, 3, 4, 5, 6])
sparse_matirx = sparse.coo_matrix((data, (row, col)), shape=(4, 4))  //三元组创建 shape指稀疏矩阵形状
//csr_matrix 压缩行格式 行操作效率高
dense = np.array([[1, 0], [0, 2], [3, 0]])
sparse_matrix = sparse.csr_matrix(dense)  //直接稠密矩阵转csr
indptr = np.array([0, 2, 3, 6])  //默认开始为0 第一行有2-0个值 二行3-2 三行6-3
indices = np.array([0, 2, 2, 0, 1, 2])  //第0行第一个值在第0列 0行第二个值在2列 ...
data = np.array([1, 2, 3, 4, 5, 6])
sparse_matrix = sparse.csr_matrix((data, indices, indptr), shape=(3, 3))
//csc_matrix 压缩列格式 列操作效率高
dense = np.array([[1, 0], [0, 2], [3, 0]])
sparse_matrix = sparse.csc_matrix(dense)  //直接稠密矩阵转csc
indptr = np.array([0, 1, 2, 4, 6])  //默认开始为0 第一列有1-0个值 二列2-1 三列4-2 四列6-4
indices = np.array([0, 2, 1, 2, 0, 3])
data = np.array([1, 3, 4, 5, 2, 6])
sparse_matrix = sparse.csc_matrix((data, indices, indptr), shape=(4, 4))
//lil_matrix 适合创建、动态添加和修改 内存开销大 计算效率低
sparse_matrix = sparse.lil_matrix((4, 4))  //创建空矩阵
sparse_matrix = sparse.lil_matrix(dense)  //稠密转lil
sparse_matrix[0, 0] = 1  //动态添加或修改
//dok_matrix 字典格式 O(1)单个元素访问 易修改和更新
sparse_matrix = sparse.dok_matrix((4, 4))  //创建空矩阵
sparse_matrix = sparse.dok_matrix(dense)  //稠密转dok
sparse_matrix[0, 0] = 1  //动态添加或修改
//dia_matrix 对角存储格式 适合带状矩阵 不适合稀疏但非带状矩阵 创建元素困难
sparse_matrix = sparse.dia_matrix(dense)  //稠密转dia
```

#### matplotlib

```
import matplotlib.pyplot as plt  //导入绘图核心模块
Figure画布为底 Axes子图为具体绘制元素
plt.rcParams["font.sans-serif"] = "Microsoft YaHei"  //设置中文字体
//创建方式一
fig = plt.figure(figsize=(8, 4))  //画布宽8英寸 高4英寸
ax = fig.add_subplot(1, 1, 1)  //添加一行一列的第一个子图 仅一图
//创建方式二
fig, ax = plt.subplots(figsize=(8, 4))  //与方式一等价

//plot 折线图 实例
x = np.linspace(0, 10, 100)
y1 = np.sin(x)
y2 = np.cos(x)
fig, ax = plt.subplots(figsize=(10, 5))
ax.plot(x, y1, label="sin(x)", color="blue", linewidth=2, linestyle="-")  //实线 label需配合ax.legend()使用
ax.plot(x, y2, label="cos(x)", color="red", linewidth=1.5, linestyle="--")  //虚线
ax.set_title("正弦函数与余弦函数曲线", fontsize=15)  //标题
ax.set_xlabel("x值", fontsize=12)  //x轴标签
ax.set_ylabel("y值", fontsize=12)  //y轴标签
ax.legend(fontsize=10)  //添加图例(右上角)
ax.grid(alpha=0.3)  //添加网格线 alpha设置透明度
plt.show()

//scatter 散点图
x = np.random.randn(100)
y = 2 * x + np.random.randn(100) * 0.5
fig, ax = plt.subplots(figsize=(8, 6))
scatter = ax.scatter(
    x, y, 
    c=y,  //点的颜色由y值决定
    cmap="viridis",  //颜色映射方案（内置多种，如"coolwarm"、"rainbow"）
    s=50,  //点的大小
    alpha=0.7,  //透明度（0-1）
    edgecolors="black"  //点的边缘颜色
)
cbar = plt.colorbar(scatter)
cbar.set_label("y值", fontsize=10)  //添加颜色条
ax.set_title("x与y的散点图（带颜色映射）", fontsize=15)
ax.set_xlabel("x变量", fontsize=12)
ax.set_ylabel("y变量", fontsize=12)
ax.grid(alpha=0.3)
plt.show()

//bar 柱状图
categories = ["产品A", "产品B", "产品C", "产品D"]
values = [35, 52, 28, 41]
fig, ax = plt.subplots(figsize=(8, 5))
ax.bar(
    categories, values,
    color=["#FF6B6B", "#4ECDC4", "#45B7D1", "#FFA07A"],  //每个柱子不同颜色
    width=0.6,  //柱子宽度
    edgecolor="black"  //柱子边缘颜色
)
//在柱子顶部添加数值标签
for i, v in enumerate(values):
    ax.text(i, v + 1, str(v), ha="center", fontsize=10)  //ha="center"水平居中
ax.set_title("各产品季度销量对比", fontsize=15)
ax.set_xlabel("产品类别", fontsize=12)
ax.set_ylabel("销量（件）", fontsize=12)
ax.set_ylim(0, max(values) + 10)  //设置y轴范围（让顶部标签不溢出）
plt.show()
//barh 水平柱状图 只需将bar改为barh，x和y互换

//hist 直方图
np.random.seed(42)
data = np.random.normal(loc=50, scale=10, size=1000)  //均值50，标准差10，1000个符合正态分布的随机数
fig, ax = plt.subplots(figsize=(10, 5))
n, bins, patches = ax.hist(
    data,
    bins=20,  //区间数量（分20个桶）
    density=True,  //是否归一化（显示概率密度）
    color="#6495ED",
    alpha=0.7,
    edgecolor="black"
)
//添加正态分布曲线（更直观展示分布）
from scipy.stats import norm  //需安装scipy：pip install scipy
mu, sigma = norm.fit(data)  //拟合均值和标准差
x = np.linspace(bins[0], bins[-1], 100)
ax.plot(x, norm.pdf(x, mu, sigma), "r--", linewidth=2)  //概率密度曲线
ax.set_title("数据分布直方图（正态分布）", fontsize=15)
ax.set_xlabel("数值", fontsize=12)
ax.set_ylabel("概率密度", fontsize=12)
ax.grid(alpha=0.3, axis="y")  //只显示y轴网格
plt.show()

//pie 饼图
labels = ["直接访问", "搜索引擎", "社交媒体", "广告推广"]
sizes = [30, 40, 20, 10]  //总和100
explode = [0, 0.1, 0, 0]  //突出显示第2部分
fig, ax = plt.subplots(figsize=(7, 7))  //饼图建议用正方形画布
ax.pie(
    sizes,
    explode=explode,  //部分扇形偏离中心
    labels=labels,  //类别标签
    autopct="%1.1f%%",  //显示百分比（保留1位小数）
    shadow=True,  //添加阴影（增强立体感）
    startangle=90,  //起始角度（90度为从正上方开始）
    colors=["#FF9999", "#66B2FF", "#99FF99", "#FFCC99"]
)
ax.axis("equal")  //保证饼图是正圆形（否则可能椭圆）
ax.set_title("网站访问来源占比", fontsize=15)
plt.show()

//save
plt.savefig("my_plot.png", dpi=300, bbox_inches="tight")  //dpi设置分辨率默认100 300适合印刷 bbox_inches="tight"去除多余空白 可修改后缀设置为矢量图
plt.show()  //注 savefig必须在show()之前否则保存空白图
```

