---
layout: post
title: "Python"
categories: [行走的问题解决机]
description: ""
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

下午在用Python的requests库的代理时遇到了问题：
```python
import requests
proxies = {'http': 'socks5://127.0.0.1:10809', 'https': 'socks5://127.0.0.1:10809'}
response = requests.get(url, headers=header, proxies=proxies)
```
报错如下：
```js
Traceback (most recent call last):
  File "C:\Users\peter\Anaconda3\lib\site-packages\urllib3\connectionpool.py", line 600, in urlopen
    chunked=chunked)
  File "C:\Users\peter\Anaconda3\lib\site-packages\urllib3\connectionpool.py", line 343, in _make_request
    self._validate_conn(conn)
  File "C:\Users\peter\Anaconda3\lib\site-packages\urllib3\connectionpool.py", line 839, in _validate_conn
    conn.connect()
  File "C:\Users\peter\Anaconda3\lib\site-packages\urllib3\connection.py", line 364, in connect
    _match_hostname(cert, self.assert_hostname or server_hostname)
  File "C:\Users\peter\Anaconda3\lib\site-packages\urllib3\connection.py", line 374, in _match_hostname
    match_hostname(cert, asserted_hostname)
  File "C:\Users\peter\Anaconda3\lib\ssl.py", line 334, in match_hostname
    % (hostname, ', '.join(map(repr, dnsnames))))
ssl.SSLCertVerificationError: ("hostname 'www.okex.com' doesn't match either of '*.facebook.com', '*.facebook.net', '*.fb.com', '*.fbcdn.net', '*.fbsbx.com', '*.messenger.com', 'facebook.com', 'fb.com', 'messenger.com', '*.m.facebook.com', '*.xx.fbcdn.net', '*.xy.fbcdn.net', '*.xz.fbcdn.net'",)
Certificate did not match expected hostname: www.okex.com. Certificate: {'subject': ((('commonName', '*.facebook.com'),),), 'subjectAltName': [('DNS', '*.facebook.com'), ('DNS', '*.facebook.net'), ('DNS', '*.fb.com'), ('DNS', '*.fbcdn.net'), ('DNS', '*.fbsbx.com'), ('DNS', '*.messenger.com'), ('DNS', 'facebook.com'), ('DNS', 'fb.com'), ('DNS', 'messenger.com'), ('DNS', '*.m.facebook.com'), ('DNS', '*.xx.fbcdn.net'), ('DNS', '*.xy.fbcdn.net'), ('DNS', '*.xz.fbcdn.net')]}
```

