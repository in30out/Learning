*其底层集成了starlette这个库，一些操作是本质是由strlette来实现*
# 使用步骤
- 1,导入FastAPI
- 2,创建FastAPI实例对象
- 3,创建路径操作函数，定义访问路径
- 4,运行FastAPI服务
	- fastapi     dev    "xxxx.py"
    - uvicorn   xxxx:app --reload
- 5,命令行中，ctrl+C中断
#### 例
```
from fastapi import FastAPI

#创建FastAPI实例
app = FastAPI()

#定义功能接口----->该函数的返回值表示API接口的返回的数据，接口访问路径为/,请求方式GET
@app.get('/')
def root():
	return {"message":"Hello World"}

#定义功通接口
@app.get('/users')
def get_usars():
	return [{'id':1,"name":"Alice","age":18}
			{'id':2,'name':'Bob','age':20}
			]
			
#uvicorn:   启动服务----->uvicorn是Python中的轻量级Web服务器
#启动FastAPI服务器
if __name__ == "__main__":
	import uvicorn
	uvicorn.run(app,host='0.0.0.0',port=8000,access_log = False)
		#access_log管理日志显示开关
```

# 相关方法
### 静态挂载（mount）
当网页返回多个同一路径下文件的请求时，可以使用该方法统一回应，而不是创建多个回应方法

#### 什么是挂载（Mounting)[¶](https://fastapi.tiangolo.com/zh/tutorial/static-files/#what-is-mounting "Permanent link")
“挂载”表示在特定路径添加一个完全“独立”的应用，然后负责处理所有子路径。
这与使用 `APIRouter` 不同，因为挂载的应用是完全独立的。主应用的 OpenAPI 和文档不会包含已挂载应用的任何内容，等等
#### 例
```
from fastapi import FastAPI
from fastapi.staticfiles import StaticFiles

app = FastAPI()

app.mout('/static',StaticFiles(directory='static'),name = 'static')
```
#### 细节[¶](https://fastapi.tiangolo.com/zh/tutorial/static-files/#details "Permanent link")
第一个 `"/static"` 指的是这个“子应用”将被“挂载”到的子路径。因此，任何以 `"/static"` 开头的路径都会由它处理。
`directory="static"` 指的是包含你的静态文件的目录名称。
`name="static"` 为它提供了一个可被 **FastAPI** 内部使用的名称。
这些参数都可以不是“`static`”，请根据你的应用需求和具体细节进行调整。

# 异常处理器
#### 例
```
@app.exception_handler(Exception)
def handle_exceptino(request:Request,exc:Exception):
	logging.error(f"处理异常，请求路径:{request.url},异常信息:{exc}")
	return JSONReponse(content={"code":500,"message":"服务器内部错误"})
```

