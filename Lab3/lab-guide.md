# Lab 3: Declarative Agent for M365 Copilot

**Time:** 40 minutes
**Difficulty:** Beginner to intermediate
**Tools:** Agent Builder, SharePoint, M365 Copilot

---

## The story

I have built agents in Copilot Studio, Power Virtual Agents, Bot Framework. You name it, I have shipped it. The number one piece of feedback I always get from end users? "Where do I find it?"

They don't want another app. They don't want another URL. They don't want another Teams bot they will forget about in two weeks. They want their AI assistant to be right there, in the tool they already have open eight hours a day.

That is what a declarative agent is. It lives inside M365 Copilot. Your users open Copilot, pick your agent from the list, and start chatting. No new app. No change management nightmare. No "where was that bot again?" conversations.

> **"Your agent lives where your users already work. No new app to sell."**

---

## What does "declarative" mean?

Before we dive in, plain English. When you build a declarative agent, you are not writing code that tells the AI HOW to think step by step. Instead, you DECLARE what the agent should be: "Here is your name, here are your instructions, here are the documents you know about." You describe the WHAT, and the platform figures out the HOW. Think of it like filling out a form that says "I want an agent that does X, knows Y, and behaves like Z," and the system builds it for you. That is declarative: you declare your intent, the platform delivers.

---

## What is a declarative agent?

Let me be direct about this because the naming is confusing.

| | Copilot Studio Agent | Declarative Agent |
|---|---|---|
| **Where it lives** | Teams, websites, custom channels | Inside M365 Copilot chat |
| **How users find it** | Separate bot or app | Agent picker in Copilot |
| **Knowledge grounding** | Uploaded docs, SharePoint, Dataverse | SharePoint, Graph connectors, M365 data |
| **Custom actions** | Power Automate, connectors, plugins | Requires Agents Toolkit (VS Code) |
| **Best for** | Standalone agent experiences | Extending M365 Copilot with domain expertise |
| **Build tool** | Copilot Studio | Agent Builder (simple) or Agents Toolkit (advanced) |

A declarative agent is essentially a scoped, customized version of M365 Copilot. You are telling Copilot: "When the user picks this agent, follow these instructions and use these specific knowledge sources." You are declaring what the agent should be, hence "declarative."

---

## What you will build

A company policy agent that lives inside M365 Copilot. Users select it and ask questions about company policies, grounded in your SharePoint documents. No hallucination, no generic answers, just your content.

---

## Part A: Prepare SharePoint content (5 minutes)

Our agent needs something to be smart about. Let's give it documents.

### Step 1: Navigate to SharePoint

1. Open [sharepoint.com](https://sharepoint.com) or go to your existing SharePoint site
2. Navigate to a **Document Library** (or create a new one)
   - To create a new library: Click **+ New** then **Document library** then name it `Company Policies`


### Step 2: Upload documents

Upload 3 to 5 policy documents. You can use:
- Company policy docs you brought with you
- Sample documents from this lab
- Any Word or PDF files about company policies, product info, or procedures

Good examples:
- Employee handbook
- Travel expense policy
- Remote work policy
- Code of conduct
- IT security guidelines

1. Click **Upload** then **Files**
2. Select your documents
3. Wait for them to upload


### Step 3: Copy the library URL

1. Copy the URL of your document library from the browser address bar
2. Keep this handy. You will need it in Part B

> The URL should look something like: `https://yourtenant.sharepoint.com/sites/YourSite/Shared Documents`

---

## Part B: Build the declarative agent in Agent Builder (20 minutes)

### Step 1: Open Agent Builder

You have several ways to get there.

**Option A:** Go to [copilot.microsoft.com](https://copilot.microsoft.com) (make sure you are signed in with your work account, not personal). Click on **Agents** in the left sidebar or top navigation.

**Option B:** Go to [microsoft365.com/copilot](https://microsoft365.com/copilot) and look for the agent builder option.

**Option C:** If you can't find the Agents section in either of those, try [copilot.microsoft.com/agents](https://copilot.microsoft.com/agents) directly, or search "Agent Builder" in the M365 app launcher (the waffle menu; the grid of dots in the top-left corner of any M365 page).


### Step 2: Create a new agent

1. Click **Create agent** (or **+ New agent**)
2. You land in the agent configuration screen


### Step 3: Configure the basics

Fill in the following.

**Name:** `Company Policy Expert`

**Description:** `Ask me anything about company policies, from travel expenses to remote work guidelines. I answer based on our actual policy documents.`

**Instructions** (this is the system prompt that shapes your agent's behavior):

```
You are a company policy expert for Contoso. Your role is to help employees find and understand company policies.

Rules:
- Always answer based on the documents provided in your knowledge sources. Do not make up policies.
- When citing a policy, mention which document it comes from.
- If a question falls outside the scope of the available policy documents, clearly state that you do not have information about that specific policy and suggest the employee contact HR directly.
- Be concise but thorough. Employees are busy. Give them the answer, not a novel.
- If a policy has specific dates, thresholds, or amounts, always include those exact numbers. Never approximate.
```

### Step 4: Add knowledge (SharePoint grounding)

This is the key part. You are telling the agent, "When users ask questions, search THESE documents for answers."

1. Scroll to the **Knowledge** section (or click the Knowledge tab)
2. Click **+ Add knowledge source**
3. Select **SharePoint**
4. Paste the URL of the document library you set up in Part A
5. Click **Add** (or **Save**)

**If you do not have a Copilot license then just upload the documents instead of addding a SharePoint site**


The agent will now ground its responses in the content of those SharePoint documents. This means:
- It answers from YOUR documents, not from its general training data
- It can cite which document the answer came from
- If the answer isn't in your documents, it should say so (based on our instructions)

> ---
> **Gotcha: SharePoint permissions.**
>
> The declarative agent respects SharePoint permissions. If a user doesn't have access to a document in that library, the agent won't use that document to answer their questions. This is actually a feature. It means your agent is security-trimmed out of the box. During this lab, make sure your test account has read access to the library you configured.
>
> ---

### Step 5: Configure conversation starters

Conversation starters are the suggested prompts users see when they first open your agent. They reduce the "blank page" problem.

1. Find the **Conversation starters** section
2. Add 3 to 4 starters:
   - `What is our remote work policy?`
   - `How do I submit a travel expense report?`
   - `What are the rules around taking time off?`
   - `Summarize the code of conduct`


### Step 6: Test in the preview pane

Agent Builder has a built-in test panel. Use it.

1. Look for the **Preview** or **Test** pane (usually on the right side)
2. Click one of your conversation starters, or type a question
3. Try these:
   - `What is the maximum amount for travel expenses without manager approval?`
   - `Can I work from home on Fridays?`
   - `What happens if I violate the code of conduct?`
4. Check that the responses:
   - Reference your actual documents (not generic knowledge)
   - Include citations or source references
   - Respect the tone you set in the instructions


> **Pro tip:** Ask a question that you know is NOT in your documents. A well-configured agent should say something like "I don't have information about that in the available policy documents." If it makes something up, revisit your instructions and be more explicit about staying grounded.

---

## Part C: Publish and use in M365 Copilot (10 minutes)

### Step 1: Publish the agent

1. Click the **Publish** button (top-right of Agent Builder)
2. Choose your audience:
   - **Just me.** Only you can see and use it (good for testing)
   - **My organization.** Everyone in your tenant can find it (requires admin approval in some tenants)
3. For this lab, start with **Just me**
4. Click **Publish**


> ---
> **Gotcha: Publishing propagation delay.**
>
> After publishing, it can take **2 to 5 minutes** for the agent to appear in the M365 Copilot agent picker. If you don't see it immediately, don't panic. Don't re-publish. Just wait.
>
> ---

### Step 2: Open M365 Copilot

1. Go to [copilot.microsoft.com](https://copilot.microsoft.com) or open Copilot in Teams
2. You should see the M365 Copilot chat interface

### Step 3: Select your agent

1. Look for the **agent picker**. It is usually a dropdown or icon near the chat input, or in the right rail
2. Find **Company Policy Expert** in the list
3. Select it. The chat interface should now indicate you are talking to your custom agent


> If your agent doesn't appear yet, remember the propagation delay. Give it a few minutes.

### Step 4: Test the full experience

Run through a real conversation:

1. **Ask:** `What is our policy on remote work?`
   - Agent should respond with details from your SharePoint document
   - Look for citations or references to the source document
2. **Follow up:** `Are there any exceptions to this policy?`
   - Agent should maintain context and dig deeper into the document
3. **Try an edge case:** `What is the company's policy on cryptocurrency trading?`
   - Unless you uploaded a doc about this, the agent should indicate it does not have that information

### Step 5: Compare with regular M365 Copilot

This is the moment that sells it.

1. Switch back to the default M365 Copilot (deselect your agent)
2. Ask the same question: `What is our policy on remote work?`
3. Compare the responses:
   - **Regular Copilot:** May give a generic answer or say it doesn't know your company's specific policy
   - **Your agent:** Gives a specific, grounded answer from your actual policy documents

That difference is the value of a declarative agent. Same Copilot interface, but now it actually knows YOUR stuff.

---

## Part D: What is next. Agents Toolkit (5 minutes, lecture or demo)

**This section is a brief walkthrough. No hands-on work required.**

You have built a declarative agent with Agent Builder. It is great for knowledge-grounded Q&A. What if you want your declarative agent to TAKE ACTIONS, like the Copilot Studio agent we built in Lab 3?

That is where the **Agents Toolkit** comes in.

### What is Agents Toolkit?

Agents Toolkit (formerly Teams Toolkit, recently expanded) is a VS Code extension that lets you build more advanced declarative agents, ones with custom API plugins and actions.

> **Finding it in VS Code:** In the VS Code Extensions marketplace, search "Teams Toolkit" or "Agents Toolkit." Extension ID: `TeamsDevApp.ms-teams-vscode-extension`

### What does the project look like?

An Agents Toolkit declarative agent project has a few key files:

```
my-agent/
|-- appPackage/
|   |-- manifest.json          (Defines the agent for M365)
|   |-- declarativeAgent.json  (Instructions, capabilities, knowledge)
|   |-- plugin.json            (API plugin definition for actions)
|-- apiSpecificationFile/
|   |-- openapi.yaml           (OpenAPI spec for your custom API)
```

- **`declarativeAgent.json`** is where you define instructions (like the system prompt in Agent Builder) and knowledge sources
- **`plugin.json`** and **`openapi.yaml`** is how you add custom actions. The agent can call your APIs

### The upgrade path

Think of it as a progression:

```
Agent Builder (no code)  ->  Agents Toolkit (pro-code)
     |                              |
     | Knowledge + Instructions     | Knowledge + Instructions
     | Conversation starters        | Conversation starters
     |                              | + Custom API actions
     |                              | + Complex logic
     |                              | + Full source control
     |                              | + CI/CD pipeline
```

**Start** with Agent Builder when you just need knowledge grounding and instructions. **Move** to Agents Toolkit when you need custom actions, complex API integrations, or want your agent definition in source control.

> I won't make you set up a whole VS Code project right now. We have a conference to enjoy. But you have seen the path. Agent Builder for quick wins, Agents Toolkit when you need more muscle.

---
