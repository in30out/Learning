[第1章. AI通识与基础 - 飞书云文档](https://my.feishu.cn/wiki/PAb6wSNnziRrlEk1PtNcH38LnJZ)

# 初始化
## 模型调用
### 开发步骤
```
---------- 1，引入依赖 -------------
uv add langchain
uv add langchain-deepseek

---------- 2,配置环境 --------------
DEEPSEEK_API_KEY = sk-xxx

---------- 3,初始化模型 ------------
##最新引入方法
from langchain.chat_models import init_chain_model
model = init_chat_model(model="deepseek-chat")

## 老方法
from langchain_openai import ChatOpenAI
model = ChatOpenAI(model="gpt-5.2")

---------- 4,当模型不被langchain支持，我们必须自定义模型参数来访问-------------
#以阿里云百炼为例
import os

base_url = os.getenv("DASHSCOPE_BASE_URL")
api_key = os.getenv("DASHSCOPE_API_KEY")

model = init_chat_model(
	model = "qwen-max"  #模型名称，这里可以自定义，我们用的是阿里的qwen-max
	model_provider = "openai"    #如果是Langchain不支持的模型，需要指定模型提供者（虽然我们用的是阿里，但是阿里兼容openai,所以这里用openai
	
	base_url = base_url   #覆写模型访问地址，不写就访问openai了
	api_key = api_key
	)
	
----------5，调整模型参数 -------------------
##init_chat_model函数允许我们调整模型参数

- temperature:控制生成文本的随机性，值越小越确定（0-2.0）
- max_tokens:控制生成文本的最大长度
- top_p:控制生成文本的多样性，值越小越多样，值越大越确定
- timeout:控制生成文本的超时时间
- max_retries:控制生成文本的最大重试次数
```
### 阻塞式调用(invoke)
response = model.invoke()
### 流式调用(stream)
```
stream = model.stream()
for chunk in stream:
	print(chunk.content,end="",flush=True)
```

## 智能体
### 创建智能体
```
from langchain_agents import create_agent

#方法一,预先初始化好模型对象
agent = create_agent(model=model)
#方法二，创建agent对象时指定模型 
agent = create_agent(model="deepseek-chat")
```
### 阻塞式调用
```
response = agent.invoke({
        "message":[
            {"role":"system","content":f"{system_prompt_1}"},
            {"role":"user","content":f"{user_prompt_1}"}
        ]
    })
```
### 流式调用
```
messages = agent.stream(
	{
		"message":[
			{"role":"system","content":f"{system_prompt_1}"},
			{"role":"user","content":f"{user_prompt_1}"}
		]
	},
	streammode = "message"
	)

#metadata  元数据
for chunk,metadata in messages:
	if chunk.conten:
		print(chunk.content,end="",flush=True)
```

# 消息(Messages)
***在LangChain中，发送给模型的消息、模型返回的消息都统一封装为BaseMessage类，并且准备了多个BaseMessage的子类对应不同角色类型的消息***
### BaseMessage
- SystemMessage -> Role :   system      # 代表系统消息，用于设定模型角色和交互背景
- HumanMessage -> Role :   user          # 代表用户输入的消息
- AIMessage         -> Role :   assistant   # 代表LLM生成的响应，包含:文本、工具调用、元数据
- ToolMessage      -> Role :   tool          # 代表工具调用时产生的结果

#### 例
```
from langchain_core.message import SystemMessage,AIMesage,HumanMessage

response = agent.invoke({
	"message":[
		SystemMessage("sysetm_prompt")
		HumanMessage("xxxx")
		AIMessage("xxxx")
		HumanMessage("xxxxx")
	]
})
```

# 千问模型调用
*先进行伪装*
```
from langchain.models import init_chat_model
import os

model = init_chat_model(
		model = "qwen3.7-plus"
		model_provider="openai"
		base_url = os.getenv("DASHSCOPE_BASE_URL")
		api_key = os.getenv("DASHSCOPE_API_KEY")
)

agent = create_agent(model = model)

message = HumanMessage([ 
		{"type": "text", "text": "Describe the content of this image."}, 
		{"type": "image", "file_id": "file-abc123"}, 
		])
```

# 结构化输出
```
from pydantic import BaseModel

#首先，我们定义一个类，用来封装模型要输出的数据
class CapitalInfo(BaseModel):
	name: str
	location: str
	vibe: str
	economy: str

agent = create_agent(
	modle = "deepseek-chat",
	system_prompt = "你是一个科幻作家，根据用户的要求创建一个太空之都"
	response_format = CapitalInfo #设置结构化输出的格式
)

response = agent.invoke(
	{"messages":[HumanMessage(content = "月球的首都是什么？")]}
)

```

# 工具
### 第一种工具注释
导包
	from langchain_core.tools import tool
装饰器定义工具
	@tool
	函数名（变量）：
	"""描述
	变量解释"""
### 第二种工具注释
*采用pydantic来描述，用pydantic介绍变量作用,函数内只用写函数作用就行了*
```
#通过自定义model来约束入参
from pydantic import BaseModel,Field
from typing import Literal #枚举类型

class WeatherInput(BaseModel):
	location: str = Field(description="City name or coordinates")
	units:Literal["celsius","fahrenheit"] = Field(
		default = "celsius",
		description = "Temperature unit preference"
	)
	include_forecast:bool = Field(
		default = False,
		description = "Include 5-day forecast"
	)
	
@tool(args_schema = WeatherInput)
def get_weather(location:str,units:str = "celsius",include_forecast:bool = False) ->str:
	""" # tool description # """
	temp = 22 if units == "celsius" else 72
	result = f"Current weather in {location}:{temp} degrees {units[0].upper()}"
	if include_forecast:
		result += "\nNext 5 days:Sunny"
	return result
```

# 记忆
## 添加短期记忆
- 导入并初始化Checkpointer
- 创建Agent，指定Checkpointer
- 调用Agent，指定thread_id
#### 例
```
from langchain.agents import create_agent
from langgraph.checkpoint.memory import InMemorySaver
from langchain.messages import HumanMessage

agent = create_agent(
	"deepseek-v4-flash"
	checkpointer=InMemorySaver()
)



agent.invoke(
	{"messages":[HumanMessage(content = "xxxx")]}，
	{“configurable”:{"thread_id":"1"}}
	
)
```

## 基于数据库的记忆管理
*先安装依赖 langgreph-checkpoint-postgres*
*导包：from langgraph.checkpoint.postgres import PostgresSaver*