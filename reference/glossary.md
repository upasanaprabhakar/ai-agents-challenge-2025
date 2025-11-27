# Terms & Definitions

Words and concepts I learned throughout the 5-Day AI Agents Challenge.

---

## Basic Stuff I Already Know

**AI (Artificial Intelligence)**  
Computer systems that can do tasks that usually need human intelligence

**LLM (Large Language Model)**  
AI trained on tons of text data. Like GPT, Gemini, Claude, etc.

**API (Application Programming Interface)**  
Way for different programs to talk to each other. How I'll use Gemini.

**Prompt**  
The text you give to an AI to tell it what to do

**Token**  
Small chunks of text the AI processes. Usually ~4 characters per token

---

## Agent-Specific Terms

**AI Agent**  
A system that can perceive its environment, make decisions, and take autonomous actions. Unlike chatbots that just respond, agents can use tools, plan sequences of actions, and work towards goals independently.

**Multi-agent system**  
Architecture where multiple specialized agents work together, each handling specific tasks. Like a team where each member has a defined role (researcher, writer, reviewer, etc.) and they coordinate to solve complex problems.

**Tool use / Function calling**  
Ability for agents to execute external functions or APIs. The agent decides WHEN and HOW to call tools based on the user's query. For example, calling a calculator for math or a search API for current information.

**RAG (Retrieval Augmented Generation)**  
Technique where agents access external knowledge bases before responding. Instead of relying only on training data, agents retrieve relevant documents and use them to generate informed responses. Like giving the agent a library to reference.

**Agent orchestration**  
Coordinating multiple agents to work together on complex tasks. Patterns include sequential (one after another), parallel (simultaneously), hierarchical (nested teams), and supervisor (one coordinating many).

**Context window**  
The maximum amount of text (measured in tokens) an AI model can process at once. For Gemini, this is around 32k-128k tokens depending on the model. Think of it as the model's working memory.

**ADK (Agent Development Kit)**  
Google's framework for building production-ready AI agents. Provides tools for agent creation, orchestration, tool integration, and deployment.

**Agentic loop**  
The cycle agents follow: Perceive (gather input) → Reason (decide what to do) → Act (execute actions) → Observe (see results) → Repeat. This continuous cycle enables autonomous behavior.

**System instruction**  
The foundational prompt that defines an agent's role, personality, and behavioral guidelines. Like a job description that shapes all agent responses.

---

## Technical Terms

**Jupyter Notebook**  
Interactive coding environment. What we're using on Kaggle. Allows mixing code, outputs, and documentation in one document.

**Virtual environment (venv)**  
Isolated Python setup so packages don't conflict. Each project gets its own dependencies.

**Git / GitHub**  
Version control. Tracking changes to code and collaborating with others.

**Environment variables**  
Secret values (like API keys) stored outside code. Keeps sensitive data secure and separate from source code.

**MCP (Model Context Protocol)**  
Standardized protocol for connecting AI agents to external data sources and tools. Like USB for AI - one interface, many tools. Enables agents to access databases, file systems, APIs, etc.

**Vector database**  
Database that stores data as mathematical vectors (embeddings). Enables semantic search - finding conceptually similar items rather than just keyword matches. Examples: ChromaDB, Pinecone, Weaviate.

**Embedding**  
Mathematical representation of text as a vector (list of numbers). Similar meanings produce similar vectors, enabling semantic search and comparison.

**Schema**  
Definition of how data or functions are structured. For tools, schemas describe parameters, types, and what the tool does. Clear schemas help agents use tools correctly.

---

## Day 1 Terms: Agent Fundamentals

**Gemini API**  
Google's interface for accessing Gemini language models programmatically.

**GenerativeModel**  
The core class in Gemini API for creating AI model instances.

**generate_content()**  
Method to get a one-time response from the model.

**start_chat()**  
Method to begin a multi-turn conversation with context retention.

**Function declaration**  
How you define tools/functions that agents can call, including parameters and descriptions.

---

## Day 2 Terms: Multi-Agent Architecture

**MCP Server**  
A service that implements the Model Context Protocol, providing specific tools or data access to agents.

**Orchestrator**  
Agent responsible for coordinating other agents, delegating tasks, and synthesizing results.

**Supervisor pattern**  
Architecture where one agent manages and delegates to multiple specialist agents.

**Sequential orchestration**  
Agents work one after another, each building on previous results.

**Parallel orchestration**  
Multiple agents work simultaneously on independent tasks.

**Hierarchical orchestration**  
Nested structure with supervisors managing teams of agents.

**Agent handoff**  
Passing context and control from one agent to another in a workflow.

**Tool interoperability**  
Ability for different agents to share and use the same tools through standard interfaces like MCP.

---

## Day 3 Terms: Tools & Memory

**Short-term memory**  
Recent conversation history stored in context window. Fast but limited by token limits.

**Long-term memory**  
Persistent storage of information across sessions, typically in vector databases.

**Semantic memory**  
Knowledge about facts and concepts, stored and retrieved based on meaning rather than exact wording.

**Episodic memory**  
Memory of specific events or interactions, timestamped and contextual.

**Context management**  
Strategy for deciding what information to keep in the context window vs retrieve from storage.

**Chunking**  
Breaking long documents into smaller pieces for processing and storage. Important for RAG systems.

**Similarity search**  
Finding items in a vector database that are semantically similar to a query.

**Retrieval strategy**  
Algorithm for deciding which historical context to bring into current conversation.

**Function signature**  
The definition of a function including its name, parameters, types, and return value.

**Tool schema**  
Detailed description of what a tool does and how to use it, helping agents make informed decisions.

---

## Day 4 Terms: Evaluation & Testing

**LLM-as-judge**  
Using one language model to evaluate outputs from another model. Enables scalable automated testing.

**Golden dataset**  
Curated collection of test cases with known good outputs, used for regression testing.

**Evaluation criteria**  
Specific dimensions to assess (accuracy, relevance, safety, coherence, etc.).

**Hallucination**  
When an AI confidently generates incorrect or fabricated information.

**Edge case**  
Unusual or extreme scenario that might cause unexpected behavior.

**Regression testing**  
Re-running tests after changes to ensure existing functionality still works.

**Latency**  
Time delay between sending a request and receiving a response. Critical for user experience.

**P50/P95 latency**  
Statistical measures: 50% and 95% of requests complete faster than this time.

**Token usage**  
Number of tokens consumed by a request, determines cost and context window usage.

**Fallback strategy**  
Backup plan when primary approach fails (e.g., switching to simpler model).

**Graceful degradation**  
System continues functioning with reduced capability rather than failing completely.

**Observability**  
Ability to understand system behavior through logging, metrics, and monitoring.

**Safety guardrails**  
Mechanisms to prevent harmful outputs (content filtering, ethical guidelines, etc.).

---

## Day 5 Terms: Production & Deployment

**Production-ready**  
Software that's tested, optimized, and reliable enough for real users.

**Cost optimization**  
Strategies to minimize API usage costs while maintaining quality.

**Model selection**  
Choosing the right model (Flash vs Pro) for each task based on complexity and cost.

**Error handling**  
Code that gracefully manages failures and unexpected situations.

**Rate limiting**  
Restrictions on how many requests can be made in a time period.

**Exponential backoff**  
Retry strategy where wait time increases exponentially after each failure.

**A/B testing**  
Comparing two versions of an agent to determine which performs better.

**Monitoring**  
Continuous tracking of agent performance, errors, and usage in production.

**Incident response**  
Procedures for handling production failures quickly and effectively.

---

## Acronyms I Keep Seeing

**ADK** - Agent Development Kit  
**API** - Application Programming Interface  
**LLM** - Large Language Model  
**MCP** - Model Context Protocol  
**RAG** - Retrieval Augmented Generation  
**SDK** - Software Development Kit  
**REST** - Representational State Transfer (API type)  
**CRUD** - Create, Read, Update, Delete (database operations)  
**JSON** - JavaScript Object Notation (data format)  
**SQL** - Structured Query Language  
**CLI** - Command Line Interface  
**IDE** - Integrated Development Environment  
**P50/P95** - 50th/95th percentile (statistics)  
**PII** - Personally Identifiable Information  

---

## Advanced Concepts

**Prompt engineering**  
Art and science of crafting effective prompts to get desired agent behavior.

**Few-shot learning**  
Providing examples in the prompt to guide agent responses.

**Chain-of-thought**  
Prompting technique where agent explains its reasoning step-by-step.

**Temperature**  
Parameter controlling randomness in responses (0 = deterministic, 1 = creative).

**Top-k / Top-p sampling**  
Methods for controlling diversity in generated text.

**Constraint satisfaction**  
Ensuring agent outputs meet specific requirements or limitations.

**Autonomous agents**  
Agents that can operate independently without human intervention.

**Human-in-the-loop**  
Design where humans review or approve critical agent decisions.

---

## Common Patterns

**Researcher-Writer-Reviewer**  
Multi-agent pattern: one gathers info, one creates content, one checks quality.

**Try-Catch-Fallback**  
Error handling pattern: attempt primary approach, catch errors, use backup.

**Retrieve-Then-Generate**  
RAG pattern: search knowledge base first, then generate response using findings.

**Decompose-Solve-Synthesize**  
Break complex problem into parts, solve each, combine results.

**Validate-Execute-Verify**  
Safety pattern: check input is safe, perform action, confirm expected outcome.

---

Last updated: November 15, 2025  
Completed during Google x Kaggle 5-Day AI Agents Intensive