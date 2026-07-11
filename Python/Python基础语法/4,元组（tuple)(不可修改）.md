# 特性
有序，可重复，不可修改
## 相关语句
tuple_instance = ()
按索引查找：       tuple\[i\]
切片：                 tuple\[start：end：step\]   
定义单元素元组：name_tuple = (x,)
解包：                 a,b,c,d = name_tuple
(\*)扩展解包：    a,\*b,c = name_tuple    (\*b指收集剩余所有元素并生成列表)
### 方法
tuple.count()    统计某元素在元组中出现的次数
tuple.index()    查找某个元素在元组中的索引位置（第一次出现的位置）