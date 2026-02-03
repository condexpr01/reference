
# git

checkout(HEAD)指定当前操作位置,`--orphan`指定到新的孤树空链开始

commit(前进)从当前HEAD往下以staged区建立子节点
reset(进退快照状态)将当前HEAD标记到新位置,重置树链,`--hard`重置当前工作区,暂存区

branch(新树链路径标记)在某个节点设置新的树链,`-D`删除树链标记,`-f`跳到指定位置


fsck(悬空检查,filesystem check)
gc (回收悬空garbage collection),`--prune`(剪14.day.ago的,=now现在就剪)


remote(设置远程源)
push(推出树链更新)
pull(拉入树链更新)

add(加入工作文件的临时快照),restore(可以恢复去除修改)
rm(去除工作文件和临时快照),`--cached`(只删除临时快照)

ls-tree(查看节点快照)

rebase(使节点成为当前HEAD的基)
merge(使节点与当前节点合并)

HEAD相对位置(^:上一个,~n:上n个)
