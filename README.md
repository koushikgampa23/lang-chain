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

### Into to the core architecture of the AI agents
    Layer 0:
        llm = ChatGoogleGenerativeAI("gemini-3.8-flash")
        tools = [TavilySearch()]
        agent = create_agent(llm, tools=tools)
    Layer 1:
        Agent loop + Function calling, (LangChain)
        @tool, bind_tools(), init_chat_models(), ToolMessage
                            |
                            v
        Agent Loop + function calling(RAW)
        Hand-written JSON schemas, ollama.chat()
    
    Layer 2:
        Agent Loop + Raw ReAct Prompt
        ReAct prompt, regex, scratchpad

### Agent Loop
    Suppose you wanted to develop a discount application and how the LLM tools works are given below
![alt text](<Agent loop and First layer.png>)

## RAG
    Retrievel Argumentation Generation
    When we are dealing with private data(like financial data) the model is not trained on, the llm doesnot know anything about it.
    We need to find a way so that the llm can effectivly answer this kind of questions.

    Solution:
        Take entire book plug entire staff to the llm
        This solution might work sometimes but there is a hard limit on how much data we can feed to llm
        If it is a long document we are going to exceed token limit.
        Even with LLMs 1 million or 2 million is still not ideal, it has many other disadvantages
        The LLM is much less effective with very long prompts and very long context.
        The larger prompts costs us more and there is a latency issue

        Problems:
            Hard token limit
            Cost, latency
    Solution2:
        We are going to store the data in chunks, when ever user going to send query, it finds the most relevant chunk, and we are only going to feed that particaular chunk to the LLM
        Drawbacks:
            we need to add pre-processing step to check large documents
            There is lot of depth in how do we chunk the document
                what are the tokens that we are spliting to the document
                how do we make sure that each chunk has relevant amount of data
                We need different chunk strategies to different type of types
                Searching mechanism searching over those chunks
    RAG(Retrieval Argumentation Generation)
        Retrieval - Retrieving the relevant chunk
        Argumentation - that we take our prompt and we argument it with those relevant chunks
        Generation - to simply send to the LLM and to use that when we make the query to LLM

### Introduction to Vector databases
    Topics to be covered:
        Embeddings, Vector stores(Pinecones), RetrievalQA Chain, LangChain document readers, LangChain text splitters
    
    Suppose we have a large document, the data might be repeated in mutlipe paragraphs and if we wanted to get relevant data we might need to make multiple chunk calls to get data
    If we start sending every chunk to llm that we will cost more
    Is there any way to revelant data as a chunk
    
    Embeddings:
        The idea is to create a vectoric space, such that distance between vectors have some meaning
        Vector: a vector is a sequence of numbers, the vector can represent more complex object like words, sentences, images, audio files in continous high dimentional space called embeddings
        
        Embeddings think of like a black box, which receives the objects and those are represented by text does something to them that outputs them array of numbers, which represents those objects in vector space.

        object1, object2 -> Embedding -> [0.6,0.3,0.1,...], [0.8,0.5,0.3...] (Objects of vector)

![alt text](<Embeddings flow.png>)

### Practical implementation at ingestion.py file that does this
    Taking our data, making it a langchain object(TextLoader)
    Split this object with a langchain splitters to smaller chuncks(TextSplitters)
    Embed the chucks and get vectors (OpenAIEmbeddings)
    Store the embeddings in pinecode vectorstore(PineconeVectorStore)

    Packages needed are:
        langchain-unstructured(text loader package), langchain-text-splitters, langchain-pinecone(PineconeVectorStore)

    Next steps:
        Retrive top k chunks from vector db and send to llm that gives the response

### Queston and answers
    In the context of RAG ingestion, what role do OpenAIEmbeddings serve?
    

    