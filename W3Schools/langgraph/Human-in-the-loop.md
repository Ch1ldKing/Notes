We could just define a **recall function** to make human interact with the LLM.
Here is an example
```python
def human_input_node(state):
    # 显示当前状态给用户
    print("Current state:", state)
    
    # 获取用户输入
    user_input = input("Please provide your input: ")
    
    # 更新状态
    state['user_input'] = user_input
    return state

graph.add_node("human_input", human_input_node)
```
Make user change the state and give it back to the graph.
