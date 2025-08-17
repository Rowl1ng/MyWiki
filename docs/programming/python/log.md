# 日志 logging



[python logging模块使用教程][11]
[logging basic tutorial][12]
![此处输入图片的描述][13]

## Logger 记录器
Logger是一个树形层级结构，在使用接口debug，info，warn，error，critical之前必须创建Logger实例，即创建一个记录器，如果没有显式的进行创建，则默认创建一个root logger，并应用默认的日志级别(WARN)，处理器Handler(StreamHandler，即将日志信息打印输出在标准输出上)，和格式化器Formatter(默认的格式即为第一个简单使用程序中输出的格式)。

```
logger = logging.getLogger(logger_name)
```

- 单独目录：例如 /var/logs/ 下，按照应用分成不同的目录，或者是文件。日志不要放在应用目录下，那样不利于自动化部署和应用升级，备份等。
- class 中设置logger `self.logger = logging.getLogger(type(self).__name__)`
- 模块、文件中设置 logger `logger = logging.getLogger(__name__)`
- 使用JSON YAML等格式来配置logging，感觉比使用代码或者 ini格式看起来更方便

简单的小应用中，单个日志文件，同时还要打印控制台

```python
import logging

logging.basicConfig(level=logging.DEBUG,
format='%(asctime)s %(name)-12s %(levelname)-8s %(message)s',
datefmt='%m-%d %H:%M',
filename='/temp/myapp.log',
filemode='w')
console = logging.StreamHandler()
console.setLevel(logging.INFO)
formatter = logging.Formatter('%(name)-12s: %(levelname)-8s %(message)s')
console.setFormatter(formatter)
# add the handler to the root logger
logging.getLogger('').addHandler(console)
```

记录 Exception 的trace 信息（很有用哦)

```python
try:
open('/path/to/does/not/exist', 'rb')
except (SystemExit, KeyboardInterrupt):
raise
except Exception, e:
logger.error('Failed to open file', exc_info=True)

```

```python
logger.warning("consider setting layer size to a multiple of 4 for greater performance")

```


# Inbox

python的logging模块：`__name__`