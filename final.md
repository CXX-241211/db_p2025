# Project 2025 for DB



## 要求



- 请使用《计算机学报》模板（[Word](http://cjc.ict.ac.cn/wltg/new/submit/index.asp)或[Latex](https://www.overleaf.com/latex/templates/ji-suan-ji-xue-bao-guan-fang-latexmo-ban-xiu-gai-wei-overleafke-yong-ban/mjhxhmnqvvyn)），并**打印**提交给助教；最晚17周周日；清楚地标明姓名、学号、题目
- 最多3个人一组
- 要求提供GitHub代码仓库链接
- 报告需要清楚地说明*关键设计*、*如何使用*等重要内容

## 题目

设计一个命令行工具（可以使用[hyper](https://github.com/fastapi/typer)等），首先初始化创建数据库`project2025`，

- 自行设计并录入题目二的关系模式
- 能够检查SQL查询语句的正确性，如果查询语句有错误，能够给出错误的原因，要求报错信息尽可能用户友好，并且能够修改建议
- 能够同时可视化查询流程以及查询结果
- 实验性加入通过自然语言查询数据库的功能
- 能够重置系统，以便录入新的关系模式