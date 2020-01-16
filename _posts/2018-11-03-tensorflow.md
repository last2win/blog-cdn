---
layout: post
title: 解决tensorflow报错ValueError Variable conv1/weights already exists, disallowed.
categories: [Python, 行走的问题解决机]
description: ""
keywords: Python, tensorflow
---

此文首发于我的个人博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}


早上在跑别人的tensorflow代码时报错：
```sh
Traceback (most recent call last):

  File "<ipython-input-23-712e8e1f026f>", line 1, in <module>
    runfile('C:/Users/peter/Downloads/tensorflow-mnist-cnn-master/mnist_cnn_train.py', wdir='C:/Users/peter/Downloads/tensorflow-mnist-cnn-master')

  File "C:\Users\peter\AppData\Local\Continuum\anaconda3\lib\site-packages\spyder\utils\site\sitecustomize.py", line 705, in runfile
    execfile(filename, namespace)

  File "C:\Users\peter\AppData\Local\Continuum\anaconda3\lib\site-packages\spyder\utils\site\sitecustomize.py", line 102, in execfile
    exec(compile(f.read(), filename, 'exec'), namespace)

  File "C:/Users/peter/Downloads/tensorflow-mnist-cnn-master/mnist_cnn_train.py", line 167, in <module>
    train()

  File "C:/Users/peter/Downloads/tensorflow-mnist-cnn-master/mnist_cnn_train.py", line 46, in train
    y = cnn_model.CNN(x)

  File "C:\Users\peter\Downloads\tensorflow-mnist-cnn-master\cnn_model.py", line 22, in CNN
    net = slim.conv2d(x, 32, [5, 5], scope='conv1')

  File "C:\Users\peter\AppData\Local\Continuum\anaconda3\lib\site-packages\tensorflow\contrib\framework\python\ops\arg_scope.py", line 183, in func_with_args
    return func(*args, **current_args)

  File "C:\Users\peter\AppData\Local\Continuum\anaconda3\lib\site-packages\tensorflow\contrib\layers\python\layers\layers.py", line 1154, in convolution2d
    conv_dims=2)

  File "C:\Users\peter\AppData\Local\Continuum\anaconda3\lib\site-packages\tensorflow\contrib\framework\python\ops\arg_scope.py", line 183, in func_with_args
    return func(*args, **current_args)

  File "C:\Users\peter\AppData\Local\Continuum\anaconda3\lib\site-packages\tensorflow\contrib\layers\python\layers\layers.py", line 1057, in convolution
    outputs = layer.apply(inputs)

  File "C:\Users\peter\AppData\Local\Continuum\anaconda3\lib\site-packages\tensorflow\python\keras\engine\base_layer.py", line 805, in apply
    return self.__call__(inputs, *args, **kwargs)

  File "C:\Users\peter\AppData\Local\Continuum\anaconda3\lib\site-packages\tensorflow\python\layers\base.py", line 362, in __call__
    outputs = super(Layer, self).__call__(inputs, *args, **kwargs)

  File "C:\Users\peter\AppData\Local\Continuum\anaconda3\lib\site-packages\tensorflow\python\keras\engine\base_layer.py", line 728, in __call__
    self.build(input_shapes)

  File "C:\Users\peter\AppData\Local\Continuum\anaconda3\lib\site-packages\tensorflow\python\keras\layers\convolutional.py", line 161, in build
    dtype=self.dtype)

  File "C:\Users\peter\AppData\Local\Continuum\anaconda3\lib\site-packages\tensorflow\python\layers\base.py", line 276, in add_weight
    getter=vs.get_variable)

  File "C:\Users\peter\AppData\Local\Continuum\anaconda3\lib\site-packages\tensorflow\python\keras\engine\base_layer.py", line 565, in add_weight
    aggregation=aggregation)

  File "C:\Users\peter\AppData\Local\Continuum\anaconda3\lib\site-packages\tensorflow\python\training\checkpointable\base.py", line 535, in _add_variable_with_custom_getter
    **kwargs_for_getter)

  File "C:\Users\peter\AppData\Local\Continuum\anaconda3\lib\site-packages\tensorflow\python\ops\variable_scope.py", line 1467, in get_variable
    aggregation=aggregation)

  File "C:\Users\peter\AppData\Local\Continuum\anaconda3\lib\site-packages\tensorflow\python\ops\variable_scope.py", line 1217, in get_variable
    aggregation=aggregation)

  File "C:\Users\peter\AppData\Local\Continuum\anaconda3\lib\site-packages\tensorflow\python\ops\variable_scope.py", line 510, in get_variable
    return custom_getter(**custom_getter_kwargs)

  File "C:\Users\peter\AppData\Local\Continuum\anaconda3\lib\site-packages\tensorflow\contrib\layers\python\layers\layers.py", line 1744, in layer_variable_getter
    return _model_variable_getter(getter, *args, **kwargs)

  File "C:\Users\peter\AppData\Local\Continuum\anaconda3\lib\site-packages\tensorflow\contrib\layers\python\layers\layers.py", line 1735, in _model_variable_getter
    use_resource=use_resource)

  File "C:\Users\peter\AppData\Local\Continuum\anaconda3\lib\site-packages\tensorflow\contrib\framework\python\ops\arg_scope.py", line 183, in func_with_args
    return func(*args, **current_args)

  File "C:\Users\peter\AppData\Local\Continuum\anaconda3\lib\site-packages\tensorflow\contrib\framework\python\ops\variables.py", line 297, in model_variable
    use_resource=use_resource)

  File "C:\Users\peter\AppData\Local\Continuum\anaconda3\lib\site-packages\tensorflow\contrib\framework\python\ops\arg_scope.py", line 183, in func_with_args
    return func(*args, **current_args)

  File "C:\Users\peter\AppData\Local\Continuum\anaconda3\lib\site-packages\tensorflow\contrib\framework\python\ops\variables.py", line 252, in variable
    use_resource=use_resource)

  File "C:\Users\peter\AppData\Local\Continuum\anaconda3\lib\site-packages\tensorflow\python\ops\variable_scope.py", line 481, in _true_getter
    aggregation=aggregation)

  File "C:\Users\peter\AppData\Local\Continuum\anaconda3\lib\site-packages\tensorflow\python\ops\variable_scope.py", line 848, in _get_single_variable
    name, "".join(traceback.format_list(tb))))

ValueError: Variable conv1/weights already exists, disallowed. Did you mean to set reuse=True or reuse=tf.AUTO_REUSE in VarScope? Originally defined at:

  File "C:\Users\peter\AppData\Local\Continuum\anaconda3\lib\site-packages\tensorflow\python\framework\ops.py", line 1717, in __init__
    self._traceback = tf_stack.extract_stack()
  File "C:\Users\peter\AppData\Local\Continuum\anaconda3\lib\site-packages\tensorflow\python\framework\ops.py", line 3155, in create_op
    op_def=op_def)
  File "C:\Users\peter\AppData\Local\Continuum\anaconda3\lib\site-packages\tensorflow\python\util\deprecation.py", line 454, in new_func
    return func(*args, **kwargs)
```
我发现这是第一次跑，为什么会有模型重加载的问题。

然后我发现这是因为我在Spyder的Python控制台里跑的原因，Python的控制台会保存上次运行结束的变量。
## 解决方法1：重开一个控制台即可
## 解决方法2：在代码的开头加上一句
```python
tf.reset_default_graph()
```


参考资料：
[ValueError: Variable rnn/basic_rnn_cell/kernel already exists, disallowed. Did you mean to set reuse=True or reuse=tf.AUTO_REUSE in VarScope?](https://stackoverflow.com/questions/47296969/valueerror-variable-rnn-basic-rnn-cell-kernel-already-exists-disallowed-did-y)

[Error in notebook: "ValueError: Variable conv1/weights already exists, disallowed. Did you mean to set reuse=True in VarScope"](https://github.com/kratzert/finetune_alexnet_with_tensorflow/issues/8)
