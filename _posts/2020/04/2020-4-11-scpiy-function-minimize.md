---
layout: post
title: "Python-Scipy 求函数局部极值/最大值/最小值"
categories: [Python]
description: ""
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

最近在搞量化交易，自己写了一个函数，有3个不同个参数值可以设置，每个参数值都有对应的历史数据模拟最终利润。

这样的函数无法用表达式写出来，而且是多变量参数，手动测试太麻烦，幸运的是`Scipy`库可以直接求极值。

官网：[scipy.optimize.minimize — SciPy v1.4.1参考指南](https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.minimize.html#scipy.optimize.minimize)

**如果函数可以解析，求导，可以使用`SymPy`**

## 单变量函数

```python
import numpy as np
from scipy.optimize import fmin
import math

def f(x):
    exp = (math.pow(x[0], 2) + math.pow(x[1], 2)) * -1
    return math.exp(exp) * math.cos(x[0] * x[1]) * math.sin(x[0] * x[1])
#           func为函数名，x0为函数参数的起始点
print(fmin(func=f,x0=np.array([0,0])))
```

因为默认的是求最小值，所以如果需要求最大值，把正负转换一下即可。

`fmin`不支持有取值范围的函数，如果是有定义域的多元函数，推荐使用`minimize`

## 多变量函数



### 限定定义域区间
```python
import scipy.optimize as opt
import numpy as np

fun = lambda x: 2 * x[0] + (x[0] + 3) ** 2 - 7 * x[1] + (x[1] - 2.5) ** 2
bnds = ((0, 5), (0, 5))  # 定义域
#           fun为函数名，x0为函数参数的起始点，bounds为每个变量的定义域范围
res = opt.minimize(fun=fun, x0=np.array([2, 1]), bounds=bnds)
print(res)
```
需要注意的是，限定区间的求极值，只能用以下方法：`L-BFGS-B`, `TNC`, `SLSQP` `trust-constr`

### 多约束条件最小化
```python
import scipy.optimize as opt

fun = lambda x: (x[0] - 1) ** 2 + (x[1] - 2.5) ** 2
cons = ({'type': 'ineq', 'fun': lambda x: x[0] - 2 * x[1] + 2},
        {'type': 'ineq', 'fun': lambda x: -x[0] - 2 * x[1] + 6},
        {'type': 'ineq', 'fun': lambda x: -x[0] + 2 * x[1] + 2})
bnds = ((0, None), (0, None)) #变量为正
res = opt.minimize(fun, (2, 0), method='SLSQP', bounds=bnds,
                   constraints=cons)
print(res)
```
结果：
```yaml
     fun: 0.8000000011920985
     jac: array([ 0.80000002, -1.59999999])
 message: 'Optimization terminated successfully.'
    nfev: 13
     nit: 3
    njev: 3
  status: 0
 success: True
       x: array([1.4, 1.7])
```
