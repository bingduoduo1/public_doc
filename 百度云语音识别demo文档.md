# 百度云语音识别demo文档（在线语音识别 ）

## 相关文档

[文档](https://ai.baidu.com/docs/#/ASR-Android-SDK/top)：主要涉及android的demo中接口介绍。

## 所有功能

![Markdown](http://i1.bvimg.com/681741/cb0c22025742dee6.png)

其中在线识别功能为：录音后上传录音文件，解析返回的识别结果。

## 在线识别流程

- 初始化：初始化EventManager类，注册自己的输出事件类。
- 开始识别：设置识别参数，发送start开始事件。
- 回调事件：先生成prams参数序列，再进行回调事件，最后生成data识别结果。
- 控制识别：控制停止识别/取消识别，向SDK发送停止事件。
- 事件管理器退出：释放资源。

## 相关信息

+ 识别参数以json格式

+ 识别结果以bytes形式的data处理为string后返回

+ 默认的音频格式为wav

