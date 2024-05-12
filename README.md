# Chinese-Parataxis-Graph-Parsing 2024
# 中文意合图语义解析评测任务
## 组织者和联系人
组织者：荀恩东（北京语言大学）；饶高琦（北京语言大学）；唐共波（北京语言大学）

联系人及联系方式：郭梦溪（guo_mengxi@foxmail.com，北京语言大学硕士生）；李梦（北京语言大学博士生）


##
+ [1.评测内容](#_link_1st)
+ [2.评测数据](#_link_2st)
+ [3.评价标准与模态](#_link_3st)
+ [4.Baseline](#_link_6st)
+ [5.奖项设置](#_link_4st)
+ [6.赛程安排](#_link_5st)

## <span id = '_link_1st'>1.评测内容 </span>
### (1) 意合图介绍

意合图是以事件为中心的语义表征图，为单根有向图，图中的节点对应承载事件、实体、属性的单元，边为有向边，表示单元间的语义关系。意合图力求能够对句子、段落、篇章等不同层级的语言单元作一贯式表示。

![图1：意合图抽象表示](https://github.com/gertrude95/Chinese-Parataxis-Graph-Parsing/assets/136043716/b76120a7-998a-48df-89e1-9df4b1cddab7)

<p align="center">图1：意合图抽象表示</p>

意合图在符合人类对语言认知的基础上，充分考虑落地应用的可操作性，使其尽可能地层次化，以便于后续语义分析路径的设计，实现通用性与扩展性兼具的语义表征方案。按照层次可以将意合图层层分解为多个子部分。意合图由事件结构与实体结构两部分构成：

事件结构分为事件内结构与事件外结构，事件内结构可进一步分为以事件词为核心的论元结构、情态结构、时空结构，事件外结构为多个事件构成的关系事件结构；

实体结构分为实体内结构与实体外结构，实体内结构即实体属性与属性值结构，实体外结构即多个实体构成的实体关系事件结构。

![图2：事件结构与实体结构](https://github.com/gertrude95/Chinese-Parataxis-Graph-Parsing/assets/136043716/0b405fb7-2f30-48c2-b352-f3035d980758)
<p align="center">图2：事件结构与实体结构</p>

意合图脱离表层句法形式，表示深层语义信息。事件可出现在任何句法位置，事件结构与句法结构不存在绝对的对应关系（示例见图3）。意合图节点与句内词级单元并非一一对应，对于不提供实际事件语义表达的单元（如轻动词、各种标记词等）（示例见图4），在意合图中不进行表示；并且意合图允许新增单元以补全非共享省略成分与隐式表达（示例见图5）。然而，我们也承认语法对语义存在一定程度的提示性，因此在标注过程中我们保留了部分形式标记。

![图3：“他们一起搭上了那艘即将远航的帆船”意合图抽象表示](https://github.com/gertrude95/Chinese-Parataxis-Graph-Parsing/assets/136043716/edd982f2-8953-4466-97d5-db6c7ebf2d95)
<p align="center">图3：“他们一起搭上了那艘即将远航的帆船”意合图抽象表示</p>

![图4：“这个问题我搞懂了”意合图抽象表示](https://github.com/gertrude95/Chinese-Parataxis-Graph-Parsing/assets/136043716/f3044b3d-e9a9-4b92-a943-dd9ec54cd6b4)
<p align="center">图4：“这个问题我搞懂了”意合图抽象表示</p>

![图5：“与其去外面吃饭，不如自己做好吃的”意合图抽象表示](https://github.com/gertrude95/Chinese-Parataxis-Graph-Parsing/assets/136043716/ef9e860d-57b4-4ee7-9629-1e49947a1f9d)
<p align="center">图5：“与其去外面吃饭，不如自己做好吃的”意合图抽象表示</p>

  **2024中文意合图语义解析评测任务仅要求生成句子级意合图框架即可，即输入单元为句子，输出为意合图框架结构，无需生成细化实体结构、情态结构、时空结构等的内部语义分类，仅判断是否属于该结构成分即可，所提供的语料也为粗粒度标签。 此外，本次评测任务参赛人员可自行借助形式标记辅助个别语义的识别，但最终不要求对形式标记进行识别。**

### (2)意合图标签体系

意合图标签层级：
| 层级     | 包含的语义标签类                                             |
| -------- | ------------------------------------------------------------ |
| 论元结构 | A0, A1, A2, 工具, 材料, 方式, 依据, 原因, 目的, 范围, 数量, 数量源点, 数量终点, 状态, 状态源点, 状态终点 |
| 情态结构 | Mod（此次评测不细分情态）                                    |
| 时空信息 | 时间, 时间源点, 时间终点,Time（此次评测时态时制等时间信息不细分） 处所, 处所源点, 处所终点, 趋向 |
| 实体属性 | EntityRel（此次评测不细分实体属性，且领属关系与整分关系也暂与实体属性作统一标注） |
| 关系事件 | 时序关系、递进关系、转折关系、因果关系、条件关系、目的关系、And、Or、Ref（即同指关系） |
| 特殊标签 | Comp，NER，插入语，Merge，离合，重叠，QS，Is             |
| 形式标记 | PN，CompPN，Conj，FW，TM，PF，SF, X |

<!-- file1.md -->
[详细内容请参考《体系及标签定义》](体系及标签定义.md)

## <span id = '_link_2st'>2.评测数据 </span>
### (1)数据样例
本次评测任务提供的数据为json格式，包括句子的分词信息和意合图表征信息，例如：
```
{"sent":["我","对不起","大家","，","我","没有","完成","任务","。\r"],

"relData":[

{"word1":{"word":"我","idx":0},"word2":{"word":"对不起","idx":1},"relVal":"A0"},

{"word1":{"word":"对不起","idx":1},"word2":{"word":"ROOT","idx":-1},"relVal":"CoreWord"},

{"word1":{"word":"对不起","idx":1},"word2":{"word":"因果关系","idx":-8},"relVal":"结果事件"},

{"word1":{"word":"大家","idx":2},"word2":{"word":"对不起","idx":1},"relVal":"A1"},

{"word1":{"word":"我","idx":4},"word2":{"word":"完成","idx":6},"relVal":"A0"},

{"word1":{"word":"没有","idx":5},"word2":{"word":"完成","idx":6},"relVal":"m否定"},

{"word1":{"word":"完成","idx":6},"word2":{"word":"ROOT","idx":-1},"relVal":"CoreWord"},

{"word1":{"word":"完成","idx":6},"word2":{"word":"因果关系","idx":-8},"relVal":"原因事件"},

{"word1":{"word":"任务","idx":7},"word2":{"word":"完成","idx":6},"relVal":"A1"},]}
```
其中：“sent“值域是句子的分词结果；”relData“值域是句子意合图的表征信息，每一个元素对应一个三元组；“word1”、“word2”、“relVal”；”word1“和”word2“值域均包含节点内容word和节点编号idx，当word为句中词汇时，idx为该词在句中的编号（从0开始），当word为隐式事件词时，idx的对应关系如下：
| ROOT     | -1   |
| -------- | ---- |
| And      | -2   |
| Or       | -3   |
| Ref      | -4   |
| 时序关系 | -5   |
| 递进关系 | -6   |
| 转折关系 | -7   |
| 因果关系 | -8   |
| 条件关系 | -9   |
| 目的关系 | -10  |
| 重叠     | -11  |
| Is       | -12  |
| QS       | -13  |
> ***需要注意的是，当句中存在不只一种某一隐式事件词时，word对应的内容将在字符串后面增加数字，并向上叠加，idx也将在-13的基础上减一。例如，句中存在三个因果关系，则其对应的word分别为：因果关系、因果关系1、因果关系2，idx分别为：-8、-14、-15。**
> E.g. 因为 他 生病 了 没 来 学校 ， 所以 由 你 转告 他 。
> (生病\_2，***因果关系_-8***，原因事件)、(来_5，***因果关系\_-8***，结果事件)、
> (因果关系\_-8， ***因果关系1_-14***，原因事件)、(转告\_11，***因果关系1_-14***，结果事件)
> 【因果关系_-8代表的是”他生病了没来学校“这个事件，因此我们称这些关系词为隐式事件词，它所代表的是一种关系事件整体。】
> 
**【提示：上述操作只是为了区分同一句子中所含有的多个同名的关系事件，参赛者可根据训练方式进行数据格式上的合理改动】**
### (2)任务说明
输入：分词后的句子
输出：与提供的数据格式一致的json文件

**注意：输入为分词后的句子，以我们所提供的分词为准，参赛选手无需做分词处理。输出按照我们的数据格式，需输出与句中词相对应的索引编号，索引错误也会影响结果判定。**

### (3)数据集
本测试任务的原始语料选自对外汉语阅读材料和宾州中文树库，分为训练集、验证集、测试集和盲测集。其中，盲测集不提供标注结果，只提供分词后的句子。提供的各项数据分布如下【暂定】：


| 数据集 | 句子数 |
| ------ | ------ |
| 训练集 | 2000   |
| 验证集 | 1000   |
| 测试集 | 1000   |
| 盲测集 | 1000   |


## <span id = '_link_3st'>3.评价标准与模态 </span>
### (1)评价标准
本任务采用F1值作为模型表现的评价标准，其计算方式如下：
<img width="890" alt="image" src="https://github.com/gertrude95/Chinese-Parataxis-Graph-Parsing/assets/136043716/1796abee-c2bf-4cf9-ab06-0df50d20830a">

***Generated Tuples***：模型预测的三元组集合数

***Gold Tuples***：测试集/盲测集的三元组集合数

***Matching Tuples***：模型预测的三元组集合与测试集/盲测集的三元组集合间的最大匹配个数

### (2)模态
本次评测任务为开放测试，预训练语言模型可自由选择，允许使用外部资源，如大语言模型、专名识别、句法分析结果等。开放测试中，参赛队使用的所有资源需要在最终提交的技术报告中给予详细说明。但不可使用人工修正自动解析结果的方式。

## <span id = '_link_6st'>4.Baseline </span>
| 依存模型 | GPT4交互式|
| ------ | ------ |
| 31.03 | 37.29 |

## <span id = '_link_4st'>5.奖项设置 </span>
本届评测将设置一、二、三等奖，提供总额为7000元的奖金，并由中国中文信息学会为获奖队伍颁发荣誉证书。

## <span id = '_link_5st'>6.评测赛程 </span>
+ 报名开始时间：2月20日（已截止）
+ 训练集与验证集发布时间：3月11日
+ 有答案的测试集发布时间：4月15日
+ 无答案的盲测集可领取时间：5月4日
+ 结果提交截止时间：5月14日【暂定】
+ 评测报告初稿提交截止时间：5月20日【暂定】

排名根据提交情况动态更新，请关注本任务网站，查看现排名情况。参赛选手如有异议，请与负责人联系。
<!-- file2.md -->
[排行榜](排行榜.md)

后续具体评测赛程请关注该网站或与联系人联系确认。

有意报名者可扫描二维码填写团队信息及联系方式，任务联系人将及时与您取得联系。
![意合图评测报名二维码](https://github.com/gertrude95/Chinese-Parataxis-Graph-Parsing/assets/136043716/d35092f0-d19b-47fa-b66e-10df5764f7c2)



