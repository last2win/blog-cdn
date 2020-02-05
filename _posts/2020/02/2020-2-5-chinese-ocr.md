---
layout: post
title: "免费OCR识别图片中的中文字软件：腾讯云，网易见外工作台，百度OCR识别"
categories: [杂类]
description: "免费OCR识别图片中的中文字软件：腾讯云，网易见外工作台，百度OCR识别"
keywords: OCR
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

一直在找可以在安卓手机上直接用的OCR软件，尤其是免费的。

有时候需要识别拍照的图片，或者截图的文章，尤其是中文。

最有名的OCR识别的库是`Tesseract`：[tesseract-ocr/tesseract: Tesseract Open Source OCR Engine (main repository)](https://github.com/tesseract-ocr/tesseract)

但这个库毕竟要自己编译，安装，不如现成的好用。

下面推荐一些免费的工具，按易用性排序

## 腾讯云-文字识别(网页版和微信小程序)

腾讯云提供免费的文字识别，图像分析功能：[腾讯云-文字识别](https://cloud.tencent.com/act/event/ocrdemo#)

网页版支持上传图片进行识别，手机端支持微信小程序`腾讯AI体验中心`，其中文字识别功能包括通用印刷体识别和通用手写体识别，很好用。微信小程序可以免安装安卓APP。



## 网易见外工作台

有人推荐：[网易见外工作台](https://jianwai.netease.com/index/0)

每天可以免费使用2小时，支持视频翻译，语音翻译，语音转写，图片翻译，文档翻译。

上传图片后立刻展示结果：



![图片]({% link /images/2020/2020-2-4-1.png %})


## 百度OCR识别

网上有很多人推荐百度的OCR，说识别准确性很高。


百度AI开发平台提供免费的语音识别API接口：[百度智能云-管理中心](https://console.bce.baidu.com/ai/)

注册后获得API_KEY

百度提供`REST API`和完整的SDK，我打算使用Python的`REST API`。

安装库：
```sh
pip install baidu-aip
```
Python代码如下：
```python
from aip import AipOcr

""" 你的 APPID AK SK """
APP_ID = 'xxx'
API_KEY = 'xxxx'
SECRET_KEY = 'xxxx'

client = AipOcr(APP_ID, API_KEY, SECRET_KEY)


# 读取文件
def get_file_content(filePath):
    with open(filePath, 'rb') as fp:
        return fp.read()


image = get_file_content('ocr.png')

""" 调用通用文字识别, 图片参数为本地图片 """
res = client.basicGeneral(image)
text = ''
words = res['words_result']
for word in words:
    text = text + word['words']+","
print(text)

```
识别效果还可以：

> 利用游戏概率BUG赚钱,福利彩票这种完全骗人,概率只有干万分之一的傻子才买的玩意我是从来不买,毕竟读过书,懂一点概率学,我要,说的就是上面利用套现的这个游戏"骰宝"。这个游戏说虽然开单双概率都是50%,但玩久了发现这个原来是有概率,可依的,我就摸索岀一个对于骰宝这个游戏几乎必嬴的一个方法,也算是概率漏洞,我敢这么说,如果用这个方,法,十分钟之内嬴不到2000我脸伸给你随便打。多了不敢说,每个月几万块是绝对没问题的。,下面我来跟你们说骰宝这个游戏的BUG(这个才是大羊毛,现在也只能用这种方法刷流水了,对押已经行不通了),因为骰宝这个游戏单双概率各50%,如果输了就继续压200元单(或者双),再输就再压400元单(或者双),就这样一直追,因为单双概率各是50%,所以只要嬴一局前面所有输的本金都会嬴回来,而且还赚钱。,你可以观察一下开奖记录,不可能连续出很多次单或者双的,因为单双概率都是50%,所以出几次单肯定就会出几,次双,就像你抛硬币一样,不可能连续100次都是人头,肯定正反各一半。,所以就按照我上面的方法,连续翻倍压单(或者双)肯定会赢,嬴了以后继续从头开始。,继续压100元单(或者双),输了翻倍投注,嬴了从头再开始。,每天只要十来分钟,几千块应该是没问题的,而且手机端投注也很方便,概率统计学可以得岀这种方法嬴的概率是,97.8%,几乎就是稳赚不赔的,而且这种投注方法可以按照投注额0.8%返点,并且这种愔投方法绝对符合博彩网站,的规定,不管你嬴多少博彩网站都必须按照博彩法规定给你提现和返点,不然他们网站就是违规就会被关停。所以,大家放心用这个方法撸。

## 其他小网站

网上还是有一些小网站提供免费的文字识别功能的，这里列举如下：

- [语音转文字 录音转文字 语音识别– 在线语音转换文字软件 - 迅捷PDF转换器在线免费版](https://app.xunjiepdf.com/voice2text/)
- [在线文字识别转换 - 在线工具 - Powered by Yelky](https://ocr.wdku.net/)
- [免费在线文字识别，文字提取，OCR服务－OCRMaker](http://ocrmaker.com/)
- [免费在线OCR - 将PDF转换为Word或图像转换为文本](https://www.onlineocr.net/zh_hans/)
- [诚华OCR - 图片转文字 - 免费在线OCR](https://zhcn.109876543210.com/)

在这里说明一下，小网站的安全性是值得考虑的，毕竟上传的文件识别后可能被用作其他用途。

其他一些软件，如：扫描全能王，要收费就不列举了。

{% raw %}
***          
{% endraw %}

最后，如果你有好的软件或接口等，可以留言告诉我。