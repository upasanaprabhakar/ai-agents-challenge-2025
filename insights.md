# Things I'm Learning

Quick insights and patterns from the 5-Day AI Agents Challenge.

---

## Main Concepts

### What even is an AI agent?

**What I think now:**
AI agents are autonomous systems that perceive, reason, and act. Unlike traditional programs that follow fixed logic, agents make decisions about what to do based on goals and context.

**How it's different from normal code:**
- Traditional code: "If X, then Y" (explicit)
- AI agents: "Given X, figure out Y" (implicit decision-making)
- Agents can use tools, remember context, and adapt to situations

---

### Agent architecture stuff

**Patterns I'm seeing:**
1. **Sequential** - Agents work one after another (research → write → review)
2. **Parallel** - Multiple agents work simultaneously on independent tasks
3. **Hierarchical** - Supervisor delegates to specialized sub-agents

**Why this is useful:**
Breaks complex problems into manageable pieces. Each agent specializes, improving accuracy and maintainability.

---

### Function calling and tools

**How this works:**
Agents read tool schemas (descriptions), decide which tool fits the query, format parameters, execute the tool, and use results to respond.

**When I'd use it:**
Anytime agents need real-world capabilities: calculations, database queries, API calls, file operations. Transforms agents from conversational to operational.

---

### Memory systems

**Types:**
- **Short-term**: Recent conversation history in context window
- **Long-term**: Vector database for persistent storage across sessions
- **Semantic**: Retrieval based on meaning, not exact words (RAG)

**How I got it working:**
Used ChromaDB for vector storage, embeddings for semantic search, and strategic retrieval to balance context window limits with relevant history.

---

### Testing agents

**What matters:**
- Accuracy (does it work correctly?)
- Safety (does it refuse harmful requests?)
- Cost (is it economically viable at scale?)
- Latency (is it fast enough for users?)

**My approach:**
Built automated test suite with LLM-as-judge, created golden dataset, implemented monitoring, and optimized based on metrics.

---

## Lightbulb Moments

### Day 1
> "Agents DECIDE when to use tools - I don't tell them!"

**What clicked:**
Function calling is declarative. I describe what tools do, agent figures out when/how to use them.

**Why it matters:**
This is the fundamental shift from programming to orchestrating. I design capabilities, agent applies them intelligently.

---

### Day 2
> "MCP is USB for AI"

**What clicked:**
Model Context Protocol standardizes tool integration. Any tool implementing MCP works with any agent.

**Why it matters:**
Eliminates custom integration code for every tool. Build once, use everywhere.

---

### Day 3
> "Memory retrieval is information retrieval"

**What clicked:**
Vector embeddings enable semantic search. Finding relevant context isn't about keywords, it's about conceptual similarity.

**Why it matters:**
Agents can "remember" intelligently by retrieving contextually relevant information, not just everything.

---

### Day 4
> "Testing AI is about patterns, not exactness"

**What clicked:**
Can't assert `output == "expected"` like traditional code. Instead, evaluate if response demonstrates desired qualities.

**Why it matters:**
Requires new testing mindset. Focus on behavioral correctness, not output matching.

---

### Day 5
> "70% of agent development is everything EXCEPT the model"

**What clicked:**
Building impressive demos is 30% of work. Making it reliable, tested, cost-effective, and production-ready is the other 70%.

**Why it matters:**
Professional agent development requires infrastructure, not just prompts.

---

## Code stuff I'm reusing

### Basic agent
```python
model = genai.GenerativeModel(
    'gemini-1.5-flash',
    system_instruction="You are a helpful assistant."
)
response = model.generate_content(query)
```

**When to use:** Simple, stateless queries where context doesn't matter.

---

### Multi-agent setup
```python
researcher = GenerativeModel('gemini-1.5-flash', 
    system_instruction="Research specialist...")
writer = GenerativeModel('gemini-1.5-flash',
    system_instruction="Content writer...")

# Orchestrate: researcher → writer
research = researcher.generate_content(topic)
content = writer.generate_content(f"Write about: {research.text}")
```

**When to use:** Complex tasks benefiting from specialization.

---

### Agent with tools
```python
def search(query: str) -> str:
    """Search the web for current information"""
    return search_api(query)

agent = GenerativeModel('gemini-1.5-flash', tools=[search])
response = agent.generate_content("What's the weather?")
```

**When to use:** Agent needs real-world data or actions.

---

## Mistakes (learning the hard way)

### Mistake: Assuming agents always work correctly
**What went wrong:** Deployed without testing, agent hallucinated facts

**Why:** Non-deterministic outputs mean occasional errors are inevitable

**Lesson:** Always test thoroughly, implement safety checks, handle failures gracefully

**Better way:**
```python
if not validate_response(response):
    return fallback_response()
```

---

### Mistake: Putting everything in context window
**What went wrong:** Hit token limits, high costs, slow responses

**Why:** Tried to include entire conversation history every time

**Lesson:** Use smart memory retrieval instead of brute-force history inclusion

**Better way:**
```python
relevant_context = retrieve_relevant_memories(query, max_tokens=2000)
```

---

## What's working

### 1. Clear system instructions
**What:** Explicit agent role, guidelines, and behavior expectations

**Why:** Well-defined instructions dramatically improve consistency

**Example:**
```python
system_instruction="You are a helpful assistant. If uncertain, say 'I don't know' rather than guessing. Always cite sources."
```

---

### 2. LLM-as-judge for evaluation
**What:** Using Gemini to evaluate agent outputs at scale

**Why:** Automates quality assessment, much faster than manual review

**Example:**
```python
judge.generate_content(f"Rate this response 1-5 for accuracy: {agent_output}")
```

---

## Good resources

### Articles/papers
1. [RAG Paper](https://arxiv.org/abs/2005.11401) - Original retrieval augmented generation research
2. [Model Context Protocol Docs](https://modelcontextprotocol.io/) - MCP specification

### Videos
1. Kaggle livestream recordings - Day 1-5 sessions with experts

### Example code
1. [Google ADK Examples](https://github.com/google/adk) - Official code samples

---

## Project ideas

### Idea 1: Personal Research Assistant
**What it does:** Reads papers, remembers my interests, answers questions with citations

**Why it's cool:** RAG + memory = contextual knowledge assistant

**Need to learn:** Better chunking strategies, citation extraction

**Plan:** Build RAG system with academic papers, implement memory for preferences

---

### Idea 2: Code Review Agent
**What it does:** Reviews pull requests, checks for bugs, suggests improvements

**Why it's cool:** Automates tedious but important task

**Need to learn:** AST parsing, code analysis tools integration

**Plan:** Multi-agent: analyzer → security checker → style reviewer

---

## Community stuff

### Discussion: When to use multi-agent vs single agent?

**Points made:**
- Multi-agent for complex, decomposable tasks
- Single agent for simple, focused queries
- Overhead of coordination only worth it for substantial complexity

**What I think:** 
Start simple, add agents only when specialization provides clear value.

---

## Questions to figure out later

1. How to implement true learning in agents (not just retrieval)?
2. What's the scalability limit of multi-agent systems?
3. Can agents learn to optimize their own prompts?
4. How to handle multi-step planning reliably?
5. What are the long-term implications of agent memory and privacy?

---

## How my thinking evolved

### Beginning
**What I thought:**
AI agents are just better chatbots. Prompt engineering is the main skill needed.

### Midway
**How it changed:**
Realized agents are systems, not just models. Architecture, tools, memory, testing matter as much as prompts.

### Now
**Current understanding:**
Building production agents is software engineering + ML. Need strong fundamentals in both. The model is one component in a larger system.

**Main shift:**
From "how do I prompt this?" to "how do I architect this system for reliability, cost, and scale?"

---

## What's next

### This week
- Polish capstone project
- Add comprehensive documentation
- Optimize for cost/performance

### This month
- Build personal agent project applying learnings
- Contribute to MCP ecosystem
- Explore agent deployment platforms

### Eventually
- Deep dive into agent planning algorithms
- Research agent safety and alignment
- Build production agent for real users

---

**Last update:** November 15, 2025  
**Challenge status:** Complete ✅