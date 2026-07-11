*ABC:Abstract Base Class*
*抽象类：是一种只能被继承，不能被直接实例化的类，作用就是规定了类必胹要实现哪些方法，强制子类必须遵守统一的代码规范*
## 例
```
from abc import ABC,abstractmethod

class Member(ABC):
	@abstractmethod          #(装饰器)
	def 方法名():
		pass
	
```