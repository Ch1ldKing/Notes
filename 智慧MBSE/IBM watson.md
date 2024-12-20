# 概述
Watson 利用自然语言处理技术，对企业及公司中大量的数据进行认知与分析，来增加其对主题的理解和洞察力。同时通过自然语言处理，基于对这些数据的分析来回答人类提出的相关业务问题，并且协助开发与编排，降本增速，甚至在数据中挖掘潜在的价值

# 组成
包括三个核心组件和一组AI助手，可针对性的利用特定企业领域的数据来为人工智能提供数据源，可以实现用多种模型进行部署，并提供监管功能，可创造人工智能价值

![image.png](https://s2.loli.net/2024/09/06/quZigATL3FBxIWP.png)

## Watson ai
用来训练、验证、微调和部署ai。其AI由基础模型和传统机器学习提供支持来形成强大的生成式AI。
### 模型
可以使用自定义模型、hugging face开源模型和IBM提供的Granite模型（开源）
**Granite** for code 3b、8b、20b、34b ，针对企业软件开发工作流程进行优化
**Granite** 语言模型 支持多语言，并且推理过程消耗GPU资源较小
**Granite** Time Series 基于企业商业工业 数据集进行预训练，轻量级进行时间序列预测
Granite 强调透明度与数据安全，并且追求高性能与低计算需求与推理成本
### Prompt lab
1. 可以选择模型
![image.png](https://s2.loli.net/2024/09/06/87BT4tEIpCGklev.png)

2. 聊天可上传文档

![image.png](https://s2.loli.net/2024/09/06/qLCh1Fd2vRriEps.png)

3. 定制prompt，并且有一系列模板帮助快速定制

![](https://s2.loli.net/2024/09/06/ViKtmeFA1gJaDPn.png)

4. 可通过结构化定制prompt，并提供插入变量以及api请求和代码，提供快捷的模型参数调整，极大的减少学习和使用成本，并且根据企业主题高度定制

![image.png](https://s2.loli.net/2024/09/06/VwtsM69zAO8gUyr.png)

### Tuning Studio

目前版本还没有模型微调功能，但可以通过一些标记的数据，快速调整模型得到的参数及权重，以及模型的输出样式和对信息的抽取汇总方式，但无需重新训练模型（不支持试用版）

![image.png](https://s2.loli.net/2024/09/06/OcHGQBL3P6smoR1.png)

### Flows Engine

帮助任何技术水平的人快速连接企业数据以及定制功能，来实现快速开发AI应用，有SDK及详细的文档。个人理解为如同langchain，不过对开发进行简化和傻瓜式包装，通过一种流式方式来定义生成逻辑，以实现定制一个RAG或页面总结工具。并且提供完备的评估、监控和调试开发套件

![image.png](https://s2.loli.net/2024/09/06/W2fjLKeidsG9DUY.png)

### MLOps
通过集中可视化及自动化来构建AI的模型流程设计以及优化
包含可视化数据、通过管道进行AI流程设计、通过AutoAI进行模型训练、超参调整、基座调整和特征工程及评估、训练数据整理和生成
![image.png](https://s2.loli.net/2024/09/06/hKE1fCLypW4ai6l.png)

## Watson.data

IBM 提供的AI驱动的数据湖，跨环境混合所有的数据，并将数据进行高效整理与统一，提供给AI应用程序进行模型的调整与RAG等等。
### 高效条目化数据
对多个环境的混合数据，进行条目化整理及可视化，来进行快速的集成向量化数据并为AI提供支持
图中数据分为数据存储桶
![image.png](https://s2.loli.net/2024/09/07/sdMR8CUzIahigSN.png)
### 工作负载优化、安全性和性能
智能化选择查询引擎、对混合数据的高效整理
### 解锁数据价值
一些结构化数据或历史数据代表许多业务状态，并且自然语言中隐藏着许多结构化数据。watson将生成式AI的语义层嵌入到数据湖中，来进行语义检索（无需SQL），并且挖掘数据价值

## watson.governance
根据一些AI风险手册（比如数据中毒、幻觉、模型漂移与偏差）来自动*评估和监控模型的健康状况、准确性、漂移、偏差和生成 AI 质量*，自动收集和记录模型元数据，并展示在仪表盘。
![image.png](https://s2.loli.net/2024/09/07/qebHaAihkpZDNWU.png)
也可以跟踪AI开发进度，生成图标和报告等

# 总结
完整的watsonx平台可帮助企业快速实现大模型驱动的智能化及数据管理，无需大量的人员成本及技术成本，并可提高现有业务的效率，挖掘潜在价值