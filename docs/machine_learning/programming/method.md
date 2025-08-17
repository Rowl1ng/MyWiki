# 常用操作

# 模型

## 保存

```python
from datetime import datetime
import socket

def get_run_name():
    """ A unique name for each run """
    return datetime.now().strftime('%b%d-%H-%M-%S') + '_' + socket.gethostname()
```