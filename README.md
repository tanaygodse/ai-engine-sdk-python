## ⭐️ Features

- Access to latest AI Engine features

- Simple and intuitive API

  

## 📦 Getting Started

  

```bash

poetry add ai-engine-sdk
# or 
pip install ai-engine-sdk

```

  

### Using the Chat API

Before you start to integrate the AI Engine into your app, you might want to get familiar with [agent functions](https://fetch.ai/docs/guides/agents/intermediate/agent-functions).

  

#### Creating the AIEngine client object

To find out how to generate an <code>apiKey</code> check out the documentation regarding [Agentverse API keys](https://fetch.ai/docs/guides/apis/agent-function-creation-apis).

```python


from ai_engine_sdk import AiEngine
ai_engine: AiEngine = AiEngine(api_key)

```

  

#### Querying the id of the function group where our to-be-used function(s) belong

```python
function_groups: list[FunctionGroup] = await ai_engine.get_function_groups()

public_group = next(
	(g for g in function_groups if g.name == "Fetch Verified"), 
	None
)
```

If you would like to use the functions in your own **My Functions** function group instead then you can use this filter instead:

```python
my_group = next(
	(g for g in function_groups if g.name == "My Functions"), 
	None
)
```

  

#### Creating a session with the AI Engine using the <code>functionGroupId</code> fetched before

```python
session = await ai_engine.create_session(function_group=public_group.uuid)
```

  

#### Starting the conversation with an arbitrary objective

```python
await session.start(objective)
```

  

#### Querying new messages

You might want to query new messages regularly ...

  

```python

while True:
	messages: list[ApiBaseMessage] = await session.get_messages()
	# throttling
	sleep(3)

```

  

#### Checking the type of the new message

There are 5 different types of messages which are generated by the AI Engine and the SDK implements methods for checking the type of the respective new <code>Message</code>:

* task selection message: <code>is_task_selection_message</code> - this message is generated for example in the case when the AI engine recommends functions based on the initial objective or in the case when the AI Engine finds multiple options to provide as an input for a function.

* AI Engine message: <code>is_ai_engine_message</code> - this message type doesn't expect the user to reply to, this is for notifying the user about something

* confirmation message: <code>is_confirmation_message</code> - when the AI Engine has managed to acquire all the inputs for the agent belonging to the to-be-executed function (= it has managed to build the context), it sends this type of message to the user

* agent message: <code>is_agent_message</code> - this is a regular question that the user has to reply to with a string

* stop message: <code>is_stop_message</code> - this is the message which is sent when the session has stopped and the AI Engine doesn't wait for any replies from the user

  

#### Replying to the different type of messages

All message types (expect for the AI engine message and stop message) expects a response from the user and the SDK implements methods for sending reply in response to those different type of messages respectively.

The first argument of all these reply methods is the <code>Message</code> object to which we want to send back the response.

The SDK methods you can use to reply to:

* task selection message: <code>session.submit_task_selection</code>

* agent message: <code>session.submit_response</code>

* confirmation message: <code>session.submit_confirmation</code> or <code>session.reject_confirmation</code> - depending on if the user wants to confirm or reject the context generated by the AI engine respectively

  

#### Deleting session with AI engine

After finishing the conversation with AI Engine you can delete the session like this:

```python

await session.delete()

```

  

If you would like to check out a complete example on how to integrate AI Engine into your app, feel free to checkout [examples/run_example.py](examples/run_example.py).

  

## ✨ Contributing

  

All contributions are welcome! Remember, contribution includes not only code, but any help with docs or issues raised by other developers. See our [contribution guidelines](https://github.com/fetchai/ai-engine-sdk-js/blob/main/CONTRIBUTING.md) for more details.

  

### ❓ Issues, Questions, and Discussions

  

We use [GitHub Issues](https://github.com/fetchai/ai-engine-sdk-python/issues) for tracking requests and bugs.