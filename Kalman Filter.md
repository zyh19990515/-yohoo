# Kalman Filter

将$\hat{x_k}$看作估计值，$z_k$看作观测值
当使用均值滤波：  
$$ \begin{aligned}
\hat{x_k}& =\frac{1}{k}(z_1+z_2+...+z_k)  \\
		& =\frac{1}{k}(z_1+z_2+...+z_{k-1})+\frac{1}{k}(z_k)\\
		& =\frac{k-1}{k}\cdot\frac{1}{k-1}(z_1+z_2+...+z_{k-1})+\frac{1}	{k}z_k\\
		& =\frac{k-1}{k}\hat{x_{k-1}}+\frac{1}{k}z_{k-1}\\
		& =\hat{x_{k-1}}+\frac{1}{k}(z_k+\hat{x_{k-1}})\\
\end{aligned} $$
$$
\boxed{\color{green}\hat{x_{k-1}}+\frac{1}{k}(z_k+\hat{x_{k-1}}),K_k=\frac{1}{k}}
$$
$z_k$中的测量误差可确定，记为$e_{meak}$
$\hat{x_k}$中的估计误差无法确定，记为$e_{estk}$

$$
\left\{\begin{matrix}\begin{aligned}
&1.e_{est_0}=rand\\
&2.\hat{x_k}=f(e_{est_0})\\
&3.更新e_{est_k}=(1-K_k)e_{est_{k-1}}\\
\end{aligned}\end{matrix}
\right.
$$
以上为迭代过程，估计值将逐渐逼近真实值

接下来看一下估计误差与测量误差的关系
1.$ e_{est_{k-1}}>>e_{mea_k}$,$ K_k\rightarrow1$,$\hat{x_k}=z_k$  
	- 当估计误差远大于测量误差的时候，我们更愿意相信测量值  

2.$ e_{est_{k-1}}<<e_{mea_k}$,$ K_k\rightarrow0$,$\hat{x_k}=x_{k-1}$  
	- 当估计误差远小于测量误差的时候，我们更愿意相信估计值

再令
$$e_{est}~(a_1;\sigma^2_1)$$
$$e_{mea}~(a_2;\sigma^2_2)$$

由于$\hat{x_k}$是一个由上一次的观测值与估计值共同计算得出的值，因此$e=\hat{x_k}-x$的方差$\sigma_{x_k}^2$需要有$e_{est}$和$e_{mea}$计算得出
$$
\begin{aligned}
\sigma_{x_k}^2&=Var(\hat{x_{k-1}}+K_k(z_k-\hat{x_{k-1}}))\\
&=Var((1-K_k)\hat{x_{k-1}}+K_kz_k)\\
&=(1-K_k)^2Var(\hat{x_{k-1}})+K_k^2z_k\\
&=(1-K_k)^2\sigma_1^2+K_k^2\sigma_2^2
\end{aligned}
$$

我们希望$\sigma_{x_k}^2$达到最小，所以要找到其极值点所对应的$K_k$值
$$
\begin{aligned}
\frac{d\sigma_{x_k}^2}{dK_k}&=\sigma_1^2(K_k-1)+K_k\sigma_2^2=0
\end{aligned}
$$

求得$K_k=\frac{\sigma_1^2}{\sigma_1^2+\sigma_2^2}$
再将$K_k$带入，即可得到$\sigma_{x_k}^2$

2022/4/2

##状态空间表达式

**状态方程：$x_k=Ax_{k-1}+Bu_{k-1}+w_k$**
**观测方程：$z_k=cx_k+v_k$**
其中$w_k$，$v_k$分别为噪声与输入噪声
$w_k$不可测，其概率满足$P(w)~(0,Q)$，其中0为期望，Q为协方差矩阵
$Q=E[w\quad w^T]$

>若$x_k\rightarrow\begin{bmatrix}x1\\x2\end{bmatrix}$,其误差矩阵为$\begin{bmatrix}w_1\\w_2\end{bmatrix}$
>$$\begin{aligned}&E[\begin{bmatrix}w_1\\w_2\end{bmatrix}\cdot\begin{bmatrix}w_1 \;w_2\end{bmatrix}]\\&=E[\begin{bmatrix}&w_1^2\quad w_1w_2\\&w_2w_1\quad w_2^2\end{bmatrix}]\\&=\begin{bmatrix}&E(w_1^2)\quad E(w_1w_2)\\&E(w_2w_1)\quad E(w_2^2)\end{bmatrix}\end{aligned}$$
>>$Var(w)=E(w^2)-E^2(w)$,其中$E^2(w)=0$, 所以

>$$=\begin{bmatrix}&\sigma w_1^2\quad \sigma w_1\sigma w_2\\&\sigma w_2\sigma w_1\quad \sigma w_2^2\end{bmatrix}$$
以上解释了误差服从的概率分布的协方差矩阵的由来

##先验估计与后验估计

先验估计：
$$\begin{aligned}
&\hat{x_k^-}=Ax_{k-1}+Bu_{k-1}\\
&z_k=Cx_k\quad \rightarrow \hat{x_{k_{mea}}}=C^{-1}z_k
\end{aligned}$$
先验估计忽略了误差的存在，通过计算得出估计值，通过观测值可以反推出基于观测值下的估计值

后验估计：
$$
\hat{x_k}=\hat{x_k^-}+G(C^-1z_k-x_k^-)\quad \left\{\begin{matrix}G=0:\hat{x_k}=\hat{x_k^-}\\G=1:\hat{x_k}=C^{-1}z_k\end{matrix}\right.
$$
其中$G=K_kC$
$$
\hat{x_k}=\hat{x_k^-}+K_k(z_k-C\hat{x_k^-})
$$
**上式为卡尔曼滤波器**
**目标：**寻找$K_k$，使$\hat{x_k} \rightarrow x_k$
引入$e_k$
$$
e_k=x_k-\hat{x_k}\\
P(e_k)~(0,P)
$$
由于e为独立事件，协方差为0，则目标可理解为：找到$K_k$使P的迹最小
从而得到**卡尔曼增益**：$\boxed{K_k=\frac{P^-C^T}{CP^-C^T+R}}\quad$ $\color{red}\bigstar$

综合来看，卡尔曼滤波共有5个公式
$$
\begin{aligned}
\left\{\begin{matrix}&\hat{x_k}=A\hat{x_{k-1}}+Bu_{k-1}\\&P_k=AP_{k-1}A^T+Q\\&\hat{x_k}=\hat{x_k^-}+K_k(z_k-Cx_k^-)\\&P_k=(I-K_kC)P_k^-\\&K_k=\frac{P^-C^T}{CP^-C^T+R}
\end{matrix}\right.
\end{aligned}
$$