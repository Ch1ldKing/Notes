# Messages
If you just want to steam the tokens of llm, following what I do:
```python
def generate_answer(app, input):
	
	config = {"configurable": {"thread_id": "abc23"}}
	
	for msg, metadata in app.stream({"input": input}, config=config, stream_mode="messages"):
	if isinstance(msg, AIMessageChunk):
	
		yield msg.content
```
This is used for Streamlit `write_stream`, because it needs a generator function that can be 