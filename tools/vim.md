# <font color=#39c5bb>:tada: Basic Vim :tada:</font>

## :sparkles: common :sparkles:
|<font color=#39c5bb>:h[elp] {subject}</font>|<font color=#ffa500>显示指定关键字的帮助</font>|
|:---|:---|
|<font color=#39c5bb>Esc</font>|<font color=#ffa500>退出模式</font>|
|<font color=#39c5bb>h</font>|<font color=#ffa500>左移光标</font>|
|<font color=#39c5bb>j</font>|<font color=#ffa500>下移光标</font>|
|<font color=#39c5bb>k</font>|<font color=#ffa500>上移光标</font>|
|<font color=#39c5bb>l</font>|<font color=#ffa500>右移光标</font>|
|<font color=#39c5bb>:redir @{a-zA-Z@"\*+} , :redir end</font>|<font color=#ffa500>重定向消息到寄存器</font>|
|<font color=#39c5bb>:w</font>|<font color=#ffa500>保存</font>|
|<font color=#39c5bb>:wa</font>|<font color=#ffa500>保存全部</font>|
|<font color=#39c5bb>:wq[!] 或 :x 或 ZZ</font>|<font color=#ffa500>保存并退出</font>|
|<font color=#39c5bb>:q[!]</font>|<font color=#ffa500>退出</font>|
|<font color=#39c5bb>:qa[!]</font>|<font color=#ffa500>全部退出</font>|
|<font color=#39c5bb>:wqa</font>|<font color=#ffa500>保存所有标签页，并全部退出</font>|
|<font color=#39c5bb>q:</font>|<font color=#ffa500>命令编辑模式</font>|
|<font color=#39c5bb>q/ 和 q?</font>|<font color=#ffa500>搜索编辑模式</font>|
|<font color=#39c5bb>w</font>|<font color=#ffa500>移动到下个单词开头</font>|
|<font color=#39c5bb>e</font>|<font color=#ffa500>移动到下个单词结尾</font>|
|<font color=#39c5bb>b</font>|<font color=#ffa500>移动到上个单词开头</font>|
|<font color=#39c5bb>r</font>|<font color=#ffa500>替换当前字符</font>|
|<font color=#39c5bb>:n</font>|<font color=#ffa500>移动到第 n 行</font>|
|<font color=#39c5bb>u</font>|<font color=#ffa500>undo - 撤销最近一次操作</font>|
|<font color=#39c5bb>ctrl-r</font>|<font color=#ffa500>redo - 重做（和 u 相反）</font>|
|<font color=#39c5bb>p</font>|<font color=#ffa500>在光标后粘贴复制的内容</font>|
|<font color=#39c5bb>P</font>|<font color=#ffa500>在光标前粘贴复制的内容</font>|
|<font color=#39c5bb>i</font>|<font color=#ffa500>从光标前开始插入字符</font>|
|<font color=#39c5bb>a</font>|<font color=#ffa500>从光标后开始插入字符</font>|
|<font color=#39c5bb>o</font>|<font color=#ffa500>在当前行之下另起一行，开始插入字符</font>|
|<font color=#39c5bb>O</font>|<font color=#ffa500>在当前行之上另起一行，开始插入字符</font>|
|<font color=#39c5bb>v</font>|<font color=#ffa500>进入可视化模式，移动光标高亮选择，然后，可以对被选中的文本执行命令</font>|
|<font color=#39c5bb>V</font>|<font color=#ffa500>进行可视化模式，以行为单位进行选择</font>|
|<font color=#39c5bb>ctrl-q</font>|<font color=#ffa500>进入可视化模式，矩阵选择（即列模式）</font>|
|<font color=#39c5bb>{Visual}o</font>|<font color=#ffa500>在可视化模式下，让光标在选择区域的开头和结尾进行切换</font>|
|<font color=#39c5bb>{Visual}O</font>|<font color=#ffa500>在可视化模式下，切换光标到选择区域的角</font>|
|<font color=#39c5bb>{Visual}aw</font>|<font color=#ffa500>在可视化模式下，选择当前单词</font>|
|<font color=#39c5bb>{Visual}ab</font>|<font color=#ffa500>在可视化模式下，选择被 () 包裹的区域的内容（包含括号）</font>|
|<font color=#39c5bb>{Visual}aB</font>|<font color=#ffa500>在可视化模式下，选择被 {} 包裹的区域的内容（包含花括号）</font>|
|<font color=#39c5bb>{Visual}at</font>|<font color=#ffa500>在可视化模式下，选择被 <tag><\/tag> 包裹的区域的内容（包含 <> 标签）</font>|
|<font color=#39c5bb>{Visual}ib</font>|<font color=#ffa500>在可视化模式下，选择被 () 包裹的区域的内容（不包含括号）</font>|
|<font color=#39c5bb>{Visual}iB</font>|<font color=#ffa500>在可视化模式下，选择被 {} 包裹的区域的内容（不包含花括号）</font>|
|<font color=#39c5bb>{Visual}it</font>|<font color=#ffa500>在可视化模式下，选择被 <tag><\/tag> 包裹的区域的内容（不包含 <> 标签）</font>|
|<font color=#39c5bb>{Visual}y</font>|<font color=#ffa500>复制选中的文本</font>|
|<font color=#39c5bb>{Visual}d</font>|<font color=#ffa500>剪切选中的文本</font>|
|<font color=#39c5bb>{Visual}u</font>|<font color=#ffa500>将选中的文本转换为小写</font>|
|<font color=#39c5bb>{Visual}U</font>|<font color=#ffa500>将选中的文本转换为大写</font>|
|<font color=#39c5bb>{Visual}></font>|<font color=#ffa500>向右缩进</font>|
|<font color=#39c5bb>{Visual}<</font>|<font color=#ffa500>向左缩进</font>|
|<font color=#39c5bb>{Visual}=</font>|<font color=#ffa500>将选中的文本自动缩进</font>|

## :sparkles: jump :sparkles:
|<font color=#39c5bb>%</font>|<font color=#ffa500>跳转到配对的符号,<br>在命令模式表示当前文件</font>|
|:---|:---|
|<font color=#39c5bb>\*</font>|<font color=#ffa500>向下查找光标处单词</font>|
|<font color=#39c5bb>#</font>|<font color=#ffa500>向上查找光标处单词</font>|
|<font color=#39c5bb>''</font>|<font color=#ffa500>移动到上次跳转的行</font>|
|<font color=#39c5bb>0</font>|<font color=#ffa500>移动到行首</font>|
|<font color=#39c5bb>$</font>|<font color=#ffa500>移动到行尾</font>|
|<font color=#39c5bb>.</font>|<font color=#ffa500>再次执行上次改变</font>|
|<font color=#39c5bb>x</font>|<font color=#ffa500>剪切当前字符</font>|
|<font color=#39c5bb>c{motion}$</font>|<font color=#ffa500>删除光标所在位置到指定文本，然后进入插入模式</font>|
|<font color=#39c5bb>dd</font>|<font color=#ffa500>剪切当前行的文本</font>|
|<font color=#39c5bb>d{motion}</font>|<font color=#ffa500>剪切当前行的文本</font>|
|<font color=#39c5bb>gg</font>|<font color=#ffa500>移动到文件第一行</font>|
|<font color=#39c5bb>cc</font>|<font color=#ffa500>将光标所在的行删除，然后进入插入模式</font>|
|<font color=#39c5bb>G</font>|<font color=#ffa500>移动到文件最后一行</font>|
|<font color=#39c5bb>y{motion}</font>|<font color=#ffa500>复制</font>|
|<font color=#39c5bb>yy</font>|<font color=#ffa500>复制当前行</font>|
|<font color=#39c5bb>nyy</font>|<font color=#ffa500>复制 n 行</font>|
|<font color=#39c5bb>zz</font>|<font color=#ffa500>移动屏幕使光标居中</font>|
|<font color=#39c5bb>fx</font>|<font color=#ffa500>移动到字符 x 下次出现的位置</font>|
|<font color=#39c5bb>Fx</font>|<font color=#ffa500>移动到字符 x 上次出现的位置</font>|
|<font color=#39c5bb>tx</font>|<font color=#ffa500>移动到字符 x 下次出现的位置的前一个字符</font>|
|<font color=#39c5bb>Tx</font>|<font color=#ffa500>移动到字符 x 上次出现的位置的后一个字符</font>|
|<font color=#39c5bb>:sav[eas] {file}</font>|<font color=#ffa500>另存为...</font>|
|<font color=#39c5bb>:clo[se]</font>|<font color=#ffa500>关闭当前窗口</font>|
|<font color=#39c5bb>:ter[minal]</font>|<font color=#ffa500>打开新的终端窗口</font>|

## :sparkles: record :sparkles:
|<font color=#39c5bb>q{0-9a-zA-Z"}</font>|<font color=#ffa500>录制宏 a</font>|
|:---|:---|
|<font color=#39c5bb>q</font>|<font color=#ffa500>停止录制宏</font>|
|<font color=#39c5bb>@a</font>|<font color=#ffa500>执行宏 a</font>|
|<font color=#39c5bb>@@</font>|<font color=#ffa500>重新执行上次执行的宏</font>|

## :sparkles: register :sparkles:
|<font color=#39c5bb>:reg[isters]</font>|<font color=#ffa500>显示寄存器的内容</font>|
|:---|:---|
|<font color=#39c5bb>"{register}y</font>|<font color=#ffa500>复制内容到寄存器 x</font>|
|<font color=#39c5bb>"{register}p</font>|<font color=#ffa500>粘贴寄存器 x 中的内容</font>|
|<font color=#39c5bb>"{register}P</font>|<font color=#ffa500>粘贴寄存器 x 中的内容</font>|
|<font color=#39c5bb>"+y</font>|<font color=#ffa500>复制内容到系统剪贴板寄存器</font>|
|<font color=#39c5bb>"+p</font>|<font color=#ffa500>粘贴系统剪贴板寄存器的内容</font>|
|<font color=#39c5bb>"+P</font>|<font color=#ffa500>粘贴系统剪贴板寄存器的内容</font>|

## :sparkles: indent :sparkles:
|<font color=#39c5bb>={motion}</font>|<font color=#ffa500>自动缩进</font>|
|:---|:---|
|<font color=#39c5bb>==</font>|<font color=#ffa500>自动缩进当前行</font>|
|<font color=#39c5bb>>></font>|<font color=#ffa500>将当前行向右缩进，宽度由 shiftwidth 控制</font>|
|<font color=#39c5bb><<</font>|<font color=#ffa500>将当前行向左缩进，宽度由 shiftwidth 控制</font>|
|<font color=#39c5bb>>{motion}</font>|<font color=#ffa500>向右缩进</font>|
|<font color=#39c5bb><{motion}</font>|<font color=#ffa500>向左缩进</font>|

## :sparkles: search :sparkles:
|<font color=#39c5bb>/pattern</font>|<font color=#ffa500>在当前文本中查找 pattern</font>|
|:---|:---|
|<font color=#39c5bb>/\vpattern</font>|<font color=#ffa500>very magic把 pattern中的非字母数字字符视为正则表达式特殊字符</font>|
|<font color=#39c5bb>?pattern</font>|<font color=#ffa500>向上查找 pattern</font>|
|<font color=#39c5bb>n</font>|<font color=#ffa500>查找下一个</font>|
|<font color=#39c5bb>N</font>|<font color=#ffa500>查找上一个</font>|
|<font color=#39c5bb>:noh[lsearch]</font>|<font color=#ffa500>移除搜索结果的高亮显示</font>|
|<font color=#39c5bb>:%s/old/new/g</font>|<font color=#ffa500>把 old 全部替换成 new</font>|
|<font color=#39c5bb>:%s/old/new/gc</font>|<font color=#ffa500>把 old 逐个替换成 new</font>|
|<font color=#39c5bb>:vim[grep] /pattern/[g][j][f] {file}</font>|<font color=#ffa500>在多个文件中搜索 pattern,\*\*递归查找</font>|
|<font color=#39c5bb>:cn[ext]</font>|<font color=#ffa500>移动至下一个搜索结果</font>|
|<font color=#39c5bb>:cp[revious]</font>|<font color=#ffa500>移动至上一个搜索结果</font>|
|<font color=#39c5bb>:cope[n]</font>|<font color=#ffa500>打开搜索结果列表</font>|
|<font color=#39c5bb>:ccl[ose]</font>|<font color=#ffa500>关闭 quickfix 窗口</font>|
|<font color=#39c5bb>ctrl-n</font>|<font color=#ffa500>在插入模式下，在光标之前插入自动补全的下一个匹配项</font>|
|<font color=#39c5bb>ctrl-p</font>|<font color=#ffa500>在插入模式下，在光标之前插入自动补全的上一个匹配项</font>|
|<font color=#39c5bb>ctrl-b</font>|<font color=#ffa500>向上滚动一屏</font>|
|<font color=#39c5bb>ctrl-f</font>|<font color=#ffa500>向下滚动一屏</font>|

## :sparkles: buffer :sparkles:
|<font color=#39c5bb>:e[dit] [++opt] [+cmd] {files}</font>|<font color=#ffa500>编辑文件,++encoding=utf-x指定编码</font>|
|:---|:---|
|<font color=#39c5bb>:bn[ext]</font>|<font color=#ffa500>切换到下一个缓冲区</font>|
|<font color=#39c5bb>:bp[revious]</font>|<font color=#ffa500>切换到上一个缓冲区</font>|
|<font color=#39c5bb>:bd[elete]</font>|<font color=#ffa500>关闭缓冲区</font>|
|<font color=#39c5bb>:b[uffer] N</font>|<font color=#ffa500>切换到第 N 个缓冲区</font>|
|<font color=#39c5bb>:b[uffer] 文件</font>|<font color=#ffa500>切换到指定文件的缓冲区</font>|
|<font color=#39c5bb>:ls! 或 :buffers!</font>|<font color=#ffa500>列出所有打开的缓冲区</font>|
|<font color=#39c5bb>:pwd</font>|<font color=#ffa500>当前位置</font>|

## :sparkles: tab :sparkles:
|<font color=#39c5bb>:{count}tabe[dit] {file}</font>|<font color=#ffa500>在新标签中打开文件</font>|
|:---|:---|
|<font color=#39c5bb>{count}gt 或 :tabn[ext]</font>|<font color=#ffa500>切换到下一个或第count个标签</font>|
|<font color=#39c5bb>{count}gT 或 :tabp[revious]</font>|<font color=#ffa500>切换到上一个或第count个标签</font>|
|<font color=#39c5bb>:tabm[ove] N</font>|<font color=#ffa500>把当前标签移动到第 N 个位置（下表从 0 开始）</font>|
|<font color=#39c5bb>tabc[lose]</font>|<font color=#ffa500>关闭当前标签</font>|
|<font color=#39c5bb>:tabo[nly]</font>|<font color=#ffa500>关闭其他标签</font>|
|<font color=#39c5bb>:tabdo {cmd}</font>|<font color=#ffa500>在所有标签中执行指定的命令</font>|

## :sparkles: window :sparkles:
|<font color=#39c5bb>:tab ba[ll]</font>|<font color=#ffa500>以标签的形式编辑所有缓冲区</font>|
|:---|:---|
|<font color=#39c5bb>:sp[lit]</font>|<font color=#ffa500>水平分割窗口</font>|
|<font color=#39c5bb>:vs[plit]</font>|<font color=#ffa500>垂直分割窗口</font>|
|<font color=#39c5bb>:vert[ical] {cmd}</font>|<font color=#ffa500>垂直窗口</font>|
|<font color=#39c5bb>:hor[izontal] {cmd}:[ical] ba[ll]</font>|<font color=#ffa500>水平窗口</font>|
|<font color=#39c5bb>:set scrollbind</font>|<font color=#ffa500>切换到每个需要的窗口执行,同步滚动</font>|
|<font color=#39c5bb>:set noscrollbind</font>|<font color=#ffa500>切换每个需要的窗口执行,取消同步滚动</font>|
|<font color=#39c5bb>:[range]windo {cmd}</font>|<font color=#ffa500>windows do对所有或range内窗口执行cmd</font>|
|<font color=#39c5bb>:windo set scrollbind\|noscrollbind</font>|<font color=#ffa500>对所有窗口执行绑定或取消同步滚动</font>|
|<font color=#39c5bb>ctrl-w=</font>|<font color=#ffa500>让每个窗口具有相同高度和宽度</font>|
|<font color=#39c5bb>ctrl-w{motion}</font>|<font color=#ffa500>切换窗口</font>|

## :sparkles: fold :sparkles:
|<font color=#39c5bb>{Visual}zf</font>|<font color=#ffa500>建立一个所选内容的折叠</font>|
|:---|:---|
|<font color=#39c5bb>:{range}fo[ld]</font>|<font color=#ffa500>建立一个所选内容的折叠</font>|
|<font color=#39c5bb>zo</font>|<font color=#ffa500>z open展开光标处的区块</font>|
|<font color=#39c5bb>zc</font>|<font color=#ffa500>z close折叠光标处的区块</font>|
|<font color=#39c5bb>zr</font>|<font color=#ffa500>z reduce展开一级折叠层级</font>|
|<font color=#39c5bb>zR</font>|<font color=#ffa500>z reduce完全展开折叠层级</font>|
|<font color=#39c5bb>zm</font>|<font color=#ffa500>z more收起一级折叠层级</font>|
|<font color=#39c5bb>zM</font>|<font color=#ffa500>z more完全收起折叠层级</font>|
|<font color=#39c5bb>zi</font>|<font color=#ffa500>z invert切换是否允许折叠</font>|

## :sparkles: mark :sparkles:
|<font color=#39c5bb>:ju[mps]</font>|<font color=#ffa500>列出所有跳转</font>|
|:---|:---|
|<font color=#39c5bb>:changes</font>|<font color=#ffa500>列出所有修改历史</font>|
|<font color=#39c5bb>:marks</font>|<font color=#ffa500>显示标记列表</font>|
|<font color=#39c5bb>m{mark}</font>|<font color=#ffa500>设置当前位置为标记</font>|
|<font color=#39c5bb>'{mark}</font>|<font color=#ffa500>跳转到标记的位置</font>|
|<font color=#39c5bb>'0</font>|<font color=#ffa500>跳转到 Vim 上一次退出时的位置</font>|
|<font color=#39c5bb>'"</font>|<font color=#ffa500>跳转到该文件上次编辑时的位置</font>|
|<font color=#39c5bb>'.</font>|<font color=#ffa500>跳转到该文件中最后一次修改的位置</font>|
|<font color=#39c5bb>''</font>|<font color=#ffa500>跳转到最后跳转的位置</font>|

## :sparkles: diff :sparkles:
|<font color=#39c5bb>:diffthis</font>|<font color=#ffa500>set diff把当前窗口的内容作为文件对比的一部分</font>|
|:---|:---|
|<font color=#39c5bb>:diffoff</font>|<font color=#ffa500>set nodiff取消当前窗口的内容作为文件对比的一部分</font>|
|<font color=#39c5bb>:windo diffthis\|diffoff</font>|<font color=#ffa500>把所有窗口的内容作为文件对比的一部分或取消</font>|
|<font color=#39c5bb>]c</font>|<font color=#ffa500>跳转到下一个不同处</font>|
|<font color=#39c5bb>[c</font>|<font color=#ffa500>跳转到上一个不同处</font>|
|<font color=#39c5bb>[count]do 或 :diffg[et]</font>|<font color=#ffa500>diff obtain拉不同的过来</font>|
|<font color=#39c5bb>[count]dp 或 :diffpu[t]</font>|<font color=#ffa500>diff put推不同的过去</font>|
|<font color=#39c5bb>:dif[fupdate]</font>|<font color=#ffa500>刷新，重新比较</font>|

## :sparkles: alternative :sparkles:
|<font color=#39c5bb>~</font>|<font color=#ffa500>对选中的文本进行大小写切换</font>|
|:---|:---|
|<font color=#39c5bb>A</font>|<font color=#ffa500>从行尾开始插入字符</font>|
|<font color=#39c5bb>B</font>|<font color=#ffa500>移动到上个单词开头（单词含标点）</font>|
|<font color=#39c5bb>E</font>|<font color=#ffa500>移动到下个单词结尾（单词含标点）</font>|
|<font color=#39c5bb>g_</font>|<font color=#ffa500>移动到行内最后一个非空白符</font>|
|<font color=#39c5bb>g,</font>|<font color=#ffa500>转到修改历史列表较新的位置</font>|
|<font color=#39c5bb>g;</font>|<font color=#ffa500>转到修改历史列表较旧的位置</font>|
|<font color=#39c5bb>U</font>|<font color=#ffa500>恢复/撤销最后修改的行</font>|
|<font color=#39c5bb>H</font>|<font color=#ffa500>移动到当前页面顶部</font>|
|<font color=#39c5bb>I</font>|<font color=#ffa500>从行首开始插入字符</font>|
|<font color=#39c5bb>J</font>|<font color=#ffa500>将下一行合并到当前行，并在两部分文本之间插入一个空格</font>|
|<font color=#39c5bb>K</font>|<font color=#ffa500>打开光标所在单词对应的 man 页面</font>|
|<font color=#39c5bb>L</font>|<font color=#ffa500>移动到当前页面底部</font>|
|<font color=#39c5bb>M</font>|<font color=#ffa500>移动到当前页面中间</font>|
|<font color=#39c5ab>R</font>|<font color=#ffa500>逐个替换当前行字符</font>|
|<font color=#39c5bb>s</font>|<font color=#ffa500>删除当前字符，然后进入插入模式</font>|
|<font color=#39c5bb>S</font>|<font color=#ffa500>将光标所在的行删除，然后进入插入模式</font>|
|<font color=#39c5bb>W</font>|<font color=#ffa500>移动到下个单词开头（单词含标点）</font>|
|<font color=#39c5bb>}</font>|<font color=#ffa500>移动到下一个段落（当编辑代码时则为函数／代码块）</font>|
|<font color=#39c5bb>{</font>|<font color=#ffa500>移动到上一个段落（当编辑代码时则为函数／代码块）</font>|
|<font color=#39c5bb>^</font>|<font color=#ffa500>移动到行首的非空白符</font>|
|<font color=#39c5bb>;</font>|<font color=#ffa500>重复之前的 f、t、F、T 操作</font>|
|<font color=#39c5bb>,</font>|<font color=#ffa500>反向重复之前的 f、t、F、T 操作</font>|

> :tada: Written by Vito :tada:
> :tada: 2025年7月18日03:29:22 :tada:
> :tada: email:condexpr01@outlook.com :tada:
