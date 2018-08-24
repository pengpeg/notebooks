# 单例样本前向推断

```
sym = mx.symbol.load('./model/resnet-0000.json')
mod = mx.mod.Module(symbol=sym)
mod.bind(data_shapes=[('data', (1,1,28,28))])
mod.load_params('./model/resnet-0000.params')
from collections import namedtuple
Batch = namedtuple('Batch', ['data'])
img = mx.nd.zeros((28,28,1))
data = img.transpose([2,0,1])
data = data.reshape([1,1,28,28])
mod.forward(Batch([data]))
out = mod.get_outputs()
prob = out[0]
```
