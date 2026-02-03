
# 运算
* <font color=#39c5bb>集项,先定符号,运算高到低,树状cache</font>    
* <font color=#39c5bb>子集可推集合,集合不可推真子集</font>    
* <font color=#39c5bb>函数套函数,0级,关系,同增异减</font>     
* <font color=#39c5bb>最小值齐次同除,最大值消元开导</font>    
* <font color=#39c5bb>同时取平方结果应回归</font>    
* <font color=#39c5bb>级函数</font>：<font color=#ffa500>$ level(n) \rightarrow function \rightarrow level(n+1) $</font>       
* <font color=#39c5bb>方差 = 平方的平均 - 平均的平方,方差越大越离散</font>,<font color=#ffa500>$ s^2 = \frac{1}{n} \sum ^{n} _{i=1} {x _i ^2} - \overline{x}^2 $</font>    
* <font color=#39c5bb>和比性质</font>,<font color=#ffa500>$ \frac {a}{b} = \frac{c}{d} = \frac{a+c}{b+d} $</font>    
* <font color=#ffa500>$ \frac{(\frac{a}{b})}{(\frac{c}{d})} = \frac{ad}{bc} $</font>    
* <font color=#ffa500>$ \frac{1}{\frac{1}{a} + \frac{1}{b}} = \frac{ab}{a + b} $,乘积比和,总阻递归</font>
* <font color=#ffa500>$ \frac{1}{ab} = -\frac{1}{a - b} ( \frac{1}{a} - \frac{1}{b} ) = +\frac{1}{a + b} ( \frac{1}{a} + \frac{1}{b} ) $</font>    

---

* 因式分解：<font color=#39c5bb>余数定理,试根长除</font>    
> <font color=#39c5bb>余数定理</font>：<font color=#ffa500>$ 除(x-a),余f(a) $</font>    
> <font color=#39c5bb>$ \%(mod)$</font>：<font color=#ffa500>$ \begin{cases}
    (x+y)\%p= (x\%p + y\%p)\%p \\
    xy\%p=(x\%p \times y\%p)\%p \\
    x\%p=y\%p \Leftrightarrow x-y=pk(k \in Z) \\
\end{cases}$</font>    
> <font color=#39c5bb>费马小定理</font>：<font color=#ffa500>$ p质数,a与p互质,a^{p-1} \% p = 1\%p $</font>    
> <font color=#39c5bb>费马大定理</font>：<font color=#ffa500>$ 整数n \geq 3,a^n + b ^n = z ^n ,无整数解 $</font>    

---
* 平方式:
> <font color=#ffa500>$ \begin{cases} (a + b)^2 = a^2 + b^2 + 2ab \\
        (a - b)^2 = a^2 + b^2 - 2ab \end{cases} $</font>    

> <font color=#ffa500>$ \begin{cases} a^2 - b^2 = (a + b)(a - b) \\
        a^2 + b^2 = (a + b)^2 - 2ab \end{cases} $</font>    

---
* 立方式
> <font color=#ffa500>$ \begin{cases} (a + b)^3 = a^3 + b^3 + 3ab(a+b) \\
    (a - b)^3 = a^3 - b^3 - 3ab(a-b) \end{cases} $</font>

> <font color=#ffa500>$ \begin{cases} a^3 + b^3 = (a + b)(a^2 + b ^2 - ab) \\
    a^3 - b^3 = (a - b)(a^2 + b^2 + ab) \end{cases} $</font>

---
* 二次根的分布：<font color=#ffa500>开口、对称轴、$\Delta$、特殊点</font>

* 求根公式:
> <font color=#ffa500>$ \Delta = b^2 - 4ac, x= \frac {-b \pm \sqrt{\Delta} }{2a} $</font>    
> <font color=#ffa500>$ 二次函数对称轴处极值坐标:(\frac{-b}{2a},\frac{4ac-b^2}{4ac}) $</font>    


---

* 双重根号,令:<font color=#ffa500>$ \sqrt {x \pm \sqrt{y}} = | \sqrt{a} \pm \sqrt{b} | $,则:$ \begin{cases} a + b = x \\ 2 \sqrt{ab} = \sqrt{y} \end{cases} $,可求得 $| \sqrt{a} \pm \sqrt{b} | $</font>    


---
* 常用集合
> <font color=#ffa500>$ N= \{0,1,2,……\},N^{*}=\{1,2,……\} $</font>    
> <font color=#ffa500>$ Z= \{……,-3,-2,-1,0,1,2,3,……\} $</font>    
> <font color=#ffa500>$ R(实数),Q(有理数),C(复数a+bi,b \neq 0) $</font>    

* 常用数值
> <font color=#39c5bb>$ 11^2 = 121, 12^2=144, 13^2=169, 14^2=196, 15^2=225, 16^2=256 , 17^2=289, 18^2=324, 19^2=361 $</font>    
> <font color=#39c5bb>$ \sqrt{2} = 1.414..., \sqrt{3} =1.732 $</font>    
> <font color=#39c5bb>$ \ln{2} = 0.693... $</font>    
> <font color=#39c5bb>$ e = 2.718... $</font>     
> <font color=#39c5bb>$ \pi(rad) = 180^{。} $</font>    
> <font color=#39c5bb>$ i = \sqrt {-1} $</font>    

---

# 不等式:
* <font color=#39c5bb>调几算平</font>:<font color=#ffa500>$ (\frac { a^{-1} + b^{-1} } {2})^{-1} \leq \sqrt{ab} \leq \frac{a + b}{2} \leq \sqrt{\frac {a^2 + b^2} {2} } $</font>    

* <font color=#39c5bb>对数均值不等式</font>:<font color=#ffa500>$ \sqrt {ab} < \frac {a - b}{\ln {a} - \ln {b}} < \frac {a + b}{2} $</font>    

* <font color=#39c5bb>柯西不等式</font>:
> <font color=#39c5bb>乘积和的平方$ \leq $ 平方和的乘积</font>     
> <font color=#ffa500>$ ( \sum ^{n} _{i=1} {a_i b_i} ) ^2 \leq (\sum ^{n} _{i=1} {a_i^2}) (\sum ^{n} _{i=1} {b_i^2}) $</font>    
> 当 <font color=#ffa500>$ \frac {a_1}{b_1} = \frac {a_2}{b_2} = ... = \frac {a_n}{b_n} $时取等</font>    

* <font color=#39c5bb>绝对值不等式</font>:<font color=#ffa500>$ |a + b| \leq |a| + |b|  $</font>     

* <font color=#39c5bb>伯努利不等式</font>：<font color=#ffa500>$ 1 + nx \leq (1 + x )^n ,( x > -1, x!=0 ,n \in N ^{*}) $</font>    
> <font color=#ffa500>证:$ su(n)<su(n+1)<...>> , su(n) \rightarrow su(n+1) , su(1)成立 $</font>    
> <font color=#ffa500>$su(n): 1+nx \leq (1+x)^n $</font>    
> <font color=#ffa500>$(1+x)^{n+1} = (1+x)(1+x)^{n} \geq (1+x)(1+nx) = 1 + (1+n)x + nx^2 \geq 1  + (1+n)x $</font>    

* <font color=#39c5bb>三角不等式:$ 第三边 \leq 两边和$</font>    
> <font color=#ffa500>$ (取等为线)\sqrt{(x _1 - x _3)^2 + (y _1 - y _3)^2} \leq \sqrt{(x _1 - x _2)^2 + (y _1 - y _2)^2} + \sqrt{(x _2 - x _3)^2 + (y _2 - y _3)^2} $</font>    


---


# 函数:
> <font color=#39c5bb>一元方程乘积式,最右根正正进,负负进,奇穿偶不穿</font>     

> <font color=#39c5bb>$ 奇 \pm 奇 = 奇 ; 奇 \pm 偶 = 非 $</font>    
> <font color=#39c5bb>$ 奇 \times\div 奇 = 偶 ;  奇 \times\div 偶 = 奇 $</font>    
> <font color=#ffa500>$ \begin{cases}f(a+x) = f(b-x) ,关于 \frac{(a+x)+(b-x)}{2}对称 \\
    f(a+x) + f(b-x) = c ,关于 ( \frac{(a+x)+(b-x)}{2} , \frac{c}{2})对称 
    \end{cases}$ </font>   

> <font color=#39c5bb>周期</font>,<font color=#ffa500>$ f(x)=f(x+T)时周期为T,最小正值为最小正周期 $</font>    

> <font color=#39c5bb>指数函数底数</font>,<font color=#ffa500>$\in (0 , 1)减 ,\in (1 , + \infty )增$</font>    
> <font color=#39c5bb>对数函数底数</font>,<font color=#ffa500>$\in (0 , 1)减 ,\in (1 , + \infty )增$</font>    
> <font color=#39c5bb>幂函数指数</font>, <font color=#ffa500>$\in (0 , 1)凸 ,\in (1 , + \infty)凹$</font>    

> <font color=#ffa500>$ a ^{ \frac{m}{n} } = ^n \sqrt{a^m} $</font>    
> <font color=#ffa500>$ log _a{MN} = log _a{M} + log _a{N} $</font>    
> <font color=#ffa500>$ log _a{ \frac{M}{N} } = log _a{M} - log _a{N} $</font>    
> <font color=#ffa500>$ log _{a^m}{b^n} = \frac{n}{m} log _a{b} $</font>    
> <font color=#ffa500>$ log _a{b} = \frac{log _c{b}}{log _c{a}} $</font>    

---

# 导数公式：
<font color=#ffa500>$ (c)' = 0 $</font>    
<font color=#ffa500>$ (a^x)' =a^x \ln {a} $</font>    
<font color=#ffa500>$ (x^n)' = nx^{n-1} ,n \in N _{+}$</font>    
<font color=#ffa500>$ (e^x)' = e^x $</font>    
<font color=#ffa500>$ (\ln x)' = \frac{1}{x} $</font>    
<font color=#ffa500>$ (\log _a {x})' = \frac{1}{x \ln {a}} $</font>    
<font color=#ffa500>$ (\sin{x})' = \cos{x} $</font>    
<font color=#ffa500>$ (\cos{x})' = -\sin{x} $</font>    

* 导数运算：
<font color=#ffa500>$ 求导:一元多项式每一导少一零点 $</font>    
<font color=#ffa500>$ [f(\mu)]' = \mu'f'(\mu),0级导\times 关系导=1级导(level0:\mu,level1:f(\mu)) $</font>    
<font color=#ffa500>$ [f(x) \pm g(x)]' = f'(x) \pm g'(x) $</font>    
<font color=#ffa500>$ [f(x)g(x)]' = f'(x)g(x) + f(x)g'(x) $,前导后+后导前</font>    
<font color=#ffa500>$ [\frac{f(x)}{g(x)}]' = \frac{f'(x)g(x) - g'(x)f(x)}{g^2(x)} $,$ \frac {上导下-下导上}{下平方} $</font>    

* 超越方程变换
> <font color=#ffa500>$ x^n 和 e^x , \ln{x}的比积差 $</font>    

> <font color=#ffa500>$ x ^n \ln{x} , x ^n e ^x $</font>    
> <font color=#ffa500>$ \frac{x ^n}{e ^x} , \frac{e ^x}{x ^n} $</font>    
> <font color=#ffa500>$ \frac{\ln{x}}{x ^n} , \frac{x ^n}{\ln{x}} $</font>    
> <font color=#ffa500>$ x - \ln{x} , x - e ^x , e^x - x$</font>    

---
# 矩阵
* 高斯消元
> <font color=#ffa500>方程求解高斯消元</font>:<font color=#39c5bb>$
\begin{cases}
    化阶梯矩阵:从上往下初等变换,依次获得阶梯 \\
    消元:从阶梯下往上代入消元 \end{cases}$</font>    

* 运算
> <font color=#ffa500>同型矩阵加减</font>:<font color=#39c5bb>$ matrix1(a,b) + matrix2(a,b) = result(a,b) $</font>    
> <font color=#ffa500>矩阵数乘</font>:<font color=#39c5bb>$ k \cdot matrix(a,b) = result(a,b) $</font>    

> <font color=#ffa500>矩阵乘法</font>:<font color=#39c5bb>$ \overset{n}{\underset{i=1}{\sum}} matrix1(a,i) \cdot matrix2(i,b) = result(a,b),
    (n=matrix1行长=matrix2列长) $,
    $方阵幂乘可用指数表示$</font>    

> <font color=#ffa500>转置</font>:<font color=#39c5bb>$ result = matrix^{T},result(a,b) = matrix(b,a) $</font>    


# 极限：

<font color=#39c5bb>洛必达法则</font>:<font color=#ffa500>$\displaystyle (0/0型)或(\infty/\infty型)可用:  \lim_{x\to{n}} \frac {f(x)}{g(x)} = \lim _{ x \to {n} } \frac{f'(x)}{g'(x)} $</font>

<font color=#39c5bb>泰勒公式</font>:<font color=#ffa500>$  p(x) = \sum _{i=0} ^{n} { \frac {f^{(i)}(x _0)}  {i!} } \cdot (x-x _0)^i ,
    f(x) = p(x) + R _n (x),(x _0为展开点, x _0 = 0时为麦克劳林展开式) $</font>    

<font color=#39c5bb>运算(四则运算的极限=极限的四则运算)</font>:
<font color=#ffa500>$\displaystyle \lim _{x \to n} [f(x) \pm g(x)] = \lim _{x \to {n}} f(x) + \lim _{x \to {n}} g(x)$</font>     
<font color=#ffa500>$\displaystyle \lim _{x \to n} [f(x)g(x)] = \lim _{x \to {n}} f(x) \lim _{x \to {n}} g(x)$</font>    
<font color=#ffa500>$\displaystyle \lim _{x \to n} [\frac{f(x)}{g(x)}] = \frac{\displaystyle \lim _{x \to {n}} f(x) }{\displaystyle \lim _{x \to {n}} g(x)}$</font>    

# 微分积分
> <font color=#39c5bb>定积分</font>: <font color=#ffa500>$ \int ^{b} _{a} f(x) dx = F(b) - F(a) $</font>    
> <font color=#39c5bb>不定积分</font>: <font color=#ffa500>$ \int f'(x)dx = f(x) $</font>    

---


# 硬解定理：


> 由:
<font color=#ffa500>$$
\begin{cases}
\frac{x^2}{p_{x2}} + \frac{y^2}{p_{y2}} = 1\\
p _x x + p _y y + p =0
\end{cases}
$$</font>

> 可得：

<font color=#ffa500>$$

a = p _{x2}  p _x ^2 + p _{y2}  p _y ^2,     
\begin{cases}
b _x = 2 p _{x2} p _x p \\
c _x =  p _{x2} (p^2 - p _{y2}p _y ^2) 
\end{cases}
,
\begin{cases}
b _y = 2 p _{y2} p _y p \\
c _y =  p _{y2} (p^2 - p _{x2}p _x ^2) 
\end{cases}

$$</font>


* 韦达定理：
> <font color=#ffa500>$ \begin{cases} x _1x _2 = \frac{c _x}{a} \\ x _1 + x _2 = - \frac{b _x}{a} \end{cases} $</font>    
> <font color=#ffa500>$ \begin{cases} y _1y _2 = \frac{c _y}{a} \\ y _1 + y _2 = - \frac{b _y}{a} \end{cases} $</font>    

> <font color=#ffa500>$ x _1 y _2 + y _1 x _2 = \frac{2 p _{x} p _{y} p _{x2} p _{y2} }{a} $</font>    
---

# 三角:

> <font color=#ffa500>$ \sin^2{a} + \cos^2{a} = 1 $</font>    
> <font color=#ffa500>$ \tan{a} = \frac{ \sin{a}}{ \cos{a}} $</font>    
> <font color=#ffa500>$ S = \frac {1}{2}ab \sin C $</font>    
> <font color=#ffa500>$ Asin(\omega x + \phi),\begin{cases}(sinx,cosx)中 \omega T = 2 \pi, \\ (tanx)中\omega T = \pi \end{cases} $</font>   

* 数值
> <font color=#ffa500>$ sin(30^{。}) = \frac{1}{2} , sin (45^{。}) =\frac{ \sqrt{2}}{2} , sin(60^{。}) = \frac {\sqrt{3}}{2} $</font>    
> <font color=#ffa500>$ cos(30^{。}) = \frac {\sqrt{3}}{2} , cos(45^{。}) =\frac{ \sqrt{2}}{2} , cos(60^{。}) =  \frac{1}{2} $</font>    
> <font color=#ffa500>$ tan(30^{。}) = \frac{\sqrt{3}}{3} , tan(45^{。}) =1 , tan(60^{。})= \sqrt{3} $</font>    

* 正弦定理
> <font color=#ffa500>$ \frac{a}{\sin A} = \frac{b}{\sin B} = \frac{c}{\sin C} =2R(外接) $</font>    

* 余弦定理
> <font color=#ffa500>$ c^2 = a^2 + b^2 -2ab \cos C $</font>    
> <font color=#ffa500>$ \cos C = \frac {a^2 + b^2 - c^2} {2ab} $</font>    


* 和差角公式:
> <font color=#ffa500>$ C(a+b)=CaCb-SaSb $</font>    
> <font color=#ffa500>$ C(a-b)=CaCb+SaSb $</font>    
> <font color=#ffa500>$ S(a+b)=SaCb+CaSb $</font>    
> <font color=#ffa500>$ S(a-b)=SaCb-CaSb $</font>    
> <font color=#ffa500>$ T(a+b)= \frac{Ta+Tb}{1-TaTb} $</font>    
> <font color=#ffa500>$ T(a-b)= \frac{Ta-Tb}{1+TaTb} $</font>     

* 诱导公式:
> <font color=#ffa500>$ -st , \pi+t, \pi-ct, \frac{ \pi}{2} - E, \frac{ \pi}{2} + Es $</font>    


* 辅助角公式:
> <font color=#ffa500>$ a>0, a \sin { \alpha} + b \cos{ \alpha} = \sqrt {a^2+b^2} \sin { (\alpha + arctan(\frac {b}{a})) } $</font>    

* 2倍角公式($ \leftarrow 和差角$):
> <font color=#ffa500>$ S2a = 2SaCa $</font>    
> <font color=#ffa500>$ C2a = C^2 a - S^2 a =2C^2 a - 1 = 1 - 2 S^2 a $ ,(可升降幂用)</font>     
> <font color=#ffa500>$ T2a = \frac {2Ta}{1-T^2 a} $</font>    

* 半角公式($ \leftarrow 2倍角C2a $):
> <font color=#ffa500>$ S^2 半a = \frac {1 - Ca} {2} $</font>    
> <font color=#ffa500>$ C^2 半a = \frac {1 + Ca} {2} $</font>    
> <font color=#ffa500>$ T^2 半a = \frac {1 - Ca} {1 + Ca} $</font>    


* 万能公式($ \leftarrow 2倍角化齐次同除 $):
> <font color=#ffa500>$ S2a = \frac {2Ta}{1 + T^2 a} $</font>     
> <font color=#ffa500>$ C2a = \frac {1 - T^2 a}{1 + T^2 a} $</font>     
> <font color=#ffa500>$ T2a = \frac {2Ta}{1 - T^2 a} $</font>    

* 积化和差($ \leftarrow 和差角和差 $):
> <font color=#ffa500>$ S(a+b) + S (a-b) = 2SaCb $</font>    
> <font color=#ffa500>$ S(a+b) - S (a-b) = 2CaSb $</font>    
> <font color=#ffa500>$ C(a+b) + C (a-b) = 2CaCb $</font>    
> <font color=#ffa500>$ C(a+b) - C (a-b) = -2SaSb $</font>    

* 和差化积($\leftarrow 积化和差 $):
> <font color=#ffa500>$Sa + Sb =2S \frac {a+b}{2} C \frac {a-b}{2}$</font>    
> <font color=#ffa500>$Sa - Sb =2C \frac {a+b}{2} S \frac {a-b}{2}$</font>    
> <font color=#ffa500>$Ca + Cb =2C \frac {a+b}{2} C \frac {a-b}{2}$</font>    
> <font color=#ffa500>$Ca - Cb =-2S \frac {a+b}{2} S \frac {a-b}{2}$</font>    


# 向量:
* 运算
> <font color=#39c5bb>向量坐标</font>:<font color=#ffa500>后-前</font>    
> <font color=#39c5bb>向量加减</font>:<font color=#ffa500>$ 
\begin{cases}
加:终点对起点,和向量为起点指向终点 \\
减:终点对终点,差向量为起点指向起点 \\
\end{cases} $</font>    
> <font color=#39c5bb>A,B,C共线</font>,<font color=#ffa500>$ a \overrightarrow {OA} + b \overrightarrow {OB} = \overrightarrow {OC} $,O不在AB上有,$ a + b = 1 $</font>    

* <font color=#39c5bb>点乘(数量积)</font>：<font color=#ffa500>$ \overrightarrow {a} \cdot \overrightarrow {b} = |\overrightarrow {a}| |\overrightarrow {b}| \cos { \theta }  $</font>    

* 叉乘
    * <font color=#39c5bb>方向</font>:<font color=#ffa500>$ \overrightarrow {a}(食指) \times \overrightarrow {b}(中指) , 方向在右手右手拇指(注：物理B总在中指) $</font>    
    * <font color=#39c5bb>大小</font>:<font color=#ffa500>$ 四边形面积S=| \overrightarrow{n} | $</font>    
    * <font color=#39c5bb>两个向量写两遍,去头去尾留中间,交叉相乘再相减</font>    
> <font color=#ffa500>e.g.由(a , b , c),(d , e ,f)描述的平面法向量$ \overrightarrow{n} $</font>    
> <font color=#ffa500>~~a~~ b c a b ~~c~~</font>
> <font color=#ffa500>~~d~~ e f d e ~~f~~</font>    
> <font color=#ffa500>$ \overrightarrow{n} (bf - ce , cd - af , ae - bd) $</font>    


# 数列:
* <font color=#39c5bb>等差</font>: <font color=#ffa500>$ a _n = a _1 + (n - 1)d $ ,$ S _n = na_1 + \frac {n}{2} (n-1)d  $</font>
> <font color=#39c5bb>等差中项</font>:<font color=#ffa500>$ 2b = a + c $</font>    

> <font color=#39c5bb>等差等下标和,和相等</font>:
> <font color=#ffa500>$ a _w + a _x = a _y + a _z ,其中w + x = y + z $</font>    

* <font color=#39c5bb>等比</font>: <font color=#ffa500>$ a _n = a_1 q ^{n-1} , S _n = \frac{a _1}{1-q} (1 - q ^n) $</font>    
> <font color=#39c5bb>等比中项</font>:<font color=#ffa500>$ b^2 = ac $</font>    

> <font color=#39c5bb>等比等下标和,积相等</font>:
> <font color=#ffa500>$ a _w a _x = a _y a _z ,其中w + x = y + z $</font>    

* <font color=#39c5bb>数列求和</font>：<font color=#ffa500>裂项相消,错位相减,倒序相加,累加累乘</font>     
* <font color=#39c5bb>不动点原理</font>:<font color=#ffa500>因式定理,若$ f(x _0) - x _0 = 0,f( x ) - x _0 = (x - x _0) h(x),其中h(x)为其它因式 $</font>    
* <font color=#39c5bb>不动点</font>:<font color=#ffa500>$ \begin{cases}
    一般方程:两边减去不动点,化为等比 \\
    分式方程:两边减去不动点,同时取倒,凑分母化为等差 \\
\end{cases} $</font>    

# 几何:

* 面积体积
> <font color=#ffa500>$ V _{球} = \frac{4}{3} \pi R^3 $</font>    
> <font color=#ffa500>$ V _{柱体} = Sh $</font>    
> <font color=#ffa500>$ V _{锥体} = \frac{1}{3}Sh $</font>    
> <font color=#ffa500>$ V _{台体} = \frac{1}{3}( S _上 + S _下 + \sqrt{ S _上 \cdot S _下}) h $</font>    

> <font color=#ffa500>$ S _{球} = 4 \pi R^2 $</font>    
> <font color=#ffa500>$ S _{锥} = (S _侧)\pi r l + \pi r^2 $</font>    
> <font color=#ffa500>$ S _{圆台} = (S _侧)\pi r l + \pi r _1 ^2 + \pi r _2 ^2 $</font>    

> <font color=#39c5bb>斜二测画法</font>: <font color=#ffa500>$ S _{斜} = \frac{ \sqrt{2}}{4} S _{原} $</font>    

> <font color=#ffa500>$ R _{内切圆} = \frac {S}{ \frac{1}{2} ( \Sigma _{i=1} ^{N} l _{i}) } $</font>    
> <font color=#ffa500>$ R _{内接球} = \frac {V}{ \frac{1}{3} ( \Sigma _{i=1} ^{N} S _{i}) } $</font>    
> <font color=#ffa500>$ R _{外接球} = \sqrt{ \frac{a^2 + b^2 - 2ab \cos{ \theta} }{ \sin^2{ \theta}} + ( \frac{l}{2} )^2 } ,(圆心位置相异取补角) (a,b圆心到公交线距离, \theta 二面角 , l公交线长) $</font>    

* 线面关系:
> <font color=#39c5bb>法向量</font>:<font color=#ffa500>$ 交线确定法向量方向 $</font>    

> <font color=#39c5bb>平行</font>:<font color=#ffa500>$ 线线 \Rightarrow 线面(垂直于面法向量) \Rightarrow 面面(法向量平行) $</font>    
> <font color=#39c5bb>垂直</font>:<font color=#ffa500>$ 线线 \Rightarrow 线面(平行于面法向量) \Rightarrow 面面(法向量垂直) $</font>    

* 角:
> <font color=#39c5bb>线线角$[0,\frac{\pi}{2}]$</font>: <font color=#ffa500>$ \cos { \theta } = | \cos < \overrightarrow a,\overrightarrow b> |  $</font>     
> <font color=#39c5bb>线面角$[0,\frac{\pi}{2}]$</font>: <font color=#ffa500>$ \sin { \theta } = | \cos < \overrightarrow a,\overrightarrow n> |  $</font>    
> <font color=#39c5bb>二面角$[0,\pi]$</font>: <font color=#ffa500>$ \sin { \theta } = \sqrt{ 1- | \cos < \overrightarrow n _1 , \overrightarrow n _2 > |^2} $</font>    

> <font color=#39c5bb>三面角定理</font>：<font color=#ffa500>$ \cos{\theta _{LR间二面角}} = \frac{ \cos{D} - \cos{L} \cos{R} } { \sin{L} \sin{R} } $</font>    

* 几何距离(转线线,线法):
> <font color=#ffa500>$ d _{投影}  = \overrightarrow{a} \cdot \overrightarrow{b} ,(a在b上投影)$</font>    
> <font color=#ffa500>$ d _{点线} = \sqrt{ | \overrightarrow {AP} |^2 - | \overrightarrow {AP} \cdot \overrightarrow {\mu} |^2 } ,(P到l,A在l上)$</font>    
> <font color=#ffa500>$ d _{点面} = \frac{|\overrightarrow{AP} \cdot \overrightarrow{n} |}{|\overrightarrow{n}|} ,(P到\alpha,A在\alpha上)$</font>     

# 平面解析:
* <font color=#39c5bb>倾斜角</font>:<font color=#ffa500>$ \alpha \in \{ k \neq 0 , k 不存在 \} $</font>    
* <font color=#ffa500>($ \alpha \neq 90 ^{。} $)时正设,($ \alpha \neq 0 ^{。} $)时反设</font>        

* 直线
> <font color=#ffa500>对于$ p _{x} x + p _{y} y + p = 0 ,(p _{x} , p _{y})为该直线的法向量$</font>    

> <font color=#39c5bb>平行时</font>,当<font color=#ffa500>$ \begin{cases} A _1 x + B _1 y + C _1 = 0 \\ A _2 x + B _2 y + C _2 = 0 \end{cases} $,平行时:$ 法向量成倍数关系 $</font>    
> <font color=#39c5bb>垂直时</font>,<font color=#ffa500>$ \begin{cases} (斜率存在)：k _1 k _2 = -1 \\ (斜率不存在)： \begin{cases} k _1 不存在 \\ k _2 = 0 \end{cases} \end{cases}$</font>    

> <font color=#39c5bb>直线方程内只含一个未知时可以变0清除未知,过定点</font>    

> <font color=#39c5bb>直线弦长</font>:<font color=#ffa500>$ \begin{cases} \sqrt {1 + k^2} \sqrt{ (x _1 - x _2)^2 } \\
\sqrt {1 + \frac{1}{k^2} } \sqrt{(y_1 - y_2)^2} \\
\end{cases} $,距离公式中代入直线方程</font>    


* 平面距离:
> <font color=#ffa500>$ d _{点线 } = \frac { |Ax_0 + By_0 + C| }{ \sqrt{A^2 + B^2} } $</font>    
> <font color=#ffa500>$ d _{平行线} = \frac{ |C_1-C_2| }{ \sqrt{A^2 + B^2} } $</font>    

* 对称
> <font color=#39c5bb>点点</font>,<font color=#ffa500>$ P(x _0,y _0)关于A(a,b)对称点为(2a-x_0,2b-y _0) $</font>    
> <font color=#39c5bb>点线</font>,<font color=#ffa500>解$ \begin{cases} 斜率积 = -1 \\ 中点属于直线 \end{cases} $</font>     

* 圆
> <font color=#ffa500>$ (x-a)^2 + (y-b)^2 = r^2 $</font>    
> <font color=#ffa500>$ x^2 + y^2 + Dx + Ey + F = 0 ,圆心( - \frac{D}{2} ,  - \frac{E}{2} ), r= \frac{1}{2} \sqrt{ D^2 + E^2 - 4F } $</font>    
> <font color=#39c5bb>直径圆</font>:<font color=#ffa500>$ (x-x _1)(x-x _2) + (y-y _1)(y-y _2) = 0 $,(x1,x2)到(y1,y2)为直径</font>    

> <font color=#39c5bb>圆幂</font>：<font color=#ffa500>$ | PO ^2 - r ^2 | = 点切割点乘积定值 $</font>    


* 椭圆,双曲线

> <font color=#39c5bb>第一定义</font>:<font color=#ffa500>`[椭圆]`$到焦点距离和为定值$,`[双曲线]`$到焦点距离差为定值$</font>     
> <font color=#39c5bb>焦半径(正减负加)(第二定义)</font>:<font color=#ffa500> `[椭圆]`:$ (负轴)a + ex _0 , (正轴)a - ex _0 $,`[双曲线]`:$ (负轴)ex _0 - a , (正轴)ex _0 + a $</font>    
> <font color=#39c5bb>斜率积定值(第三定义)</font>:<font color=#ffa500> $ AB关于O中心对称,曲线上任意点P,有k_{PA}\times k_{PB}为定值 $</font>    
> <font color=#39c5bb>双曲线渐近线</font>:<font color=#ffa500>$ \frac{x}{a} \pm \frac{y}{b} = 0 或 \frac{y}{a} \pm \frac{x}{b} = 0 $</font>    
> <font color=#39c5bb>焦点三角面积</font>:<font color=#ffa500>`[椭圆]`：$ b^2\tan \frac{\theta}{2} $ ,`[双曲线]`: $ \frac {b^2}{\tan \frac{\theta}{2}} $</font>    
> <font color=#39c5bb>参数方程</font>:<font color=#ffa500>$ [椭圆]\begin{cases} x=a\cos\theta \\ y=b\sin\theta \end{cases},
[双曲线]\begin{cases} x=a\sec\theta \\ y=b\tan\theta \end{cases} $</font>    

* 抛物线
> <font color=#39c5bb>第一定义</font>:<font color=#ffa500>$到准线距离相等的点集$</font>    
> <font color=#ffa500>$ p:焦点到准线距离,\frac{p}{2}:到原点距离$</font>    
> <font color=#ffa500>$ y^2 = 2px, y^2 = 2p(-x) , x^2 = 2py , x^2 = 2p(-y) $</font>    


> <font color=#39c5bb>弦平均性质</font>,<font color=#ffa500>$ 交x轴(a,0) $:$ \begin{cases} x _1 x _2 = a^2 \\ y _1 y _2 = -2ap \end{cases} $</font>    
> <font color=#39c5bb>焦点三角面积</font>:<font color=#ffa500>$ \frac{p^2}{2\sin\alpha} $</font>    

* <font color=#39c5bb>圆锥曲线</font>:<font color=#ffa500>切线、中点弦方程:`(半代=全代)`, $ (x _m , y _m)为切点或中点,不同时为0 $</font>     
> <font color=#39c5bb>圆1</font>,<font color=#ffa500>$ (x - a)(x _m - a) + (y _m - b)(y - b) - r^2 = (x _m - a)^2 + (y _m - b)^2 - r^2 $</font>    
> <font color=#39c5bb>圆2</font>,<font color=#ffa500>$ x x _m + y y _m - a(x + x _m) - b(y + y _m) + (a ^2 + b^2 - r ^2) = x _m ^2 + y _m ^2 - 2a x _m -2b y _m + (a ^2 + b^2 - r ^2) $</font>    
> <font color=#39c5bb>椭圆</font>,<font color=#ffa500>$ \frac {x x _m}{p _{x2} ^ 2} +  \frac {y y _m}{p _{y2} ^ 2} - 1 = \frac {x _m x _m}{p _{x2} ^ 2} +  \frac {y _m y _m}{p _{y2} ^ 2} -1 $</font>    
> <font color=#39c5bb>双曲线</font>,<font color=#ffa500>$ \frac {x x _m}{p _{x2} ^ 2} -  \frac {y y _m}{p _{y2} ^ 2} - 1 = \frac {x _m x _m}{p _{x2} ^ 2} -  \frac {y _m y _m}{p _{y2} ^ 2} -1 $</font>    
> <font color=#39c5bb>抛物线1</font>,<font color=#ffa500>$ y y_m - p(x + x_m) = y _m ^2 - p (x _m + x _m) $</font>    
> <font color=#39c5bb>抛物线2</font>,<font color=#ffa500>$ y y_m + p(x + x_m) = y _m ^2 + p (x _m + x _m) $</font>    
> <font color=#39c5bb>抛物线3</font>,<font color=#ffa500>$ x x_m - p(y + y_m) = x _m ^2 - p (y _m + y _m) $</font>    
> <font color=#39c5bb>抛物线4</font>,<font color=#ffa500>$ x x_m + p(y + y_m) = x _m ^2 + p (y _m + y _m) $</font>    

* 证明<font color=#39c5bb>(半代=全代)</font>:
> <font color=#ffa500>$ \begin{cases} 点差法求k \\ (x _m , y _m) 求b \end{cases} $ $ \Rightarrow 直线方程 \Rightarrow (半代=全代) $</font>    

> <font color=#ffa500>$下注: (x _1,y _1),(x _2,y _2)交点,(x _m, y _m)中点,k=\frac{y _1 - y _2}{x _1 - x _2} $</font>

---
> <font color=#ffa500>$圆中, \begin{cases} (x _1 - a)^2 + (y _1 - b)^2 = r^2 [1] \\ (x _2 - a)^2 + (y _2 - b)^2 = r^2 [2] \end{cases} $
    $ [1] - [2] \Rightarrow \begin{cases} k = -\frac{x _m - a}{y _m - b} \\
        b = y _m + \frac{x _m - a}{y _m - b} \end{cases} 
    \Rightarrow 直线方程 \Rightarrow 化为(半代=全代)$</font>

---
> <font color=#ffa500>$椭圆中(双曲线同理): \begin{cases} \frac {x _1 ^2}{a ^ 2} +  \frac {y _1 ^2}{b ^ 2} = 1 [1] \\
        \frac {x _2 ^2}{a ^ 2} +  \frac {y _2 ^2}{b ^ 2} = 1 [2] \end{cases} $ , 
    $ [1] - [2] \Rightarrow \begin{cases} k = - \frac{b^2}{a^2} \cdot \frac{x _m^2}{y _m^2} \\
        b = \frac{b^2}{a^2} \cdot \frac{x _m ^2}{y _m} + y _m \end{cases} 
    \Rightarrow 直线方程 \Rightarrow 化为(半代=全代)$</font>

---
> <font color=#ffa500>$ 抛物线中: \begin{cases} y _1 ^2 = 2px _1 [1]\\ y _2 ^2 = 2px _2 [2]\end{cases} $ 
    $ [1] - [2] \Rightarrow \begin{cases} k = \frac{p}{y _m} \\
        b = y _m - \frac{px _m}{y _m} \end{cases} 
    \Rightarrow 直线方程 \Rightarrow 化为(半代=全代)$</font>

---


# 统计:
* 树
> <font color=#39c5bb>决策树</font>:<font color=#ffa500>$ 决策：左往右，每次完全处理一个节点，之后处理不包含该节点 $</font>    
> <font color=#39c5bb>完全树</font>:<font color=#ffa500>$ 每个节点处理时包含其它所有节点 $</font>    

* 排列组合(优化计算排除树遍历树):
> <font color=#ffa500>$ A_{n}^{m} = \frac{n!}{(n-m)!} $</font>    
> <font color=#ffa500>$ C_{n}^{m} = \frac{n!}{(n-m)!m!} $</font>     

> <font color=#39c5bb>平分概率时</font>:<font color=#ffa500>$ (分相同)隔板,(不同到不同)枚举、插空、捆绑,(不同到相同)分堆消序 $</font>    
> <font color=#39c5bb>有向无环图(拓扑排序检测)</font>:<font color=#ffa500>dp</font>    

* p百分位数
> <font color=#39c5bb>小数则取下一位,整数取平均</font>    

* <font color=#39c5bb>最小二乘法</font>：<font color=#ffa500>设方程,以最小二次方和拟合方程</font>    

* <font color=#39c5bb>残差</font>：<font color=#ffa500>观测O-期望E</font>    

* 相关系数(拟合直线模型,非无偏估计)：
> <font color=#ffa500>$ r = \frac{x _i y _i协方差}{ x _i标准差 y _i标准差} = \frac{ \frac{1}{n} \sum ^n _{i=1} x _i  y _i- \overline x \overline y }{ \sqrt{ \frac{1}{n} \sum ^n _{i=1} x _i ^2 - \overline x^2 } \sqrt{ \frac{1}{n} \sum ^n _{i=1} y _i ^2 - \overline y^2 }} $</font>    

* 一元线性回归方程
> <font color=#ffa500>$ \widehat y = \widehat k x + \widehat b $</font>    
> <font color=#ffa500>$ \begin{cases} \widehat k = \frac {x _i y _i协方差}{x _i方差 } = \frac{ \frac{1}{n} \sum ^n _{i=1}x _i y _i - \overline x \overline y}{ \frac{1}{n} \sum ^n _{i=1} x _i ^2 - \overline x^2 } \\ \widehat b 代入平均点解 \end{cases} $</font>   

> <font color=#39c5bb>决定系数</font>:<font color=#ffa500>$ R^2 = 1 - \frac{\widehat y _i替平均的y _i方差}{y _i方差} ,越大\Rightarrow 拟合越好 $</font>    

* 列联表

> <font color=#ffa500>$ 期望频率 = p(r = i , c = j ) \cdot n $,
    $= p(r=i) \cdot p(c=j) \cdot n $,
    $= \frac{ f _i \cdot f _j }{n} $</font>    

> <font color=#ffa500>$ \chi^2 = \sum \frac{(O _{ij}观测频率-E _{ij}期望频率)^2}{E _{ij}期望频率} $</font>    

> <font color=#ffa500>$ \chi ^2 同方差,E表期望(相互独立假设H _0(原假设)) $</font>     
> <font color=#ffa500>$ 稳定 \Rightarrow 独立,不稳定 \Rightarrow 不独立(有关系) $</font>        

> <font color=#39c5bb>$ \chi ^2 表 $：(Row $ \times $ Column)的检验的自由度 = (R - 1)(C - 1)</font>    
> <font color=#ffa500>在2x2中:$ \chi ^2 = \frac{总数(交差相乘相减)^2}{行列和乘积} =\frac{n(ad-bc)^2}{(a+b)(a+c)(d+b)(d+c)}  $</font>    


# 概率:
* 抽样
> <font color=#39c5bb>系统抽样(等距抽样),分层抽样,随机抽样</font>    

* 事件关系
    * <font color=#39c5bb>包含</font><font color=#ffa500>($ A \subseteq B $)</font>    
    * <font color=#39c5bb>和、并事件</font><font color=#ffa500>($ P( A \cup B )= P(A) + P(B) - P(A \cap B) )$)</font>    
    * <font color=#39c5bb>交、积事件</font><font color=#ffa500>($ P(A \cap B) = P(AB) $)</font>    
    * <font color=#39c5bb>互斥</font><font color=#ffa500>($ A \cap B = \emptyset $)</font>    
    * <font color=#39c5bb>对立</font><font color=#ffa500>($ \begin{cases} A \cap B = \emptyset \\ A \cup B = \Omega \end{cases} $)</font>

* <font color=#39c5bb>独立等价条件</font>:<font color=#ffa500>$ P(A)P(B) = P(AB) \Leftrightarrow A与B相互独立 $</font>    

* <font color=#39c5bb>条件概率(可能方式->结果)</font>:<font color=#ffa500> $ P(上|下)= \frac{P(上下)}{P(下)}, P(A|B) = \frac{P(AB)}{P(B)} $</font>    

* <font color=#39c5bb>全概率</font>: <font color=#ffa500>$ P(上下)= P(上|下)P(下),P(AB)= P(A|B)P(B) $</font>    

* <font color=#39c5bb>条件概率(结果->可能方式)</font>: <font color=#ffa500>$ P(A _i|B) = \frac{ P(A _i B)}{P(B)} = \frac{ P(A _i)P(B|A _i) }{ \sum ^n _{i=1} P(A _i)P(B|A _i) },(A _i 并集 \Omega) $</font>    

> <font color=#39c5bb>先验概率</font>:<font color=#ffa500>以往经验得到的概率,后验概率:更多补充经验后的概率</font>    

# 分布:
* 均值方差
> <font color=#ffa500>$ E(x) = 加权平均数 = \sum _{i=1} ^n x _i p _i $</font>    
> <font color=#ffa500>$ D(x) = 方差 = 平方的平均 - 平均的平方 = E(x^2) - E(x)^2 $</font>    

> <font color=#39c5bb>性质</font>:<font color=#ffa500>$E(ax+b)=aE(x)+b,D(ax+b)=a^2D(x)$</font>    

* 二项式定理
> <font color=#39c5bb>$ (a + b)^n $通项公式</font>:<font color=#ffa500>$ T _{k+1} = C _n ^k a ^{n-k} b ^k $</font>    

* 分布
> <font color=#39c5bb>API参数,B(n重伯努利试验,概率)</font>    
> <font color=#39c5bb>二项分布:<font color=#ffa500>$ X $ ~ $ B(n,p), P(X = k) = C _n ^k (1-p) ^{n-k} p ^k$</font>    
> <font color=#39c5bb>$ \begin{cases} E(x)=np \\ D(x)=np(1-p) \end{cases} $</font>    

> <font color=#39c5bb>API参数,H(产品数,抽取数,次品数)</font>    
> <font color=#39c5bb>超几何分布: <font color=#ffa500>$ X $ ~ $ H(N,n,m),P(X = k) = \frac{C ^k _m C ^{n-k} _{N-m}}{C ^n _N} $</font>    
> <font color=#39c5bb>$ \begin{cases} E(x)=\frac{mn}{N}=np \\ D(x)=\frac{mn}{N} \cdot \frac{N-n}{N} \cdot \frac{N-m}{N-1} = np(1-p)\frac{N-n}{N-1}\end{cases} $</font>    

> <font color=#39c5bb>API参数,N(均值,方差)</font>
> <font color=#39c5bb>正态分布</font>:<font color=#ffa500>$ X $ ~ $ N( \mu, \sigma ^2) = N(E(x),D(x)) = \frac { e ^{ \frac{-(x- \mu)^2}{ 2 \sigma ^2 } } }{ \sigma \sqrt{2 \pi}}  $</font>    
> <font color=#39c5bb>$关于(x=均值)$对称</font>
> <font color=#39c5bb>68-95-99原则</font>:<font color=#ffa500>区间:$ (x - \sigma , x + \sigma),(x - 2\sigma , x + 2\sigma),(x - 3\sigma , x + 3\sigma)$的积分</font>

<font color=#ff0044>
<center>Written by Vito Devlin :tada: </center>
<center>condexpr01@outlook.com</center>
</font>
