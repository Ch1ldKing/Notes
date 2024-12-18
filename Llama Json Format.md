## Llama.cpp 语法文件
1. 给出一个你需要的模式，以TypeScript格式给出
```JavaScript
interface answer {
    id: number;
    name: string;
}
```
2. 然后在这个转换网站中进行生成 https://grammar.intrinsiclabs.ai/
3. 得到一份模板文件，插入到
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


