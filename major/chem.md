## 标准电极电势
<font color=#39c5bb>
$$ 能斯特方程: \varphi = \varphi^{\theta} + \frac{2.303RT}{nF} lg(\frac{c(Ox)/c^{\theta}}{c(Red)/c^{\theta}}) \approx \varphi^{\theta} + \frac{0.0592}{n} lg(\frac{c(Ox)/c^{\theta}}{c(Red)/c^{\theta}}) $$</font>

> <font color=#ff0044>$ c^{\theta} = 1mol/L,p^{\theta} = 100kpa,ln(10) \approx 2.303 $</font>

```json
//三守恒：电子守恒，电荷守恒，质量守恒
[
    "钾钙钠镁铝,水锌[铁]锡铅"    
    "氢硫铜[氧]碘,[铁]汞银溴酸"    
    "铂[氧]氯锰金"
]
```

<font color=#39c5bb>

||||||
|---|---|---|---|---|
|$ K^+/K $|$ Ca^{2+}/Ca $|$ Na^+/Na $|$ Mg^{2+}/Mg $|$ Al^{3+}/Al $|
|$ -2.931 $|$ -2.868 $|$ -2.71 $|$ -2.372 $|$ -1.676 $|

||||||
|---|---|---|---|---|
|$ (B)H_{2}O/[H_{2}] $|$ Zn^{2+}/Zn $|$ Fe^{2+}/Fe $|$ Sn^{2+}/Sn $|$ Pb^{2+}/Pb $|
|$ -0.8277 $|$ -0.7618 $|$ -0.447 $|$ -0.1375 $|$ -0.1262 $|

||||||
|---|---|---|---|---|
|$ H^{+}/[H_{2}] $|$ S/H_{2}S $|$ Cu^{2+}/Cu $|$ (B)[O_{2}]/OH^{-} $|$ I_{2}/I^{-} $|
|$ 0 $|$ 0.142 $|$ 0.3419 $|$ 0.401 $|$ 0.5355 $|

||||||
|---|---|---|---|---|
|$ Fe^{3+}/Fe^{2+} $|$ Hg^{+}/Hg $|$ Ag^{+}/Ag $|$ Br_{2}/Br^{-} $|$ 浓HSO_{4}/SO_{2},浓HNO_{3}/NO_{2}$|
|$ 0.771 $|$ 0.7973 $|$ 0.7996 $|$ 1.066 $|$ nullptr $|

||||||
|---|---|---|---|---|
|$ Pt^{+}/Pt $|$ O_{2}/H_{2}O $|$ Cl_{2}/Cl^{-},HClO/Cl_{2} $|$ 强酸MnO_{2}/Mn^{2+},MnO_{4}^{-}/MnO_{2} $|$ Au^{+}/Au $|$ Au^{+}/Au $|
|$ 1.18 $|$ 1.229 $|$ 1.35827,1.611 $|$ nullptr ,1.679 $|$ 1.692 $|

</font>

## 周期表

<font color=#ff0044>

$ \overset{化合价}{ ^{质量数} _{质子数} E ^{电荷数} _{原子数} }$
</font>

<font color=#ffa500>

|A1|A2|A3|A4|A5|A6|A7|0|
|---|---|---|---|---|---|---|---|
|H|||||||He|
|Li|Be|B|C|N|O|F|Ne|
|Na|Mg|Al|Si|Al|S|Cl|Ar|
|K|Ca|[21,30]Ga|Ge|As|Se|Br|Kr|

|B3|B4|B5|B6|B7|8|B1|B2|
|---|---|---|---|---|---|---|---|
|Sc|Ti|V|Gr|Mn|Fe,Co,Ni|Cu|Zn|

</font>

> <font color=#ffa500>s区</font>:<font color=#39c5bb>1~2A</font>
> <font color=#ffa500>p区</font>:<font color=#39c5bb>3~7A,0族</font>
> <font color=#ffa500>d区</font>:<font color=#39c5bb>3~7B,8族</font>
> <font color=#ffa500>ds区</font>:<font color=#39c5bb>1~2B</font>
> <font color=#ffa500>f区</font>:<font color=#39c5bb>镧系、锕系</font>


## 物质的量

<font color=#ff0044> $ 阿伏加德罗常数 (N _{A})$ </font>=<font color=#39c5bb>$ 1mol \approx 6.02 \times 10 ^{23}个 $ </font>    

<font color=#ff0044> $ 摩尔质量(相对分子/原子质量) $</font>:<font color=#39c5bb>$ g/mol = \frac{相对质量g}{1N_{A}个} = \frac{相对质量}{1} $</font>    
<font color=#ff0044> $ 0C ^{。},100kpa下气体摩尔体积(分子/原子体积)为$</font>:<font color=#39c5bb>$ 22.4L/mol $ </font>    

<font color=#ff0044> $ 物质的量浓度$</font>:<font color=#39c5bb>$ M=mol/L=\frac{个数}{溶液体积} $ </font>    

## 物质反应

* 物质分类
```json
{
    "纯净物":[

        "单质(游离态)",
        //"同素异形体定义":"同一种元素形成的几种性质不同的单质",
        //"核素定义":"具有一定数量的质子和中子的一种原子",
        //"同位素定义":"相同质子数不同中子数",

        "化合物(化合态)"
        /*["电解质":{"强电解质":"完全电离",
                    "弱电解质":"部分电离"},
          "非电解质"]*/
    ],

    //"分散系":["分散质","分散剂"]
    "混合物":{
        "溶液":"(0,1)nm",  
        "胶体":"[1,100]nm,性质:丁达尔效应,电泳,聚沉",  
        "浊液":"(100,+infinity)nm",  
    }
}
```

* 离子反应
```json
{
    "离子反应":{
        "拆":"强电解质（强酸强碱大部分盐）",
        "不拆":"弱电解质，氧化物，弱酸的酸式酸根，沉淀，气体"
    }
}
```

* 离子检验
```json
{
    "Mg2+":{
        "试剂":"NaOH",
        "现象":"产生白色沉淀，NaOH过量时不溶解"
    },
    "Al3+":{
        "试剂":"NaOH",
        "现象":"产生白色沉淀，NaOH过量时溶解"
    },
    "Fe3+":[{
        "试剂":"NaOH",
        "现象":"产生红褐色沉淀"
    },{
        "试剂":"KSCN",
        "现象":"溶液变为血红色"
    }],
    "Fe2+":[{
        "试剂":"NaOH",
        "现象":"产生白色沉淀，迅速变为灰绿色，过一段时间后变为红褐色"
    },{
        "试剂":"K3[Fe(CN)6]",
        "现象":"变为蓝色"
    },{
        "试剂":"KSCN,氯水",
        "现象":"加入KSCN无明显变化，滴加氯水后溶液变红"
    }],
    "Cu2+":{
        "试剂":"氨水",
        "现象":"产生蓝色沉淀，氨水过量时沉淀溶解"
    },
    "Cl-":{
        "试剂":"稀硝酸酸化的AgNO3",
        "现象":"白色沉淀"
    },
    "Br-":{
        "试剂":"稀硝酸酸化的AgNO3",
        "现象":"淡黄色沉淀"
    },
    "I-":{
        "试剂":"稀硝酸酸化的AgNO3",
        "现象":"黄色沉淀"
    },
    "SO4 2-":{
        "试剂":"稀盐酸、BaCl2",
        "现象":"先加入稀盐酸无明显现象，再加入BaCl2产生白色沉淀"
    },
    "SO3 2-\HSO3-":{
        "试剂":"稀硫酸，品红溶液",
        "现象":"加入稀硫酸产生无色、刺激性气味气体，该气体能使品红溶液褪色"
    },
    "CO3 2-,HCO3-":{
        "试剂":"稀盐酸，澄清石灰水",
        "现象":"加入稀盐酸产生无色无味能使澄清石灰水变浑浊的气体"
    },

    //焰色反应
    "Na+":{
        "试剂":"火",
        "现象":"红火"
    },
    "K+":{
        "试剂":"火",
        "现象":"紫火（蓝色钴玻璃）"
    },
    "Ca2+":{
        "试剂":"火",
        "现象":"砖红色火"
    }
}
```


## 仪器

* 读数：仰小俯大    
> <font color=#ffa500>($小：$<font color=#39c5bb>读出数值<实际数值</font>$,大：$<font color=#39c5bb>读出数值>实际数值$</font>)</font>    

* 收集方法
> <font color=#39c5bb>排水法，向上排空气法，向下排空气法</font>    

* 干燥


> <font color=#ffa500>酸性干燥剂</font>:<font color=#39c5bb>($ H _2SO _4 $)</font>    
> <font color=#ffa500>中性干燥剂</font>:<font color=#39c5bb>($ 无水CaCl _2 $)</font>    
> <font color=#ffa500>碱性干燥剂</font>:<font color=#39c5bb>($ CaO $)</font>    

* 尾气
<font color=#39c5bb>燃烧式、收集式、吸收式、防倒吸式</font>

* 容量瓶
> <font color=#ffa500>规格(必须指定)</font>:<font color = #39c5bb>[50,100,250,500,1000]ml</font>

* 冷凝管
> <font color=#ffa500>直形冷凝管</font>：<font color=#39c5bb>冷凝蒸气</font>    
> <font color=#ffa500>球形冷凝管</font>：<font color=#39c5bb>液体回流</font>    
> <font color=#39c5bb>蒸气流与水流方向应相反</font>    

* 分离提纯
```json
{
    "固-固":{
        "加热":"升华",
        "加水":"(均易溶)重结晶、(易溶难溶)过滤",
        "磁场":"磁性分离"
    },
    "固-液":{
        "互溶":{
            "溶解于萃取剂":"萃取",
            "沸点不同":"蒸馏",
            "高温结晶":"蒸发结晶，趁热过滤",
            "低温结晶":"蒸发浓缩，冷却结晶"
        },

        "不互溶":"过滤"
    },
    "液-液":{
        "互溶":"蒸馏",
        "不互溶":"分液"
    }
}
```

## (地壳前五:氧硅铝铁钙)重要元素
### 冶炼,热重分析

* 冶炼
> <font color=#39c5bb>电解法、热还原法、热分解法...</font>    

* 热重分析


> <font color=#ffa500>$ 变化$</font>：<font color=#39c5bb>$ 失H _2O \rightarrow 失非金属氧化物 \rightarrow 失O _2 \rightarrow 金属/金属氧化物 $</font>    
> <font color=#ffa500>$ 推测产物 $</font>:<font color=#39c5bb>$ 物质质量比 \rightarrow 相对分子质量比 $</font>    


### Si
* 高纯硅
> <font color=#ff0044>$ \begin{cases}
        SiO _2 + C \overset{1800-2000C^{。}}{==} 2CO \uparrow + Si \\
        Si + 3HCl \overset{300C^{。}}{==} SiHCl _3 + H _2 \\
        SiHCl _3 + H _2 \overset{1100C^{。}}{==} Si + 3HCl\end{cases} $</font>    

### Al
* 钝化
> <font color=#39c5bb>$ 常温下Al与浓H_{2}SO_{4},浓HNO_{3}发生钝化 $</font>    

* $ 两性: Al(OH)_3 ,Al_2O_3$

> <font color=#ff0044>$ \begin{cases} Al(OH) _3 + 3H ^{+} == Al ^{3+} + 2H _2 O \\ Al(OH) _3 + OH ^{-} == [Al(OH)_4]^{-} \end{cases} $</font>    

> <font color=#ff0044>$ \begin{cases} Al _2O_3 + 6H ^{+} == 2Al ^{3+} + 3H _2 O \\ Al_2 O_3 + 2OH ^{-} + 3H _2 O == 2[Al(OH)_4]^{-} \end{cases} $</font>    

> <font color=#ff0044>$ 2Al(OH) _3 \overset{\Delta}{==} Al_2O_3 + 3H_2O $</font>    


---

### Fe

* 钝化
<font color=#39c5bb>$ 常温下Fe与浓H_{2}SO_{4},浓HNO_{3}发生钝化 $</font>    

* 铁离子
> <font color=#39c5bb>$ Fe ^{2+} 保护可加入过量铁粉$</font>    
> <font color=#39c5bb>$ Fe ^{3+} 在pH > 3.3时认为沉淀完全(10^{-5}mol/L),可控制pH去除Fe ^{3+} $</font>    


---

### Na

* 与氧反应    
> <font color=#ff0044>$ Na + O _{2} == 2Na _{2} O(白色固体)$</font>    
> <font color=#ff0044>$ 2Na + O _{2} \overset{\Delta}{==} 2Na _{2} O _{2}(淡黄色固体)$</font>    

* 侯氏制碱法
> <font color=#39c5bb>$(饱和食盐水充氨析出小苏打)$</font>:<font color=#ff0044>$ NH _{3} + H_{2}O + CO _{2} + NaCl == NaHCO_{3} \downarrow + NH_{4}Cl $</font>        
> <font color=#39c5bb>$(制纯碱苏打)$</font>:<font color=#ff0044>$ 2NaHCO3 \overset{\Delta}{==} Na _{2}CO_{3} + H _{2} O + CO _{2} \uparrow $</font>    


### S
* 硫酸酸酐
> <font color=#ff0044>$ \begin{cases}
        2SO _2 + O _2 \overset{V_2O_5}{\underset{\Delta}{\rightleftharpoons}} 2SO_3 \\
        SO_3 + H_2O ==H_2SO_4 
        \end{cases} $</font>    

* 硫酸
> <font color=#39c5bb>酸入水</font>   
> <font color=#39c5bb>氧化性，吸水性，脱水性</font>    

* 酸雨:<font color=#39c5bb>($ pH = -\lg{c(H^{{+}})}<5.6 $)</font>

### N

> <font color=#39c5bb>$ 浓HNO _3被还原为 NO _2 $</font>    
> <font color=#39c5bb>$ 稀HNO _3被还原为 NO $</font>    

> <font color=#ff0044>$ \begin{cases} 2NO + O _2 == 2NO _2 \\ 3NO  _2 + H _2O == 2HNO_3 + NO \end{cases}$</font>    

> <font color=#39c5bb>$ 正常写NH _3 \cdot H_2O ,加热时写NH_3 $</font>    

## 化学键

* 原子核外电子排布

> <font color = #39c5bb>s球形，p哑铃形</font>

> <font color = #ff0044>填充顺序</font>: <font color=#39c5bb>$ [s][sp][sp][sdp][sdp][sfdp][sfdp] $</font>//df为能级交错原因，书写时恢复能层顺序    
> <font color = #ff0044>填充规则</font>：<font color=#39c5bb>能量最低原理，洪特规则（优先占满），泡利原理（自旋相反）</font>    
> <font color = #ff0044>简化电子排布式</font>：<font color=#39c5bb>[0族元素]价电子</font>   




> <font color = #ff0044>电负性</font>：<font color = #39c5bb>1~4周期A族元素同非金属性</font>    
> <font color = #ff0044>第一电离能</font>：<font color = #39c5bb>1~4周期A族元素同非金属性，2A(s全满)与3A,5A(p半满)与6A反转</font>    
> <font color = #ff0044>半径</font>：<font color = #39c5bb>$ 电子层(r\uparrow) \rightarrow 质子数(r\downarrow) \rightarrow 核外电子(r\uparrow) $</font>    
> <font color = #ff0044>最高正价</font>=<font color = #39c5bb>主族序数（O、F除外）</font>    
> <font color = #ff0044>最低负价</font>=<font color = #39c5bb>8-最高正价</font>    

* VSEPR(Valence Shell Electron Pair Repulsion model)

> <font color = #39c5bb>$ [单键:\sigma] , [双键:\sigma + \pi] , [三键:\sigma + 2\pi] $</font>    
> <font color = #ff0044>孤电子对数</font>：<font color = #39c5bb>$ \frac{a(中心) - xb(外层)}{2}$</font>    
> <font color = #ff0044>直线形</font>：<font color = #39c5bb>$ \sigma + 孤电子 = 2对电子$</font>    
> <font color = #ff0044>平面三角形(V型)</font>：<font color = #39c5bb>$ \sigma + 孤电子=3对电子 $</font>    
> <font color = #ff0044>四面体形(正四面体形$109C^{。}28'$、三角锥形、V形)</font>：<font color = #39c5bb>$ \sigma + 孤电子=4对电子 $</font>    


* 电子式
```json
{
    "金属阳离子":"离子符号",
    "非金属阴离子":"标价电子，[]括起并加上电荷数",
    "原子":"标最外层电子"，
    "共价化合物及多原子单质":"标出外层电子对"
}
```

* 键参数比较
```json
{
    "键角":[
        "VSEPR model",
        "孤电子对数",
        "电子位置斥力:电负性、反馈派键"
    ],
    "键长":"原子半径"
    "键能":"长键低能，短键高能"
}
```

* 分子
```json
{
    "作用力":{
        "氢键":"H与电负性大原子（N，O，F）成键",
        "范德华力":"分子间作用力，小于氢键"
    },
    "手性":{
        "手性分子":"互为镜像，三维中不重叠",
        "手性C":"连接4个不同的基团"
    },
    "配位键":{
        "配位键":"电子对给予接受键",
        "配位化合物":"e.g. H[Al(OH)4],配位数4,配位体OH",
        "配位稳定性":"配位体电负性更小，电子对位置偏中心原子，更稳定"
    }
}
```

* 晶体
```json
{
    //获取：凝固、凝华、析出
    //粒子数：均摊法
    "晶体":{
        "晶体分类":{
            "单晶体":["无隙并置","确定熔点","各向异性","自范性"],
            "多晶体":["不规则","确定熔点","各向同性","自范性"]
        },
        "稳定性比较":{
            "不同类晶体":"共价晶体 > 离子晶体 > 分子晶体",
            "同类晶体间":{
                "共价晶体":"键能比较",   
                "离子晶体":"电荷数、离子半径",   
                "分子晶体":"作用力比较",   
                "金属晶体":"价电子数、离子半径"   
            } 
        }
    },

    "非晶体":["不规则","不定熔点","各向同性","无自范性"]
}
```


## 反应能量

* 反应热
```json
{
    "焓变":"Delta H = 末 - 初",
    "热化学方程式":[
        "标状态(s,l,g,aq)",
        "标温度压强(25C,101kpa时可不标)",
        "标Delta H(含符号单位)"
    ],

    "燃烧热":"101kpa,1mol纯物质完全燃烧生成指定产物放出的热量",
    "中和热":"Delta H = -57.3 kJ/mol(混合液比热容中和测定试验)",
    "过渡态":"催化剂可使活化能降低更容易到过渡态",

    "盖斯定律":"化学反应一步完成和分步完成的反应热是相同的"
}
```

## 速率和平衡

* 速率
> <font color =#ffa500>"反应速率"</font>:<font color =#39c5bb>"$ v = \frac{ \Delta c}{ \Delta t} (mol \cdot L ^{-1} min^{-1}) or (mol \cdot L ^{-1} s^{-1}) $"</font>
> <font color =#ffa500>"影响反应速率外因"</font>:<font color =#39c5bb>"浓度、温度、压强、催化剂、表面积...etc"</font>
> <font color =#ffa500>"勒夏特列原理"</font>:<font color =#39c5bb>"反应向着减弱改变的方向进行"</font>
> <font color =#ffa500>"$ \Delta G = \Delta H - T \Delta S (吉布斯自由能) $"</font>:<font color =#39c5bb>"$ \begin{cases} \Delta G < 0 放出自由能,自发反应 \\
               \Delta G = 0 平衡状态 \\
               \Delta G > 0 吸收自由能,不自发反应
              \end{cases} $"</font>


> <font color =#ffa500>"浓度幂"</font>:<font color =#39c5bb>"浓度的系数次方"</font>    
> <font color =#ffa500>"速率常数"</font>:<font color =#39c5bb>"$ \begin{cases} v _{正} = k _{正} \cdot (前浓度幂积) \\
               v _{逆} = k _{逆} \cdot (后浓度幂积) \end{cases} $"</font>    

> <font color =#ffa500>"正逆速率常数与平衡常数"</font>:<font color =#39c5bb>"$ \frac{k _{正}}{ k _{逆}} = K_{平衡常数}  $"</font>    

> <font color =#ffa500>"阿伦尼乌斯公式"</font>:<font color =#39c5bb>$ k = A \cdot e ^{ -\frac{E _a}{RT} } ,
(k:速率常数,A:指前因子,E _a活化能,R:摩尔气体常数,T:热力学温度) $</font>

> <font color =#ffa500>"范特霍夫公式"</font>:<font color =#39c5bb>$ -RTln K = \Delta G ,
(K:平衡常数,R:摩尔气体常数,T:热力学温度,G:自由能) $</font>


* 平衡
> <font color =#ffa500>"浓度商Q"</font>:<font color =#39c5bb>"$ Q = \frac{后浓度幂积}{前浓度幂积} $"</font>    
> <font color =#ffa500>"平衡常数K"</font>:<font color =#39c5bb>"$ K = \frac{平衡时后浓度幂积}{平衡时前浓度幂积} $"</font>    


> <font color =#ffa500>"平衡常数K计算"</font>:<font color =#39c5bb>"$ \begin{cases}
        摩尔商 = \frac{后mol幂积}{前mol幂积} \\
        K _{p} = 摩尔商 \cdot (p \cdot \frac{1}{总mol})^{后系和-前系和} \\
        K _{c} = 摩尔商 \cdot (\frac{1}{V})^{后系和-前系和}
               \end{cases} $"</font>    

> <font color =#ffa500>"平衡常数K运算"</font>:<font color =#39c5bb>"$ \begin{cases} K _{正} \cdot K _{逆} = 1  \\
               (a方程,K _{a}) + (b方程,K _{b}) =(a+b 方程,K _{a} \cdot K _{b})
               \end{cases} $"</font>    

> <font color =#ffa500>"转化率"</font> = <font color =#39c5bb>"$ \frac{转化量}{起始量} $"</font>    
> <font color =#ffa500>"产率"</font> = <font color =#39c5bb>"$ \frac{实际产量}{理论产量} $"</font>    

> <font color =#ffa500>"三段式找参数"</font> = <font color =#39c5bb>"$ 起始、转化、平衡 $"</font>    


* 电离平衡

> <font color =#ffa500>"$
              \begin{cases}
               (acid)K _a ,\\
               (base)K _b ,\\
               (hydrolysis)K _h \\
               (solubility\ product)K _{sp} \\
               (nagative\ logarithm)p \\
              \end{cases} $" : "$ \begin{cases}
        (K _1 >> K _2 >> K _3) ,\\
         K _h = \frac{K _w}{K _{a/b}} \\
         pH = pK _a + lg(\frac{后}{前})(缓冲液pH公式)\\
         \end{cases}
         $"</font>    

> <font color =#ffa500>"$(water)K _w$"</font> = <font color =#39c5bb>"$1 \times 10 ^{-14}(常温),K_w正比于温度$"</font>    
> <font color =#ffa500>"完全沉淀"</font>:<font color =#39c5bb>"$ 小于1 \times 10^{-5}mol/L认为完全沉淀 $"</font>    


* 水溶液三守恒方程式(电荷守恒、质子守恒、物料守恒)
> <font color =#ffa500>"标箭头法"</font>:<font color =#39c5bb>"$ 向上升一质子，向下降一质子 $"</font>    

* 中和滴定
> <font color =#ffa500>"仪器"</font>:<font color =#39c5bb>"$ \begin{cases}玻璃酸式滴定管 \\ 橡胶碱式滴定管 \end{cases}$"</font>    

> <font color =#ffa500>"试剂"</font>:<font color =#39c5bb>"$ \begin{cases} (中终)石蕊:红色,紫色[5,8],蓝色 \\
                    (酸终)甲基橙:红色,橙色[3.1,4.4],黄色 \\
                    (碱终)酚酞:无色,粉红色[8.2,10],红色
               \end{cases}$"</font>    

> <font color =#ffa500>"终点判断"</font>:<font color =#39c5bb>"$ 最后半滴 \rightarrow 颜色变化 \rightarrow 30s内不恢复 $"</font>    




## 有机
* IUPAC命名法
> <font color = #ffa500>{位置编号-取代基}主碳链</font>:<font color =#39c5bb>$ \begin{cases}
                        母体：选含官能团的最长最多分支主链 \\
                        编号：近简小,(甲乙丙丁戊己庚辛壬癸、中文数字) \\
                        书写：双键考虑顺反Z/E，数字间逗号，数字与汉字间杠 \\
                        \end{cases}$</font>    

> <font color = #ffa500>母体顺序</font>:<font color = #39c5bb> 
> $ 硝卤烷醚双三胺,酚醇酮酰醛酯羧 $
> $ -NO _2 < -X < 烷基 < -OR < 双键 < 三键  < -NH _2 $
> $ < -OH酚 < -OH 醇  < -COR 酮 < -CONH _2 < -CHO < -COOR < -COOH $</font>


* 测定
> <font color = #ffa500>"1不饱和度=2H"</font>:<font color = #39c5bb>"$\begin{cases} 观察: (几刀几个环)环1，双键1，三键2 \\
计算:饱和氢(2n+2)与其差值除2,氧不算,卤算到H,每个N减2个H \end{cases}$"</font> 


> <font color = #ffa500>"元素分析"</font>:<font color = #39c5bb>"燃烧确定实验式"</font> 
> <font color = #ffa500>"质谱法"</font>:<font color = #39c5bb>"最大质量值为相对分子质量(非丰度)"</font> 
> <font color = #ffa500>"核磁共振氢谱"</font>:<font color = #39c5bb>"峰种数和面积比 对应 等效氢种数和比"</font> 
> <font color = #ffa500>"红外光谱"</font>:<font color = #39c5bb>"确定官能团"</font> 

* 反应类型
> <font color = #ffa500>"氧化还原"</font>:<font color = #39c5bb>"$ 加氢去氧为还原反应,相反为氧化反应 $"</font> 

> <font color = #ffa500>"取代反应"</font>:<font color = #39c5bb>"$ 烷基 \sigma 特征反应 $"</font> 
> <font color = #ffa500>"卤代反应"</font>:<font color = #39c5bb>"$ 烷基 \sigma 特征反应 $"</font> 

> <font color = #ffa500>"加成反应"</font>:<font color = #39c5bb>"$ 张开\pi 特征反应 $"</font> 
> <font color = #ffa500>"消去反应"</font>:<font color = #39c5bb>"$ 闭合\pi 特征反应,氢少脱氢 $"</font> 

> <font color = #ffa500>"加聚反应"</font>:<font color = #39c5bb>"$ 张开多个\pi 特征反应 $"</font> 
> <font color = #ffa500>"缩聚反应"</font>:<font color = #39c5bb>"$ 脱水缩合反应 $"</font> 

> <font color = #ffa500>"Diels-Alder反应"</font>:<font color = #39c5bb>"$ 二烯和烯的成环反应 $"</font> 


* 卤代烃

> <font color = #ffa500>"水解反应(取代反应)"</font>:<font color = #39c5bb>"$ C-X \overset{NaOH,H _2O}{\underset{\Delta}{\rightarrow}} C-OH $"</font> 
> <font color = #ffa500>"消去反应"</font>:<font color = #39c5bb>"$ (一个含H,一个含X)C-C \overset{NaOH,醇}{\underset{\Delta}{\rightarrow}} C==C $"</font> 

* 胺
> <font color = #ffa500>"碱性"</font>:<font color = #39c5bb>"$ -NR _1 R _2 + H _2 O \rightleftharpoons -NR _1 R _2 H ^{+} + OH ^{-} $"</font> 

* 酚

> <font color = #ffa500>"显色反应"</font>:<font color = #39c5bb>"$ FeCl _3显紫色 $"</font> 
> <font color = #ffa500>"氧化反应"</font>:<font color = #39c5bb>"$ 被氧化显粉色 $"</font> 
> <font color = #ffa500>"缩聚反应"</font>:<font color = #39c5bb>"$
\begin{cases}
酚醛缩合：\\
1.Ph-OH与R _1-CHO反应,醛CO双键张开取代Ph邻位\alpha-H \\
2.结合产物脱水缩合
\end{cases} $"</font> 

* 醇

> <font color = #ffa500>"取代反应"</font>:<font color = #39c5bb>"$ C-OH + HX \rightarrow C-X + H _2 O $"</font> 
> <font color = #ffa500>"醇间脱水"</font>:<font color = #39c5bb>"$ 2CH-C-OH \overset{浓H _2SO _4}{\underset{140C^{。}\Delta}{\rightarrow}} C-O-C + H _2 O $"</font> 
> <font color = #ffa500>"消去反应"</font>:<font color = #39c5bb>"$ CH-C-OH \overset{浓H _2SO _4}{\underset{170C^{。}\Delta}{\rightarrow}} C==C + H _2 O $"</font> 
> <font color = #ffa500>"氧化反应(Cu/Ag催化)"</font>:<font color = #39c5bb>"$ -OH \rightarrow -CHO $"</font> 

* 酮(羰)
> <font color = #ffa500>"羰基"</font>:<font color = #39c5bb>"$ -CO-' $"</font> 
> <font color = #ffa500>"酮基"</font>:<font color = #39c5bb>"$ C-CO-C $"</font> 
> <font color = #ffa500>"催化加氢"</font>:<font color = #39c5bb>"$ -CO- + H _2 \overset{催化}{\rightarrow} -C-OH  $"</font> 

* 酰
> <font color = #ffa500>"酰基"</font>:<font color = #39c5bb>"$ RCO-,(含氧酸脱去-OH) $"</font> 

> <font color = #ffa500>"傅-克酰基化"</font>:<font color = #39c5bb>"$ RCOZ(Z可为卤素、OH、OR'、NHR'...) + HG(被酰化物) \overset{AlCl _3}{\rightarrow} RCOG + HZ $"</font> 
> <font color = #ffa500>"傅-克烷基化"</font>:<font color = #39c5bb>"$ RZ(Z可为卤素、OH、OR'、NHR'...) + HG(被烷化物) \overset{AlCl _3}{\rightarrow} RG + HZ $"</font> 

* 醛
> <font color = #ffa500>"银镜反应"</font>:<font color = #39c5bb>"$ -CHO + 2[Ag(NH _3) _2] OH \overset{\Delta}{\rightarrow} -COONH _4 + 3NH _3 + 2Ag \downarrow + H _2O  $"</font> 
> <font color = #ffa500>"新制$Cu(OH) _2$反应"</font>:<font color = #39c5bb>"$ -CHO + 2Cu(OH) _2 + NaOH \overset{\Delta}{\rightarrow}
-COONa + Cu _2O \downarrow + 3H _2O  $"</font> 
> <font color = #ffa500>"催化加氢"</font>:<font color = #39c5bb>"$ -CHO + H _2 \overset{催化剂\Delta}{\rightarrow} -C-OH  $"</font>     

> <font color = #ffa500>"羟醛缩合"</font>:<font color = #39c5bb>"$
\begin{cases}
1.R _1CH _2 CHO与R _2 CHO反应,R _2 CHO双键张开加成\alpha-H与剩余基团,得到羟醛 \\
2.氢少脱氢形成双键
\end{cases} $"</font> 

* 羧酸

> <font color = #ffa500>"酸性"</font>:<font color = #39c5bb>"$ -COOH \rightarrow -COO^{-} + H^{+} $"</font> 

> <font color = #ffa500>"酯化反应"</font>:<font color = #39c5bb>"$ -CO-OH + H-OR \overset{浓硫酸}{\underset{\Delta}{\rightleftharpoons}} -COOR $"</font> 
> <font color = #ffa500>"皂化反应"</font>:<font color = #39c5bb>"$ 油脂 + 3NaOH\overset{\Delta}{\rightleftharpoons} 甘油 + 硬脂酸钠(C _{17} H _{35} COONa) $"</font> 

---
* 苯
> <font color = #ffa500>"苯取代活化点位"</font>:<font color = #39c5bb>"$ 邻位、对位 $"</font> 
> <font color = #ffa500>"活化烷基"</font>:<font color = #39c5bb>"$ Ph-含氢C \overset{KMnO _4(H^{+})}{\rightarrow} Ph-COOH $"</font> 

> <font color = #ffa500>同分异构:</font> 

> <font color=#39c5bb>苯异构圆桌问题</font>:<font color = #ffa500>$ num(异构数) = \frac { \frac{ \frac{1}{6} A ^6 _6}{相同元素的全排列} + 不同对称种数(轴对称,中心对称) }{ C _2 ^1 (顺逆时针) } $ </font>

```json
{
    "苯常用异构":{
        "单取代":1,   
        "双取代":3,//邻间对  
        "三取代":{
            "三相同":3,
            "两同一异":6,
            "三相异":10
        }
    },

    "烷异构":{
        "甲基":1,
        "乙基":1,
        "丙基":2,
        "丁基":4,
        "戊基":8,
        "...":"..."
    }
}
```



<font color=#ff0044>
<center>Written by Vito Devlin :tada: </center>
<center>condexpr01@outlook.com</center>
</font>

