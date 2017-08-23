# SVM
## 基础
**SVM的核心思想**： 找到一个超平面$\textbf{w}^T\textbf{x} + b = 0$将两类数据分离，且数据距离超平面有一定的间隔（margin）。<br>
**推导与理解**：样本$x_i$到超平面$\textbf{w}^T\textbf{x} + b = 0$的距离为
<p>

$$
\gamma_i = \frac{y_i(\textbf{w}^Tx_i + b)}{||\textbf{w}||}
$$

几何间隔(Geometric Margin)为所有样本点中到超平面距离最小值（SVM目的在于让这个最近的距离最大化）：
$$
\gamma = \min\limits_{i=1,2,...,N}\frac{y_i(\textbf{w}^Tx_i + b)}{||\textbf{w}||}
$$

函数间隔(Functional Margin): 所有样本点中到超平面的未经规范化的距离的最小值
$$
\hat{\gamma_i} = y_i(\textbf{w}^Tx_i + b)
$$

$$
\hat{\gamma} = \min\limits_{i=1,2,...,N}\hat{\gamma_i}
$$

</p>

因此，SVM目标函数为
<p>

$$
\max\limits_{\textbf{w}, b} \gamma
$$
$$
s.t.{~~~} \frac{y_i(\textbf{w}^Tx_i + b)}{||\textbf{w}||} \ge \gamma, ~~i = 1, 2,...,N
$$

因为$\gamma = \frac{\hat{\gamma}}{||\textbf{w}||}$,所以目标函数可转化为<br>
$$
\max\limits_{\textbf{w}, b} \frac{\hat\gamma}{||\textbf{w}||}
$$
$$
s.t.{~~~} y_i(\textbf{w}^Tx_i + b)\ge\hat\gamma, ~i=1,2,...,N
$$

因为函数间隔$\hat\gamma$不影响最优化问题的解，假设将$\textbf{w}$和$b$按比例变为$\lambda\textbf{w}$和$\lambda b$，则函数间隔为$\lambda\hat\gamma$。函数间隔的这一改变并不影响上面最优化问题的不等数约束，也不影响目标函数优化的结果，即变化前后是等价的最优化问题。所以，可以令$\hat\gamma=1$。

<b>理解</b>: 换个角度思考下，因为成比例$\lambda$改变$\textbf{w}, b$，$\hat\gamma$也会成比例改变，所以如果取$\lambda = \frac{1}{\hat\gamma}$，最优化问题也是不变的。这样就可以理解为什么令$\hat\gamma=1$是可以的了。

则目标函数可以写为，
$$
\max\limits_{\textbf{w}, b} \frac{1}{||\textbf{w}||}
$$
$$
s.t.{~~~} y_i(\textbf{w}^Tx_i + b)\ge1, ~i=1,2,...,N
$$
转化为最小化问题为，
$$
\min\limits_{\textbf{w}, b}\frac{1}{2}||\textbf{w}||^2
$$
$$
s.t.{~~~} y_i(\textbf{w}^Tx_i + b)\ge1, ~i=1,2,...,N
$$
(注：$||\textbf{w}||^2 = \textbf{w}^T\textbf{w}$)

该优化问题为凸优化问题(有约束的)。

<b>凸优化问题</b>: 指约束优化问题
$$
\min\limits_{w} f(w)
$$
$$
s.t.{~~~} g_i(w) \le 0, {~~}i=1,2,...,k
$$
$$
{~~~~~~~} h_i(w) = 0, {~~}i=1,2,...,l
$$
其中，目标函数$f(w)$和约束函数$g_i(w)$都是$\textbf{R}^n$上连续可微的凸函数，约束函数$h_i(w)$是$\textbf{R}^n$上的仿射函数。
(注：当函数满足$f(x) = ax + b,~a\in\textbf{R}^n,~b\in\textbf{R}^n,~x\in\textbf{R}^n$时，其为仿射函数。即函数为一次函数，对应空间中对向量线性变换再平移。)
</p>

**最优化求解**: 利用拉格朗日对偶性(Lagrange duality)。在约束最优化问题中，常常利用对偶性将原始问题转化为对偶问题，通过解对偶问题而得到原始问题的解。
<br>
**原始问题**: 假设$f(x), c_i(x), h_j(x)$是定义在$\textbf{R}^n$上的连续可微函数，考虑约束最优化问题
<p>

$$
\min\limits_{w} f(w)
$$
$$
s.t.{~~~} c_i(w) \le 0, {~~}i=1,2,...,k
$$
$$
{~~~~~~~} h_j(w) = 0, {~~}j=1,2,...,l
$$
此约束最优化问题为原始问题。
引入
</p>



## 核函数
当数据是非线性可分时（无法用$\textbf{w}^T\textbf{x} + b$超平面分开），通常需要进行一个非线性变换，将非线性问题转化为线性问题。

Kernel是用来计算在高维特征空间中两个向量内积(dot product)。假设我们有从$ R^n \Rightarrow R^m $的映射$\varphi$, 将向量$\textbf{x}, \textbf{y} y$从特征空间$R^n$映射到$R^m$。在$R^m$中，$\textbf{x}$和$\textbf{y}$的内积为$\varphi(\textbf{x})^T\varphi(\textbf{y} )$。核函数$K$定义为$K(\textbf{x}, \textbf{y}) = \varphi(\textbf{x})^T\varphi(\textbf{y})$。

定义核函数的好处（为什么使用核方法？）在于，核函数提供了在不知道是什么空间和是什么映射$\varphi$时计算内积。

例如：定义polynomial kernel
$$K(\textbf{x}, \textbf{y}) = (1 + \textbf{x}^T\textbf{y})^2$$

$$
\textbf{x}, \textbf{y} \in R^2, \textbf{x} = (x_1, x_2), \textbf{y} = (y_1, y_2)
$$

看起来$K$并没有表示出这是什么映射，但是：

$$
K(\textbf{x}, \textbf{y}) = (1 + \textbf{x}^T \textbf{y})^2 = (1 + x_1y_1 + x_2y_2)^2 = 1 + x_1^2y_1^2 + x_2^2y_2^2 + 2x_1y_1 + 2x_2y_2 + 2x_1x_2y_1y_2
$$

相当于将$\textbf{x}$从$(x_1, x_2)$映射为$(1, x_1^2, x_2^2. \sqrt{2}x_1, \sqrt{2}x_2, \sqrt{2}x_1x_2)$，将$\textbf{y}$从$(y_1, y_2)$映射为$(1, y_1^2, y_2^2. \sqrt{2}y_1, \sqrt{2}y_2, \sqrt{2}y_1y_2)$

即

$$\varphi(\textbf{x}) = \varphi(x_1, x_2) = (1, x_1^2, x_2^2. \sqrt{2}x_1, \sqrt{2}x_2, \sqrt{2}x_1x_2)$$

$$\varphi(\textbf{y}) = \varphi(y_1, y_2) = (1, y_1^2, y_2^2. \sqrt{2}y_1, \sqrt{2}y_2, \sqrt{2}y_1y_2)$$

则可知
$$K(\textbf{x}, \textbf{y}) = (1 + \textbf{x}^T \textbf{y})^2 = \varphi(\textbf{x})^T\varphi(\textbf{y})$$

**理解**：对于一些特征，其在低维空间中是线性不可分，但是如果将其映射到高维空间中，就可能会是线性可分的。处理的步骤为：特征 -> 映射到高维空间(使用映射函数$\varphi$) -> 分类算法(定义loss function，多表达为内积的形式)。采用核函数的优点在于，不必确定具体的映射函数$\varphi$是什么，不必显示的计算每个特征向量在映射到高维空间的表达是什么样的，而可以直接用低维空间的数据(坐标值)去计算得出向量在高维空间中内积的结果。

## SVM常用核函数
- Linear kernel
- Polynomial kernel
    $$
    K(\textbf{x}, \textbf{z}) = (\textbf{x} \cdot \textbf{z} + 1)
    $$

- RBF kernel
    A **radial basis function(RBF)** is a real-valued function whose value depends only on the distance from the origin, so that $\phi(\textbf{x}) = \phi(\textbf{||x||})$; or on the distance from some other point $c$, called a *center*, so that $\phi(\textbf{x}, c) = \phi(||\textbf{x - c}||)$.
    <br>
    One common RBF kernel is Gaussian kernel
    $$
    K(\textbf{x}, \textbf{z}) = exp(-\frac{||\textbf{x} - \textbf{z}||^2}{2\sigma^2})
    $$

**Kernel的选择**
- 如果特征维数很高（维数高往往线性可分），可以采用*Least Square*或者*线性核的SVM*
- 如果特征维数较小，而且样本数量一般，不是很大，可以选用*高斯核的SVM*
- 如果特征维数较小，而样本数量很多，需要手工添加一些feature使其变为第一种情况（虽然特征维数较小，但是样本数量太大，SVM需要计算两两样本特征的内积，高斯核计算量过多，所以不适合使用）。

***Reference***
- [How to intuitively explain what a kernel is?](https://stats.stackexchange.com/questions/152897/how-to-intuitively-explain-what-a-kernel-is)
- [知乎：SVM的核函数如何选取？](https://www.zhihu.com/question/21883548)