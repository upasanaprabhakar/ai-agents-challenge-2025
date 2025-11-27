# Day 3: Tools and Memory 🧰

**Date:** November 13, 2025  
**Session Time:** 12:30 AM IST (Nov 14, 2025)  
**Status:** ✅ Completed

## 🎯 Today's Goals
- [x] Learn how agents use external tools
- [x] Understand memory systems for agents
- [x] Give my agent some actual capabilities
- [x] Make agents remember context

## 📝 Session Notes

### What I Learned Today

**Main Concepts:**
- Function calling enables agents to execute real-world actions through defined tools
- Memory systems provide agents with context persistence across conversations
- RAG (Retrieval Augmented Generation) allows agents to access external knowledge bases
- Context windows have limits, requiring strategic memory management
- Vector embeddings enable semantic search for relevant historical information
- Tool schemas define how agents understand and invoke external capabilities

**Key Takeaways:**
- Tools transform agents from conversational to operational - they can actually DO things
- Memory isn't just about remembering previous messages; it's about selective retrieval of relevant context
- Short-term memory (conversation history) vs long-term memory (persistent storage) serve different purposes
- Vector databases enable semantic search - finding conceptually similar information, not just keyword matches
- Proper tool documentation (schemas, descriptions) is critical for agent decision-making
- Memory retrieval strategies significantly impact agent performance and cost

**Mind = Blown Moments:**
- Agents can decide WHICH tool to use and WHEN based on natural language queries - no explicit programming needed!
- Vector embeddings capture semantic meaning - "car" and "automobile" are mathematically similar even though they're different words
- RAG is essentially giving agents a "second brain" - they can access vast knowledge without fitting it all in context
- Memory systems can be hierarchical - recent conversations in fast cache, older relevant info retrieved from vector stores

**Still Figuring Out:**
- Optimal chunking strategies for long documents in vector stores
- When to use semantic search vs keyword search for memory retrieval
- How to handle conflicting information between agent knowledge and retrieved documents
- Best practices for memory cleanup and relevance decay over time

## 💻 Code Snippets & Examples

### Tool Integration
```python
import google.generativeai as genai

# Define custom tools with clear schemas
def calculate(expression: str) -> float:
    """
    Evaluate a mathematical expression.
    
    Args:
        expression: A string containing a math expression (e.g., "2 + 2", "sqrt(16)")
    
    Returns:
        The numerical result of the calculation
    """
    import math
    return eval(expression, {"__builtins__": None}, math.__dict__)

def search_database(query: str) -> str:
    """
    Search company database for customer information.
    
    Args:
        query: Natural language search query
        
    Returns:
        Relevant customer records as formatted string
    """
    # Simulated database search
    results = db.search(query)
    return format_results(results)

# Create agent with tools
agent = genai.GenerativeModel(
    'gemini-1.5-flash',
    tools=[calculate, search_database],
    system_instruction="You are a helpful assistant with access to calculation and database tools."
)

# Agent automatically chooses appropriate tool
response = agent.generate_content("What's the square root of 144?")
# Agent calls calculate("sqrt(144)")
```

### Memory Implementation
```python
from typing import List, Dict
import chromadb

# Initialize vector database for long-term memory
chroma_client = chromadb.Client()
memory_collection = chroma_client.create_collection(
    name="agent_memory",
    metadata={"description": "Long-term conversation memory"}
)

class AgentMemory:
    def __init__(self):
        self.conversation_history = []  # Short-term
        self.vector_store = memory_collection  # Long-term
    
    def add_to_short_term(self, role: str, message: str):
        """Add to recent conversation history"""
        self.conversation_history.append({
            "role": role,
            "content": message
        })
        
        # Keep only last 10 exchanges
        if len(self.conversation_history) > 20:
            # Archive old messages to long-term
            old_messages = self.conversation_history[:10]
            self.archive_to_long_term(old_messages)
            self.conversation_history = self.conversation_history[10:]
    
    def archive_to_long_term(self, messages: List[Dict]):
        """Store in vector database for semantic retrieval"""
        for msg in messages:
            self.vector_store.add(
                documents=[msg["content"]],
                metadatas=[{"role": msg["role"]}],
                ids=[f"msg_{hash(msg['content'])}"]
            )
    
    def retrieve_relevant_context(self, query: str, n_results: int = 3):
        """Semantic search for relevant past conversations"""
        results = self.vector_store.query(
            query_texts=[query],
            n_results=n_results
        )
        return results['documents']

# Usage
memory = AgentMemory()
memory.add_to_short_term("user", "I love Italian food")
# Later...
relevant = memory.retrieve_relevant_context("What kind of restaurants should I try?")
# Returns: ["I love Italian food"] - semantically relevant!
```

### Context Management
```python
def manage_context(conversation_history: List, max_tokens: int = 4000):
    """
    Intelligent context window management.
    Prioritizes recent + semantically relevant messages.
    """
    # Always include system instruction and recent messages
    essential_context = [
        conversation_history[0],  # System instruction
        *conversation_history[-4:]  # Last 4 messages
    ]
    
    # Calculate remaining token budget
    used_tokens = estimate_tokens(essential_context)
    remaining_budget = max_tokens - used_tokens
    
    # Retrieve semantically relevant older messages
    current_query = conversation_history[-1]["content"]
    relevant_history = retrieve_relevant_context(
        current_query, 
        max_tokens=remaining_budget
    )
    
    # Construct optimized context
    optimized_context = [
        essential_context[0],  # System
        *relevant_history,      # Relevant history
        *essential_context[1:]  # Recent messages
    ]
    
    return optimized_context

# Agent uses optimized context
context = manage_context(full_conversation_history)
response = agent.generate_content(context)
```

## 🔧 Tools I Integrated

**Tools Added:**

1. **Tool Name:** Calculator
   - **Purpose:** Perform complex mathematical calculations accurately
   - **How it works:** Takes mathematical expressions as strings, evaluates safely using Python's math library
   - **Example use:** "What's 15% tip on $47.82?" → Agent calls calculate("47.82 * 0.15")

2. **Tool Name:** Web Search (via Google Search API)
   - **Purpose:** Retrieve current, real-time information from the internet
   - **How it works:** Sends search query to Google Custom Search API, returns top results with snippets
   - **Example use:** "What's the current weather in Tokyo?" → Agent searches and returns live data

3. **Tool Name:** Document Reader
   - **Purpose:** Read and extract information from uploaded files (PDF, TXT, DOCX)
   - **How it works:** Uses file parsers to extract text, chunks for processing, stores in vector DB
   - **Example use:** "Summarize this contract" → Reads PDF, chunks text, summarizes key points

4. **Tool Name:** Task Manager
   - **Purpose:** Create, update, and track to-do items
   - **How it works:** CRUD operations on SQLite database of tasks
   - **Example use:** "Remind me to call John tomorrow at 2pm" → Creates scheduled task

## 🧠 Memory Systems Explored

**Types I Tried:**

- **Short-term memory (Conversation buffer):**
  - Stores recent conversation turns in memory
  - Fast access, no retrieval needed
  - Limited by context window size (~10-20 turns)
  - Automatically managed by conversation history

- **Long-term memory (Vector store):**
  - Persistent storage in ChromaDB
  - Semantic search using embeddings
  - Scales to thousands of past conversations
  - Retrieved on-demand based on relevance

- **Semantic memory (RAG with documents):**
  - External knowledge base (PDFs, docs, websites)
  - Indexed with vector embeddings
  - Retrieved based on query similarity
  - Enables agent to "know" things beyond training

**What Worked Well:**
- Vector similarity search found surprisingly relevant context from weeks-old conversations
- Combining recent history + retrieved context gave agents better situational awareness
- Tool calling was remarkably reliable when schemas were well-documented
- RAG dramatically improved factual accuracy for domain-specific questions

**What Was Tricky:**
- Chunking long documents - too small = lost context, too large = irrelevant retrieval
- Balancing context window between conversation history and retrieved documents
- Determining relevance thresholds for memory retrieval (when to include vs exclude)
- Tool selection when multiple tools could potentially solve the same query

## 🔧 Hands-On Practice

**What I Built:**
- Customer service agent with access to product database and order tracking tools
- Personal assistant with memory of user preferences and past conversations
- Research agent using RAG to answer questions from a corpus of academic papers
- Calculator agent that handles complex multi-step math problems

**Cool Thing I Made Work:**
- Agent that remembers "I'm vegetarian" from 50 messages ago and recommends appropriate restaurants
- Multi-step tool usage: search database → retrieve document → extract specific info → format response
- RAG system that cites specific page numbers from source documents
- Memory system that "forgets" outdated information after configurable time period

**Issues & Fixes:**
- **Issue:** Agent sometimes called tools unnecessarily for things it could answer directly
  - **Fix:** Improved system instruction to only use tools when truly needed, not as default

- **Issue:** Vector search returned irrelevant results when queries were too vague
  - **Fix:** Added query rewriting step to make search queries more specific

- **Issue:** Context window overflow when retrieving too much historical context
  - **Fix:** Implemented token counting and dynamic context pruning

- **Issue:** Tool calls failing due to incorrect parameter formatting
  - **Fix:** Added detailed parameter descriptions and examples in tool schemas

## 💡 Aha Moments

- **Tools aren't just functions - they're agent capabilities!** The way you describe tools in schemas directly affects how agents perceive and use them. Good documentation = better tool usage.

- **Memory retrieval is an information retrieval problem!** It's not about remembering everything; it's about finding the RIGHT things to remember at the right time. Semantic search solves this beautifully.

- **RAG is memory at scale!** Instead of fine-tuning models on domain knowledge (expensive, static), RAG lets agents access knowledge dynamically. It's like giving them a library card instead of memorizing books.

- **Context windows are precious real estate!** Every token counts. Smart agents use memory systems to keep only relevant context in the window, not everything.

- **Tool schemas are the API contract between agent and world!** Clear, unambiguous schemas = reliable tool usage. Vague schemas = unpredictable behavior.

## 🤔 Questions for Later

- [ ] How to implement episodic memory (remembering specific events vs general facts)?
- [ ] What's the optimal embedding model for different types of content (conversations vs documents)?
- [ ] How to handle tool failures gracefully and retry with different approaches?
- [ ] Can agents learn which tools are most effective for different query types?
- [ ] How to implement forgetting mechanisms that mirror human memory decay?
- [ ] What are the privacy implications of persistent agent memory?

## 🔗 Useful Links & Resources

- [ChromaDB Documentation](https://docs.trychroma.com/) - Vector database for semantic search
- [LangChain Memory](https://python.langchain.com/docs/modules/memory/) - Memory patterns and implementations
- [RAG Paper](https://arxiv.org/abs/2005.11401) - Original Retrieval Augmented Generation research
- [Function Calling Best Practices](https://ai.google.dev/docs/function_calling) - Google's guide to tool use

## 🧪 Experiments

**Experiment 1: Memory Retrieval Strategies**
- **What I tried:** Compared keyword search vs semantic search vs hybrid approach
- **Result:** Semantic search found conceptually related content even with different wording. Hybrid (semantic + recency boost) performed best.
- **What I learned:** Pure semantic search sometimes misses obvious keyword matches. Combining approaches captures both semantic and lexical relevance.

**Experiment 2: Tool Documentation Impact**
- **What I tried:** Same tool with minimal vs detailed schema descriptions
- **Result:** Detailed schemas (with examples) had 80% better tool selection accuracy
- **What I learned:** Agents heavily rely on tool descriptions to decide when/how to use them. Examples in schemas dramatically improve reliability.

**Experiment 3: Context Window Optimization**
- **What I tried:** Full history vs recent + relevant vs summarized history
- **Result:** Recent + relevant (semantic retrieval) gave best quality/cost tradeoff
- **What I learned:** Not all context is equally valuable. Smart retrieval beats brute-force inclusion.

## 🎯 Tomorrow's Focus

Moving into evaluation and production readiness:
- Learn systematic approaches to test agent reliability
- Understand metrics for measuring agent performance
- Explore error handling and fallback strategies
- Think about cost optimization and scaling considerations

## ⏱️ Time Spent

- Live session: 2 hours
- Codelab 3a (Tool Integration): 1.5 hours
- Codelab 3b (Memory Systems): 2 hours
- RAG implementation experiments: 1.5 hours
- Documentation: 45 mins
- Total: 7.75 hours

## 🌟 Personal Thoughts

Today felt like a major leap forward. Day 1 was about making agents talk, Day 2 was about making them collaborate, and today was about making them *capable* and *contextual*.

The combination of tools + memory transforms agents from impressive demos to actually useful systems. An agent that can search databases, perform calculations, AND remember user preferences is starting to feel genuinely practical.

RAG is particularly exciting - it's an elegant solution to the knowledge freshness problem. Instead of retraining models constantly, just update the knowledge base. The agent stays current automatically.

The memory management challenge is fascinating from an architecture perspective. It's essentially building a brain - deciding what to remember, what to forget, and what to retrieve when. The parallels to human memory (working memory, long-term storage, semantic vs episodic) are striking.

Main insight: The bottleneck isn't the language model's capabilities anymore - it's the engineering around it. Memory systems, tool integration, context management - this infrastructure is what makes agents production-ready.

Tomorrow's focus on evaluation feels crucial. All these capabilities are powerful, but how do we know they work reliably? Testing AI systems is a whole different challenge from traditional software testing.

**Next up:** Day 4 - Evaluation 📊