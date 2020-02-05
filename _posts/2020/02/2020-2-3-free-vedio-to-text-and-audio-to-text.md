---
layout: post
title: "免费视频转文字-音频转文字软件：网易见外工作台, Speechnotes, autosub, Speech to Text, 百度语音识别"
categories: [杂类]
description: "免费视频转文字-音频转文字软件：网易见外工作台, Speechnotes, autosub, Speech to Text, 百度语音识别"
keywords: 
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

应该在暑期的时候就有这个想法，想把录音转文字，语音转文字甚至是视频转文字。

因为有些时候有大段的音频内容，但我只需要其中的几分钟，从头开始播放感觉太浪费时间了。

网上有很多收费的解决方案软件，比如讯飞就做的不错，安卓端的APP用起来也很方便，就是贵了点。

然后我看到了这个知乎问题：[有能把录音变成文字的软件么？ - 知乎](https://www.zhihu.com/question/20124290)

下面按识别准确率和易用性来排序各个软件。

## 网易见外工作台(推荐)

有人推荐：[网易见外工作台](https://jianwai.netease.com/index/0)

每天可以免费使用2小时，支持视频翻译，语音翻译，语音转写，图片翻译，文档翻译。

上传录音后等待几分钟，识别结果出来。

中文的识别准确率不错：

> 反正我虽然说我我的转变过程吧，我当时这样的就是因为我在找工作。我在研一研二下个月上半学期的时候要找工作，所以我要去频繁的刷这个算法。因为我那时候发现我要找的是外企吗？外企的那个侧重角度更偏向于算法数据结构，还有一些的系统设计。在当时我的那个项目经历还不是很丰富的时候，你要想给面试官一个眼前一亮或者是印象深刻的一个想法的话，其实算法是一个比较性价比高点的事情。


优点：网页版直接使用，无需编程基础，识别准确率非常高

缺点：每天限制使用2小时

## Chrome插件 Speechnotes

有人推荐Chrome插件：[Speechnotes 听写记事本 - Chrome 网上应用店](https://chrome.google.com/webstore/detail/speechnotes-speech-to-tex/opekipbefdbacebgkjjdgoiofdbhocok?utm_source=chrome-ntp-icon)

不用注册，在线实时声音转文字。

试了一下，对中文的实时识别效果不错：

> 要找工作,所以我要去平凡的策略算法.因为我那时候发现我要找的是外企吗,买气的那个特种角度更偏向于算法数据结构一些的系统设计.在当时我的那个项目经历还不是很丰富的时候,你要想给面试官一个眼前一亮或者印象深刻的一个想法的话,其实算法是一个比较轻价比高的一个事情。


优点：直接使用，支持实时语音，对会议记录的帮助很大，准确性高

缺点可以忽略不计

## autosub

有人推荐这个库：[ [NO LONGER MAINTAINED] Command-line utility for auto-generating subtitles for any video file](https://github.com/agermanidis/autosub)。但这个库的原作者一年前就不维护这个库了，现在由一个国人维护：[ Command-line utility to transcribe/translate from video/audio/subtitles to subtitles](https://github.com/BingLingGroup/autosub)

这个Python库做的是通过`Google Web Speech API`把`video or an audio`转换成SRT字幕或者json格式的文本。

安装`ffmpeg`和`autosub`：
```sh
apt install ffmpeg python python-pip git -y
pip install git+https://github.com/BingLingGroup/autosub.git@alpha ffmpeg-normalize
```

使用示例：
```sh
->% autosub -i 54.mp3 -S zh
........
Converting speech regions to short-term fragments.
Converting: 100% |#############################################################################################################################| Time:  0:02:37

Sending short-term fragments to Google Speech V2 API and getting result.
Speech-to-Text: 100% |#########################################################################################################################| Time:  0:02:22
Speech language subtitles file created at "42.zh.srt".

All works done.
```

结果如下：

> 我当时这样的就是因为我在找工作我在严依然而下班上面学期的首要找工作所以,作者我要去平凡的穿着算法,因为我那时候发现我要找的是外企吗,白起的那个特征角度,更偏向于算法数据结构而一切的系统设计,在当时我的那个项目经历还不是很丰富的时候呢,你要想给面试官一个眼前一亮或者印象深刻的一个想法的话其实算法是一个逼,是一个比较现在的高低的事情。


准确性稍差

优点：不限时转换

缺点：需要谷歌，而且准确性没前2个好


## 百度语音识别API

百度AI开发平台提供免费的语音识别API接口：[百度智能云-管理中心](https://console.bce.baidu.com/ai/)

注册后获得API_KEY

百度提供`REST API`和完整的SDK，其中`REST API` 仅支持整段语音识别的模式，即单段语音音频时长不超过60s；完整的SDK识别不限时长。

本来想用linux-C++-SDK，下载：[百度AI开放平台-全球领先的人工智能服务平台-百度AI开放平台](https://ai.baidu.com/sdk#asr)

但这个SDK只支持g++4.8和x64，无语了。

随后我打算使用Python的`REST API`，支持pcm, wav, m4a格式音频，而且每段限时1分钟，转换格式：

ffmpy 安装教程参考：[anaconda 下安装ffmpeg](https://zhang0peter.com/2019/03/15/ffmpeg/)

安装库：
```sh
pip install baidu-aip
```
Python代码如下：
```python
from aip import AipSpeech
import ffmpy
import os

""" 你的 APPID AK SK """
APP_ID = '10540827'
API_KEY = 'xxxx'
SECRET_KEY = 'xxxx'

client = AipSpeech(APP_ID, API_KEY, SECRET_KEY)


# 读取文件
def get_file_content(filePath):
    with open(filePath, 'rb') as fp:
        return fp.read()


FILENAME = "42.mp3"
# mp3转换为wav
ff = ffmpy.FFmpeg(
    inputs={FILENAME: None},
    global_options=['-y'],
    outputs={'output.wav': '-ar 16000'})
ff.run()
# 把wav文件切割为小块
ff_split = ffmpy.FFmpeg(
    inputs={'output.wav': None},
    global_options=['-y'],
    outputs={'temp_wav%d.wav': '-f segment -segment_time 30 -c copy'})#每段30秒
ff_split.run()

text = ''

files = [f for f in os.listdir('.') if os.path.isfile(f) and 'temp_wav' in f]
for filename in files:
    # 识别本地文件
    result = client.asr(get_file_content(filename), 'wav', 16000, {
        'dev_pid': 1536,
    })
    if 'error_msg' in result or result['err_no'] != 0:
        print(result)
    else:
        print(result['result'][0])
        text = text + result['result'][0]
    os.remove(filename)

with open('result.txt', 'w') as f:
    f.write(text)
```

我进行测试的时候，返回结果不正常，有色情内容，可能有黑客侵入了百度的系统，我就不做评判了。






## IBM的Speech to Text(不推荐)

另外一个免费的工具是IBM推出的Speech to Text 工具，也是免费的：[Watson Speech to Text - 概述 - 中国  IBM](https://www.ibm.com/cn-zh/cloud/watson-speech-to-text)

注册账户后打开页面[Speech to Text - IBM Cloud](https://cloud.ibm.com/catalog/services/speech-to-text)

选择`Lite`套餐，每个月有500分钟免费的音频转文字套餐，缺点是不能转录100M以上大小的音频(异步调用可以转录最大1G的音频)。

推荐使用Python操作api，直接使用curl返回的结果是json，不利于进一步操作。

安装库：`pip install --upgrade ibm-watson`

代码如下：
```python
from ibm_watson import SpeechToTextV1
from ibm_cloud_sdk_core.authenticators import IAMAuthenticator

authenticator = IAMAuthenticator('API')
speech_to_text = SpeechToTextV1(
    authenticator=authenticator
)
speech_to_text.set_service_url(
    'https://api.us-south.speech-to-text.watson.cloud.ibm.com/instances/xxxxxxxx')

f = open("42.mp3", "rb")
result = speech_to_text.recognize(audio=f, content_type='audio/mp3', model='zh-CN_NarrowbandModel')
l = result.result['results']
text = ''
for i in l:
    text = text + i['alternatives'][0]['transcript'] + ','

fo = open("audio-text.txt", "w")
fo.write(text)
fo.close()
```

对中文的识别效果并不好：

> 反正 我 涮 肉 我 我的 转 亮果厂 吧 ,我 当时 这样的 就是 因为 我的 找工作 我 再 延期 你 而 下的 上面 学习 的 首要 找工作 索要 去 平方米 川 流 ,你 没有 那时候 发现 我要 找 的 是 外企 吗 ,那个 特种 较多 ,更 偏向 与 结构 ,写 的 系统 设计 ,在 当时 我的 那个 项目 经理 还不是 很 丰富 的 时候 ,你 要想 给 面食 馆 一个 眼前 一 亮 或者 印象 深刻 的一个 想法 的话 其实 上海 是一个 比较 性价比 高的 的 事情


IBM和谷歌的接口效果不好的原因是，他们把音频切为一段一段进行识别，并不会根据上下文来调整文字，准确性自然低。

如果不加@符号会报错：
```json
{
   "code_description": "Bad Request", 
   "code": 400, 
   "error": "Stream was 15 bytes but needs to be at least 100 bytes."
}                
```
如果文件大小超过100M，会报错：
```sh
<HTML><HEAD>
<TITLE>Internal Server Error</TITLE>
</HEAD><BODY>
<H1>Internal Server Error - Write</H1>
The server encountered an internal error or misconfiguration and was unable to
complete your request.<P>
Reference&#32;&#35;4&#46;c8142017&#46;1580636884&#46;75fa462e
</BODY></HTML>
```

