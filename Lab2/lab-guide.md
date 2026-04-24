# Lab 2: The Knowledge Source Shootout

**Duration:** 70 minutes
**Difficulty:** Intermediate

---

## Overview

Almost nobody talks about this at conferences: the FORMAT of your knowledge source matters as much as the CONTENT. I learned that the hard way.

I built a simple tax agent. Same questions, same answers, same data. I fed it to Copilot Studio in five different formats: a PDF, an Excel file, a folder of Word docs, a SharePoint site, and an AI Search index. Then I asked all five agents the same question, "How do I need to pay VAT?"

I got five different answers. Some were great. Some were okay. One confidently told me the VAT rate was 21% when the correct answer was 16%. That is a hallucination: the AI generates something that sounds confident but is factually wrong. It is not "lying," it just doesn't know it is wrong. That is not a quirky demo fail. That is an audit finding.

In this lab you run the same experiment. Build five agents, test them with identical questions, and see for yourself which knowledge source format works best. "It depends" is not an answer your stakeholders want. They want data. That is what we produce here.

**What you will build:** Five Copilot Studio agents, each using the same tax Q&A data in a different format. Test each with the same questions. Compare the results in a structured matrix.

**Business scenario:** You are the team lead who has been told to "build a chatbot for our internal knowledge base." Before you pick a knowledge source, you need to know which format actually works for YOUR data. This lab gives you the methodology.

### Lab pacing

This lab is structured differently from Lab 1. Instead of build-test-build-test one at a time, we batch the setup to avoid waiting on indexing. Indexing is when the system reads and processes your uploaded files so the agent can search through them. It takes 5 to 20 minutes per source.

- **First 10 minutes:** Create ALL 5 agents with just names and instructions. Upload ALL 5 knowledge sources. Start indexing everything simultaneously.
- **While indexing (around 10 minutes):** Rick walks through the comparison framework and shows pre-built examples.
- **Next 30 minutes:** Test agents as they become indexed. PDF and Excel usually index fastest. Start there.
- **After Agent 3 testing:** You can build and test Agents 4 and 5 yourself, or take the shortcut. Rick will show pre-built results for SharePoint and AI Search so you can fill the matrix either way.

---

## Prerequisites Check (3 minutes)

1. **Copilot Studio access.** Go to [copilotstudio.microsoft.com](https://copilotstudio.microsoft.com). You should see the Copilot Studio home screen. If you are prompted to start a trial, do it now. It takes about 2 minutes.

2. **SharePoint site ready.** You should have a SharePoint site called something like "AI Workshop [YourName]." We use this for the SharePoint knowledge source (Agent #4).

3. **Workshop materials.** Confirm you have the `lab-02-knowledge-sources` folder in your workshop materials. It contains:
   - `tax-qa-complete.pdf`
   - `tax-qa-complete.xlsx`
   - A subfolder `tax-qa-individual-docs` with 10 Word files

4. **AI Search index (optional).** AI Search is Microsoft's enterprise search service that understands meaning, not just keywords. It is the most advanced option. If you have an Azure subscription, create your own index. If not, you can skip this part. 

---

## The test data

I have pre-prepared 10 Belgian-style tax Q&A pairs. Same data, different formats

---

## Part A: Create all agents and start indexing (10 minutes)

**Important:** Create all agents and upload all knowledge sources NOW so everything indexes in parallel. Don't test yet. Just create and upload.

### Agent #1: PDF Knowledge

This first agent takes the most time because we walk through every click. The next four follow the same pattern with different sources, so they go faster.

#### Step 1: Create a new agent

1. In Copilot Studio, click **+ Create** in the left navigation, then select **New agent**.

2. You see the agent creation wizard. You have two options: the guided "describe your agent" experience, or skipping to manual configuration. Click **Skip to configure** at the top.

3. Fill in the basics:
   - **Name:** `Tax Agent - PDF`
   - **Description:** `Tax Q&A agent using PDF knowledge source`
   - **Instructions:** Type: `You are a helpful tax advisor. Answer questions about tax rates, VAT, deductions, and filing requirements based only on the knowledge provided to you. If you don't know the answer, say so. Do not guess or make up information. Always cite which part of your knowledge the answer comes from.`

4. Click **Create** at the top-right.

#### Step 2: Add the PDF as a knowledge source

1. Your agent is created. You should be on the agent's overview page. Click **Knowledge** in the left panel (or find the Knowledge section on the overview).

2. Click **+ Add knowledge**.

3. You see options for knowledge sources. Select **Files**.

4. Click **Browse** or drag and drop your `FinanceQuestions.pdf` file.

5. Wait for the upload to complete. A progress indicator appears.

6. Once uploaded, click **Add** to confirm.

> **Gotcha: Indexing lag.** After you add a knowledge source, Copilot Studio needs to index it. Remember, indexing means the system is reading and processing your file so the agent can search it. This can take 5 to 20 minutes. You see a status like "Processing" or "Indexing." The agent CAN answer questions during indexing, but the results may be incomplete. Don't wait here. Immediately move on to creating the next agent.

### Agents #2 through #5: Batch creation

Create the remaining four agents as fast as you can. Same process, different names and knowledge sources. Don't test yet. Just create and upload.

1. **Agent #2 (Excel):** Create an agent named `Tax Agent - Excel`, same instructions, upload `FinanceQuestions.xlsx` as Files knowledge source.

2. **Agent #3 (Individual Docs):** Create an agent named `Tax Agent - Individual Docs`, same instructions, upload ALL 10 Word documents from `FinanceQuestions` folder as Files knowledge source.

3. **Agent #4 (SharePoint):** Create an agent named `Tax Agent - SharePoint`, same instructio ns, add your SharePoint site as the knowledge source (see Part D below for SharePoint steps if needed). If your SharePoint site is not ready, skip this one and come back.

4. **Agent #5 (AI Search):** Create an agent named `Tax Agent - AI Search`, same instructions, connect to the AI Search index (see Part E below for connection details). If the shared index is unavailable, skip this.

> **You should now have 5 agents all indexing simultaneously.**

---

## Part B: Test your agents (30 minutes)

Check your agents' indexing status. PDF and Excel usually finish first. Start testing whichever is ready. Use the same three test questions for every agent.

### How to test each agent

1. On the agent page, look for the **Test your agent** panel on the right side. If it is collapsed, click the chat bubble icon or **Test** button to open it.

2. Check that the knowledge source status shows "Ready" (green checkmark). If it still says "Indexing," wait a few more minutes.

3. Type the first test question:

   > How do I need to pay VAT?

4. Wait for the response. Write down:
   - Did it answer the question? (Yes or No)
   - Is the answer accurate? 
   - Did it provide a reference or citation to the PDF?
   - Did it hallucinate any details not in the source?

5. Type the second test question:

   > I'm a freelance designer. What can I write off?

6. Note the same things.

7. Type the third test question:

   > If I sell software to a German company, do I charge VAT?

8. Note the same things. 

---

### Testing Agent #1 (PDF)

Open the `Tax Agent - PDF` agent in Copilot Studio and run the three test questions. Record your results.

### Testing Agent #2 (Excel)

Open the `Tax Agent - Excel` agent. Run the same three questions.

Pay attention to whether the agent handles the tabular format differently. Does it quote the Question column? Does it combine information across rows?

> **Gotcha:** Excel files get parsed differently depending on whether you have headers, merged cells, or multiple sheets. Keep your Excel clean. One sheet, clear headers in row 1, no merged cells. Our sample file is already clean, but remember this for your real projects.

### Testing Agent #3 (Individual Docs)

Open the `Tax Agent - Individual Docs` agent. Run the same three questions.

Pay attention: does the agent cite WHICH document it pulled the answer from? With individual files, you often get cleaner references. "Based on Q7.docx" is more useful than "FinanceQuestions.pdf, page 4."

---

> **Shortcut checkpoint:** After testing Agents 1 through 3, you have enough data to see the pattern. If you are running low on time, you can skip building and testing Agents 4 and 5.

---

### Testing Agent #4 (SharePoint)

If you have not set up the SharePoint knowledge source yet, here are the SharePoint steps.

1. Go to your workshop SharePoint site.

2. Upload the `financeQuestions.pdf` and all 10 Word docs to a document library.

3. In Copilot Studio, open `Tax Agent - SharePoint`. Go to **Knowledge** then **+ Add knowledge** then select **SharePoint**.

4. Paste your SharePoint site URL (e.g., `https://yourtenant.sharepoint.com/sites/AIWorkshop-YourName`).

5. Copilot Studio connects and shows available libraries and pages. Select the library

6. Click **Add**. SharePoint sources can be the slowest to index because Copilot Studio needs to crawl and process the site.

7. Once indexed, run the same three test questions. Record results.

   Watch for this: SharePoint knowledge often includes the site structure, page titles, and metadata in its context. This can help (more context) or hurt (more noise). Note which.

---

### Testing Agent #5 (AI Search)

> **Important:** Agent #5 requires either the shared workshop AI Search index or your own Azure subscription.

#### Use your own Azure AI Search

If you have an Azure subscription and want to set it up yourself:

1. Go to the Azure portal, create an Azure AI Search resource (Free tier works).
2. Create an index and upload the tax Q&A data using the Import Data wizard.
3. Enable **Semantic search** on the index. This significantly improves results.
4. Use the endpoint and API key from your resource.


#### Test it

1. Run the same three test questions. Record results.

2. AI Search with semantic search typically gives the best results because it understands meaning, not just keywords. It also requires the most setup and an Azure subscription. Note the trade-off.

---

## Discussion: Which won? Why? When would you use each?

Here is what I typically see when I run this experiment.

**PDF** works well for Q1 (direct match) but can struggle with Q3 (inference). The model sees one big wall of text and sometimes has trouble isolating the relevant section.

**Excel** is hit or miss. The tabular format is great when the question maps cleanly to a row, but the model sometimes ignores the Answer column and tries to infer from the Question column alone. It also doesn't handle combining information across rows well.

**Individual Docs** often surprises people by performing well. Each document is a focused chunk about one topic. The retrieval step can pick the right document more easily, and there is less noise. The citations are also cleaner.

**SharePoint** is the most "realistic" enterprise source but adds noise from site structure and metadata. Results depend heavily on how well the SharePoint content is organized.

**AI Search** (with semantic search) typically wins on accuracy because semantic search understands meaning. "What can I write off?" matches "deductible expenses" even though the words are different. It requires Azure, costs money, and takes more setup.

**The real answer:** It depends on your scenario. Now you have a methodology to test it instead of guessing.

---

## Bonus: Publish the best agent to Teams (5 minutes)

Pick your best-performing agent and put it somewhere people can actually use it.

1. In Copilot Studio, open your winning agent.

2. Click **Channels** in the left navigation.

3. Click **Microsoft Teams**.

4. Click **Turn on Teams** (or **Publish** if it is already configured).

5. Click **Open in Teams** to test it live. Your agent appears as a chat bot in Teams.

6. Share it with your neighbor. Have them ask it a question and see if the answer holds up.

---