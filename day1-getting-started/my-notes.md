# Day 1: Getting Started with AI Agents 🚀

**Date:** November 11, 2025   
**Session Time:** 12:30 AM IST (Nov 12, 2025)     
**Status:** ✅ Completed

## 🎯 Today's Goals
- [x] Understand what AI agents actually are
- [x] Get familiar with the Gemini API
- [x] Build my first simple agent
- [x] Complete the Kaggle notebook exercises

## 📝 Session Notes

### What I Learned Today

**Main Concepts:**
- AI agents are systems that can perceive their environment, make decisions, and take actions autonomously
- The Agent Development Kit (ADK) is Google's framework for building production-ready AI agents
- Agents differ from traditional chatbots by their ability to use tools, plan actions, and maintain context
- Gemini API provides the foundation model that powers agent reasoning and decision-making
- Function calling enables agents to interact with external tools and APIs

**Key Takeaways:**
- Setting up a basic agent requires configuring the Gemini model with appropriate instructions and tools
- Agents can be given specific roles and personas to guide their behavior and responses
- Google Search integration allows agents to access real-time information beyond their training data
- Multi-agent systems enable specialized agents to collaborate on complex tasks
- Proper prompt engineering is crucial for agent reliability and accuracy

**Things That Clicked:**
- The concept of "tool use" - agents can decide WHEN and HOW to use external functions based on user queries
- Multi-agent architectures are like having a team of specialists working together, each handling their domain
- The agent loop: perceive → reason → act → observe → repeat
- ADK abstracts away complexity while still giving fine-grained control when needed

**Things I'm Still Confused About:**
- How to optimize agent prompts for better decision-making in edge cases
- Best practices for error handling when tools fail or return unexpected results
- How to balance agent autonomy with safety constraints in production environments

## 💻 Code Snippets & Examples

### Setting Up Gemini API
```python
import google.generativeai as genai
from google.generativeai import types

# Configure API
genai.configure(api_key=api_key)

# Initialize model with system instructions
model = genai.GenerativeModel(
    'gemini-1.5-flash',
    system_instruction="You are a helpful AI agent assistant."
)

# Generate response
response = model.generate_content("Hello!")
print(response.text)
```

### First Agent with Tool Use
```python
# Define a simple tool
def get_weather(location: str) -> str:
    """Get current weather for a location."""
    return f"The weather in {location} is sunny, 72°F"

# Create agent with tools
agent = genai.GenerativeModel(
    'gemini-1.5-flash',
    tools=[get_weather]
)

# Agent decides when to use the tool
response = agent.generate_content("What's the weather in San Francisco?")
```

### Multi-Agent Setup
```python
# Researcher agent
researcher = genai.GenerativeModel(
    'gemini-1.5-flash',
    system_instruction="You are a research specialist. Find and verify information."
)

# Writer agent
writer = genai.GenerativeModel(
    'gemini-1.5-flash',
    system_instruction="You are a content writer. Create engaging articles."
)

# Coordinator orchestrates the workflow
# Researcher gathers info → Writer creates content
```

## 🔧 Hands-On Practice

**What I Built:**
- Basic conversational agent using Gemini API
- Agent with Google Search capability for real-time information retrieval
- Multi-agent system with specialized researcher and writer agents
- Function calling implementation for weather lookup

**Problems I Ran Into:**
- Initial confusion about when to use `generate_content` vs `start_chat` for conversations
- Tool function signatures needed to be properly typed for ADK to understand them
- Managing context across multiple agent interactions required careful state handling

**How I Fixed Them:**
- Read ADK documentation on conversation management and session handling
- Added proper type hints and docstrings to all tool functions
- Implemented simple state management using Python dictionaries for agent memory

## 💡 Aha Moments

- **Agents are autonomous!** Unlike traditional programs where I call specific functions, agents *decide* which tools to use based on the query. This is the key difference from rule-based systems.

- **Function calling is declarative, not imperative!** I describe what a function *does* (via docstrings), and the agent figures out when to call it. Mind blown! 🤯

- **Multi-agent systems aren't just multiple models running.** It's about orchestration - having specialized agents with clear roles that pass information between each other. Like a real team!

- **System instructions are incredibly powerful.** A well-crafted system prompt can completely change agent behavior and reliability.

## 🤔 Questions for Later

- [ ] How do I implement memory that persists across multiple sessions?
- [ ] What are the cost implications of running multi-agent systems at scale?
- [ ] How can I evaluate agent performance systematically?
- [ ] What's the best way to handle sensitive data when agents use external tools?
- [ ] Can agents learn from their interactions and improve over time?

## 🔗 Useful Links & Resources

- [Gemini API Documentation](https://ai.google.dev/docs) - Comprehensive guide to all features
- [ADK GitHub Repository](https://github.com/google/adk) - Examples and code samples
- [Function Calling Guide](https://ai.google.dev/docs/function_calling) - Deep dive into tool use
- [Agent Design Patterns](https://www.kaggle.com/learn-guide/5-day-agents) - Official course materials

## 📸 Screenshots & Diagrams

Check `media/screenshots/day1/` for:
- Agent responding to user queries with tool use
- Multi-agent workflow diagram
- Console output showing function calling in action

## 🎯 Tomorrow's Focus

Based on today, here's what I want to explore next:
- Dive deeper into multi-agent orchestration patterns (supervisor, hierarchical, sequential)
- Learn about agent memory systems and state management
- Understand how to architect scalable agent systems
- Explore more complex tool integrations beyond simple function calls

## ⏱️ Time Spent

- Codelab 1a (First Agent): 1 hour
- Codelab 1b (Multi-Agent): 1.5 hours
- Experimentation: 30 mins
- Documentation: 30 mins
- Total: 3.5 hours

## 🌟 Personal Thoughts

Today was eye-opening! The shift from thinking about "calling APIs" to "agents deciding what to do" is a fundamental change in how I approach building AI systems. 

The most exciting part was seeing the multi-agent system in action - watching specialized agents collaborate felt like witnessing a real team at work. It's not just about chaining prompts; it's about designing intelligent systems with clear responsibilities.

The ADK makes a lot of complex concepts accessible, but I can tell there's significant depth here. Looking forward to understanding the architectural patterns better in Day 2.

Challenge: Balancing agent autonomy with predictability will be crucial for production systems. Need to learn more about testing and evaluation strategies.

**Next up:** Day 2 - Architecture 🏗️