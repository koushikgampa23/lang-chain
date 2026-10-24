# lang-chain

### why dont we just a F string
    Prompt template is to enforce that we supply exact variables that we expect, and if we forget an input or misspell it, we get error instead sending us the broken prompt
    It is also going to give us reusability and more clarity because those prompts can become resuable in another langchain.

### why do we use temparture in llms
    temparture will control how random or creative versus tricked and deterministic the model response is going to be.
    values between 0 and 0.3 will make model deterministic, factual and probably repeatable, good for code summerization, instructions
    high values from 0.8 to 1 its going to give us more creative results and its good for poetry and fiction and out of the box ideas

### what are AI Agents
    A High level overview:
        An agent is a software ecosystem that uses llms as a reasoning engine to decide what actions to take, and then execute their actions
        So unlike simple chains and runnables, where the sequence of actions are hard coded, agents dynamically determine which tools or steps need to be taken to solve a specific task or to answer specific questions.
        The important thing to note is that LLM in an agent is deciding what to do next.
        where in chains we might use LLM to summerize something or to generate some text, However we as developers we define the entire control flow. we decide what is going to happen after next.
        LLM may be used in one of the steps but its not deciding what to do next
### Evolution of react agents
    In 2022, the langchain reat agent relied purely on react prompting where the model would reason about actions and observations, in a text based fomat. 
    And the LLM landscape evolved with the native function calling feature capabilities of LLMs, this architecture is transformed into tool calling agents, which levered the structure calling instead of prompt based tool selection.
    this made tool selection more reliable and more efficient.

### what is a tool
    A tool is a function that an agent can execute
    it can be any function that we want, so we can write internal implementation of that function, so this means we can give our agent endless possibilites of actions it can perform

    so we can make it call an API or search in a database to run and execute code.
     