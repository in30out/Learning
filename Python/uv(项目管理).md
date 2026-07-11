[uv 中文文档](https://uv.doczh.com/#_1)
## 初始化(Terminal)
```
----cmd-------
uv init -p 3.14
```
### 为uv添加镜像源
```
--------  cmd   ------------
setx UV_DEFAULT_INDEX "https://pypi.tuna.tsinghua.edu.cn/simple"
```

### 配置环境变量
##### 1，创建（.env）文件
##### 2，写入配置
	deepseek
	DEEPSEEK_API_KEY=sk-1234567890
	阿里云
	DASHSCOPE_API_KEY=sk-1234567890
##### 3,安装python-dotenv。
	uv add python-dotenv
##### 4,读取环境变量
	#导包
	from dotenv import load_dotenv
##### 5,加载环境变量
文件内写程序
```
from openai import OpenAI
from dotenv import load_dotenv

load_dotenv()

client = OpenAI( api_key=os.getenv("DEEPSEEK_API_KEY"), 
			base_url="https://api.deepseek.com" )
```
	
	

