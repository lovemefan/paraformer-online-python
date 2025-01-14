<br/>
<h2 align="center">Paraformer online python</h2>
<br/>

[![versions](https://img.shields.io/badge/python-3.8|3.9|3.10|3.11-blue)](https://github.com/lovemefan/paraformer-online-python)

[English readme](README-EN.md)

Paraformer是达摩院语音团队提出的一种高效的非自回归端到端语音识别框架，多个公开数据集上取得SOTA效果，缺点是该模型没有标点符号。
该项目为Paraformer中文通用语音识别模型，采用工业级数万小时的标注音频进行模型训练，保证了模型的通用识别效果。
模型可以被应用于语音输入法、语音导航、智能会议纪要等场景。


本项目为Paraformer封装，基于onnxruntime的推理包，代码主要来自[Funasr](https://github.com/alibaba-damo-academy/FunASR)官方

另外praformer的接口服务在另一个项目[Paraformer-webserver](https://github.com/lovemefan/Paraformer-webserver)中

***由于github的lfs限制，单个文件最大不能超过100MB，因此将onnx切分成了多个文件上传，但每个模块实际导出只有一个onnx，
读取的模型文件列表会合并二进制文件然后加载onnx模型***

## 目前的进度
* [2023年10月18日] 
  * [x] vad模型流式和非流式onnx推理
  * [x] 标点模型onnx推理
  * [x] 流式asr onnx推理
  * [x] 非流式asr onnx推理
  * [x] 说话人识别 onnx推理
  * [x] 热词功能 (神经网络热词)
  * [x] 时间戳功能 (粗粒度，由vad给出的句子级别的时间戳)
  * [x] 语言模型
  * [x] 支持多进程和多线程，同一线程共享模型参数（单例模式实现）。每个进程初始化需约3G内存（标点、vad、asr、spk加起来）
  * [ ] itn逆文本正则化规则
  * [ ] 纠错模型 （可能有必要）
  * [ ] 说话人日志


## CER 对比
测试时间：2023.8.29 数据来源：https://github.com/SpeechColab/Leaderboard

| 测试集                              | 领域             | paraformer |paraformer热词版(不加热词)|  bilibili | 思必驰 | 阿里  | 百度   | 讯飞  | 微软  | 腾讯  | 依图 |
| :---------------------------------- | ---------------- | ---------- |----- | -------- | ------ | ----- | ------ | ----- | ----- | ----- | ---- |
| 直播带货 李佳琪薇娅 （770条, 0.9H） | 电商、美妆       | 6.28⬇️ | 6.3 | 6.45⬆️    | 10.04⬆️ | 4.33⬇️ | 16.69⬇️ | 9.10⬇️ | 5.29⬇️ | 6.56⬆️ | 7.33 |
| 新闻联播 （5069条, 9H）             | 时政             | 0.6⬆️ | 0.62 | 0.57⬇️    | 0.98⬇️  | 0.32⬇️ | 1.56   | 0.81⬇️ | 0.25⬇️ | 1.02⬇️ | 0.76 |
| 访谈 鲁豫有约 （2993条, 3H）        | 工作、说话       | 3.57⬇️| 3.51 | 2.81⬇️    | 3.3⬆️   | 2.29⬇️ | 5.86   | 3.39⬇️ | 2.74⬇️ | 3.51⬆️ | 2.94 |
| 场馆演讲罗振宇跨年 （1311条, 2.7H） | 社会、人文、商业 | 1.98| 1.83 | 1.57⬇️    | 1.72⬇️  | 1.17⬇️ | 3.23   | 2.18⬆️ | 1.16⬆️ | 1.75⬆️ | 1.49 |
| 在线教育 李永乐 （3148条, 4.4H）    | 科普             | 2.61| 2.50 | 1.44⬇️    | 2.2⬆️   | 1.0⬇️  | 6.90   | 2.03⬇️ | 1.31⬇️ | 1.78⬇️ | 1.81 |
| 播客 创业内幕 （2251条, 4.2H）      | 创业、产品、投资 | 4.72| 3.98 | 3.22⬇️    | 4.24⬇️  | 2.43⬇️ | 7.28⬇️  | 3.82⬇️ | 3.61⬇️ | 3.78⬇️ | 3.7  |
| 线下培训 老罗语录 （884条,1.3H）    | 段子、做人       | 4.64| 4.60 | 3.81⬆️    | 6.46⬆️  | 3.30⬇️ | 14.13⬇️ | 5.66⬇️ | 3.98⬇️ | 5.50⬇️ | 4.76 |
| 直播 王者荣耀 （1561条, 1.6H）      | 游戏             | 6.69⬇️| 7.35 | 5.69⬇️    | 8.14⬆️  | 4.01⬇️ | 10.32⬇️ | 8.31⬆️ | 5.48⬇️ | 6.14⬆️ | 6.92 |
| 电视节目 天下足球 （1683条, 2.7H）  | 足球             | 1.29⬆️| 1.29 | 0.91⬇️    | 1.54⬇️  | 0.61⬇️ | 5.38   | 1.64⬇️ | 0.88⬇️ | 2.68⬇️ | 0.83 |
| 播客故事FM （3466条, 4.5H）         | 人生故事、见闻   | 3.50| 3.59 | 3.22⬇️    | 3.82⬆️  | 2.22⬇️ | 5.62⬇️  | 3.72⬇️ | 3.28⬇️ | 3.65⬇️ | 3.67 |
| 罗翔   法考（1053条, 4H）           | 法律 法考        | 2.02| 1.78 | 1.81⬇️    | 2.86⬇️  | 0.94⬇️ | 5.55   | 2.90⬇️ | 1.19⬇️ | 2.02⬇️ | 1.65 |
| 张雪峰 在线教育考研(1170条, 3.5H)   | 考研 高校报考    | 3.43| 3.30 | 2.05⬇️    | 3.2⬇️   | 1.38⬇️ | 9.34   | 3.15⬇️ | 2.01⬇️ | 2.71⬆️ | 2.61 |
| 谷阿莫 短视频 影剪(1321条, 2.5H)    | 美食、烹饪       | 3.92⬆️| 3.79 | 3.01⬇️    | 4.02⬇️  | 1.94⬇️ | 7.65   | 3.95⬇️ | 4.22⬇️ | 2.94⬇️ | 2.81 |
| 琼斯爱生活 美食&烹饪(856条, 2H)     | 美食、烹饪       | 4.71⬆️| 4.63 | 3.61⬇️    | 6.29⬇️  | 2.53⬇️ | 13.17  | 4.85⬇️ | 3.07⬇️ | 4.56⬇️ | 3.99 |
| 单田芳 评书白眉大侠(1168条, 2.5H)   | 江湖、武侠       | 5.1⬇️| 4.80 | 4.64⬇️    | 9.22⬇️  | 2.5⬇️  | 15.42  | 9.51⬇️ | 5.47⬇️ | 5.89⬆️ | 5.45 |

## 测试

>结论： Transformer语言模型会拖慢整个链路的实时率,性能提升有限，性价比不高。
> 神经网络热词模型在实时率影响较小的情况下，在某一领域的优化效果性价比高。

```
测试环境

CPU： Intel(R) Xeon(R) CPU E5-2670
内存： 128G

onnxruntime 使用4线程解码，python使用单线程解码，
实时率每次测试略有不同，没办法保证机器空载运行，会有一定的误差，仅供参考
```




| 测试集 \ (CER/RTF)                  | 领域             | paraformer | paraformer热词版(不加热词) | paraformer+Transformer语言模型 |
| :---------------------------------- | ---------------- | ---------- | -------------------------- | ------------------------------ |
| 直播带货 李佳琪薇娅 （770条, 0.9H） | 电商、美妆       | 6.28       | 6.3/0.044                  | 6.0/0.37                       |
| 新闻联播 （5069条, 9H）             | 时政             | 0.6        | 0.62/0.041                 | 0.60/0.32                      |
| 访谈 鲁豫有约 （2993条, 3H）        | 工作、说话       | 3.57       | 3.51/0.047                 | 3.4/0.33                       |
| 场馆演讲罗振宇跨年 （1311条, 2.7H） | 社会、人文、商业 | 1.98       | 1.83/0.041                 | 1.7/0.34                       |
| 在线教育 李永乐 （3148条, 4.4H）    | 科普             | 2.61       | 2.50/0.043                 | 2.2/0.35                       |
| 播客 创业内幕 （2251条, 4.2H）      | 创业、产品、投资 | 4.72       | 3.98/0.042                 | 3.6/0.34                       |
| 线下培训 老罗语录 （884条,1.3H）    | 段子、做人       | 4.64       | 4.60/0.043                 | 4.5/0.34                       |
| 直播 王者荣耀 （1561条, 1.6H）      | 游戏             | 6.69       | 7.35/0.045                 | 7.1/0.36                       |
| 电视节目 天下足球 （1683条, 2.7H）  | 足球             | 1.29       | 1.29/0.042                 | 1.30/0.37                      |

结论： 语言模型在某些数据集上效果不明显，不如针对某一数据集加入热词。

### 特定数据集热词优化

数据集选择字错率最高的 `直播带货李佳琪薇娅`

#### 数据集分析

* 数据集中其实有少量标注错误，有些语气词错误，但是影响不大，例如 （诶，哎）（哟，哦）（咯，喽），（哇哦，wow）

* 数据集但也有漏标，模凌两可的情况，例如 （这个，这），部分儿化音
* paraformer会将 `哇哦`识别成`wow`

| 模式                                         |                           热词列表                           | CER  | RTF   |
| -------------------------------------------- | :----------------------------------------------------------: | ---- | ----- |
| Paraformer-large-quant                       |                              -                               | 6.0  |       |
| Paraformer-large-contextual + 不加热词       |                              -                               | 6.3  | 0.044 |
| Paraformer-large-contextual + 加热词         | 诶 浆果 一支 两支 三支 天呐 美宝莲眼唇卸 诗香气质 半唇 口红 当妮留香珠 玫紫 珂莱欧 薇娅 李佳琦 | 6.0  | 0.05  |
| Praformer-large-contextual + 语言模型        |                              -                               | 6.0  | 0.37  |
| Praformer-large-contextual +加热词+ 语言模型 | 诶 浆果 一支 两支 三支 天呐 美宝莲眼唇卸 诗香气质 半唇 口红 当妮留香珠 玫紫 珂莱欧 薇娅 李佳琦 | 6.06 | 0.36  |



### 语言模型Beam size测试

语言模型的权重为0.15

测试集： `直播带货李佳琪薇娅`



|               策略                |  CER  | RTF  |
|:-------------------------------:|:-----:|:-----:|
|          greedy search          | 6.28  | 0.048 |
| lm + beam serach  (beam size=1) | 6.33  | 0.12  |
| lm + beam search (beam size=2)  | 6.32  | 0.18  |
| lm + beam search (beam size=3)  | 6.32  | 0.24  |
| lm + beam search (beam size=4)  | 6.33  | 0.30  |
| lm + beam search (beam size=5)  | 6.32  | 0.37  |
| lm + beam search (beam size=10) | 6.32  | 0.65  |



## 快速使用

请参考test文件下的测试脚本，脚本运结果可以参考这个[github action](https://github.com/lovemefan/paraformer-online-python/actions)

| 测试脚本                         | 功能                                                                     |
|------------------------------|------------------------------------------------------------------------|
| test_asr_all_in_one          | 整合online、offline、标点和说话人识别的功能，时间戳，目前依赖vad切割，会出现一句话多个说话人的情况|
| test_paraformer_offline.py   | 一句话识别，支持热词，有标点                                                         |
| test_paraformer_online.py    | 流失识别，无标点                                                               |
| test_speaker_verification.py | 说话人识别，自动注册，返回说话人id                                                     |
| test_vad_offline.py          | vad 离线版                                                                |
| test_vad_online.py           | vad 在线版                                                                |
| test_punctuator.py           | 流式标点和非流式标点， 标点模型在v0.0.3中更换了一个将近1G的模型，加载速度有影响，推理速度有少许影响，需要原版模型可以下载之前版本，或者使用funasr转换 |

```bash
git clone https://github.com/lovemefan/paraformer-online-python.git
cd paraformer-online-python && pip install .
python test/test_asr_all_in_one.py
```
