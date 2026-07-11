# 特性
无序，不可重复，可修改

## 定义方式
set_instance = {x,x,....}
set_instance = set()     不可以直接用大括号创建（能的是字典）
集合推导式
解包：\*set_instance

### 常用方法
add()
	添加元素到集合中
remove()
	移除集合中的指定元素（指定元素不存在将报错）
pop()
	随机删除集合中的元素并返回
clear()
	清空集合
difference()
	求取两个集合的差集（包含在第一个集合但不包含在第二个集合的元素）（-）
union()
	求取两个集合的并集（|）
intersection() 
	求到两个集合的交集（&）