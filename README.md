# AI Builder in a Day

A hands-on workshop that takes you from zero to three working AI solutions on the Microsoft Power Platform and M365 stack. You will use prebuilt AI models, build multiple Copilot Studio agents to compare knowledge sources head-to-head, and ship a declarative agent that lives inside M365 Copilot.

**Total time:** ~2 hours 35 minutes of lab work (plus breaks and discussion)
**Audience:** Makers, citizen developers, and IT pros who want practical experience with AI Builder, Copilot Studio, and M365 Copilot agents.

---

## What you will need

Before you start any of the labs, make sure you have:

- A Microsoft 365 work or school account (personal Microsoft accounts do not work here)
- Access to [Power Automate](https://make.powerautomate.com) and [Copilot Studio](https://copilotstudio.microsoft.com)
- An **AI Builder trial** activated (Lab 1). The trial usually appears in the AI Hub on first visit and activates in ~60 seconds
- A **SharePoint site** you can upload files to (Labs 2 and 3)
- A modern browser. Edge or Chrome recommended
- Optional for Lab 2: an Azure subscription if you want to build your own AI Search index
- Optional for Lab 3: an M365 Copilot license to test the agent in the full Copilot experience. Without a license you can still build and preview the agent in Agent Builder

Each lab has a short prerequisites check at the top. Run through it before diving in.

---

## The labs

### [Lab 1 — Invoice Processing with Prebuilt AI](./Lab1/lab-guide.md)

**Duration:** 45 minutes · **Difficulty:** Beginner

Build a Power Automate flow that accepts an invoice PDF, runs it through AI Builder's prebuilt invoice model, formats the extracted fields (invoice ID, vendor, totals, tax, confidence scores) into an HTML table, and emails the result back to you with the original PDF attached. You then explore Document Intelligence Studio to see what the underlying models can and cannot do on receipts, IDs, and free-form layouts.

**Sample file:** `Lab1/sample-invoice-contoso.pdf`

**Key takeaways:**
- Using prebuilt AI Builder actions with no training required
- Reading and reasoning about confidence scores
- Routing low-confidence results to human review (stretch goal uses SharePoint)

---

### [Lab 2 — The Knowledge Source Shootout](./Lab2/lab-guide.md)

**Duration:** 70 minutes · **Difficulty:** Intermediate

Build **five Copilot Studio agents** with the *same* tax Q&A content delivered in five *different* formats — PDF, Excel, a folder of individual Word docs, a SharePoint site, and an Azure AI Search index. Ask each agent the same three questions and record the answers side-by-side. You will see firsthand why the *format* of your knowledge source can matter more than its content, and how hallucinations show up when retrieval goes wrong.

**Sample files:** `Lab2/FinanceQuestions.pdf`, `Lab2/FinanceQuestions.xlsx`, and the `Lab2/FinanceQuestions/` folder of individual Word documents.

**Pacing note:** Create all five agents and start them indexing *first* (indexing takes 5–20 minutes per source), then test them in order as they become ready. Agents 4 (SharePoint) and 5 (AI Search) are optional if you are short on time or lack Azure access.

**Key takeaways:**
- A repeatable methodology for picking a knowledge source
- How PDFs, spreadsheets, docs, SharePoint, and AI Search each behave under the same prompts
- Bonus: publish the winning agent to Microsoft Teams

---

### [Lab 3 — Declarative Agent for M365 Copilot](./Lab3/lab-guide.md)

**Duration:** 40 minutes · **Difficulty:** Beginner to Intermediate

Build a **declarative agent** — a scoped, customized version of M365 Copilot — that lives *inside* the Copilot chat your users already have open. You will upload policy documents to SharePoint, configure the agent in Agent Builder (name, description, instructions, knowledge sources, conversation starters), publish it, and test it alongside regular M365 Copilot to see the grounding difference. The lab closes with a quick look at **Agents Toolkit** in VS Code for when you outgrow the no-code experience.

**Sample file:** `Lab3/Travel_Expense_Policy_SAMPLE.pdf` (bring 2–4 more of your own policy-style documents for the best experience)

**Key takeaways:**
- The difference between a Copilot Studio agent and a declarative agent
- SharePoint grounding (including security trimming that respects document permissions)
- The no-code → pro-code upgrade path via Agents Toolkit

---

## How the labs fit together

The three labs walk up the AI-on-Microsoft ladder:

1. **Lab 1** — use AI someone else trained (prebuilt models, no data of your own)
2. **Lab 2** — point AI at *your* data and learn how the format shapes the outcome
3. **Lab 3** — embed *your* AI inside the tool your users already live in (M365 Copilot)

You can do them in order for the full arc, or cherry-pick the one that maps to your current project. Each lab is self-contained.

---

## Repository layout

```
.
├── Lab1/
│   ├── lab-guide.md
│   └── sample-invoice-contoso.pdf
├── Lab2/
│   ├── lab-guide.md
│   ├── FinanceQuestions.pdf
│   ├── FinanceQuestions.xlsx
│   └── FinanceQuestions/          (individual Word docs, one Q&A per file)
├── Lab3/
│   ├── lab-guide.md
│   └── Travel_Expense_Policy_SAMPLE.pdf
└── README.md
```

---

## Tips before you start

- **Run through the prerequisites section of each lab first.** Trial activations and SharePoint setup are the most common blockers and the easiest to fix in advance.
- **Watch your AI Builder credits.** The trial pool is limited. Don't loop-test the invoice flow on 100 files.
- **Indexing is not instant.** Especially in Lab 2 and Lab 3, give knowledge sources time to index before testing. The agent will answer during indexing, but the results will be misleading.
- **Microsoft renames things.** If an action or menu item has a slightly different name than what the lab shows, search for it by keyword. The underlying capability is almost always still there.

Have fun. Break things on purpose. Ask why.
