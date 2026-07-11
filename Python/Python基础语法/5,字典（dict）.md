# 定义
键不可重复
dict_instance = {x:y,x:y,....}
dict_instance = dict()
dict_instance = {}

## 常见操作
添加
	dict_instance\[key\] = value
删除
	dict_instance.pop(key)   删除并返回
	del  dict_instance\[key\]
查询
	dict_instance.keys()
		获取所有的key
	dict_instance.get(key)
		根据key获取value
	dict_instance.values()
		获取所有的value
	dict_instance.items()
		获取所有的key-value键值对