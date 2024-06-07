# 数据构建

## 1  从规程规章制度等文件中生成InternLM2模型*Xtuner*训练数据格式

1.1  输入如下命令，会在data目录下生成CoalQA_data.json

```shell
python  Generate_QAdata.py  ../data/CoalQA_data.json
```

CoalQA_data.json内容结构形如：

```shell
[
  {
    "input": "请提供新建矿井立井井筒冻结法施工的具体要求。",
    "output": "新建矿井立井井筒冻结法施工需要遵守以下要求：冻结深度必须穿过风化带延深至稳定的基岩10m以上，第一个冻结孔必须全孔取芯，钻孔时必须测定钻孔的方向和偏斜度，偏斜度超过规定时必须及时纠正，冻结管必须采用无缝钢管并焊接或螺纹连接，开始冻结后必须经常观察水文观测孔的水位变化，并在确定冻结壁已交圈后才能进行试挖。"
  },
  ...
  
]
```

1.2  接上面生成json，处理成Xtuner可训练数据格式，运行下面命令即可

```shell
python format_internlm2.py  
```

```shell
[
    {
        "conversation": [
            {
                "system": "你是一个煤矿安全领域的知识达人，你对相关煤矿安全规章规程制度、技术等文档非常熟悉。请你专业正确地解答用户想问的煤矿安全相关问题。",
                "input": "请提供新建矿井立井井筒冻结法施工的具体要求。",
                "output": "新建矿井立井井筒冻结法施工需要遵守以下要求：冻结深度必须穿过风化带延深至稳定的基岩10m以上，第一个冻结孔必须全孔取芯，钻孔时必须测定钻孔的方向和偏斜度，偏斜度超过规定时必须及时纠正，冻结管必须采用无缝钢管并焊接或螺纹连接，开始冻结后必须经常观察水文观测孔的水位变化，并在确定冻结壁已交圈后才能进行试挖。"
            }
        ]
    },
    {
        "conversation": [
            {
                "system": "你是一个煤矿安全领域的知识达人，你对相关煤矿安全规章规程制度、技术等文档非常熟悉。请你专业正确地解答用户想问的煤矿安全相关问题。",
                "input": "煤矿安全规程中，对于新建矿井的立井井筒冻结法施工有哪些具体要求？",
                "output": "新建矿井立井井筒冻结法施工需要遵守以下要求：冻结深度必须穿过风化带延深至稳定的基岩10m以上，第一个冻结孔必须全孔取芯，钻孔时必须测定钻孔的方向和偏斜度，偏斜度超过规定时必须及时纠正，冻结管必须采用无缝钢管并焊接或螺纹连接，开始冻结后必须经常观察水文观测孔的水位变化，并在确定冻结壁已交圈后才能进行试挖。"
            }
        ]
    },
...
]
```

## 2  从整理好的题库生成InternLM2模型*Xtuner*训练数据格式

输入如下命令即可

```shell
python Generate_Question_bank.py
```

这里展示多选题生成

```shell
[
    {
        "conversation": [
            {
                "system": "你是一个煤矿安全领域的知识达人，你会解答很多题。用户给出一个多选题，你能从几个选项中，选择出多个正确选项。",
                "input": "掘进巷道在下列哪些情况下不能爆破()。\nA、掘进工作面或炮眼有突水预兆时\nB、探水孔超前距不够时\nC、空顶距超过规定时\nD、掘进工作面支架不牢固时",
                "output": "ABCD"
            }
        ]
    },
    ...
]
```

若想生成其他题目类型训练数据，请在相应位置替换成需要的内容
```shell
csv_filename = '../data/多选题.csv'
# csv_filename = '../data/单选题.csv'
# csv_filename = '../data/判断题.csv'
# csv_filename = '../data/填空题.csv'
# csv_filename = '../data/简答题.csv'

json_filename = '../data/multiple_choice.json'
# json_filename = '../data/single_choice.json'
# json_filename = '../data/true_or_false.json'
# json_filename = '../data/fill_in.json'
# json_filename = '../data/shot_answer.json'

 "system": "你是一个煤矿安全领域的知识达人，你会解答很多题。用户给出一个多选题，你能从几个选项中，选择出多个正确选项。"
# "system": "你是一个煤矿安全领域的知识达人，你会解答很多题。用户给出一个单选题，你能从几个选项中，选择出一个正确选项。"
# "system": "你是一个煤矿安全领域的知识达人，你会解答很多题。用户给出一个判断题，然后你作出判断，是正确还是错误。"
# "system": "你是一个煤矿安全领域的知识达人，你会解答很多题。用户给出一个填空题，然后你在题目中括号中，填写合适的答案"
#  "system": "你是一个煤矿安全领域的知识达人，你对相关煤矿安全规章规程制度、技术等文档非常熟悉。请你专业正确地解答用户想问的煤矿安全相关问题。"
```
## 3  煤矿事故多轮对话生成

```shell
"""
""{accident}""
通过上面提供的事故调查事故报告，请你帮我生成多轮对话文本，格式为[
    {
        "conversation": [
            {
                "system": "你是一名煤矿安全领域的知识达人，提供有关煤矿安全规程、事故预防措施和应急响应的知识。",
                "input": "你好！",
                "output": "您好，我是一名煤矿安全领域的知识达人，请问有什么可以帮助您？"
            },
            {
                "input": " ",
                "output": " "
            }，...
        ]
    }
]
"""
```
使用GLM-4模型，构建煤矿事故知识图谱。暂时不开源

##  补充细节
- 1.合并两个json文件的脚本：merge_json.py

- 2.格式化json文本的脚本：format_json.py

- 3.打乱json中数据顺序的脚本：shuffle.py
