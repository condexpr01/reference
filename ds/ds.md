
# <font color=#ffe211> :star: 数据结构 </font>

## <font color=#ffe211> :sparkles: 链表 </font>

* <font color=#39c5bb> :rocket: 数组,单链表,循环链表,双向链表 </font>

```ebnf
<list> ::= {<node>}

(***************************************)
(*   单链表(<data> <p_right>)的尾node p_right指向nullptr *)
(* 循环链表(<data> <p_right>)的尾node p_right指向起始node *)
(* 双向链表(<p_left> <data> <pright>)的节点pleft pright分别指向左右node *)
(* 块状链表带块数据的链表,如<data>展开为数组 *)
(* 数组(<data>)是地址顺序的,可随机访问,不需要<p_left>,<p_right> *)

(* 哈希表:用哈希函数映射到可随机访问的空间,冲突时多层哈希表或用树 *)

<node> ::= <p_left> <data> <p_right>

(***************************************)
<data>    ::= ? data ?

<p_left>   ::= <node> (* pointer to left node(prev) *)
<p_right>  ::= <node> (* pointer to right node(next) *)
```

## <font color=#ffe211> :sparkles: 栈 </font>

```ebnf
(* LIFO:last in first out *)
(* 底层栈向low address增长 *)
<stack> ::= {<node>}

(***************************************)
(* sp  : stack pointer,指向当前node(top) *)
(* pop : 令sp指向<p_down>的指向,弹出<p_up> *)
(* push: 令sp指向的新空间,压入目标并连接<p_down> <p_up> *)

(* Tow-stack双端栈:将线性表的两端作为bottom,可以有更高的空间利用率 *)
(*          单调栈:严格单调性的栈 *)
(* 底层的栈是地址相关的,不需要<p_up>,<p_down> *)
<node> ::= <p_up> <data> <p_down>

(***************************************)
<data>    ::= ? data ?

<p_up>   ::= <node> (* pointer to up   node(prev) *)
<p_down> ::= <node> (* pointer to down node(next) *)
```


## <font color=#ffe211> :sparkles: 队列 </font>

* <font color=#39c5bb> :rocket: 顺序队列 </font>

```ebnf
<queue> ::= {<nodes>}

(***************************************)
(* queue队列    :<p_up>出  ,<p_down>进,  FIFO:first in first out *)
(* deque双端队列:<p_up>进出,<p_down>进出 *)
(*      单调队列:严格单调性的双端队列 *)
<node> ::= <p_up> <data> <p_down>

(***************************************)
<data>    ::= ? data ?

<p_up>   ::= <node> (* pointer to up   node(prev) *)
<p_down> ::= <node> (* pointer to down node(next) *)
```

* <font color=#39c5bb> :rocket: 堆序队列 </font>

```ebnf
(* 优先队列(弹出抽象堆序树顶最值的队列) *)
(* 优先队列以数组形式储存,下标映射为堆序 *)
<heap> ::= <node>

(***************************************)
(* 堆是完全二叉树,按层序编号 *)

(* 映射： *)
(* 从0开始,若node的下标为n,则pleft:2n+1,pright:2n+2  *)
(* 若pleft,pright的下标为n,则node:floor(n/2-1) *)

(* 大根堆：根最大,每层的节点都大于下一层,子节点必须小于等于父节点 *)
(* 小根堆：根最小,每层的节点都小于下一层,子节点必须大于等于父节点 *)

(* 左偏树:以merge操作进行插入删除 *)
(* 左偏树:左子distence大于右子,distence为到含空节点的距离 *)
(* merge(x,y):若有空堆返回非空堆,递归按堆序取根,合并右子与非根堆 *)
(* merge(x,y):回溯时满足左子大于右子distence *)

(* 二项树:0degree有1个节点,ndegree由[0,n-1]degree的树构成 *)
(* 二项堆:由二项树构成,两个ndegree合并按堆序令一个根变成n+1degree,另一个直接变其子节点 *)

<node> ::= <p_left> <data> <p_right>

(* 压入:创建空闲空间s   ,循环上升目标,循环后压入目标到s *)
(* 弹出:弹出最值产生空位,循环下降空位,循环后用末尾补充空位 *)

(***************************************)
<data>    ::= ? data ?

<p_left>  ::= <node> (* pointer to left  node(prev) *)
<p_right> ::= <node> (* pointer to right node(next) *)
```

* <font color=#39c5bb> :rocket: 倍增队列 </font>

```ebnf
(* Sparse Table:稀疏表 *)

(* 若有数组q[i] *)
(* t[i][j]表示队列q从m开始的长为n的区间的信息,O(n^2) *)

(* 那么t可以写成st[i][exp],表示队列q从i开始的长为2^exp的区间的信息,O(nlogn) *)
(* 每个(i,j)的查询,通过j的bit位,转换为多个i的多段exp实现查询 *)

<ST> ::= {<i> <exp_list>}

(***************************************)
<i> ::= ? index ?
<exp_list> ::= {<exp>}

(***************************************)
<exp> ::= ? exponent ?

```



## <font color=#ffe211> :sparkles: 树 </font>

* <font color=#39c5bb> :sparkles: 二叉树 </font>

```ebnf
<bitree> ::= <node>

(***************************************)

(* 插入节点:比较节点,新建空间并压入,O(logn) *)

(* 删除节点: *)
(*   节点没有子节点:直接删除   *)
(*   一个子节点:子节点替换后删除子节点 *)
(*   两个子节点:中序前驱(左子最大)或后继(右子最小)替换后删除前驱或后继节点  *)

(* bfs序: 按广度遍历(压入根到FIFO队列,循环展开节点,压入其子节点,直到队列空) *)
(* dfs序: 按深度遍历(压入根到LIFO栈  ,循环展开节点,压入其子节点,直到队列空) *)

(* LCA: Lowest Common Ancestor,两个节点在树中深度最大的共同祖先  *)

(* 中序: 左根右 *)
(* 前序: 根左右 *)
(* 后序: 左右根 *)

(*   满二叉树:所有层都是满的 *)
(* 完全二叉树:除最后一层叶子不满,其它层都是满的 *)

(* 线段树:对区间不断进行二分 *)
(* 树状数组:前缀区间信息,线段树中减去多余信息,不改变数组长度 *)

(* k-d树:对k个dimension中每个维度进行排序再取中位二分 *)

(* 哈夫曼树  :按频率排序编码对象,每次合并两个最小的,直到完成 *)
(* 哈夫曼编码:按哈夫曼树左0右1产生的前缀编码 *)

(* AVL树(BBT):所有节点平衡因子factor(左右hight的差)小于2       *)
(* left  rotate: 节点变成右子的左子(节点右子-继承->右子原来的左子) *)
(* right rotate: 节点变成左子的右子(节点左子-继承->左子原来的右子) *)
(*                                               *)
(* 不平衡时有4种情况(LL,RR,LR,RL),调平后记得更新 *)
(* LL: 节点right rotate                          *)
(* RR: 节点left  rotate                          *)
(* LR: 左子节点left  rotate转为LL,再right rotate *)
(* RL: 右子节点right rotate转为RR,再left  rotate *)

(* 红黑树(red black tree):左根右,根叶黑,不红红,黑路同 *)
(* 左根右:BST性质,             *)
(* 根叶黑:根和叶黑颜色,        *)
(* 不红红:红色不连续出现,      *)
(* 黑路同:任意路径黑色数量相同 *)
(*                             *)
(* 插入节点为红色(只可能违反根叶黑或不红红) *)

(* B树: *)

(* splay树:高频热点搜索,把每次搜索的节点旋转为根节点 *)
(* zig:无祖父节点,左旋父节点 *)
(* zag:无祖父节点,右旋父节点 *)
(* LL:祖父节点right rotate,父节点right rotate *)
(* RR:祖父节点left  rotate,父节点left  rotate *)
(* LR:父节点left  rotate,祖父节点right rotate *)
(* RL:父节点right rotate,祖父节点left  rotate *)

(* TREAP树:期望平衡的BST,随机数作为priority,单旋使priority满足堆序 *)
(* left  rotate: 节点变成右子的左子(节点右子-继承->右子原来的左子) *)
(* right rotate: 节点变成左子的右子(节点左子-继承->左子原来的右子) *)

(* 笛卡尔树:以k,w构成节点,要求k满足BST性质,w满足堆序性质,treap就是一种情况 *)

<node> ::= <p_left> <data> <p_right>

(***************************************)
<data>    ::= ? data ?

<p_left>  ::= <node> (* pointer to left  node(prev) *)
<p_right> ::= <node> (* pointer to right node(next) *)
```

* <font color=#39c5bb> :sparkles: 树链剖分 </font>

```ebnf
<tree> ::= <node>

(***************************************)

(* bfs序: 按广度遍历(压入根到FIFO队列,循环展开节点,压入其子节点,直到队列空) *)
(* dfs序: 按深度遍历(压入根到LIFO栈  ,循环展开节点,压入其子节点,直到队列空) *)

(* 树链剖分 >>                                             *)
(*          重子:拥有最多节点的子节点(只有一个,相等时随机) *)
(*          轻子:除重子外的子节点                          *)
(* 剖分(n为dfs1序号,dfs1/dfs2:第1/2次dfs)>> *)
(*           top[n]:链的顶端节点(必为轻子)  *)
(*            id[n]:优先重子的dfs2产生的序号*)
(*     heavy_son[n]:重子                    *)
(*                                          *)
(*        father[n]:父节点                  *)
(*         depth[n]:节点树深                *)
(*       sub_num[n]:子节点数                *)
(*          node[n]:n对应树中的节点         *)
(*                                          *)
(* top/id为树链开头和链接,heavy_son为0链结尾*)

<node> ::= <data> {<p_node>}

(***************************************)
<data>    ::= ? data ?

<p_node>  ::= <node> (* pointer to sub node *)
```

* <font color=#39c5bb> :sparkles: Link-Cut Tree </font>

```ebnf
(* LCT:Link-Cut Tree是辅助树森林,中序遍历splay对应实链上层到下层 *)
(* 原树对应辅助树森林,每棵辅助树由多个splay树由虚边连接成 *)
<forest> ::= <tree>

(***************************************)
<tree> ::= <node>

(***************************************)
(* 起始时辅助树是孤立的节点 *)
(* 实链剖分:每个点只能有一个实儿子,根据访问动态变化虚实 *)
(* 实链:父  指向子,子指向父,以原树深度作为权值,用splay树维护 *)
(* 虚链:父不指向子,子指向父,splay树之间的连接 *)

(* pushdown: 下放当前lazy标记   *)
(* pushup:   更新当前点信息 *)
(* reverse(x):交换左右子并置位lazy标记 *)

(* splay(x):先找到x所在splay的根,循环pushdown,再旋转使x成为当前splay的树根 *)

(* access(x):循环splay(x),rc(x)变前个splay树根,x=parent(x) *)
(* access(x):从下往上更新信息,并连通,x会变成实链最深的节点 *)

(* makeroot(x):access(x),splay(x),reverse(x) *)
(* makeroot(x):变为中序遍历的第一个,也就成为原树的根 *)
(* makeroot(x):access后x最为深的,实链倒序后成为最浅的 *)

(* findroot(x):查找所在辅助树的根 *)
(* findroot(x):access(x),splay(x)后,根为最左边的节点 *)

(* split(x,y):分离出x到y路径上的信息到y上,x变为y左子,这条路径分离为splay树 *)
(* split(x,y):makeroot(x),access(y),splay(y) *)

(* link(x,y):确定不在同一辅助树上,再移动到根上并连条虚边 *)
(* link(x,y):makeroot(x),father(x)=y *)

(* cut(x,y):双向断开同一辅助树上的边 *)
(* cut(x,y):split(x,y),father(y)=rc(x)=0 *)

<node> ::= <data> {<p_node>}

(***************************************)
<data>    ::= ? data ?

<p_node>  ::= <node> (* pointer to sub node *)
```


* <font color=#39c5bb> :sparkles: 树 </font>

```ebnf
<tree> ::= <node>

(***************************************)

(* 树套树:node继续嵌套树 *)

(* 虚树:只保留关键节点和关键节点的共同祖先的树 *)

(* 字典树(Trie):按出现字符建立分支,如英文可以用26叉树 *)

<node> ::= <data> {<p_node>}

(***************************************)
<data>    ::= ? data ?

<p_node>  ::= <node> (* pointer to sub node *)
```

## <font color=#ffe211> :sparkles: 图 </font>

```ebnf
<graph> ::= <V> <E>

(* 并查集:起始是孤立的令节点指向自身,union并,find查 *)
(* find(x):parent(a)!=a则parent(a)=find(parent(a)),最后返回parent(a) *)
(* union(a,b):令parent(find(a))=find(b) *)

(* 邻接矩阵:用二维平面表示点的连接 *)
(* 邻接矩阵:用链表表示每个点的连接情况 *)
(* 稀疏矩阵:用三元组(row,column,value)表示等 *)

(* 偶图(二分图):G<V,E>中V分为两个集合,E中的每条边的两端分别在这两个集合中 *)

(***************************************)
<V> ::= ? Vertex set ?
<E> ::= ? Edge set ?

```


# <font color=#ffe211> :star: 算法 </font>

## <font color=#ffe211> :sparkles: 复杂度 </font>

```ebnf
(* 以时间，空间复杂度描述某规模下的时间空间消耗 *)
(* 通常以O参数描述上界 *)
(* 通常以T参数为执行步骤数 *)
```


## <font color=#ffe211> :sparkles: 暴力 </font>

```ebnf
(* 枚举:依次遍历 *)
(* 模拟:模拟某个行为 *)
```

## <font color=#ffe211> :sparkles: 策略 </font>

```ebnf
(* 贪心:每次要最优解 *)
(* 递推:从上一个推导下一个 *)
(* 递归:问题可以被看成某个单元,产生的子问题也可以看成这个单元 *)

(* 二分:排序,循环取中位,分出左右 *)
(* 倍增:用2^[0,n]长度作为区间,指数作为下标,任意区间可按其bit位对应,单维复杂度降到logn *)

(* 分治:复杂问题分解为更小的子问题求解 *)

(* 前缀和:以前缀数组信息计算区间信息 *)
(* 差分:当前减去前一个的差值的数组,与前缀和互逆 *)

(* 离散化:把无限空间中的有限个体映射到有限的空间 *)

(* 扫描线:扫描有有相同特征的区间 *)

(* 分块:划分并块加上预处理信息 *)

(* 离线处理思想:对已收集的大量数据进行批处理,在固定时间段执行 *)

(* 平衡规划思想:寻求稳定的最优状态,而非极端 *)

(* 构造思想:不是求答案是多少,而是给出构造满足条件的例子,构造模式 *)

(* 数值处理 FFT:快速傅丽叶变换 *)
```

## <font color=#ffe211> :sparkles: 排序 </font>

> <font color=#39c5bb> 快排亲和cache. </font>

```ebnf
(* 冒泡排序:大的往指定方向冒泡,每次一个有序,O(n^2) *)

(* 选择排序:每次选择最值,每次有序一个,O(n^2)  *)

(* 插入排序:每次从无序部分插入到有序部分,O(n^2) *)
(* 希尔排序:以gap分组用插排,先大gap上有序,到gap=0全有序,pratt:gap=2^p*3^q,O(nlognlogn) *)

(* 归并排序:二分到长度为1有序,回溯时合并,或从0次开始每次合并2^n长序列,O(nlogn) *)
(* 快速排序:双指针,按基准二分序列,递归直到不能再分,可O(logn)+const的栈写,O(nlogn) *)

(* 基数排序:按n进制数位,如十进制,从个十百...排序,每轮映射到n个桶后取出,O(d*(n+k))  *)
(* 计数排序:按数值范围,映射到数组下标记录统计,排序整数,O(n+k)  *)

(* 桶排序:将数值分为多个区间,对每个区间选用排序算法,合并  *)
(* 堆排序:建堆(从最后一个非叶下标floor(n/2-1)开始),循环堆顶与最后叶交换后恢复堆序,O(nlogn)  *)
```

## <font color=#ffe211> :sparkles: 字符串算法</font>

* <font color=#39c5bb> :rocket: BMH</font>
```cpp
```

* <font color=#39c5bb> :rocket: exKMP,KMP</font>

```cpp
namespace exKMP{
	//next生成:
	//next [0,1]时:
	next[0]=next[1]=0;
	//next [2,s.length()-1]时:	
	if (s[n-1]==s[next[n-1]])
		//前字符s[n-1]
		//前面串最长后缀后字符s[next[n-1]]
		//相等时当前加1
		next[n] = next[n-1] + 1;
	else next[n] = 0;

	//nextval生成:
	//nextval [0,1]时:
	nextval[0]=nextval[1]=0;
	//nextval [2,s.length()-1]时:	
	if (s[n-1]==s[nextval[n-1]]){
		nextval[n] = nextval[n-1] + 1;
		//要求跳转到不同字符
		if (s[n] == s[nextval[n]])
			nextval[n] = nextval[nextval[n]];
	}else nextval[n] = 0;
}

namespace KMP{
	//串查找：通过nextval跳表,每次失败跳到对应下标,O(n)
}
```

* <font color=#39c5bb> :rocket: AC自动机 </font>

```cpp
//Trie上KMP,一个或多子串的匹配查找
//将next跳转表直接变为fail失配指针

//建立Trie树后
//以BFS序(队列)遍历
//n[0,1]层时,第0层root,第1层节点fail固定指向root
//n[2,+inf)层节点,
//循环遍历父节点的fail指向节点(含匹配的最大后缀信息),
//含子节点的字符与当前相同则fail指向它(同exKMP,用前面的计算当前),
//直到根节点也没有为止fail指向根
```


* <font color=#39c5bb> :rocket: 后缀自动机SAM </font>

```cpp
//识别所有子串的最小状态机

struct st_sam {

	struct node {
		int len;// 当前状态对应的最长串长度
		int father;// 后缀链接(suffix link,形成endpos字符为分支的树)
		int son[maxc];//转移表,有向无环图
	};

	int spaces;//节点总数(记录使用空间的下标位置)
	node *sam;

	int last;//当前上次插入节点下标

	void init() {//sam的起始状态
		spaces = last = 0;

		sam[0].len = 0;
		sam[0].father = -1;
		memset(sam[0].son, 0, sizeof sam[0].son);
	}

	st_sam(const string &s) {
		sam = new node[maxn];
		init();

		for (char c : s)insert(c);
	}


	// 插入一个字符
	void insert(int c) {
		c -='a';//映射到son的下标

		//新建节点,初始化length, son
		int cur = ++spaces;
		sam[cur].len = sam[last].len + 1;
		memset(sam[cur].son, 0, sizeof sam[cur].son);

		//顺着sam[last].father往树根加入该节点
		int p = last;
		while (p != -1 && sam[p].son[c] == 0){
			sam[p].son[c] = cur;
			p = sam[p].father;
		}

		if (p == -1) {
			//顺到根了就为0
			sam[cur].father = 0;
		} else {
			//没顺到根,停在已有转移的冲突位置
			
			int q = sam[p].son[c];
			if (sam[p].len + 1 == sam[q].len) {
				//对于x节点,sam[x].father状态为len的串是sam[x]所有有串的最长公共后缀

				//cur顺着sam[last].father往上,
				//当冲突节点转移1个字符后的len+1,与转移到的节点的len相同,
				//此时最长公共后缀可以由冲突节点转移而来
				sam[cur].father = q;

			} else {
				//启用用clone节点来作为冲突节点的转移节点和cur节点的最大公共后缀
				//同时继承冲突节点转移到的节点,clone会作为他们的父节点,
				int clone = ++spaces;
				sam[clone] = sam[q];
				sam[clone].len = sam[p].len + 1;

				//顺着sam[p].father往上遍历可以转移到q的节点
				//并把每个的转移都变为clone
				while (p != -1 && sam[p].son[c] == q) {
					sam[p].son[c] = clone;
					p = sam[p].father;
				}

				//此时clone为q,cur的最大公共后缀
				sam[q].father = sam[cur].father = clone;
			}
		}

		//令当前成为前次
		last = cur;
	}

	~st_sam() {delete[] sam;}
};
```

* <font color=#39c5bb> :rocket: MANACHER </font>

```cpp
//回文:从前和后遍历结果一样
//求最大回文需要对n个字符间插入字符(n+1个),使总数变为奇数2n+1

//右端最远子回文为c,其回文半径cr=d[c];当前位置i,回文半径ir=d[i]
//i在半径外时:从回文长ir=1扩展
//i+d[i']-1(镜像右端),c+d[c]-1(最远右端)
//i在半径内时:i+d[i']-1 <  c+d[c]-1,则d[i]=d[i']
//           :i+d[i']-1 >= c+cr-1,则ir从c+cr-i往外扩

//string s = "#1#2#3#2#1#";
//int n = s.length();
//int *d = new int[n];
int c=0,cr=1;d[0]=1;//d[0]

for (int i=1,ir; i<= n-1 ; i++) {//d[1,n-1]

	if (i>c+cr-1)ir=1;//i在c开始回文cr-1外,从1长开始扩展

	//i在c开始回文cr-1内:i'回文
	//不超过时为d[(c<<1)-i]
	//超过时令ir满足i+ir-1=c+cr-1
	else ir=min(d[(c<<1)-i],c+cr-i);

	do{
		//扩展前边界检查
		if(i < ir)break;
		if(n-1 < i+ir)break;

		//不让i+d[i']-1 < c+d[c]-1进行扩展
		if(i+d[(c<<1)-i]-1 < c+cr-1)break;

		//左右扩展
		if(s[i+ir] == s[i-ir])ir++;
		else break;
	}while(true);

	d[i] = ir;//不含插入字符回文长=ir-1

	//i加回文半径ir-1更远时更新c,cr
	if (i + ir - 1 > c + cr - 1) {
		c = i;
		cr = ir;
	}
}
```


## <font color=#ffe211> :sparkles: 搜索算法 </font>

```ebnf
(* 深度优先搜索:栈 *)
(* 广度优先搜索:队列 *)
(* 搜索的剪枝优化:去除不可能的分枝以优化 *)
(* 记忆化搜索:dp *)
(* 启发式搜索:根据特征搜索,A-star(贪心最小代价的bfs,最小代价常以两端曼哈顿距离和) *)
(* 双向广度优先搜索:终点和起点进行bfs,相遇产生路径 *)
(* 迭代加深搜索:深度受限的dfs栈实现,查找bfs序找到的第一个,O(depth) *)

(* bfs序:广优先产生,dfs序,深优先产生,欧拉序(dfs进入顺序并带上离开顺序) *)
```

## <font color=#ffe211> :sparkles: 图论算法 </font>

```ebnf
(* 最小生成树:PRIM(贪心最近点),KRUSKAL(贪心最短边) *)

(* 询问单源最短路dijkstra:记录表,贪心最近点,更新新最近 *)
(* 单源次短路:dijkstra维护一个次优解 *)

(* 询问多源最短路floyd-warshall:遍历每个作为经过点,更新邻接矩阵和路径表 *)
(* floyd每次不需要更新经过点的行列,对角线:若有n个点,更新表格数n^2-(n/2-2)-n == n^2-(3/2)n+2 *)

(* 拓扑排序检测有向无环图:贪心入度为0的点,删除它和它的出边,还有点但没有入度为0的点时有环 *)

 BELLMAN-FORD spfa

(* 匈牙利算法:二分图G<x,y,e>,对x每个节点增广,取得与y相连最多不相交边 *)
(* 增广:在二分图中某节点开始找交替的匹配边a和不匹配边b,*)
(* 增广从找b开始并以找不到a后找到b结尾,此时数量上b=a+1,交换a,b使匹配数+1 *)

(* KM算法:二分图构造出顶权,x点集的顶权为可行最值,y点集的为0 *)
(* 在可行边构成的图上对x每个点增广,失败时调整顶权(x+a,y-a)使能增广 *)
(* a=min(l(x)+l(y)-w(x,y)) *)
```

## <font color=#ffe211> :sparkles: dp </font>

```ebnf
(* dp(有向无环图):定义状态,状态转移方程,初始状态,计算最优解 *)

(* dp的常用优化:降低状态数量或回忆状态转移 *)
(* 复杂dp模型的构建:树型dp,区间dp,DAG DP,状态压缩dp,数位dp,环形dp,期望dp *)
(* 复杂dp模型的优化:维度重构,子问题重构,拓扑或顺序优化,数学性质,离线处理,前缀处理 *)
```

## <font color=#ffe211> :sparkles: 博弈论 </font>

```ebnf
(* 尼姆博弈:结果只和初始状态有关,如一定次数的异或 *)
(* SG函数:SG(x)=mex({SG(y),y in next(x)}) *)
(* mex(s)=min(x in N_0 | x not in s) *)
```

## <font color=#ffe211> :sparkles: 最优化 </font>

```ebnf
(* 单纯形法:线性规划,不等式表示半n维,二维中小于对应左下,大于对应右上 *)
(* 多维的单纯形法可能需要多次迭代,使检验小于0 *)
```

## <font color=#ffe211> :sparkles: 数学 </font>

```ebnf
(* 质数(素数):因式分解只能是1和它本身 *)
(* 整数唯一分解定理:任何一个大于1的整数都可以唯一地分解为多个质数n次方的乘积 *)
(* 整数分解:质数以乘法分解,进制以加法分解 *)

(* 辗转相除法(欧几里得算法):求a,b最大公因数gcd(a,b) *)
(* 0的因数是所有非零整数,gcd(a,b)=gcd(b,a%b),循环取余直到gcd(a,b)中b=0,返回a *)
(* gcd(a,b)实现为for(int r;b!=0;r=a%b,a=b,b=r);return a; *)
(* gcd(a,b)实现为if(b==0)return a;return gcd(b,a%b); *)

(* 素数筛法:埃氏筛法与线性筛法*)
(* [2,n]中埃氏筛法:循环从最小质数2开始,把它的倍数标记非质数,到下一个无标记的数必为质数 *)
(*                                                  *)
(* [2,n]中线性筛法:循环i in [2,n],从最小质数p=2开始 *)
(* i无标记则放入primes,将i*p标记为合数,i*p>n或i%p==0则停止,否则p=primes中下个质数继续或停止 *)

(* 互质:gcd(a,b)==1,那么a,b互质 *)
(* 欧拉函数:phi(n)为[1,n]中与n互质的数的个数 *)
(* 欧拉函数:[1,n]中质数有p[1,k],phi(n)=n(1-1/p1)(1-1/p2)...(1-1/pk)) *)

(* 阶乘factorial(p-1)为数学上(p-1)! *)
(* 威尔逊定理:p>1,p是质数充要条件是factorial(p-1)%p==p-1 *)

(* 裴蜀定理:存在x,y使ax+by=gcd(a,b) *)
(* 扩展欧几里得算法:
// 返回 gcd(a,b),并求出x,y使ax+by=gcd(a,b)
int exgcd(int a, int b, int &x, int &y) {
    if (b == 0) {x = 1; y = 0;return a;}
    int d=exgcd(b, a % b, y, x); //x,y顺序交换
    y -=(a/b)*x;return d;}
*)

(* 模运算意义下的逆元:存在x使a*x和1模m同余,则x为a模m的逆元 *)

(* 中国剩余定理:x的多个同余式(与a1,a2,...,an同余,mod分别为m1,m2,...,mn) *)
(* 存在唯一的整数解x(mod M),M=m1m2m3...mn *)

(* 多重集合:集合但元素可以重复出现,但不同于数列是顺序无关的 *)

(* 错排列:新排列中元素全都不在原来的位置 *)
(* 圆排列:顺时针/逆时针产生的新排列 *)

(* 鸽巢原理:n鸽子数>m巢数,必有巢中有2个以上鸽子 *)

(* 容斥原理:集合相交时,去除多余部分 *)

(* 卡特兰数:n个元素出入栈的种数 *)
(* 种数:catalan(n)=C(2n,n)/(n+1), (n in [0,+inf)) *)
(* 通项:ctlk(k,n-k)=catalan(k)*catalan(n-k) *)
(* 递归地看为出入栈内外,{(push)内(pop)外},所以通项为内部catalan种数乘外部catalan种数 *)
(* catalan(0)=1,catalan(1)=1,catalan(n)=ctlk(0,n-0)+ctlk(1,n-1)+...+ctlk(n,n-n) *)
<catalan> ::= <push> <catalan> <pop> <catalan>

(* 大步小步,BSGS(baby step giant step) *)
(* 小步:枚举g^j for j=0,1,...,m-1到哈希表 *)
(* 大步:枚举(g^m)^i for i=0,1,...,m-1在哈希表中查找匹配 *)
(* 复杂度从O(p)降到O(sqrt(p)) *)

```



<font color=#ff0044>
<center>Written by Vito Devlin :tada: </center>
<center>condexpr01@outlook.com</center>
</font>

