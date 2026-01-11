
## Model
Wrap OpenAI (or other providers) into a **standard interface** (`invoke`, `stream`, `batch`, etc.)

```python
from langchain_openai import ChatOpenAI

model = ChatOpenAI(model="gpt-4o-mini")
print(model.invoke("Explain LangChain").content)
```

## Messages
How inputs are formatted and exchanged between prompts and models.

```python
from langchain.schema import HumanMessage, SystemMessage

messages = [
    SystemMessage(content="You are a helpful assistant."),
    HumanMessage(content="Explain embeddings.")
]
```

## Prompts
Templates for constructing structured prompts with variables.

```python
from langchain.prompts import ChatPromptTemplate

prompt = ChatPromptTemplate.from_messages([
    ("system", "You explain concepts."),
    ("user", "Explain {topic}")
])

messages = prompt.format_messages(topic="RAG")
```

## Tools
Expose Python functions for the LLM to call.

```python
from pydantic import BaseModel, Field
from langchain.tools import tool
from langchain_openai import ChatOpenAI

class WeatherInput(BaseModel):
	location: str = Field(description="City name or coordinates")
	units: Literal["celsius", "fahrenheit"] = Field(
		default="celsius",
		description="Temperature unit preference"
	)

@tool(args_schema=WeatherInput)
def get_weather(location: str, units: str = "celsius"):
    # Get weather function
    return result

print(model.invoke("What is the weather in Singapore in Celsius?"))
```

