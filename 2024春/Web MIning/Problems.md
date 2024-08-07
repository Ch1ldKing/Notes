1. Python与Springboot交互
2. 将优秀的菜肴传递给GPT
3. 如何进行检索
4. 数据导入数据库
5. 解析GPT输出做正则表达式

# 大纲
## IDEA
1. 想做点不一样的，并且要有价值
2. 我们的项目希望带来什么价值
## 技术与方法
1. 数据挖掘
2. 数据清洗
3. 模型的建立
   1. 按热度推荐
   2. 食材的关联规则
   3. 菜肴的聚类分析
   4. LLM，创新菜肴
4. 模型的评估
5. 搭建前后端服务器及页面
6. 集成测试和系统测试
## 项目的结果
录个视频，得到一些反馈（测试的结果）
## 结论
遇到问题解决的思路，团队合作
未来

This project aims to develop a functional application. Below is an overview of the technical implementation:
Data Mining: Using web crawlers to gather a large variety of recipe data from the internet.
Data Cleaning: Utilizing pandas to remove blanks and duplicate data, and to extract ingredient information.
Data Models: Popularity-based recipe recommendation model, Clustering-based menu generation model, 
Association analysis-based ingredient recommendation model, LLM recipe generation model
Model Evaluation: Coverage and satisfaction to evaluate recommendation models, Cohesion and separation 
to evaluate clustering models, Mean squared error to evaluate association models
Tech Stack: Frontend: Vue 3, Backend: Spring Boot, Database: PostgreSQL, Models: Flask framework
Agile Development: Analyze software requirements, Plan phase increments, Collaborate via Git, Implement CI/CD with GitHub Actions