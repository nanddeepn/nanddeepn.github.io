---
title: "AI Agents vs Agentic AI in the Microsoft Ecosystem"
date: "2025-06-22"
share: true
header:
  image: media/2025-07-09-ai-agents-vs-agentic-ai/01.png
  teaser: media/2025-07-09-ai-agents-vs-agentic-ai/01.png
categories:
  - Microsoft 365
  - AI
  - Copilot
tags:
  - "2025"
  - July 2025
last_modified_at: 2025-07-09T00:00:00-00:00
---

## Introduction

Artificial Intelligence has rapidly evolved from simple chatbots to sophisticated assistants that can perform complex tasks. In the Microsoft ecosystem, this evolution is evident in products like **Microsoft 365 Copilot**, which acts as an AI assistant across Office apps, and emerging frameworks that enable even greater autonomy. 

Two terms often discussed are **AI agents** and **agentic AI**. While they sound similar, they refer to different levels of AI capability. In simple terms, an **AI agent** is a single AI-driven assistant focused on a specific task or domain, whereas **agentic AI** refers to a more advanced system where multiple AI agents or components work together autonomously toward broader goals. 

This article will explain these concepts in easy terms, trace their development from generative AI, and use Microsoft-focused examples to highlight
key differences.


## Journey from Generative AI to AI Agents to Agentic AI

The journey of modern AI can be viewed in three stages: **Generative AI** , **AI Agents** , and **Agentic AI**. It began with _generative AI_ models like GPT-3 and GPT-4 that can produce human-like text, images, or code when given a prompt. Generative AI excels at content creation – for example, ChatGPT can write an email or summarize a document when asked. However, generative models by themselves are essentially reactive: they respond to each user prompt and don't _act_ beyond providing output.

**AI agents** emerged as the next step by embedding generative AI into interactive applications and giving them a goal-oriented context. An AI agent uses a generative model under the hood but is _packaged as a software program that observes its environment, makes decisions, and takes actions toward a goal_. Unlike a basic chatbot that only answers questions, an AI agent is often **context-aware and can operate with some memory and logic** to carry out a task without needing the user to spell out every step. 

For example, Microsoft's Bing Chat combines GPT-4 with web browsing tools – it not only generates answers but can search the web and then formulate a response. Similarly, Microsoft 365 Copilot acts as an AI agent in Office apps: it not only generates text but also interacts with your data and commands in Word, Excel, Outlook, etc., to help you get things done.

Finally, the frontier of this evolution is **agentic AI** – a term for AI systems with a higher degree of autonomy that can _manage multi-step processes and coordinate multiple agents or tools_ to achieve a larger objective. In other words, agentic AI is what you get when multiple AI agents work in concert under an overarching “conductor” or framework. Agentic AI systems can take an initial instruction and then **plan, execute, and adjust a sequence of actions across different apps or services, with minimal human intervention**. This is particularly relevant to complex workflows. Microsoft's recent efforts — like the **Semantic Kernel** SDK and **AutoGen** framework — are aimed at enabling this kind of orchestrated, multi-agent autonomy in a reliable way for enterprises.

In summary, generative AI gave us the content engines, AI agents turned those engines into helpful assistants in specific domains, and agentic AI is now combining those assistants and capabilities to tackle broader, multi-faceted tasks autonomously.


## What is an AI Agent? (Simple Definition with Microsoft Examples)

**AI Agent** (or AI assistant) refers to an AI-driven software entity that can perceive input, process information, and take action to help accomplish a specific task or goal. Think of it as a focused digital assistant. It typically operates within a defined scope (like an application or a domain) and uses AI (often an LLM like GPT-4) to decide _what_ to do. Crucially, an AI agent can handle tasks without needing constant step-by-step instructions – you give it an objective or query, and it figures out the steps to complete it within its scope.

_Figure: Microsoft 365 Copilot acting as an AI agent in Word, drafting a proposal based on a prompt. Copilot
understands the user's request and generates a first draft directly in the document, saving the user time._

A simple example of an AI agent is **Cortana** (Microsoft's former virtual assistant), which could set reminders or send emails when you asked. Modern AI agents are far more powerful. **Microsoft 365 Copilot** is a prime example: it behaves as an AI agent embedded in the Office apps. For instance, Copilot in **Outlook** can read your emails and _summarize a long email thread, then draft a suggested reply for you_ , helping you clear your inbox quickly. In **Word** , Copilot can act on a prompt like “Draft a two-page project proposal based on the outline” – it understands the request and generates a draft document for you (which you can then refine). 

In **Teams** , Copilot can serve as an intelligent meeting assistant: during a meeting it listens to the conversation
and _summarizes key discussion points, notes who said what, and even suggests action items in real time_. These are not just one-off tricks; they indicate that the agent is aware of context (the email thread, the meeting content, etc.) and is taking goal-directed actions (summarizing, drafting) on behalf of the user.

Other Microsoft examples include **GitHub Copilot** – an AI agent for software developers. It integrates into code editors, understands the context of your code, and produces suggestions or whole functions for you.

When you type a comment saying “// function to reverse a linked list”, GitHub Copilot (powered by an OpenAI model) acts on that goal and writes the code snippet. Likewise, **Copilot Studio** in the Microsoft Power Platform allow creation of chatbots (AI agents) that can answer customer questions or assist with tasks like resetting a password by following predefined conversational flows enhanced with AI.

The key idea is that an AI agent has a _focused purpose_. It might use sophisticated AI internally, but it's devoted to a particular domain or problem. It also generally works **independently within its domain** – e.g.the Outlook Copilot won't try to analyze your Excel spreadsheet unless it's specifically asked or given that capability. **It operates within set boundaries and typically responds to user input or triggers in a goal-directed way** (often using some built-in rules or toolset). In short, an AI agent is like a single skilled assistant.


## What is Agentic AI? (Simple Definition with Microsoft Context)

**Agentic AI** refers to an AI system architecture where multiple agents (or AI components) are orchestrated to work together autonomously, exhibiting **higher-level autonomy, proactive goal pursuit, and adaptive decision-making**. If a single AI agent is like one assistant focusing on a task, agentic AI is like a team of specialized AI agents collaborating to accomplish a more complex objective. An agentic AI system can break down a large goal into sub-tasks, assign those to different AI agents (each possibly with different skills or access), and coordinate the results to achieve the overall goal – _all with minimal human guidance_.

There are a few hallmark capabilities of agentic AI systems that distinguish them from a lone AI agent:

- **Autonomous decision-making:** Agentic AI can analyze a situation or goal and decide on its own what actions to take next, rather than just reacting to a single user command.

- **Goal-driven, multi-step actions:** Agentic AI works towards high-level objectives by planning and executing multiple steps or tool calls in sequence. It doesn't require a person to prompt each step.

- **Learning and adaptation:** An agentic system can learn from feedback and outcomes. It may adjust its strategy if one approach fails, improving over time (this might involve techniques like reinforcement learning or simply iterative refinement based on results).

- **Advanced reasoning and tool use:** Agentic AI can use multiple tools, connect to various data sources or systems, and reason about how to combine these to fulfill the task. It's not limited to one function – it's flexible in orchestrating resources to meet its goal.

In practice, agentic AI often means **multiple AI agents working in concert**. Microsoft's context provides some great examples. Consider an **IT support scenario** in an enterprise. A traditional single agent (non-agentic) might be a chatbot that collects info and then hands off to a human, or just resets a password when asked. In contrast, an _agentic AI_ support system could handle an entire ticket autonomously: one agent understands the user's help request (using natural language processing), another agent accesses the relevant user account system, another agent performs the password reset or fixes the issue, and another communicates the resolution back to the user – _all coordinated automatically_. Such a system can figure out what the user needs, decide which "sub-agent" should do what, and carry out the solution end-to-end (for example, **resetting a user's password or changing permissions without human intervention** when appropriate). The user experiences it as one AI service that “just handles it.” This is agentic AI in action: it's **proactive, goal-driven, and able to execute multi-step solutions** rather than a single-step answer.

In the Microsoft ecosystem, a concrete illustration was shown at Ignite 2024 with the new **Azure AI Agent Service**. In one demo scenario, they described a _blog-writing assistant pipeline_ built with agentic AI. The task was to create a blog post using AI. Instead of one agent trying to do everything, they used three specialized agents: 
    1. a **Content Collection agent** that uses Bing search to gather information on the blog topic, 
    2. a **Writer agent** that uses an LLM (like GPT) to draft the blog content, 
    3. a **Content Storage agent** that uses a code execution tool to save or publish the content.

These agents were orchestrated to work together: the first finds relevant info, passes it to the second which writes the article, and the third then stores the result. All of this can happen without a person manually prompting each step – the system knows the overall goal (“write a blog about X and save it”) and each agent plays its part in sequence. The coordination of these multiple agents to achieve the final published blog post is a prime example of agentic AI. Microsoft's **AutoGen** framework was used to facilitate this orchestration, highlighting how agentic AI solutions are built (more on that in the frameworks section).

Another everyday example: **smart home automation** can be seen through the lens of agentic AI. Imagine Microsoft's AI orchestrating your office environment: one agent (thermostat control) ensures the temperature is comfortable, another agent (lighting control) adjusts smart lights based on time of day, another (energy optimizer) schedules heavy power usage tasks when energy is cheaper, etc. The agentic AI system oversees these individual agents and keeps the overall goal (energy efficiency and comfort) on track. If conditions change (say, a sudden drop in temperature), the system can re-plan (maybe the
thermostat agent kicks in more heating while the energy agent temporarily relaxes efficiency rules) – this
adaptability and multi-agent coordination is what makes it _agentic_.

In summary, **agentic AI = multiple AI agents + an orchestration brain + autonomy**. Microsoft is actively developing this domain with enterprise use in mind – enabling AI to not just assist in single tasks, but to _run whole processes_. Now, let's break down the differences between these two concepts in a clear way.

## Key Differences between AI Agents and Agentic AI

At a high level, an AI agent is like a single specialist, whereas agentic AI is like a self-managing team of
specialists. Both involve AI making decisions and taking actions, but the **scale of autonomy and
complexity** differs. The table below summarizes the key differences:


| Aspect | AI Agents | Agentic AI (Autonomous Systems) |
|--------|-----------|---------------------------------|
Scope of Tasks | Focused on specific, well-defined tasks or domain. An AI agent usually handles one aspect (e.g. answering questions, drafting text in one app). | Handles complex, multi-step objectives that may span multiple domains or systems. Orchestrates several tasks to achieve a larger goal. |
| Autonomy & Decision Making | Semi-autonomous : Operates within predefined rules or frameworks. Makes limited decisions independently but often in response to direct prompts or triggers. Typically reactive (waits for user input or a defined event). | Highly autonomous : Can proactively make decisions on what actions to take next to achieve the goal. Evaluates options and can initiate actions without explicit instructions at each step. Goal-driven rather than just reactive. |
| Planning & Proactiveness | Generally handles one step at a time (one prompt → one action). Little long-term planning; if any multi-step logic exists, it's usually pre-programmed or linear. Tends to be reactive , executing an action after a request.| Capable of multi-step planning. Given an objective, it can sequence a plan of actions and adjust on the fly if needed. It can act proactively - for example, monitoring for issues and initiating actions to resolve them before being asked. |
| Learning & Adaptability | Often improves via model updates or static learning on background data. It might adapt within a conversation (short-term memory), but broader learning usually requires developer intervention. | Adaptive and evolving : Learns from interactions and outcomes to refine its approach in real time. For instance, it can learn which strategies work best and modify its behavior on its own, within safety guardrails. This makes it better suited for dynamic environments that change over time. |
| Tool Use & Collaboration | May have access to some tools (e.g. an AI agent might call a translation API), but usually operates independently. If multiple capabilities are needed, they might be combined in a single agent in a limited way. No concept of multiple agents working together (one agent = one “brain”). | Orchestrates multiple agents/tools. Different agents can specialize (one for data retrieval, one for analysis, one for execution, etc.) and the agentic system coordinates them. Essentially, it's a collaborative approach: outcomes from one agent inform actions of another. The system acts as a “conductor” for these agents to work in unison toward the goal. |


An AI agent, like a chatbot or personal assistant, is constrained to _its specific role_ and
**operates within set parameters**. It might be very intelligent in that role (thanks to AI models) but it won't
generally step outside its lane. For example, a scheduling bot will schedule meetings when asked, but it
won't on its own decide to also draft the meeting agenda unless that's explicitly part of its programming.

Agentic AI, on the other hand, is characterized by **broader autonomy and initiative**. It's built to handle
scenarios where achieving the goal may require coordinating various sub-tasks and responding to changing
conditions. One way to think of it: _AI agents are the building blocks, while agentic AI is the whole system that
uses those blocks to solve bigger problems_. Agentic AI can take on tasks that are **too complex for a single
agent** by essentially deploying a team of agents and managing them. It also inherently deals with more
uncertainty – so it includes mechanisms to reason about what to do when, say, an action fails or more
information is needed. For instance, if one tool doesn't yield an answer, an agentic system might try a
different approach or summon a different agent, much like a human project manager would adjust a plan
when encountering a roadblock.

To put it simple words: **AI agents = individual doers; agentic AI = autonomous orchestrator of doers**. Both
are valuable – AI agents bring efficiency to specific tasks, while agentic AI can tackle _process automation and
complex decision-making_ that involves many moving parts.

## Overview of Agentic AI Frameworks in Microsoft's Ecosystem

Building agentic AI solutions (i.e. systems of multiple coordinated agents) requires a robust framework.
Microsoft is investing in this area through projects like **Semantic Kernel** and **AutoGen** , which are designed
to help developers create and manage AI agents and agentic systems. Let's look at these frameworks:

- **Semantic Kernel (SK):** This is an open-source SDK from Microsoft that allows integration of large
language models (LLMs) into applications with ease. At its core, Semantic Kernel helps you define
skills or functions for the AI (for example, a function to calculate something or fetch data) and
manage prompts and memory. It's production-ready (version 1.0 as of late 2024) and supports
multiple programming languages (C#, Python, Java). Importantly, SK has an Agent
Framework (in preview) that enables developers to create agents that can use these skills/tools to
solve problems, and even a Process Framework for long-running, stateful processes with possible
human-in-the-loop steps. In simpler terms, SK provides the structure to build an AI agent
that can use tools and maintain context, and it's robust enough for enterprise use (with support and
reliability). For instance, you could use Semantic Kernel to build a single agent that knows how to use
a database and an email API – giving it the ability to answer a question by both querying data and
sending an email with the result.

- **AutoGen:** This is another open-source framework (originating from Microsoft Research's AI Frontier
Lab) aimed at more experimental, bleeding-edge agentic AI scenarios. AutoGen is specifically
designed to simplify the creation of multi-agent systems , where multiple LLM-based agents,
possibly with different roles, can converse and cooperate to solve a task. It provides an event-
driven, distributed runtime which is useful for long-running autonomous agents that might need
to wait for events or talk to each other over time. A hallmark of AutoGen is enabling advanced
patterns like agents that critique each other's answers, or a helper-agent and solver-agent working
together (these patterns were popularized in research as ways to boost accuracy). AutoGen supports
Python and C#, and while it's in active development, it's more of a playground for cutting-edge agent
behaviors at the moment. For example, with AutoGen one could spin up a “Researcher Agent” and a
“Writer Agent” that chat with each other – the researcher finds information and the writer composes
a report, iterating until they are satisfied. This kind of multi-agent loop is what AutoGen makes easier
to implement.


Beyond SK and AutoGen, Microsoft has also introduced the **Azure AI Agent Service** (in preview) as part of
Azure AI. This is a cloud service that allows organizations to deploy and manage AI agents easily. One
exciting aspect is that Azure AI Agent Service is flexible with models – it can use OpenAI's models (like
GPT-4) or open-source models (like Llama 3, etc.), and it has built-in connectors to Microsoft 365 data and
other enterprise systems. Essentially, Azure AI Agent Service provides the plumbing to hook an AI agent
into your business data and tools securely, and it works hand-in-hand with frameworks like SK or AutoGen
for orchestrating multiple agents. For instance, a company could use Azure AI Agent Service to create
several internal agents (one that knows HR policies, one that can query the sales database, one that can
execute workflows via Azure Functions). Then using Semantic Kernel, these agents could be orchestrated: if
an employee asks a complex question (“I need to onboard a new vendor and set up a contract”), the system
might invoke the policy agent to check rules, the sales agent to retrieve forms, and a communication agent
to draft an email or Teams message – all automatically.

In summary, Microsoft's agentic AI frameworks and services (Semantic Kernel, AutoGen, Azure AI Agent
Service, etc.) are the **toolkits to build your own Copilot-like agents or multi-agent systems**. They handle
the heavy lifting of connecting AI to real-world actions: things like function calling (i.e., an agent calling an
API or plugin), maintaining long-term context, and orchestrating multiple AI components. We'll see these
frameworks appear in various Microsoft products (some are already under the hood of Copilot experiences),
as well as being available to developers to create custom solutions.


## How AI Agents and Agentic AI are used in Microsoft 365 (Outlook, Word, Teams)

Microsoft 365 (Office apps and related services) is one of the most visible places where AI agents are
making an impact on everyday productivity. With the introduction of **Microsoft 365 Copilot** , AI agents are
now embedded in Outlook, Word, Excel, PowerPoint, Teams, and more, acting as intelligent assistants. Let's
explore how they work in these apps and how agentic principles might be at play:

- **Outlook (Email):** In Outlook, Copilot functions as an AI agent that helps you manage
communications. It can summarize long email threads to give you the gist of a conversation without
reading every message, and it can suggest or even draft replies for you. For example, if you have
a lengthy email chain about a project, you can ask Copilot “What was decided in this thread?” and it
will generate a concise summary. This agent has access to your email content (respecting
permissions) and uses generative AI to produce useful output – effectively reducing the cognitive
load on you. While this is primarily an AI agent scenario, it shows a hint of agentic behavior in that
Copilot could, for instance, pull in your calendar info if needed when drafting a reply (“I'm available
for a meeting at 3pm”). It's using multiple data sources autonomously to help achieve the goal of
handling your email.

- **Word (Document creation):** In Word, Copilot serves as a writing assistant agent. You can ask it to
draft a document on a topic and provide some pointers, and it will generate content for you right in
the document. It can also rewrite or format existing text. For example, you might prompt, “Copilot,
draft a project proposal for Project X based on these bullet points,” and it will produce a multi-
paragraph draft. The AI agent here leverages generative AI to create content, but also integrates
with Word's functionality (it can insert content, format, etc., just as if a human user was using Word).
This is an AI agent working within the scope of Word to assist with content creation. It saves time by
giving you a starting point, which you can then edit – you remain in control of the final output.


- **Teams (Meetings and Chat):** Copilot in Teams acts as a meeting assistant and knowledge agent.
During meetings, it will listen to the discussion and generate real-time summaries and action items. By
the end of a Teams meeting, you can have Copilot recap the key points (“Alice agreed to send the
data by Friday, Bob will draft the slides, next meeting on Monday”). It recognizes who said what
and identifies where there was agreement or dissent. This is a great example of an AI agent doing
something dynamic: it's not pre-scripted; it uses AI to understand natural conversation. In chat, Teams Copilot can answer questions based on your work context. For instance, you could ask “@Copilot,
what did we decide about Project X last week?” and it will pull from meeting transcripts, emails, and
documents (with permissions) to give you an answer. This crosses into agentic territory because the
Teams Copilot's Business Chat mode can aggregate information from multiple sources (emails,
documents, calendar) to fulfill a request. It's orchestrating across Microsoft Graph data – one
might say it's an agentic behavior built into a single agent interface.

Overall, in Microsoft 365, **AI agents aim to boost productivity by handling the grunt work** : reading and
summarizing content, drafting materials, extracting insights, and even taking actions like scheduling
meetings or composing responses. At present, each Copilot in an app is like a dedicated agent for that
context. However, Microsoft 365 also offers **Business Chat** , which is a unified conversational agent that
works across apps. Business Chat can be seen as a step toward agentic AI because it isn't tied to one
application – it looks at your Outlook, Teams, OneDrive, etc. to answer questions or complete tasks that
involve multiple apps (“Draft an update for my team about our Q4 targets and last week's client feedback”).
Internally, Business Chat might be invoking multiple functions: one to gather relevant info from meetings,
one to pull recent files, one to format the response. This kind of orchestration behind a single-chat interface
shows how agentic patterns are being used to deliver a seamless experience.

To summarize, **AI agents in Microsoft 365 Copilot handle individual productivity tasks** in Outlook, Word,
Teams and other apps. They are already incredibly useful on their own. As these agents become more
connected (through Business Chat and future updates), we'll see more agentic AI behavior – where the
Copilot feels like it's “aware” of broader workflows and can string together tasks across the suite. Microsoft
365 is essentially bringing an _AI co-worker_ to sit alongside you, one that can juggle different applications to
get the job done.

## How AI Agents and Agentic AI are used in Copilot Studio (Bot flows, Plugins, Memory, etc.)

**Copilot Studio** is Microsoft's platform for developers and “makers” to **build custom AI agents** and extend
Copilot's capabilities. If Microsoft 365 Copilot is the out-of-the-box AI assistant, Copilot Studio lets
organizations create their own tailored AI agents (sometimes called **custom agents** or **Copilot extensibility** ). In Copilot Studio, one can configure an agent's behavior, provide it with domain-specific
knowledge, connect it to tools or plugins, and even define conversation flows (bot logic). Let's break down
how AI agents and agentic AI concepts appear in Copilot Studio:

- **Bot Flows (Topics):** In Copilot Studio, you can define conversational topics or flows that outline how
the bot (agent) should handle certain user intents. This is similar to designing a dialogue tree or
using topics. For example, you might create a topic for “Employee Onboarding”
which guides the agent to ask for the new employee's name, role, start date, etc., and then provide
relevant info. These flows ensure the AI agent covers all necessary steps. Traditional bots were
heavily reliant on these kind of scripted flows, but with Copilot Studio, you can intermix these flows
with generative AI – giving the agent flexibility in how it handles each step. The result is a more
natural conversation that still hits all the important points. Memory comes into play here: the agent
keeps track of context within a conversation (e.g. it remembers the employee's name you provided
earlier so it doesn't ask again). Copilot Studio has features to maintain context between turns so that
the agent can handle multi-turn conversations coherently. For instance, if you ask a custom HR agent “I need to onboard someone new,” it might go through the steps, and if you later just say
“What's next on the checklist?”, the agent recalls where it left off. Ensuring the agent has this
conversation memory is crucial for a good experience.

- **Tools / Plugins (Actions):** One of the most powerful features is the ability to extend an agent with
tools , which are essentially plugins or APIs the agent can call. In Copilot Studio, you can add tools to
your agent that allow it to do things like query a database, call an external REST API, fetch data from
a SaaS service, or even trigger a Power Automate flow. These are described to the agent as actions it
can use. For example, you might add a “Create Support Ticket” tool to an IT support agent, or a
“Lookup Order Status” tool to a customer service agent. The AI agent, when it determines it needs
that functionality, can invoke the tool to get real-time data or perform an action. From the
agent's perspective, it's like having special skills beyond just chatting. Technically, this leverages the
function calling ability of the underlying LLM: the model can decide, “Aha, I should use this tool now,”
and then the platform executes the tool and returns the result back to the model. Microsoft's
documentation calls these custom actions and uses connectors to integrate with enterprise systems. The maker just has to configure the connector (for auth and details), and then the agent
can, say, retrieve a customer record from Dynamics 365 or post a message to Teams via an action.
This is foundational for agentic behavior because tools let the agent affect the external world and
gather information autonomously.

- **Knowledge Sources:** Beyond tools, Copilot Studio allows adding knowledge sources to agents. This
means you can give your agent access to internal documents, SharePoint sites, or other data (using
Microsoft Graph connectors, for example). A custom agent could be created to answer, for
instance, HR policy questions by grounding it on your company's HR handbook and FAQs. The
knowledge source is essentially a retrieval-augmented generation (RAG) setup: the agent will search
the given data (say all HR PDFs) to find relevant content and then formulate an answer. The agent
remains constrained to only use that data for answering if you set it that way, ensuring accuracy.
Memory also plays a role with knowledge: the agent might remember what it already searched or
which document was relevant as the conversation continues. In Copilot Studio, makers can manage
these sources so the agent stays up to date (for example, connecting a SharePoint that updates with
new info automatically updates what the agent can know).

- **Other Agents (Agent orchestration):** Interestingly, Copilot Studio even has a concept of adding
“other agents” to an agent (this is in preview). This means a custom agent could call upon
another agent as a tool. It's a truly agentic concept – one agent leveraging another agent's expertise.
Imagine you have a Finance agent and an IT agent; you could design a scenario where if the Finance
bot gets a technical question, it passes it to the IT bot. Copilot Studio's inclusion of this feature shows
Microsoft anticipates multi-agent orchestration scenarios even in their low-code platform. It's
basically allowing agentic AI patterns: e.g., a Copilot agent might route a user's query to one of
several sub-agents depending on what it's about (much like a human manager directing you to the
right department). Under the hood, this is likely using the same orchestration frameworks (SK or
others) to manage the hand-off between agents.

- **Persistent Memory / Context:** A challenge with LLM-based agents is maintaining long-term context
(memory of past conversations or user preferences beyond the current session). Microsoft has been
exploring “Copilot memory” features – for example, you might want an agent that remembers user
preferences or past interactions even if you start a new chat next week. In Copilot Studio as of mid-2025, persistent memory is a controlled feature (for privacy reasons, long-term memory may be
opt-in). The platform does allow storing conversation state or extracting important pieces of info to
memory so that the agent can recall them later. For instance, an agent could remember that a user's
favorite project is “Alpha” so next time the user doesn't need to specify it. This moves the agent
closer to how a human assistant would personalize interactions over time.


In practice, Copilot Studio gives organizations the toolkit to create **their own AI agents** that can do exactly
what they need, within Teams or other interfaces. You can think of it as building a custom Copilot. These
agents are **declaratively authored** – you describe in natural language and through configuration what the
agent should do, what knowledge it has, and what actions it can take. The heavy lifting of prompt
generation, calls to the LLM, etc., is handled by Microsoft's platform.

For example, suppose a company wants an “Expense Report Copilot.” In Copilot Studio, they create a
custom agent with knowledge of the company's expense policy (by attaching a SharePoint with policy docs),
a tool that connects to their expense system (via an API connector), and some conversational topics like
“Submit an expense” and “Reimburse status.” Now an employee can ask this agent in Teams, “How do I
submit an expense for a client dinner?” The agent will use the policy knowledge to answer and guide them
through it, maybe even pull up the expense form (using a tool). If the employee says, “Actually, I submitted
an expense last week, can you check status?”, the agent can call the expense system API to retrieve that and
answer. This single agent is showing a bit of agentic flavor by juggling knowledge and tools and maintaining
context across the dialogue.

Copilot Studio essentially supports _both AI agent creation and some agentic AI patterns_ , depending on how
you use it. If you simply make a Q&A bot with one knowledge source, that's a straightforward AI agent. If
you give it multiple tools, multiple knowledge sources, and even other agents to cooperate with, you are
crafting an agentic system – albeit all abstracted under one “agent” interface to the user. The inclusion of
**connectors for enterprise systems** means these agents can truly act on enterprise data, making them
a lot more useful than an isolated chatbot.

Finally, Copilot Studio places a big emphasis on **security and governance** – ensuring these agents don't
access data they shouldn't and that any actions they take are auditable. This is critical because as soon as
you allow an AI to take actions (like sending an email or updating a record), you want tight control over
permissions and scope. Microsoft provides tools to configure _allowed connectors, scopes, and to sanitize
inputs/outputs_ so that the AI agent remains reliable and secure.

In summary, **Copilot Studio is where Microsoft's AI agents can be tailored and extended**. It brings
together the generative power of LLMs with structured conversational design, plugins (tools), memory, and
even multi-agent orchestration. This allows businesses to create AI agents that are not just clever in
conversation but actually actionable and integrated into business workflows – a big step toward practical
agentic AI in everyday use.

## How AI Agents and Agentic AI are used in Azure OpenAI (GPT models, Plugins, Orchestration)

Microsoft Azure provides the backbone for many AI solutions, and with **Azure OpenAI Service** ,
organizations can harness powerful GPT-3.5, GPT-4, and other models with enterprise-grade security and scalability. Azure OpenAI on its own provides the _generative AI engine_ , but to build AI agents or agentic
systems, developers often combine it with Azure's other services and orchestration tools. Here's how AI
agents and agentic AI come into play in Azure:

- **Azure OpenAI + Function Calling (Plugins):** Azure OpenAI Service supports the same function
calling feature that OpenAI's APIs offer. This means you can program functions (or use Azure
Functions, APIs, etc.) that the model is allowed to call, and the model will output a JSON indicating
when it wants to use those functions. In practice, this is how you implement “plugins” or tools for an
AI agent in Azure. For example, if you are building a chatbot on Azure that helps users get weather
information and book calendar events, you could define two functions: get_weather(city) and
create_event(date, title). The GPT-4 model, when asked “Schedule a meeting on the next
sunny day,” might call get_weather to find a sunny day then call create_event to schedule.
This capability essentially turns a raw model into an agent that can act , because it's not just
generating text, it's deciding to invoke operations. Azure OpenAI function calling is a core way to
create AI agents that interact with enterprise systems – you provide the functions for whatever
actions are needed (database queries, sending emails, invoking workflows) and the AI will learn
when to use them from natural language prompts. Azure has documentation and samples for
how to do this (using the REST API or SDKs). In short, you can extend GPT models with custom
skills in Azure, enabling your agent to be useful in a business context.

- **Azure AI Studio and Orchestration:** Azure AI Studio (the portal for Azure OpenAI) provides a visual
interface to prototype chat-based assistants, including adding what they call “Enterprise data” and
“extensions”. This is Azure's way of letting you ground an AI on your data (via cognitive search or a
vector index for retrieval) and add APIs it can call (similar to plugins). If you've heard of “ChatGPT on
your data”, Azure OpenAI enables that securely – it's an AI agent that knows about your internal
documents. For example, a company can stand up an Azure OpenAI-powered chatbot that
employees can ask questions like “How do I file PTO?” and it will retrieve the answer from the HR
policy files and respond. When multiple functions and data sources are integrated, this becomes
quite an agentic system – the assistant might decide it needs to use a knowledge base vs. call an API
depending on the query, exhibiting decision-making on how to fulfill the request.

- **Workflows and Logic Apps:** Azure also has tools like Logic Apps and Power Automate that can be
used in conjunction with AI. One emerging pattern is using an AI agent to handle the unstructured
decision-making, and then calling a workflow to carry out structured steps. For example, an AI agent might parse an incoming customer email (using GPT to extract intent like “customer wants to return
an item”) and then hand off to a predefined workflow that processes a return in SAP and sends a
confirmation email. The AI agent part is making sense of human language and deciding which
workflow to trigger – the rest is handled by traditional automation. This combination is very powerful
for enterprise scenarios and Microsoft's ecosystem is well-suited for it (with Power Platform, Azure
Logic Apps, etc., all integrable). In a way, the AI agent is the brain deciding what needs to be done,
and the existing automation is the muscle executing the task reliably.


- **Semantic Kernel in Azure:** Many Azure solutions incorporate Semantic Kernel for orchestrating
complex interactions with OpenAI. For instance, a developer might use Semantic Kernel to
implement a multi-turn reasoning process with memory (storing intermediate results) and tool use
(SK can define plugins which are analogous to functions the AI can call). This is essentially writing an
agent or even an agentic system. A concrete example: using Semantic Kernel, you could create a
chatbot that, when asked a question, first checks if it should retrieve information from a company
knowledge base (using a search plugin) and then either answers from that or uses the LLM directly.
The kernel would manage prompt construction and tracking the conversation state. With the
Process Framework preview, SK can even manage long-running processes, e.g., an agent that
doesn't just respond in one chat session but maybe monitors a folder for new files and comments on
them (mixing human-in-loop). Azure is the deployment ground for such solutions – you might run
them as an Azure Function or a Web App that users can interact with.


## Summary

In conclusion, AI agents and agentic AI represent a shift from AI as a passive tool to AI as an active
collaborator in work. Microsoft's ecosystem provides real-world examples of both: from the straightforward
agent that helps draft your Word document, to the ambitious agentic frameworks aiming to automate
multi-step business processes. By understanding the difference, organizations and users can better
envision how to leverage these technologies. AI agents can make individuals more productive today, and
agentic AI has the potential to revolutionize entire workflows tomorrow – all while keeping humans in
charge of setting goals and reviewing outcomes. With continued innovation (and responsible development),
the line between what work is done by humans and what is handled by AI assistants will keep shifting,
ideally freeing us up to focus on creativity, strategy, and the uniquely human aspects of our work.
The era of having a “copilot” for every task is just beginning, and Microsoft's tools are at the forefront of
making that a practical reality.

## References

- [Introducing Microsoft 365 Copilot – your copilot for work - The Official Microsoft Blog](https://blogs.microsoft.com/blog/2023/03/16/introducing-microsoft-365-copilot-your-copilot-for-work/?WT.mc_id=M365-MVP-5003693)

- [Using Azure AI Agent Service with AutoGen / Semantic Kernel to build a multi-agent's solution - Microsoft Community Hub](https://techcommunity.microsoft.com/blog/educatordeveloperblog/using-azure-ai-agent-service-with-autogen--semantic-kernel-to-build-a-multi-agen/?WT.mc_id=M365-MVP-5003693)

- [Microsoft's Agentic Frameworks: AutoGen and Semantic Kernel - AutoGen Blog](https://devblogs.microsoft.com/autogen/microsofts-agentic-frameworks-autogen-and-semantic-kernel/?WT.mc_id=M365-MVP-5003693)

- [Extend Microsoft 365 Copilot with agents - Microsoft Copilot Studio - Microsoft Learn](https://learn.microsoft.com/en-us/microsoft-copilot-studio/microsoft-copilot-extend-copilot-extensions?WT.mc_id=M365-MVP-5003693)

- [Extend the capabilities of your agent - Microsoft Copilot Studio - Microsoft Learn](https://learn.microsoft.com/en-us/microsoft-copilot-studio/copilot-connectors-in-copilot-studio?WT.mc_id=M365-MVP-5003693)

- [Agentic AI vs. generative AI: The core differences - Thomson Reuters](https://www.thomsonreuters.com/en/insights/articles/agentic-ai-vs-generative-ai-the-core-differences)

- [Agentic AI Vs AI Agents: 5 Differences and Why They Matter - Moveworks](https://www.moveworks.com/us/en/resources/blog/agentic-ai-vs-ai-agents-definitions-and-differences)
