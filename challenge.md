# 1. What is relational in “relational database”



> Relating to, using, or being a method of organizing data in a database so that it is perceived by the user as a set of tables (Merriam-Webster, n.d.).
>
> Relational databases is considered (Oracle, June 18, 2021)  as a database based on the relational model that stores and provides access to data points that are related to one another through key and the value of a record and the value to it.
>
> Codd (1970) first introduced the concept of relational databases.



总的来说，关系指数据以二维表的组织，每一个表都对应着一个笛卡尔积的子集。从内部看，关系体现在每行数据对应一个元组，每一列对应一个属性，可能也包含每个属性与关系的一致性（有某种逻辑关系在）；从外部看，大概体现表格之间可以受外键关联。



###### **Reference**

> Codd, E. F. (1970). *A relational model of data for large shared data banks. Communications of the ACM, 13(6), 377–387.* doi:10.1145/362384.362685
>
> Merriam-Webster. (n.d.). *relational*. In *[Merriam-Webster.com](http://merriam-webster.com/) dictionary*. Retrieved June 15, 2023, from https://www.merriam-webster.com/dictionary/relational
>
> Oracle. (June 18, 2021). What Is a Relational Database? (RDBMS)?Oracle. Retrieved February 28, 2025, from https://www.oracle.com/database/what-is-a-relational-database/
>
> Silberschatz, A., Korth, H. F., & Sudarshan, S. (2020). *Database system concepts* (7th ed.). McGraw-Hill.



# 2. After 2023: the Cutting-edge of Text2SQL

###### 注：下述文本由AI生成，首先我找了相关文献，然后交由deepsseek梳理

近年来，随着自然语言处理与大语言模型（LLM）的深度融合，Text-to-SQL技术通过跨领域创新与场景化应用取得显著突破：混合SLM-LLM框架通过任务分解与知识迁移机制，在零样本场景下实现执行准确率提升16.4%（ZERONL2SQL）；LLM与数据库系统的深度整合催生新型交互范式，DB-GPT通过提示工程与物理特征优化使查询延迟降低28%，工业级系统OlaChat日均处理百万级复杂查询；多任务学习与对抗训练技术显著增强模型鲁棒性，SQL扰动框架使抗干扰能力提升41%，动态注意力校准机制在MultiHopQA数据集上误差率下降35%。在医疗等垂直领域，研究者提出基于XLM的Text2GraphQL模型，通过Schema-Utterance对齐Adapter注入领域知识，结合指针网络与自注意力修正机制，有效提升医疗实体复制精度和查询生成效率。大规模数据集如BIRD（12,751对文本-SQL）与Gretel（全球最大隐私保护合成数据集）推动技术边界拓展，评估体系新增模式兼容性、可解释性等维度；模型轻量化设计取得突破，TaLM框架以0.8B参数实现与GPT-4相当的多跳推理能力，NanoSQL通过知识蒸馏将延迟压缩至50ms以下。安全与隐私保护成为重要研究方向，DB-GPT的敏感列掩码策略实现99.8%医疗数据隐私保护率，联邦学习框架FedSQL在欧盟《AI法案》框架下支持跨机构协作。这些进展共同推动Text-to-SQL技术从实验室研究走向金融、医疗等垂直领域的规模化应用，标志着人机交互范式从关键词匹配向语义理解与认知决策的范式转变。



Ju Fan, Zihui Gu, Songyue Zhang, Yuxin Zhang, Zui Chen, Lei Cao, Guoliang Li, Samuel Madden, Xiaoyong Du, and Nan Tang. 2024. Combining Small Language Models and Large Language Models for Zero-Shot NL2SQL. Proc. VLDB Endow. 17, 11 (July 2024), 2750–2763. https://doi.org/10.14778/3681954.3681960

Zhou, X., Sun, Z. & Li, G. DB-GPT: Large Language Model Meets Database. Data Sci. Eng. 9, 102–111 (2024). https://doi.org/10.1007/s41019-023-00235-6

Ni, P., Okhrati, R., Guan, S. *et al.* Knowledge Graph and Deep Learning-based Text-to-GraphQL Model for Intelligent Medical Consultation Chatbot. *Inf Syst Front* **26**, 137–156 (2024). https://doi.org/10.1007/s10796-022-10295-0

Xiaokang Zhang, Sijia Luo, Bohan Zhang, Zeyao Ma, Jing Zhang, Yang Li, Guanlin Li, Zijun Yao, Kangli Xu, Jinchang Zhou, Daniel Zhang-Li, Jifan Yu, Shu Zhao, Juanzi Li, Jie Tang, TableLLM: Enabling Tabular Data Manipulation by LLMs in Real Office Usage Scenarios,https://doi.org/10.48550/arXiv.2403.19318 





# 3. API Call 



###### P.S.`deepseek-chat` 模型对应 **DeepSeek-V3**；`deepseek-reasoner` 模型对应 **DeepSeek-R1**。夜间使用半价:new_moon_with_face:，R1白天贵一倍，夜间同价。



### 3.1 V3


```
from openai import OpenAI

client = OpenAI(api_key="sk-6ad8421e3c56439882dbb63cacbda090", base_url="https://api.deepseek.com")

response = client.chat.completions.create(
    model="deepseek-chat",
    messages=[
        {"role": "system", "content": "You are a helpful assistant"},
        {"role": "user", "content": "给出解决下列问题的代码：考虑：create table classroom(building  varchar(15),room_number  varchar(7),capacity  numeric(4,0),primary key (building, room_number));找出容量最大的教室房间号"},
    ],
    stream=False
)

print(response.choices[0].message.content)

```

````
要找出容量最大的教室的房间号，可以使用 SQL 查询语句。以下是一个示例代码：

```sql
SELECT room_number
FROM classroom
WHERE capacity = (SELECT MAX(capacity) FROM classroom);
```

### 解释：
1. `SELECT MAX(capacity) FROM classroom`：这个子查询会找出 `classroom` 表中 `capacity` 列的最大值。
2. 外层查询会返回 `capacity` 等于这个最大值的 `room_number`。

### 注意：
- 如果有多个教室的容量相同且都是最大值，这个查询会返回所有容量最大的教室的房间号。
- 如果你只想返回一个房间号（即使有多个教室容量相同），可以使用 `LIMIT 1` 来限制结果集：

```sql
SELECT room_number
FROM classroom
WHERE capacity = (SELECT MAX(capacity) FROM classroom)
LIMIT 1;
```

这个查询会返回容量最大的教室中的第一个房间号。

Process finished with exit code 0

````



### 3.2 R1


```
# 换个模型
...
    model="deepseek-creasoner",
... 

```

````
要找出容量最大的教室房间号，可以使用以下SQL查询：

```sql
SELECT building, room_number
FROM classroom
WHERE capacity = (SELECT MAX(capacity) FROM classroom);
```

**步骤解释：**
1. **子查询获取最大容量**：`(SELECT MAX(capacity) FROM classroom)` 计算出所有教室中的最大容量值。
2. **主查询匹配容量**：外层查询选择所有容量等于该最大值的记录，返回对应的建筑和房间号。

**注意**：由于主键由`building`和`room_number`共同构成，需同时选择这两个字段以确保结果唯一性。如果仅需房间号且允许重复，可只选`room_number`，但建议包含建筑信息以准确定位。

Process finished with exit code 0

````



###### reference

[首次调用 API | DeepSeek API Docs](https://api-docs.deepseek.com/zh-cn/)



# 4. 为什么在Python中{[1,2]}不合法



上述本质是一个以集合为元素的列表，而集合又是基于哈希表实现的数据结构，所以要求元素可哈希（不可变=可哈希）。

形成集合→哈希函数把集合内容转换为哈希值→映射到对应位置→集合存的不是“值”而是一个位置，你不是通过“更改集合”来进行变化，只单一变动其中部分哈希值（自身属性）会变化，但是地址不会，好比你搬家了（自己的真实住址）但是居委会只知道你的原住址（存储时的地址），就找不到你，会出现丢失问题。为了避免则些事情，直接设置为不合法。

听起来比较局限，但局限本身也带来效率，哈希表的设计主要是为了更快嘛。





2025.03.08	42233092 XingChen
