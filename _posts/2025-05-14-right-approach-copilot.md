---
title: "Choosing the Right Approach to Build Your Copilot"
date: "2025-05-14"
share: true
header:
  image: media/2025-05-14-right-approach-copilot/01.png
  teaser: media/2025-05-14-right-approach-copilot/01.png
categories:
  - Copilot
  - Microsoft 365
tags:
  - "2025"
  - May 2025
last_modified_at: 2025-05-14T00:00:00-00:00
---
**Declarative Agents, Copilot Studio, and Azure OpenAI**

## Introduction

If you're planning to build your own Copilot (AI assistant), there are different ways to do it depending on how much control you need, what systems you want to connect, and how advanced your solution needs to be.

Microsoft offers three main ways to build these AI assistants:

1. **Declarative Agents** — easiest, built inside Microsoft 365  
2. **Copilot Studio Agents** — more flexible, can connect to outside systems  
3. **Azure OpenAI-based Agents** — most powerful, fully customized AI solutions

    ![](/media/2025-05-14-right-approach-copilot/01.png)

Let's understand each option in simple terms.

---

## 1. Declarative Agents (Built into Microsoft 365 Copilot)

### What It Is

This is the easiest option. It's built into Microsoft 365 apps like Outlook, Teams, and SharePoint. You just tell the system *what* you want, and it figures out *how* to do it.

- No coding needed
- Works with your existing Microsoft 365 data
- Uses things like plugins and Graph API behind the scenes

### Good For

- Showing documents from SharePoint or OneDrive  
- Answering questions about company policies  
- Small tasks like meeting notes or task summaries

### Pros

- Very quick to set up (takes just a few days)  
- No extra infrastructure or software  
- Safe and secure (runs inside Microsoft 365)

### Cons

- Only works with Microsoft 365 tools  
- Hard to connect with other systems  
- Not customizable beyond a point

---

## 2. Copilot Studio Agents

### What It Is

This option gives you more flexibility. You use a tool called Copilot Studio (previously Power Virtual Agents) to build chatbots that can talk to outside systems like SAP, Salesforce, or your own databases.

- Low-code to pro-code (easy or advanced, your choice)  
- You can design your chatbot's flow and actions  
- Can be embedded in Teams, websites, or apps

### Good For

- HR bots that pull data from Workday, SAP, SharePoint  
- Sales assistants showing data from Salesforce  
- Customer support bots connected to CRM tools

### Pros

- Connects with many systems  
- More control over the user experience  
- Can run inside Microsoft 365 or outside it

### Cons

- Licensing fees apply  
- May be slower for very complex flows  
- Needs planning for large-scale usage

---

## 3. Azure OpenAI-based Custom Agents

### What It Is

This is the most powerful option. You use Azure and OpenAI models (like GPT-4) to create fully custom AI agents. You control everything — the data, the model, the design, and where it runs.

- Built and hosted in your own Azure environment  
- You can use your own data and tune the AI to your needs  
- Supports advanced AI techniques like RAG (Retrieval-Augmented Generation)

### Good For

- Custom-branded AI assistants on websites or mobile apps  
- Advanced use cases like legal, finance, research, etc.  
- Complex automation tasks that go beyond Microsoft 365

### Pros

- Full control over the look, behavior, and AI model  
- Can run anywhere (not tied to Microsoft 365)  
- Great for large companies or unique business needs

### Cons

- Needs technical skills (AI, Azure, APIs, security)  
- Takes more time to build (weeks or months)  
- Higher cost due to Azure usage  
- Requires strict data security and governance

---

## Comparison at a Glance

| Feature                  | Declarative Agents | Copilot Studio Agents | Azure OpenAI Agents     |
|--------------------------|--------------------|------------------------|--------------------------|
| **Skill Level Needed**   | Beginner            | Intermediate            | Advanced                  |
| **Customization**        | Low                | Medium                  | Very High                 |
| **Data Integration**     | Microsoft 365 only | M365 + External Systems | Any Data or System       |
| **Where It Runs**        | Microsoft 365       | Microsoft-hosted        | Your Azure environment   |
| **Best For**             | Quick tasks         | Business processes       | Deep custom AI solutions |
| **Time to Build**        | Days               | Weeks                   | Weeks or Months          |

---

## Conclusion

### Which One Should You Use?

- **Use Declarative Agents** if you're staying inside Microsoft 365 and need quick results.  
- **Use Copilot Studio Agents** if you need to connect to external systems or build more interactive bots.  
- **Use Azure OpenAI-based Agents** if you need full control, customization, and advanced AI power.

### You Can Combine Them Too

Start simple with Declarative Agents, and as your needs grow, add Copilot Studio or Azure OpenAI solutions to cover more complex scenarios.
