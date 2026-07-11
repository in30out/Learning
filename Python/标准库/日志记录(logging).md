![[日志记录.png]]
## 日志级别
(日志级别依次升高)
- DEBUG
- INFO
- WARNING
- ERROR
- FATAL
#### 例
```
import logging

logging.basicConfig(
	level=logging.INFO,
	format = '%(asctime)s-%(levelname)s-[%(filename)s:%(lineno)d]-%(message)s'
)

logging.info('访问首页~')
```