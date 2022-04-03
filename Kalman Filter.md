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

