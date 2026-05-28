---
layout: post
title: "From Generational Memory to an Agent Memory OS: Lifecycle Governance for Long-Term Memory"
date: 2026-05-28 21:55:00 +1000
categories: ai-agent memory architecture
lang: en
description: "A lifecycle governance framework for Agent Memory OS: storage, retrieval, promotion, compression, archiving, forgetting, and skill formation."
translation_key: agent-memory-os-lifecycle-governance
permalink: /posts/agent-memory-os-lifecycle-governance-en/
---

# From Generational Memory to an Agent Memory OS: Lifecycle Governance for Long-Term Memory

In the previous essay, I used JVM generational garbage collection as an analogy for long-term memory in agents. The core idea was simple: an agent's memory should not merely "store things." It should behave like a system with a lifecycle. New information first enters a short-term area, where it is observed, used, and verified. Information that proves to have long-term value is then compressed, promoted, and consolidated. Repeated procedures should be further organized into skills. Outdated, incorrect, or low-value information should be archived or forgotten.

This essay continues one step further.

If the previous essay was more like a design intuition about why agent memory should be generational, this one asks a more concrete set of questions: how does this idea relate to existing research on agent memory? How is it different from RAG, vector databases, long-term memory stores, and file-based memory? Compared with existing systems such as OpenClaw, Hermes, Claude Code, Letta/MemGPT, and Mem0/OpenMemory, what layer does it actually add? And if we really wanted to implement a generational memory system, what might the engineering shape look like?

The more I think about long-term agent memory, the more I feel that the hard problem is not saving information. The hard problem is governing information.

It is not merely a database problem. It is a Memory OS problem.

## 1. Agent Memory Is Not Just "Storage"; It Is "Management"

When many people talk about agent memory, their first reaction is: save the chat history, or put it into a vector database and retrieve it when needed.

This is certainly useful. Without persistence, the agent behaves as if every session were the first meeting. Without retrieval, historical information may exist somewhere, but it is still difficult to bring it back into the current task.

But this only solves half of the problem.

The core question of long-term memory is not only:

```text
Can this piece of information be saved?
```

It is also:

```text
Is this piece of information worth saving?
How long should it be saved?
Does it belong to the current task, a specific project, or global long-term preference?
Is it a fact, a preference, a procedure, a failure lesson, or a skill candidate?
Has it expired?
Does it conflict with newer information?
Should it be compressed?
Should it be promoted?
Should it be forgotten?
```

Without these judgments, a memory system easily becomes an ever-growing warehouse. It appears to remember everything, but when the agent actually needs something, it becomes more likely to be distracted by noise.

A person does not become smarter by remembering every detail. A person becomes smarter by extracting experience from events, forgetting useless noise, preserving important patterns, and turning repeated actions into abilities.

For this reason, a mature agent memory system should not be merely memory storage. It should be memory management.

## 2. Existing Research Is Already Moving Toward "Memory Management"

In recent years, a lot of research on LLM agent memory has approached this issue from different angles.

MemGPT is a typical example. It compares the LLM to an operating system, using layered memory and virtual context management to address the limitation of finite context windows. Its concern is: which information should remain in the current context, which information should be placed in external memory, and when should external information be brought back into the current context?

This is similar to operating-system memory management, and it also overlaps with my earlier analogy to JVM generational garbage collection. The difference is that MemGPT focuses more on how to extend the context window, while generational memory focuses more on how memory is filtered, promoted, and consolidated through use.

MemoryBank is closer to human long-term memory. It not only allows the model to recall relevant memories from historical interactions, but also introduces an idea similar to a forgetting curve: memories decay over time, while important memories can be reinforced. This is crucial, because it suggests that agents should not mechanically preserve all information forever. They should be allowed to forget.

The memory stream in Generative Agents is also highly instructive. It does not simply retrieve the most recent memories in chronological order. Instead, retrieval jointly considers recency, importance, and relevance. In other words, whether a memory should be recalled depends not only on whether it happened recently, but also on whether it matters in the current situation.

A-MEM further emphasizes memory organization. It borrows from the Zettelkasten note-taking method, turning memories into a knowledge network with tags, keywords, contextual descriptions, and associations. Memory is no longer a set of isolated entries. It becomes a structure that can be linked, updated, and reorganized.

Newer work such as AgeMem turns memory operations themselves into actions that an agent can actively choose. The agent does not merely passively use an external memory store. It can decide when to store, when to retrieve, when to update, when to summarize, and when to discard.

These lines of research differ in emphasis, but they point to the same thing: agent memory is not simply "storing history." It is a continuously changing management system.

## 3. What Have Existing Agent Systems Already Achieved?

If generational memory is discussed only at the conceptual level, it can easily sound like an abstract design. In practice, however, many agent projects have already implemented similar capabilities to different degrees. They may not explicitly use the term "generational memory," but they have separately implemented file memory, project memory, session retrieval, automatic summarization, background consolidation, external memory providers, and related mechanisms.

So the more accurate statement is not that generational memory should invent an entirely new memory system from scratch.

Instead, it is more like a reorganization and reinterpretation of existing agent memory mechanisms.

### OpenClaw: The Engineering Implementation Closest to Generational Memory

OpenClaw's memory mechanism is very close to generational memory. It uses Markdown files as the source of truth for memory. `MEMORY.md` stores long-term facts, preferences, and important decisions. `memory/YYYY-MM-DD.md` stores daily records. `DREAMS.md` stores summaries produced by dreaming sweeps, making them easier for humans to review.

This design already resembles:

```text
MEMORY.md
= long-term memory / old generation

memory/YYYY-MM-DD.md
= daily memory / young generation

DREAMS.md
= memory consolidation log / GC log
```

OpenClaw goes further by not only saving files, but also providing memory search. It indexes memory files through keyword search, vector search, and hybrid retrieval. In other words, it preserves the readability of Markdown while adding the recall capabilities of databases and vector retrieval.

OpenClaw's dreaming mechanism is especially worth noting. It extracts strong signals from short-term memory, processes them through different stages, and promotes sufficiently important content into `MEMORY.md`. This is already very close to what I call memory GC and memory consolidation.

However, OpenClaw's emphasis still leans toward "workspace files + retrieval + background consolidation." It already has a promotion mechanism from short-term to long-term memory, but from the perspective of generational memory, it could make the lifecycle state of each memory object more explicit: young, survivor, old, archived, skill_candidate. In that model, each memory is not merely placed in a file; it has a clearer state and promotion path.

### Hermes: Lightweight, Restrained, and Suitable for a Portable Agent

Hermes has a more restrained built-in memory mechanism. It mainly uses two core files: `MEMORY.md` and `USER.md`. The former stores environment facts, project conventions, workflow experience, and other knowledge learned by the agent. The latter stores user preferences, communication style, and personal profile information.

The advantage of this design is that it is very lightweight. It forces the agent to preserve only information that is truly important, compact, and information-dense, avoiding uncontrolled expansion of long-term memory.

At the same time, Hermes provides session search. Complete sessions are stored in SQLite and searched with FTS5 full-text search. This allows `MEMORY.md` and `USER.md` to store only key facts, while fuller and colder historical records remain in the session database and can be retrieved when needed.

From the perspective of generational memory, Hermes can be understood as:

```text
USER.md
= user long-term preference / permanent memory

MEMORY.md
= agent long-term working memory / old generation

SQLite session search
= historical session archive / cold archive
```

Hermes is strong because its boundaries are clear, its cost is low, and it is easy to run. Its limitation is that the built-in hierarchy is relatively small. Layers such as daily memory, project memory, dreaming, and skill candidates need to be provided by an external provider or designed by the user.

If Hermes were extended into a more complete generational memory system, the most natural direction would be to add daily notes, project memory, and skill promotion mechanisms on top of the existing `MEMORY.md`, `USER.md`, and session search structure.

### Claude Code: Project-Level Automatic Memory

Claude Code's auto memory is more focused on coding-agent scenarios. It maintains automatic memory inside a project directory, allowing Claude to remember build commands, debugging experience, architecture notes, code style preferences, and workflow habits.

This design is well suited to code projects, because the memory needed by a coding agent is often not a global user preference. It is project context. Examples include how a repository runs tests, why a certain module is designed in a particular way, or how a previous bug was fixed. This information should not pollute global long-term memory. It should be constrained to the scope of the project.

From the perspective of generational memory, Claude Code's strength is project memory. It demonstrates an important principle: memory must have scope. Not all long-term information should be globally visible. Some memories should only take effect inside the current repository or current project.

But Claude Code's memory is closer to "project notes + automatic maintenance." It solves the continuity problem of project context, but it does not provide a particularly complete generational lifecycle model for how memory flows across daily memory, project memory, long-term memory, and skills.

### Letta / MemGPT: A Research Path Closer to a Memory OS

Letta/MemGPT is closer to redesigning agent memory from a system architecture perspective. It divides memory into recall memory, archival memory, and memory blocks. Recall memory is closer to complete conversation history. Archival memory is closer to an external long-term knowledge store. Memory blocks are structured memories that can be placed into the context window and updated by the agent itself.

This maps strongly to generational memory:

```text
memory blocks
= currently visible working memory

recall memory
= retrievable conversation history

archival memory
= external long-term knowledge store

context rewriting
= memory compression and rewriting
```

Letta/MemGPT's advantage is that its theory is more complete and closer to an "LLM Operating System." It treats the finite context window as a managed resource, allowing the agent to write, evict, recall, and update information.

However, from the perspective of generational memory, it emphasizes context management and external memory invocation more than it emphasizes the lifecycle of memory objects from young generation to old generation and then to skills. In other words, it solves the question of how an agent can have manageable long-term context. Generational memory further emphasizes how information inside that long-term context matures, gets filtered, gets promoted, and is forgotten.

### Mem0 / OpenMemory: A Shared Memory Layer Across Tools

Mem0 and OpenMemory are closer to independent memory infrastructure. OpenMemory provides a shared persistent memory layer through MCP for tools such as Cursor, Claude, and VS Code, allowing different coding agents to access the same user preferences, coding habits, and project context.

The value of this approach is cross-tool continuity. A user may use Claude Code today, Cursor tomorrow, and a VS Code extension the day after. If each tool has its own isolated memory, the user has to configure the same things repeatedly, and it becomes difficult to form continuous experience. OpenMemory/Mem0 tries to solve this problem by making memory an independent service rather than a private attachment of a single agent.

From the perspective of generational memory, these systems are more suitable as lower-level memory infrastructure. They handle storage, retrieval, sharing, and management of memory. But the upper layer still needs rules for deciding:

```text
Which memories are only short-term observations?
Which memories should become long-term preferences?
Which memories belong only to a specific project?
Which memories should be shared across tools?
Which memories should be deleted or downweighted?
Which repeated procedures should become skills?
```

So Mem0/OpenMemory can serve as infrastructure for generational memory, but generational memory still needs to define the lifecycle governance logic.

## 4. After the Comparison, the Role of Generational Memory Becomes Clearer

When these systems are placed side by side, we can see that each solves part of the agent memory problem:

```text
OpenClaw
= file-based memory + daily notes + dreaming + retrieval

Hermes
= lightweight long-term memory + user profile + session archive + external providers

Claude Code
= project-level auto memory

Letta / MemGPT
= Memory OS / context-window management / recall + archival memory

Mem0 / OpenMemory
= cross-tool shared persistent memory infrastructure
```

What generational memory tries to add is not a single feature, but a unified perspective:

```text
How should memory move from short-term record to long-term experience?
How should long-term experience become stable skill?
How should a system decide whether a memory should be promoted, compressed, demoted, archived, or forgotten?
How can we avoid placing all memories on one flat plane?
```

Therefore, generational memory is not meant to replace OpenClaw, Hermes, Claude Code, Letta, or Mem0. It is more like an interpretive and governance framework placed above these systems.

If OpenClaw provides a strong file and dreaming implementation, Hermes provides lightweight and restrained portable memory, Claude Code emphasizes project memory, Letta/MemGPT emphasizes the Memory OS, and Mem0/OpenMemory emphasizes cross-tool infrastructure, then generational memory emphasizes this:

```text
Memory is not static storage. It is a dynamic lifecycle.
```

That is its biggest difference from many existing systems.

Most existing systems are answering:

```text
Where should memory be placed?
How should it be retrieved?
How should it be injected into context?
How can continuity be preserved across sessions?
```

Generational memory is more concerned with:

```text
How does memory grow?
How is it verified?
How is it promoted?
How does it become a skill?
How does it expire?
How is it forgotten?
```

These two sets of questions do not conflict. A truly mature Agent Memory system should have the storage and retrieval capabilities of existing systems, while also having lifecycle governance capabilities of generational memory.

## 5. The Core Value of Generational Memory: Giving Memory a Lifecycle

In my view, the most valuable aspect of "generational memory" is that it explicitly proposes a lifecycle for memory.

Many systems discuss short-term memory and long-term memory, but the middle process is often vague. How does information move from short-term to long-term memory? Under what conditions should it be promoted? Under what conditions should it be forgotten? Under what conditions should a factual memory become a skill? If these questions are not designed clearly, long-term memory will eventually become dirty.

Generational memory can separate this process:

```text
Current Context
-> Daily Memory
-> Project / Topic Memory
-> Long-term Memory
-> Skill / Stable Procedure
-> Archive / Forgetting
```

This is not simply layering by time. It is layering by maturity and scope of effect.

Current context serves only the task at hand. It is important, but short-lived.

Daily Memory preserves important processes from the day. It catches the working scene first. It does not need to be highly refined, but it must preserve enough context for later review.

Project / Topic Memory preserves information that remains useful within a specific project or topic. It may not belong in global long-term memory, but it is very important inside the relevant project.

Long-term Memory preserves information that is truly stable and likely to repeatedly affect the agent's future behavior, such as user preferences, long-term rules, common technology stacks, important project background, and high-cost lessons.

Skill is a further stage of consolidation. When a class of tasks is repeated, the agent should not only "remember having done it." It should organize the repeated experience into "how to do it next time."

Archive preserves complete history or low-frequency cold data. It does not remain resident in context, but it can be retrieved when needed.

The key point of this structure is that memory is not static. It moves, compresses, promotes, demotes, gets rewritten, and can be forgotten.

## 6. Why RAG Alone Is Not Enough

RAG is useful, but RAG itself is not a complete memory mechanism.

RAG solves the problem of retrieving relevant content from external material. It is good for recall. It is good at finding information related to the current task from large histories, documents, web pages, codebases, and logs.

But RAG does not naturally solve these questions:

```text
Which content should enter long-term memory?
Which content is only temporary noise?
Which content has expired?
Which content should be merged?
Which content should be compressed?
Which content should be deleted?
Which procedures should become skills?
```

If an agent puts all chat history into a vector database and performs top-k retrieval every time, it will not completely forget the past. But the problem is that retrieved information may be semantically related without being currently reliable.

For example, the user may have configured a path three months ago, but later changed it. If the agent retrieves the old path and continues to use it, it will make a mistake.

Another example: a debugging process may contain many failed attempts. If these failed attempts are not consolidated, future retrieval may recall failure paths as well, interfering with judgment.

So RAG is more like the search tool inside a memory system. It is not the governance mechanism.

Generational memory addresses the higher-level problem:

```text
RAG is responsible for recall.
The generational mechanism is responsible for deciding what is worth keeping, how to keep it, how long to keep it, when to update it, and when to forget it.
```

A good Agent Memory system should place RAG inside a larger lifecycle framework, rather than treating RAG as the whole answer.

## 7. File-Based Memory, Database Memory, and Generational Memory Do Not Conflict

Some systems prefer to store memory in Markdown files, such as `MEMORY.md`, `CLAUDE.md`, project notes, and topic files. The advantage is transparency, editability, and auditability. Users can directly open the files to see what the agent remembers, and they can modify them manually.

Some systems prefer to store memory in SQLite, vector databases, full-text search, or graph databases. The advantage is structure, queryability, and scalability, especially for large histories and automated retrieval.

But I do not think these approaches are opposed.

Generational memory is not saying that memory must use files or must use databases. It is more like a set of organizational principles. It can be implemented with Markdown, SQLite, vector databases, or a hybrid design.

For example:

```text
MEMORY.md
= lightweight index for the current project

SQLite / FTS
= retrievable historical facts and event records

Vector DB
= semantic recall over historical sessions and documents

Topic files
= human-readable summaries of project experience

Skill files
= reusable operating procedures

Archive
= raw session logs and cold data
```

This is a more reasonable arrangement.

Files provide transparency. Databases provide query capability. Vector retrieval provides semantic recall. The generational mechanism provides lifecycle governance.

The truly important question is not the storage medium. It is whether the system can answer:

```text
Which generation is this memory currently in?
Why is it here?
When was it verified?
When should it be checked, compressed, or deleted in the future?
```

## 8. A Practical Generational Memory Structure

If I were to implement this, I would treat each memory as an object with metadata, not as a plain text fragment.

At minimum, a memory should contain:

```text
id: unique identifier
content: memory content
type: fact / preference / project / procedure / warning / skill_candidate
scope: global / project / topic / session
source: user_explicit / task_result / observation / imported_doc
created_at: creation time
last_accessed_at: most recent access time
access_count: number of uses
importance: importance score
confidence: confidence level
status: young / survivor / old / archived / skill_candidate
related_files: related files or evidence
conflicts: potentially conflicting memories
```

Only then can the agent manage memory rather than merely save memory.

For example, if the user says:

```text
In the future, whenever you generate English content, attach a Chinese translation underneath it.
```

This kind of information should be represented as:

```text
type: preference
scope: global
source: user_explicit
importance: high
confidence: high
status: old or permanent
```

As another example, suppose the agent debugged a Hermes skill today and found that an incorrect path causes tool loading to fail. When this information first appears, it can enter daily memory:

```text
type: warning
scope: project
source: task_result
importance: medium
confidence: medium
status: young
```

If this problem later appears repeatedly, or if this experience is used multiple times to fix similar issues, it can be promoted into project memory, or even organized into the troubleshooting section of a skill.

## 9. Memory Promotion Rules: Frequency Is Not Enough

A common misconception is that only repeated information is important.

High-frequency information is indeed usually worth preserving. If the user repeatedly requests a particular writing style, or if a project repeatedly uses the same deployment commands, those should enter long-term memory.

But some information appears only once and is still very important.

For example, the user may explicitly say, "Use this structure from now on." Or a task may determine the core technical direction of a project. Or a configuration may be foundational for the future operation of an entire system. Such information does not need to appear repeatedly before entering long-term memory.

So I think memory promotion should have at least two paths.

The first is ordinary promotion:

```text
Appears
-> saved to daily memory
-> accessed multiple times
-> helps multiple tasks
-> compressed and summarized
-> promoted to project / long-term memory
```

The second is special promotion:

```text
Appears once
-> but is explicitly requested by the user or has major significance
-> directly enters long-term / permanent memory
```

This resembles certain objects in JVM generational garbage collection being allocated directly into the old generation. Some objects are not important because they have lived a long time. They are special from the beginning.

From an engineering perspective, a simple promotion score can assist the decision:

```text
promotion_score
= importance
+ access_count
+ user_confirmation
+ task_success_signal
- age_decay
- conflict_penalty
```

But this formula is only an aid. It should not be applied mechanically. The most important signals in agent memory will always be explicit user expression, successful task outcomes, repeated reuse value, and real evidence.

## 10. Memory Compression: From Events to Experience

Long-term memory cannot preserve too many raw processes, or it becomes another chat-log warehouse.

A good long-term memory should be compressed experience, not a complete running log.

For example, the raw record might be:

```text
The user tried to connect to Windows through SSH today. The first password attempt failed. Later, OpenSSH Server was confirmed to be installed. The Parsec path was C:\Program Files\Parsec\parsecd.exe. Double-clicking it worked, and the final conclusion was that this path can be used to start Parsec.
```

After compression, it should become:

```text
On the user's Windows machine, the Parsec executable path is C:\Program Files\Parsec\parsecd.exe. If Parsec needs to be started remotely, try this path first.
```

This is the transformation from episodic memory to semantic memory.

Daily Memory preserves events. Long-term Memory preserves experience.

Without compression, the agent remembers too many minor details. If compression is too aggressive, important context is lost. A more reasonable approach is to preserve two layers:

```text
Long-term memory: refined conclusions
Historical archive: complete evidence
```

In ordinary use, the agent can rely on long-term memory. When provenance or details are needed, it can search the archive for the original records.

## 11. From Memory to Skill: Where Agents Really Become Stronger

I think many long-term memory systems underestimate the importance of skills.

If an agent only remembers "what it did before," its capability growth is limited. The truly valuable ability is to consolidate stable procedures from repeated tasks.

For example, the user may often ask the agent to do things such as:

```text
Write a Hermes skill
Configure SSH remote control for Windows
Handle Git branch conflicts
Organize technical blog posts
Rewrite Chinese content into more natural language
Turn a paper methodology into SOUL.md
```

If the agent reasons through these tasks from scratch every time, that is wasteful.

A better approach is: when a class of tasks appears repeatedly, the system should mark it as a skill_candidate and organize it into a skill during a review stage.

A skill is not just memory. It should contain:

```text
Trigger conditions
Applicable scenarios
Pre-checks
Execution steps
Common commands
Failure handling
Rollback plan
User preferences
Example outputs
```

This also maps naturally to the JVM's JIT analogy. Repeatedly executed logic should not remain in the interpreted execution stage forever. It should be optimized into a more efficient and more stable path.

For a long-term agent, factual memory lets it "know more," while skills let it "do better."

## 12. Review / Dreaming: The Real Core of the Memory System

If a system only saves and never consolidates, any memory system will decay.

Daily Memory will become longer and longer. Project Memory will mix in outdated content. Long-term Memory will preserve old preferences. Skills may also become invalid as the environment changes.

Therefore, review or dreaming is the core of the entire system.

It can run daily, weekly, or after a project ends. It mainly performs several tasks:

```text
Clean up low-value daily memory
Merge duplicated information
Compress important events into long-term experience
Keep project information constrained to the project scope
Promote global preferences into long-term memory
Mark repeated procedures as skill_candidate
Check whether old memories have expired
Find conflicting memories and request user confirmation
```

This step is equivalent to memory GC.

Without GC, long-term memory eventually becomes a garbage heap. Without dreaming, an agent can hardly grow from experience.

Humans do not organize memory at every moment. Much of this organization happens afterward: review, sleep, summarization, note-taking, and habit formation. Agents should have a similar mechanism.

## 13. Memory Also Needs Explainability and Auditability

The stronger a memory system becomes, the more transparent it needs to be.

If an agent can remember user preferences, project details, operation paths, account environments, and writing habits over the long term, it must let the user know:

```text
What has it remembered?
Why did it remember this?
Where did it come from?
When was it last verified?
Can it be modified?
Can it be deleted?
Will it be used across projects?
```

Otherwise, long-term memory changes from a capability into a burden.

A good Agent Memory UI should allow users to inspect different levels of memory:

```text
Global Memory
Project Memory
Daily Memory
Skill Candidates
Archived Sessions
Conflicting Memories
```

It should also support direct user actions:

```text
Confirm
Modify
Delete
Demote
Promote
Restrict scope
Organize into skill
```

This is very important.

Many memory judgments cannot be fully automated. The agent can make suggestions, but the user should retain final control. Especially when privacy, identity, long-term preferences, and project rules are involved, the memory system must be editable, reversible, and auditable.

## 14. Risks of Generational Memory

Of course, generational memory is not a universal solution.

The first risk is complexity.

If the system is designed to be too complex, it may increase maintenance cost before it produces clear benefits. This is especially true for personal agents. If every memory must be scored, classified, migrated, and verified, the system can easily become over-engineered.

The second risk is incorrect promotion.

Some information is only temporary, but the agent may save it as a long-term rule. For example, the user may once ask for "simpler writing" because an assignment is urgent. That does not necessarily mean the user always prefers a simple style. If this is promoted incorrectly, it will affect all future responses.

The third risk is incorrect forgetting.

Some low-frequency information is rarely used but extremely important. If cleanup is based only on access frequency, the system may delete key configurations, important preferences, or long-running project background.

The fourth risk is old-memory pollution.

Information that was correct in the past may become wrong in the future. Software versions change, paths change, project requirements change, and user preferences change. If long-term memory is not updated, it will create hallucinations.

The fifth risk is privacy and boundary management.

The more a long-term agent can remember, the more carefully it must handle user data. Not everything should be saved, and not every memory should be used across contexts.

Therefore, generational memory must be paired with three principles:

```text
Explainable
Editable
Deletable
```

Without these three principles, stronger long-term memory creates greater risk.

## 15. Generational Memory Does Not Replace Existing Systems; It Organizes Them

Finally, I think one point needs to be emphasized: generational memory is not meant to replace RAG, vector databases, Markdown memory, SQLite, FTS, session archives, or skill systems.

It is more like a higher-level framework that organizes these components.

One way to understand it is:

```text
RAG
= recall mechanism

Vector DB / FTS / SQLite
= storage and retrieval infrastructure

Markdown Memory
= human-readable memory interface

Session Archive
= raw evidence layer

Project Memory
= scope isolation

Long-term Memory
= stable experience layer

Skill System
= procedural capability layer

Review / Dreaming
= memory governance and lifecycle management
```

What generational memory truly emphasizes is this: information should not be permanently saved the moment it enters the system, and it should not disappear immediately after a task ends. It should first be captured, then observed, then filtered, then compressed, and then either promoted or forgotten.

This is memory lifecycle governance.

## 16. An Ideal Agent Memory OS

If all the ideas above are combined, an ideal Agent Memory OS might look like this:

```text
Current Context
= current task workspace, serving only immediate reasoning and execution

Daily Memory
= daily records, preserving short-term scene and process information

Project / Topic Memory
= project-level or topic-level context, loaded only for relevant tasks

Long-term Memory
= stable facts, preferences, rules, and important experience

Permanent Memory
= core preferences and rules that the user explicitly asks the agent to follow over the long term

Session Archive
= complete historical sessions, cold data retrieved only when needed

Memory Search
= keyword, full-text, vector, and hybrid retrieval

Review / Dreaming
= regular consolidation, compression, promotion, and forgetting

Skill Candidate
= repeated procedures that have not yet been hardened

Skill System
= stable operating capabilities distilled from experience
```

The key behind it is not any particular database or file format. It is a complete mechanism for memory flow:

```text
Occurrence
-> Recording
-> Retrieval
-> Verification
-> Compression
-> Promotion
-> Archiving
-> Forgetting
-> Skill formation
```

An agent truly becomes stronger not because it saves more text, but because it can gradually turn that text into structure, experience, and capability.

## 17. Conclusion: Long-Term Agents Need Better Memory Governance, Not Just Larger Memory

The goal of a long-term agent is not to save everything like a hard drive.

Saving everything appears safe, but in practice it makes the system increasingly bloated. A truly useful memory system should resemble a person who can organize experience: it knows which details are temporary, which items are project rules, which are long-term preferences, which are high-cost lessons, which tasks should be consolidated into skills, and which old information should be discarded.

This is why I think the analogy of generational memory is valuable.

It reminds us that an agent's long-term memory should not be a static warehouse. It should be a dynamic system. It has a young generation, a project layer, an old generation, an archive layer, a skill area, and mechanisms for garbage collection and memory consolidation.

From this perspective, truly mature Agent Memory is not merely:

```text
I can remember what you said.
```

It should be:

```text
I can extract experience from the things we have done together.
I know what should be kept temporarily and what should be preserved for the long term.
I know what should be forgotten and what should become a skill.
Over time, I become more useful in practice, rather than more bloated.
```

That is what makes long-term agents genuinely worth looking forward to.

## Reference Directions

This essay mainly references and compares several categories of systems and research directions: OpenClaw's file-based memory, daily notes, and dreaming; Hermes's `MEMORY.md`, `USER.md`, session search, and memory providers; Claude Code's project-level auto memory; Letta/MemGPT's recall memory, archival memory, and memory blocks; Mem0/OpenMemory's cross-tool persistent memory layer; and research such as MemGPT, MemoryBank, Generative Agents, A-MEM, and AgeMem on long-term memory, layered memory, memory retrieval, and memory management.
