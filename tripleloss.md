# Triplet-Loss简介及在分类任务中的修改和mxnet实现

## 一、Triplet Loss简介

详见：[Triplet-Loss原理](https://blog.csdn.net/u013082989/article/details/83537370)

输入是一个三元组：<a, p, n>

a：anchor

p：positive，和a是同一类别的样本

n：negative，和a是不同类别的样本

公式：

$ L = max(d(a,p) - d(a,n) + margin, 0) $

* 所以优化目标是拉近 $ a, p $ 的距离，拉远 $a,n$ 的距离
* `easy triplets`: 
* `hard triplets`:
* `semi-hard triplets`:

## 二、Triplet Loss用于图像分类任务

 对于一个batch中的样本 $ \left \{ x_{1},x_{2},...,x_{B} \right \} $，我们有两种修改方法

**最大最小法**

$ L = \sum_{i=1}^{B} max(\max_{p}d_{ip} - \min_{n}d_{in} + margin, 0) $

**遍历所有**

$ L = \sum_{i}^{B}\sum_{p}\sum_{n}max(d_{ip} - d_{in} + margin, 0) $

code:

```python
class ClsTripletLoss(Loss):
    def __init__(self, margin=1.0, weight=1., batch_axis=0, **kwargs):
        super(ClsTripletLoss, self).__init__(weight, batch_axis, **kwargs)
        self._margin = margin

    def hybrid_forward(self, F, features, labels):
        """根据triplet loss修改，同类样本间的距离小于一类一个margin.
        
        Parameters
        ----------
        features : Symbol or NDArray
                   The sample features, shape: (batch_size, dims)
        labels : Symbol or NDArray
                 The sample Labels, shape: (batch_size,) 
        """
        num_p = (labels.expand_dims(axis=1) == labels.expand_dims(axis=0)).sum().astype(np.float32) - 128
        num_n = (labels.expand_dims(axis=1) != labels.expand_dims(axis=0)).sum().astype(np.float32)

        w = ((labels.expand_dims(axis=1) == labels.expand_dims(axis=0)).astype(np.float32) * 2 - 1)
        distance = ((features.expand_dims(axis=1) - features.expand_dims(axis=0)) ** 2).sum(axis=-1)
        loss = (distance.expand_dims(axis=2) - distance.expand_dims(axis=0) + self._margin).relu()
        w = (w.expand_dims(axis=2) - w.expand_dims(axis=0)).relu() / 2.0
        loss = w * loss
        return loss
```

