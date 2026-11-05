# 10 · Agent Memory

There are four types of memory you need to know to architect memory for an AI agent.

## Short-term memory

Every user and agent message gets appended to a messages array, and that array is sent to the model with every new request. The agent isn't remembering anything. It re-reads the full conversation every time.

<p align="center">
  <img src="images/short-term-memory.png" alt="Short-term memory: user messages and agent replies are appended to a messages array, which goes into the context window as short-term memory alongside the new message." width="500">
</p>

The problem is that the context window grows with every message, and eventually you hit the model's limit. So you either drop older messages or summarize them.

<p align="center">
  <img src="images/limit-reached.png" alt="Claude error message: you've reached the limit for Claude messages at this time, please wait before trying again." width="500">
</p>

This works well for a single session. For anything beyond that, you need long-term memory.

## Long-term memory

There are three types of long-term memory.

### Semantic memory

Instead of storing raw messages, you use an LLM to extract meaningful facts, like preferences, skills and goals. You convert them into embeddings and store them in a vector database.

<p align="center">
  <img src="images/semantic-store.png" alt="Storing semantic memory: the user message goes to an LLM that extracts facts such as people, companies, places, numbers, dates, likes, dislikes, skills and goals, and stores them in a vector database." width="500">
</p>

When the user comes back, you run a similarity search, pull the relevant facts and inject them as context.

<p align="center">
  <img src="images/semantic-retrieve.png" alt="Retrieving semantic memory: a similarity search between the new user message and the vector database pulls relevant facts, which go into the context window with the new message." width="500">
</p>

This sounds like RAG, and the implementation is similar, but the role is completely different.

| Aspect         | RAG                              | Semantic memory                     |
| -------------- | -------------------------------- | ----------------------------------- |
| Nature         | Static                           | Dynamic                             |
| Data source    | Loaded once from docs            | Written mid-conversation            |
| Update trigger | Manual reload                    | Agent learns as it talks            |
| What it stores | Knowledge base content           | User-specific context               |
| Access pattern | Query for facts                  | Recalled automatically next session |
| Role           | Answers questions from knowledge | Keeps the agent coherent over time  |
| Think of it as | A library                        | A diary                             |

### Episodic memory

Instead of facts, episodic memory stores events. Keywords like "attended", "met" and "experienced" signal the LLM to extract and store an episode before answering. It captures what happened, when, with whom and the outcome.

Each episode is stored as a timestamped JSON key-value pair.

<p align="center">
  <img src="images/episodic-store.png" alt="Storing episodic memory: the LLM extracts only the main episode from the user message, covering when, what and who, and stores it with a timestamp in MongoDB or Redis." width="500">
</p>

Retrieval works differently here. Besides searching by similarity, you can filter by time.

<p align="center">
  <img src="images/episodic-retrieve.png" alt="Retrieving episodic memory: the new user message is matched against MongoDB or Redis by filter or similarity, and the retrieved context goes into the context window with the new message." width="500">
</p>

Semantic memory answers who the user is. Episodic memory answers what happened and when.

### Procedural memory

Procedural memory stores how to do things: the workflows and step-by-step processes the user prefers. The only difference from semantic memory is the extraction prompt. Instead of pulling facts, you pull procedures, the steps involved and when to use each one. Everything else works like semantic memory.

<p align="center">
  <img src="images/procedural.png" alt="Procedural memory: the LLM extracts workflows, step-by-step processes and procedures and stores them in a vector database. A similarity search later pulls relevant workflows into the context window with the new message." width="500">
</p>
