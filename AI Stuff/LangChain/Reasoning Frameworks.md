
| Method                     | Core Idea                                                            | Strengths                                             | Weaknesses                                     | Best Use                                      |
| -------------------------- | -------------------------------------------------------------------- | ----------------------------------------------------- | ---------------------------------------------- | --------------------------------------------- |
| **ReAct**                  | “Think, act, observe, repeat.” LLM reasons + calls tools in a loop.  | Tool use, interactive reasoning, multi-step workflows | Unreliable, hallucinated tools, infinite loops | When you need model-driven tool orchestration |
| **MRKL**                   | Router model chooses _modules_ (tools, calculators, retrievers).     | Modular, scalable, hybrid symbolic-neural systems     | Requires routing model tuning                  | Systems with many specialized tools           |
| **ReWoo**                  | LLM writes a _plan_ before acting (Write), executes, then organizes. | Better planning than ReAct, more controllable         | Still LLM-driven, can overplan                 | Tasks needing planning + action               |
| **Tree-of-Thoughts (ToT)** | Model explores branching reasoning paths (search tree).              | Much higher accuracy, global search                   | Too slow for real-time, expensive              | Hard tasks: puzzles, math, logic              |
| **Chain-of-Thought (CoT)** | Model reveals internal reasoning before answer.                      | Improves accuracy, simple                             | Doesn’t call tools, doesn’t act                | Pure reasoning tasks                          |
| **Self-Refine / SoT**      | Model critiques itself and improves the answer.                      | Better final outputs                                  | Multi-step, expensive                          | Writing, coding, summarization                |

# MRKL (Modular Reasoning, Knowledge, and Language)
**Use when:** MANY tools or subsystems exist; model must choose _which module_ handles the query.
**Mechanism:** Router model chooses _modules_ (tools, calculators, retrievers).

| Workflow                                                                                                                                                                                               | Example                                                                                                                                                                                                                 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| USER QUERY<br>   ↓<br>ROUTER MODEL decides:<br>   - math module?<br>   - search module?<br>   - DB module?<br>   ↓<br>SELECTED EXPERT<br>   ↓<br>Expert Output<br>   ↓<br>LLM Summarize → Final Answer | **Task:** “Solve: sqrt(144) + 12 * 3”<br><br>1. Router LLM inspects input<br>    <br>2. Router → routes to **math-tool**<br>    <br>3. Math-tool computes → `52`<br>    <br>4. LLM formats answer → “The result is 52.” |
```python

```
# ReAct
**Use when:** model drives tool usage by reasoning about actions.
**Mechanism:** Reason → Act → Observe → Repeat / Final

| Workflow                                                                                                                                                                                                                                                                                 | Example                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| USER QUERY<br>   ↓<br>LLM: Thought ("I need to search...")<br>LLM: Action(search) + Input<br>   ↓<br>TOOL: executes search and returns result<br>   ↓<br>LLM: Observation ("Search says: ...")<br>LLM: Thought ("Now I can answer...")<br>LLM: Action(final)<br>   ↓<br>Final Answer<br> | **Task:** “What’s the population of Singapore + 25%?”<br><br>1. **Reason:**  <br>    “I should search for population.”<br>    <br>2. **Act:**  <br>    `search("population of Singapore")`<br>    <br>3. **Observe:**  <br>    `"5.92M"`<br>    <br>4. **Reason:**  <br>    “Now calculate 25% of this.”<br>    <br>5. **Act:**  <br>    `calculator("5.92M * 1.25")`<br>    <br>6. **Observe → Final Answer**  <br>    `"7.4M"` |

```python
from langchain.agents import create_react_agent, AgentExecutor
from langchain_openai import ChatOpenAI
from langchain.tools import tool

@tool
def search(q: str):
    return "Singapore population is 5.92 million."

@tool
def calc(expr: str):
    return str(eval(expr))

model = ChatOpenAI(model="gpt-4o-mini")
agent = create_react_agent(model, [search, calc])
executor = AgentExecutor(agent=agent, tools=[search, calc], verbose=True)

executor.invoke({"input": "What is Singapore's population plus 25%?"})
```