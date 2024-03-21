# 文本相似度实战

基于Huggingface 的Bert实现文本相似度匹配

 
## 目录

- [文本相似度实战](#文本相似度实战)
  - [目录](#目录)
    - [上手指南](#上手指南)
          - [开发前的配置要求](#开发前的配置要求)
          - [**安装步骤**](#安装步骤)
    - [文件目录说明](#文件目录说明)
    - [项目运行说明](#项目运行说明)
      - [如何训练](#如何训练)
      - [如何推理](#如何推理)
    - [部署](#部署)
    - [使用到的框架](#使用到的框架)
    - [贡献者](#贡献者)
      - [如何参与开源项目](#如何参与开源项目)
    - [版本控制](#版本控制)
    - [作者](#作者)
    - [版权说明](#版权说明)
    - [鸣谢](#鸣谢)

### 上手指南
数据集：
https://github.com/CLUEbenchmark/SimCLUE

我也保存在文件目录下了，不需要直接下载

###### 开发前的配置要求

有深度学习GPU

###### **安装步骤**

1. 安装torch
2. `pip install transformers`

### 文件目录说明
eg:

```
filetree 
├── train_pair_1w.json  # 数据集
├── dual_model.ipynb  # 代码
├── README.md  # 说明文档
├── /dual_model2/  # 我训练好的checkpoint
├── /metrics/  # 测评函数


```

### 项目运行说明 

#### 如何训练 
1. 首先下载预训练模型
https://huggingface.co/hfl/chinese-macbert-base
一般都下载在本地，因为国内容易被墙。
2. 所有的`.from_pretrained` 后面跟的url都改成你的下载地址。
3. 训练配置,你可以更改`output_dir` 更改输出的checkpoint目录
```
train_args = TrainingArguments(output_dir="./dual_model2",      # 输出文件夹
                               num_train_epochs = 5,            # 迭代次数
                               per_device_train_batch_size=32,  # 训练时的batch_size
                               per_device_eval_batch_size=32,  # 验证时的batch_size
                               logging_steps=10,                # log 打印的频率
                               evaluation_strategy="epoch",     # 评估策略
                               save_strategy="epoch",           # 保存策略
                               save_total_limit=5,              # 最大保存数
                               learning_rate=2e-5,              # 学习率
                               weight_decay=0.01,               # weight_decay
                               metric_for_best_model="f1",      # 设定评估指标
                               load_best_model_at_end=True)     # 训练完成后加载最优模型
```

#### 如何推理
1. 模型用我们训练好的，注意改你的路径
`
model = DualModel.from_pretrained("/home/wangtuo/workspace/Homework/src/text_classfication/dual_model2/checkpoint-112")
`
2. 分词器用现成的
`
tokenizer = AutoTokenizer.from_pretrained("/home/wangtuo/workspace/Homework/Huggingface/models--hfl--chinese-macbert-base")
`
3. 创建pipeline
```
pipe = SentenceSimilarityPipeline(model, tokenizer)
```
4. 输入句子推理
```
pipe("找一部小时候的动画片","求一部小时候的动画片。谢了")
```
他会给文本的相似度

### 部署

暂无

### 使用到的框架

无
### 贡献者

无

#### 如何参与开源项目

贡献使开源社区成为一个学习、激励和创造的绝佳场所。你所作的任何贡献都是**非常感谢**的。


1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request



### 版本控制

该项目使用Git进行版本管理。您可以在repository参看当前可用版本。

### 作者

wangtuo

### 版权说明

该项目签署了MIT 授权许可，详情请参阅 [LICENSE.txt](https://github.com/shaojintian/Best_README_template/blob/master/LICENSE.txt)

### 鸣谢
感谢


