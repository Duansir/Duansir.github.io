---
layout: post
title: 姿态更新
categories: 姿态解算
description: 学习飞控算法的第一篇笔记。
keywords: 姿态 
---

## ●欧拉角法

欧拉角与机体角速度关系：

​      
$$
\begin{bmatrix} 
  w_x \\
  w_y \\
  w_z 
\end{bmatrix}=  
 \begin{bmatrix} 
   1 & 0 & -\sin{\theta} \\
   0 & \cos{\phi} & \cos{\theta}\sin{\phi}\\ 
   0 & -\sin{\phi} & \cos{\theta}\cos{\phi} 
 \end{bmatrix}
 \begin{bmatrix} 
   \dot{\phi} \\
   \dot{\theta} \\
   \dot{\psi}  \\
 \end{bmatrix}
$$


矩阵求逆，得欧拉角微分方程：

​         
$$
\begin{bmatrix} 
   \dot{\phi} \\
   \dot{\theta} \\
   \dot{\psi} 
 \end{bmatrix} = 
  \begin{bmatrix} 
    1 & \tan{\theta}\sin{\phi} & \tan{\theta}\cos{\phi} \\
    0 & \cos{\phi} & -\sin{\phi}\\ 
    0 & \cfrac{\sin\phi}{\cos{\theta}} & \cfrac{\cos\phi}{\cos\theta} 
  \end{bmatrix}
  \begin{bmatrix} 
   w_x \\
   w_y \\
   w_z \\
  \end{bmatrix}
$$


根据这个方程，只有在三个角度非常小的时候，才有：


$$
\begin{bmatrix} 
  w_x \\
  w_y \\
  w_z 
\end{bmatrix}=
\begin{bmatrix} 
  \dot{\phi} \\ 
  \dot{\theta} \\ 
  \dot{\psi}\\ 
\end{bmatrix}
$$


&emsp;&emsp;这就是为什么我们建模的时候会强调一句，假设角度非常小。只有在角度非常小的时候，角速度的积分才会等于欧拉角。 

&emsp;&emsp;欧拉角方法直观、简便，可直接解算出欧拉角，而且解算过程中没有非正交化误差。然而欧拉角法计算过程中涉及较多三角函数运算，使得计算量比较大，在工程实际中不好实现。而且因微分方程中含有正切函数，在俯仰角为90°时，方程将会出现奇点。<u>所以这种方法只适用于水平姿态变化不大的情况，而不适用于全姿态运载体的姿态确定</u>。

## ●方向余弦矩阵——DCM（通常也称为旋转矩阵）

旋转矩阵的微分方程，这个公式表示了机体角速度与姿态导数之间的关系。


$$
\frac{dR_b^e}{dt} = {dR_b^e}[^bω]×
$$

$$
其中[^bω]×≜   
  \begin{bmatrix} 
    0 & w_z & w_y \\
    w_z & 0 & -w_x\\ 
    -w_y & w_x & 0 \\
  \end{bmatrix}
  是斜对称矩阵
$$

   从机体坐标系到地球固连坐标系的旋转矩阵为：


$$
\begin{align}
{R_b^e} 
   & =  (R_e^b)^{-1}    \\
   & =  R_z^{-1}(\psi)R_y^{-1}(\theta)R_x^{-1}(\phi)\\
   & =  R_z^{T}(\psi)R_y^{T}(\theta)R_x^{T}(\phi)\\
   & =
       \begin{bmatrix} 
           cos\theta{cos\psi} & {cos\psi}{sin\theta}{sin\phi}-{sin\psi}{cos\phi} &                {cos\psi}{sin\theta}{cos\phi}+{sin\psi}{sin\phi} \\
           cos\theta{sin\psi} & {sin\psi}{sin\theta}{sin\phi}+{cos\psi}{cos\phi} &                {sin\psi}{sin\theta}{cos\phi}-{cos\psi}{sin\phi} \\
           -sin\theta & sin\phi{cos\theta} & cos\phi{cos\theta} \\
        \end{bmatrix}
\end{align}
$$
​        

&emsp;&emsp;方向余弦法对姿态矩阵微分方程作求解，避免了欧拉角法中方程的退化问题，<u>可全姿态工作</u>。但姿态矩阵微分方程实质上是包含九个未知量的线性微分方程组，与四元数法相比，计算量大，实时计算困难，所以工程上并不实用。

## ●旋转矢量法

&emsp;&emsp;在力学中，刚体的有限转动是不可交换的[^1]。在利用毕卡逼近法求方向余弦矩阵时用到了角速度矢量的积分，而当刚体不是定轴转动时，ω的方向是随时间变化的，因此对角速度矢量进行积分,只有在积分区间很小时，ω近似看作方向不变，才近似成立，这就不可避免地引入了不可交换误差。显然，采样周期必须很小，否则，计算结果中会有较大的不可交换误差，而采样周期太小，使计算机实时计算的工作量增大。为有效减小不可交换误差，1971年John E.Bortz提出了等效旋转矢量概念。

​        等效旋转矢量微分方程：	

$$
\dot{\Phi}= w + \cfrac{1}{2}{\Phi} \times{w} + \cfrac{1}{\Phi^2}(1-{\Phi}\cfrac{sin\Phi}{2(1-cos\Phi)})\Phi \times(\Phi\times w)
$$
​         

​        将上式第三项系数按泰勒级数展开后，略去高次项，可得工程常用的近似微分方程：	
$$
\dot{\Phi}= w + \cfrac{1}{2}{\Phi} \times{w} + \cfrac{1}{12} \Phi \times(\Phi\times w)
$$
​         

&emsp;&emsp;从上式可以看出,旋转矢量的导数等于ω再加上两个修正项，而修正项反映了不可交换误差产生的影响。在上式中根据角速度ω求解出等效旋转矢量Φ，用Φ代替四元数解式中的[Δθ]，则可消除计算的四元数中的不可交换误差。 

&emsp;&emsp;旋转矢量法中的多子样算法精度高，且针对最容易产生算法漂移误差的圆锥运动有优化算法。然而，<u>旋转矢量法对噪声比较敏感</u>，在陀螺精度不高或干扰较重的情况下算法精度下降极快，以至于不如四元数法的四阶龙格库塔算法。另外<u>旋转矢量法以角增量的形式展开</u>，比较适用于激光或光纤陀螺。而MEMS陀螺直接输出角速率，且精度较低。因此，对于基于MEMS传感器的惯性测量单元旋转矢量法并不实用。<u>特别适用于角机动频繁激烈或存在严重角振动的运载体姿态更新</u>。同时，<u>旋转矢量法的计算量比四元数法大约增加了 30%</u>。因此，等效旋转法目前在导航系统中的应用较少。

## ●四元数法

### 一、四元数微分方程

四元数的实时更新可通过求解四元数微分方程而得到，四元数微分方程为：	  


$$
\dot{Q}=\frac{1}{2}[w]Q
$$

$$
\begin{bmatrix} 
   \dot{q_0} \\
   \dot{q_1} \\
   \dot{q_2} \\
   \dot{q_3}  
  \end{bmatrix} = 
  \frac{1}{2}
  \begin{bmatrix} 
    0 & -w_x & -w_y & -w_z\\
    w_x & 0 & w_z & -w_y\\
    w_y & -w_z & 0 & w_x \\
    w_z & w_y & -w_x & 0 
  \end{bmatrix}
  \begin{bmatrix} 
   q_0 \\
   q_1 \\
   q_2 \\
   q_3  
  \end{bmatrix}
$$

### 二、常微分方程的数值解法

#### 1、毕卡近似法（<u>角增量</u>）

$$
Q(t_{k+1})=e^{\frac{1}{2}\int_{t_k}^{t_{k+1}} [w]dt}Q(t_k)
$$

$$
\begin{align}
令[\Delta\theta]=\int_{t_k}^{t_{k+1}} [w]dt 
& = 
 \begin{bmatrix} 
    0 & -w_x & -w_y & -w_z\\
    w_x & 0 & w_z & -w_y\\
    w_y & -w_z & 0 & w_x \\
    w_z & w_y & -w_x & 0 
  \end{bmatrix} dt \\&=
  \begin{bmatrix} 
    0 & -\Delta\theta_x & -\Delta\theta_y & -\Delta\theta_z\\
    \Delta\theta_x & 0 & \Delta\theta_z & -\Delta\theta_y\\
    \Delta\theta_y & -\Delta\theta_z & 0 & \Delta\theta_x \\
    \Delta\theta_z & \Delta\theta_y & -\Delta\theta_x & 0 
  \end{bmatrix}
  \end{align}
$$

$$
则Q(t_{k+1})=e^{\frac{1}{2}[\Delta\theta]}Q(t_k)
$$

$$
将e^{\frac{1}{2}[\Delta\theta]}按泰勒级数展开
$$

$$
\begin{align}
e^{\frac{1}{2}[\Delta\theta]} &=I 
+ \frac{1}{1!}({\frac{[\Delta\theta]}{2}})^1 
+ \frac{1}{2!}({\frac{[\Delta\theta]}{2}})^2
+ \frac{1}{3!}({\frac{[\Delta\theta]}{2}})^3
+ \cdots
+ \frac{1}{n!}({\frac{[\Delta\theta]}{2}})^n
+ \cdots \\
&= Icos\frac{\Delta\theta}{2}
+ \frac{[\Delta\theta]}{\Delta\theta}sin\frac{\Delta\theta}{2}
\end{align}
$$

#####  一阶近似

$$
Q(t_{k+1})=(I+\frac{[\Delta\theta]}{2})Q(t_k)
$$

#####  二阶近似

$$
Q(t_{k+1})=[I(1-\frac{\Delta\theta^2}{8})+\frac{[\Delta\theta]}{2}]Q(t_k)
$$

##### 三阶近似

$$
Q(t_{k+1})=[I(1-\frac{\Delta\theta^2}{8})+(\frac{1}{2}-\frac{\Delta\theta^2}{48})[\Delta\theta]]Q(t_k)
$$

#####  四阶近似

$$
Q(t_{k+1})=[I(1-\frac{\Delta\theta^2}{8}+\frac{\Delta\theta^4}{384})+(\frac{1}{2}-\frac{\Delta\theta^2}{48})[\Delta\theta]]Q(t_k)
$$

$$
其中\Delta\theta_x、\Delta\theta_y、\Delta\theta_z为陀螺在[t_k,t_{k+1}]采样时间间隔内的角增量。
$$

$$
\Delta\theta^2=\Delta\theta_x^2+\Delta\theta_y^2+\Delta\theta_z^2
$$

#####  一、二、三、四阶毕卡逼近算法实现

```
一阶近似算法
q0 = q0 + (-q1*gx - q2*gy - q3*gz)*halfT;
q1 = q1 + (q0*gx + q2*gz - q3*gy)*halfT;
q2 = q2 + (q0*gy - q1*gz + q3*gx)*halfT;
q3 = q3 + (q0*gz + q1*gy - q2*gx)*halfT; 

二阶近似算法
delta2 = (gx*gx + gy*gy + gz*gz)*T*T;

q0 = q0_last*(1-delta2/8) + (-q1_last*gx - q2_last*gy - q3_last*gz)*halfT;
q1 = q1_last*(1-delta2/8) + (q0_last*gx + q2_last*gz - q3_last*gy)*halfT;
q2 = q2_last*(1-delta2/8) + (q0_last*gy - q1_last*gz + q3_last*gx)*halfT;
q3 = q3_last*(1-delta2/8) + (q0_last*gz + q1_last*gy - q2_last*gx)*halfT;

三阶近似算法
delta2 = (gx*gx + gy*gy + gz*gz)*T*T;

q0 = q0_last*(1-delta2/8) + (-q1_last*gx - q2_last*gy - q3_last*gz)*T*(0.5 - delta2/48);
q1 = q1_last*(1-delta2/8) + (q0_last*gx + q2_last*gz - q3_last*gy)*T*(0.5 - delta2/48);
q2 = q2_last*(1-delta2/8) + (q0_last*gy - q1_last*gz + q3_last*gx)*T*(0.5 - delta2/48);
q3 = q3_last*(1-delta2/8) + (q0_last*gz + q1_last*gy - q2_last*gx)*T*(0.5 - delta2/48);

四阶近似算法
delta2 = (gx*gx + gy*gy + gz*gz)*T*T;

q0 = q0_last*(1 - delta2/8 + delta2*delta2/384) + (-q1_last*gx - q2_last*gy - q3_last*gz)*T*(0.5 - delta2/48);
q1 = q1_last*(1 - delta2/8 + delta2*delta2/384) + (q0_last*gx + q2_last*gz - q3_last*gy)*T*(0.5 - delta2/48);
q2 = q2_last*(1 - delta2/8 + delta2*delta2/384) + (q0_last*gy - q1_last*gz + q3_last*gx)*T*(0.5 - delta2/48);
q3 = q3_last*(1 - delta2/8 + delta2*delta2/384) + (q0_last*gz + q1_last*gy - q2_last*gx)*T*(0.5 - delta2/48);
```

&emsp;&emsp;捷联陀螺的输出一般情况下是采样间隔内的角增量，为避免噪声的微分放大，应直接应用角增量来确定四元数，而不必将角增量换算成角速度 。

#### 2、龙格库塔法（Runge-Kutta）（<u>角速度</u>）

​        龙格库塔是一种用于非线性常微分方程中的一类隐式或者显式迭代方法，是一种用于数值求解微分方程的高精度单步算法，由于此法精度高且是单步离散的算法，便于用计算机实现，故在工程上应用非常的广泛。

​        该方法主要是在已知方程导数和初值的前提下，利用计算机进行计算迭代，完成对微分方程的求解。令初值问题表述如下：


$$
\left\{\begin{array}\~y^{'}(t)=f(t,y(t)) \\y(t_0)=y_0 \\ \end{array}\right.
$$
​	

​       取时间间隔
$$
h = t_{n+1}-t_n
$$
则

##### 一阶龙格库塔：对应于“一阶精度欧拉公式”

$$
\left\{\begin{array}{l}y_{n+1}=y_n+hK_1
\\K_1=f(x_n,y_n)
\end{array}\right.
$$



##### 二阶龙格库塔（中点公式）

$$
\left\{\begin{array}{l}y_{n+1}=y_n+hK_2
\\K_1=f(x_n,y_n)
\\K_2=f(x_n+\frac{h}{2},y_n+\frac{h}{2}K_1)
\end{array}\right.
$$

##### 三阶龙格库塔

$$
\left\{\begin{array}{l}y_{n+1}=y_n+\frac{h}{6}（K_1+4K_2+K_3）
\\K_1=f(x_n,y_n)
\\K_2=f(x_n+\frac{h}{2},y_n+\frac{h}{2}K_1)
\\K_3=f(x_n+h,y_n-hK_1+2hK_2)
\end{array}\right.
$$

##### 四阶龙格库塔

$$
\left\{\begin{array}{l}y_{n+1}=y_n+\frac{h}{6}(K_1+2K_2+2K_3+K_4)\\K_1=f(x_n,y_n)\\K_2=f(x_n+\frac{h}{2},y_n+\frac{h}{2}K_1)\\K_3=f(x_n+\frac{h}{2},y_n+\frac{h}{2}K_2)\\K_4=f(x_n+h,y_n+hK_3)\end{array}\right.
$$

eg：四元数的四阶龙格库塔法

​           在采样周期为 T 的时间间隔内 
$$
\left\{\begin{array}{l}Q(t+T)=Q(t)+\frac{T}{6}(K_1+2K_2+2K_3+K_4)
\\K_1=\frac{1}{2}[w(t)]Q(t)
\\K_2=\frac{1}{2}[w(t+\frac{T}{2})](Q(t)+\frac{K_1}{2}T)
\\K_3=\frac{1}{2}[w(t+\frac{T}{2})](Q(t)+\frac{K_2}{2}T)
\\K_4=\frac{1}{2}[w(t+T)](Q(t)+K_3T)\end{array}\right.
$$
​      

&emsp;&emsp;因为四阶龙格-库塔法会存在" w(t+T/2) "和" w(t+T) "项，需要知道 t+T/2 和 t+T 时刻的角速度才可以求解。使用一阶龙格-库塔法会更为简单，因为只需要 w(t)，也就是直接使用陀螺仪读出來的角速度來更新四元數。

&emsp;&emsp;四元数法只需求解四个未知量的线性微分方程组，计算量比方向余弦小，且算法简单，易于操作，是较实用的工程方法。但四元数实质上是旋转矢量算法中的单子样算法，对有限转动引起的不可交换误差的补偿不够。<u> 所以只适用于低动态运载体，如运输机等的姿态解算。而对高动态运载体姿态解算中的算法漂移会十分严重</u> 。

##### 二、三、四阶龙格库塔算法实现

```
下面给出c语言实现的代码：

#include<stdlib.h> 
#include<stdio.h> 
/*n表示几等分，n+1表示他输出的个数，方程组中的h为差分的步长*/ 
int RungeKutta(double y0,double a,double b,int n,double *x,double *y,int style,double (*function)(double,double)) 
{ 
	double h=(b-a)/n,k1,k2,k3,k4; 
	int i; 
	x[0]=a; 
	y[0]=y0; 
	switch(style) 
	{ 
	case 2: 
		for(i=0;i<n;i++) 
		{ 
			x[i+1]=x[i]+h; 
			k1=function(x[i],y[i]); 
			k2=function(x[i]+h/2,y[i]+h*k1/2); 
			y[i+1]=y[i]+h*k2; 
		} 
		break; 
	case 3: 
		for(i=0;i<n;i++) 
		{ 
			x[i+1]=x[i]+h; 
			k1=function(x[i],y[i]); 
			k2=function(x[i]+h/2,y[i]+h*k1/2); 
			k3=function(x[i]+h,y[i]-h*k1+2*h*k2); 
			y[i+1]=y[i]+h*(k1+4*k2+k3)/6; 
		} 
		break; 
	case 4: 
		for(i=0;i<n;i++) 
		{ 
			x[i+1]=x[i]+h; 
			k1=function(x[i],y[i]); 
			k2=function(x[i]+h/2,y[i]+h*k1/2); 
			k3=function(x[i]+h/2,y[i]+h*k2/2); 
			k4=function(x[i]+h,y[i]+h*k3); 
			y[i+1]=y[i]+h*(k1+2*k2+2*k3+k4)/6; 
		} 
		break; 
	default: 
		return 0; 
	} 
	return 1; 
} 
double function(double x,double y) 
{ 
	return y-2*x/y; 
} 
//例子求y'=y-2*x/y(0<x<1);y0=1; 

int main() 
{ 
	double x[6],y[6]; 
	printf("用二阶龙格-库塔方法\n"); 
	RungeKutta(1,0,1,5,x,y,2,function); 
	for(int i=0;i<6;i++) 
		printf("x[%d]=%f,y[%d]=%f\n",i,x[i],i,y[i]); 
	printf("用三阶龙格-库塔方法\n"); 
	RungeKutta(1,0,1,5,x,y,3,function); 
	for(i=0;i<6;i++) 
		printf("x[%d]=%f,y[%d]=%f\n",i,x[i],i,y[i]); 
	printf("用四阶龙格-库塔方法\n"); 
	RungeKutta(1,0,1,5,x,y,4,function); 
	for(i=0;i<6;i++) 
		printf("x[%d]=%f,y[%d]=%f\n",i,x[i],i,y[i]); 
	return 1;
}
```

![](/images/posts/AA1.jpg)



参考文献：

1、《基于角速率与角增量的捷联惯导姿态算法 》

2、《捷联惯导系统姿态算法比较 》

3、《导弹姿态矩阵更新算法推导和仿真验证 》

4、《捷联姿态计算中方向余弦与四元数法分析比较 》

5、《多旋翼无人机的姿态与导航信息融合算法研究》

6、《基于MEMS传感器的嵌入式捷联惯性测量单元及其应用研究》

7、《高精度捷联式惯性导航系统算法研究》

8、《水下姿态解算的算法研究》

9、《低成本GPS_SINS组合导航系统》

参考资料：

[4阶经典龙格库塔公式求解微分方程][1]

[龙格库塔C 语言编程实现][2]

[欧拉积分、中点积分与龙格－库塔积分][3]

[常微分方程数值解->龙格－库塔方法][4]

[^1]: 在力学中,刚体的有限转动不可交换的。如果刚体先绕x轴转动90°,再绕y轴转动90°,和先绕y轴转动90°,再绕x轴转动90°。两种情况的结果显然是不同的。这就是转动的不可交换性。这个转动的不可交换性决定了转动不是矢量,也就是两次以上的转动不能相加。

[1]: http://59.110.0.194/article/qq_26025363/53433492
[2]: http://59.110.0.194/article/on_way_/8836758
[3]: http://www.liuxiao.org/2018/05/%E6%AC%A7%E6%8B%89%E7%A7%AF%E5%88%86%E3%80%81%E4%B8%AD%E7%82%B9%E7%A7%AF%E5%88%86%E4%B8%8E%E9%BE%99%E6%A0%BC%EF%BC%8D%E5%BA%93%E5%A1%94%E7%A7%AF%E5%88%86/
[4]: https://www.bb.ustc.edu.cn/jpkc/xiaoji/szjsff/jsffkj/chapt8_2_1.htm







