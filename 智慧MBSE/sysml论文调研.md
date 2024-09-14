# System Architects are not alone Anymore: Automatic System Modeling with AI
## 1. TTool
利用TTool开源Sysml框架。在提示词中加入系统工程知识，该系统的定义，需要在以往的Sysml图上进行修改的Sysml代码，以及新问题
个人认为可以采用RAG，不过原理是一样的
![image.png](https://s2.loli.net/2024/09/14/LoO3qeHNQMcWPfg.png)

## 2. **自动反馈循环机制**

设置一个循环机制检查输出中是否有错误，然后再将错误嵌入到prompt中让LLM重新生成，最大20次

- **输入初始问题**：系统首先向LLMs提供系统规范和问题（如要求识别系统的构件或生成SysML图），并接收LLMs的初次回答。
- **检测输出中的错误**：
    - 检查LLMs生成的答案是否符合指定的格式（例如JSON格式）。
    - 验证生成结果是否满足预定义的约束条件（如图中的构件是否有重复名称、信号是否正确连接等）。
- **生成新的问题**： 如果系统检测到生成的输出中存在错误，它会根据错误的类型生成一个新的问题，并重新向LLMs提问。例如，如果发现格式错误，系统会生成一个关于正确格式的提示性问题。
- **反馈循环**：
    - 每当LLMs生成的输出不符合预期时，系统就会更新问题和上下文，并将新的问题重新提交给LLMs。
    - 这个过程会持续多次，直到LLMs生成的结果符合所有的格式和约束要求，或者达到预设的反馈循环次数上限。
- **结束条件**： 自动反馈循环在以下两种情况下结束：
    - 生成的结果完全符合所有要求，且没有格式或约束错误。
    - 达到预设的最大反馈次数上限（例如默认20次迭代）。在这种情况下，系统可能会要求用户手动调整或提供进一步的反馈。

> 验证生成结果是否满足预定义的约束条件是什么？
> 	预定义的约束条件是对SysML图和系统模型设计的规则和要求。
> 	- **格式约束**：例如，SysML模型必须符合特定的格式（如JSON或SysML v2的文本格式）。
> 	- **命名约束**：图中的元素（如构件、信号、端口）必须满足特定的命名规则（如不能含有空格，必须使用下划线）。
> 	- **数据类型约束**：构件的属性必须具有正确的数据类型（如`int`、`bool`等），这些数据类型在生成图时需要明确约束。
> 	- **信号连接约束**：信号必须正确地连接，即输入信号和输出信号必须具备相同的属性，且连接的信号必须分别定义为输入和输出。
> 	- **逻辑约束**：例如，状态机中的状态必须有一个起始状态，且状态之间的过渡必须基于有效的条件（如`guard`条件必须为逻辑表达式）。


## 3. 系统设计流程
在具体应用中，系统首先生成结构图（如Block图），随后再生成行为图（如状态机图）
![](https://s2.loli.net/2024/09/14/w6UO9tKWEpZmjBo.png)
如图，分两步进行

- **问题1** 的重点是识别系统的**构件及其属性**。其任务是从系统规范中提取出构件的名称、属性及其数据类型（如`int`或`bool`），并按要求的JSON格式输出。
  
```json
  {
  "blocks": [
    {
      "name": "CoffeeMachine",
      "attributes": [
        {"name": "coinsDeposited", "type": "int"},
        {"name": "beverageSelected", "type": "bool"}
      ]
    },
    {
      "name": "CoinSensor",
      "attributes": [
        {"name": "coinInserted", "type": "bool"}
      ]
    }
  ]
}
```

- **问题2** 的重点是识别系统构件之间的**信号连接**。在问题1识别出的构件基础上，问题2进一步识别这些构件之间如何通过信号进行通信，并定义信号的输入、输出属性及其连接规则。
  
  ```json
  {
  "blocks": [
    {
      "name": "CoffeeMachine",
      "signals": [
        {"name": "coinAccepted", "type": "bool", "direction": "input"}
      ]
    },
    {
      "name": "CoinSensor",
      "signals": [
        {"name": "coinInserted", "type": "bool", "direction": "output"}
      ]
    }
  ],
  "connections": [
    {"from": "CoinSensor.coinInserted", "to": "CoffeeMachine.coinAccepted"}
  ]
}
```

## 4. 可能的升级
作者认为未来可以通过引入更高级的建模方法（如分层建模、增量建模等）来进一步提高TTool-AI的性能

# Leveraging Generative AI to Create, Modify, and Query MBSE Models

在零样本、少样本、RAG对GPT4和3.5进行测试。其po