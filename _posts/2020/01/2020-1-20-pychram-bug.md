---
layout: post
title: "Python-PyCharm 报错解决：ImportError: cannot import name 'InteractiveConsole' from 'code'"
categories: [行走的问题解决机]
description: "Python-PyCharm 报错解决：ImportError: cannot import name 'InteractiveConsole' from 'code'-重命名解决问题"
keywords: Python, PyCharm
---

此文首发于我的个人博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

早上在用PyCharm跑Python代码时遇到报错：
```sh
Traceback (most recent call last):
  File "C:\Users\peter\AppData\Local\JetBrains\Toolbox\apps\PyCharm-P\ch-0\193.5662.61\plugins\python\helpers\pydev\_pydevd_bundle\pydevd_console_integration.py", line 4, in <module>
    from code import InteractiveConsole
ImportError: cannot import name 'InteractiveConsole' from 'code' (C:\Users\peter\Documents\GitHub\code.py)

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "C:\Users\peter\AppData\Local\JetBrains\Toolbox\apps\PyCharm-P\ch-0\193.5662.61\plugins\python\helpers\pydev\pydevd.py", line 37, in <module>
    from _pydevd_bundle.pydevd_comm import CMD_SET_BREAK, CMD_SET_NEXT_STATEMENT, CMD_STEP_INTO, CMD_STEP_OVER, \
  File "C:\Users\peter\AppData\Local\JetBrains\Toolbox\apps\PyCharm-P\ch-0\193.5662.61\plugins\python\helpers\pydev\_pydevd_bundle\pydevd_comm.py", line 91, in <module>
    from _pydevd_bundle import pydevd_console_integration
  File "C:\Users\peter\AppData\Local\JetBrains\Toolbox\apps\PyCharm-P\ch-0\193.5662.61\plugins\python\helpers\pydev\_pydevd_bundle\pydevd_console_integration.py", line 6, in <module>
    from _pydevd_bundle.pydevconsole_code_for_ironpython import InteractiveConsole
  File "C:\Users\peter\AppData\Local\JetBrains\Toolbox\apps\PyCharm-P\ch-0\193.5662.61\plugins\python\helpers\pydev\_pydevd_bundle\pydevconsole_code_for_ironpython.py", line 305
    exec code in self.locals
            ^
SyntaxError: Missing parentheses in call to 'exec'

Process finished with exit code 1
```
看到报错`ImportError: cannot import name 'InteractiveConsole' from 'code' `

我一下子就知道哪里出了问题，这是因为我的代码文件名就叫`code.py`与PyCharm中用来处理输出的代码冲突了。

解决方法是把文件重命名就可以了。