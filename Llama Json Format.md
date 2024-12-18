## Llama.cpp 语法文件
1. 给出一个你需要的模式，以TypeScript格式给出
```JavaScript
interface answer {
    id: number;
    name: string;
}
```
2. 然后在这个转换网站中进行生成 https://grammar.intrinsiclabs.ai/
3. 得到一份模板文件，插入到llama.cpp中
```
root ::= answer
answer ::= "{"   ws   "\"id\":"   ws   number   ","   ws   "\"name\":"   ws   string   "}"
answerlist ::= "[]" | "["   ws   answer   (","   ws   answer)*   "]"
string ::= "\""   ([^"]*)   "\""
boolean ::= "true" | "false"
ws ::= [ \t\n]*
number ::= [0-9]+   "."?   [0-9]*
stringlist ::= "["   ws   "]" | "["   ws   string   (","   ws   string)*   ws   "]"
numberlist ::= "["   ws   "]" | "["   ws   string   (","   ws   number)*   ws   "]"
```
4. 可以得到正确的输出结果
```json
{"id"：1，"name"："Mercury"}
```
## Ollama Json_mode （类OpenAI）
最早由GPT4o实现，现在Ollama同样支持
此处使用**langchain-ollama**实现
1. 首先，要开启json_mode，同时注意温度要适当降低，来实现更好的遵循效果
```python
llm = ChatOllama(model="llama3.1", temperature=0.6, format="json")
```
2. system提示词中要加入
