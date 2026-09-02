# commands

```shell
help  #看指令帮助
run   #运行
start #在一开始下断点
kill  #杀死当前程序
step  #s单步但进入函数
next  #n单步但不进入函数
finish #完成当前函数
until #u执行到特定行或循环结束
stepi/nexti #汇编单步

break/continue #b/c断点
info breakpoints #查看断点
info files # 查看节区的位置
delete #删除断点

frame     #f选择和打印栈帧
backtrace #bt回溯调用栈,-full完整的回溯

print #p打印
list  #l列出当前代码
x/fmt #查看

layout #代码布局
tui disable/enable #终端的ui
tui layout asm #汇编的布局
```

* see also
```shell
objdump -adM intel #反汇编为intel风格
readelf -a  #查看elf的信息, 通过类型判断PIE(position-independent executable)
```
