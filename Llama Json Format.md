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
2. system提示词中要加入“输出JSON格式”
```
SystemMessage(["... Answer in json format"])
```
3. 使用少样本提示Json格式，下面是一个完整的例子
```python
ner_prompt = ChatPromptTemplate.from_messages(
    [
        (
            "system",
            "You extract economic entities from the article, such as countries, companies, organizations, and individuals, output in JSON format",
        ),
        (
            "human",
            """
            Apple Inc., a leading technology company based in the United States, partnered with the United Nations and MIT to launch an initiative aimed at improving digital literacy, with Elon Musk as a key advisor to the project.
            """,

        ),

        (

            "ai",
            """
            ```json
            {{"entities":["Apple Inc","United States","MIT","United Nations","Elon Musk"]}}
            ```
            """,
        ),
        ("human", "{content}"),
        (
            "ai",
            "",
        ),
    ]
)
```
请注意我加入了` ```json`来明确这个信息。经过测试，此方法稳定且简单