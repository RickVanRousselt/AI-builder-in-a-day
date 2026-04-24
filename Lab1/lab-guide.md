# Lab 1: Invoice Processing with Prebuilt AI

**Duration:** 45 minutes
**Difficulty:** Beginner

---

## Overview

Anyone who runs a small company knows what invoice day looks like. A stack of PDFs arrives, you squint at numbers, type them into your accounting system, eventually fat-finger the VAT amount, and then your accountant sends passive-aggressive emails.

AI Builder has a prebuilt model that reads invoices. It pulls out the invoice ID, the date, the totals, the line items, even the vendor address. It also tells you how confident it is about each field. Today you will build a Power Automate flow that drops an invoice in, lets AI read it, and emails back a formatted table with the results.

**What you will build:** A Power Automate flow that takes a sample invoice, extracts all key fields using AI Builder's prebuilt invoice model, formats them into an HTML table, and emails the results to you with the original invoice attached.

**Business scenario:** Accounts Payable automation. Your AP team processes hundreds of invoices a month. Even partial automation (extracting and validating key fields) saves hours and reduces errors.

---

## Prerequisites Check (2 minutes)

Before you start, verify each of these.

1. **Power Automate access.** Go to [make.powerautomate.com](https://make.powerautomate.com). You should see the Power Automate home screen with your environment listed in the top-right corner.

2. **AI Builder trial active.** In the left navigation, click **AI Builder** (or **AI Hub**). You should NOT see a banner saying "Start your AI Builder trial." If you do see that banner, click it now and activate the trial. It takes about 60 seconds.

   > **Note:** The trial may appear as "AI Builder trial" or as part of a "Copilot Studio trial" depending on your tenant. Either one gives you the credits needed.

3. **Sample invoice ready.** Check your workshop materials folder. You should have a file called `sample-invoice-contoso.pdf`.

---

## Part A: Build the Power Automate Flow (20 minutes)

### Step 1: Create a new Instant Cloud Flow

1. From the Power Automate home screen, click **+ Create** in the left navigation panel.


2. On the Create screen, click **Instant cloud flow**.
]

3. A dialog pops up. Fill it in as follows:
   - **Flow name:** `Lab 1 - Invoice Processing`
   - **Choose how to trigger this flow:** Select **Manually trigger a flow**
   - Click **Create**


> **Why manual trigger?** In production you would trigger this when an email arrives with an attachment, or when a file lands in a SharePoint document library. We use a manual trigger for testing so we don't have to wait for emails. You can swap the trigger later.

### Step 2: Add a file input to the manual trigger

1. You should see the flow designer with a single step: **Manually trigger a flow**. Click on it to expand it.

2. Click **+ Add an input** at the bottom of the trigger card.

3. From the input type options, select **File**.

4. A file input appears. Leave the label as **File** or rename it to **Invoice**. This is where you upload the invoice when testing.

### Step 3: Add the AI Builder "Extract information from invoices" action

1. Click the **+** button below the trigger step, then click **Add an action**.

2. In the search bar, type `Extract information from invoices`.

3. Click on the **Extract information from invoices** action. It has the AI Builder icon (a small brain or sparkle icon). This is the prebuilt model. You do not need to train anything.

   > **Can't find it?** If you don't see this exact action, type `invoice` in the action search bar instead. The AI Builder invoice processing action should appear regardless of its exact display name. Microsoft occasionally renames these actions.

4. Configure the **Invoice File** input:
   - Click in the **Invoice File** field
   - In the dynamic content panel on the right, under **Manually trigger a flow**, select **File Content**

5. Leave **Pages** empty (it will process all pages). Leave the model version at the default.

> **Gotcha:** You might see two similar actions, "Extract information from invoices" and "Extract information from documents." Pick the **invoices** one. The documents one is a general-purpose model. The invoices one is specifically trained on invoice layouts and knows about fields like "InvoiceTotal" and "VendorName" out of the box.

### Step 4: Add a Compose action to format the results

We will format the extracted data into a readable HTML table using the Compose action. A Compose action takes whatever inputs you give it and packages them into a single output. Think of it as a scratchpad step.

1. Click **+** below the AI Builder action, then **Add an action**.

2. Search for `Compose` and select the **Compose** action from the Data Operations connector. A connector is a pre-built bridge that lets Power Automate talk to a specific service.

> **Alternative:** You could use the Create HTML table action, but it requires an array expression. The Compose approach is simpler for this lab.

3. In the **Inputs** field of the Compose action, build the email body as HTML. Click in the Inputs field and type the HTML structure below. Where you see `[DYNAMIC: field name]`, delete that placeholder and use the dynamic content panel to insert the real field from the AI Builder step.

   ```
   <h2>Invoice Processing Results</h2>
   <table border="1" cellpadding="8" cellspacing="0">
   <tr><th>Field</th><th>Value</th><th>Confidence</th></tr>
   <tr><td>Invoice ID</td><td>[DYNAMIC: Invoice Id value]</td><td>[DYNAMIC: Invoice Id confidence score]</td></tr>
   <tr><td>Invoice Date</td><td>[DYNAMIC: Invoice Date value]</td><td>[DYNAMIC: Invoice Date confidence score]</td></tr>
   <tr><td>Vendor Name</td><td>[DYNAMIC: Vendor Name value]</td><td>[DYNAMIC: Vendor Name confidence score]</td></tr>
   <tr><td>Vendor Address</td><td>[DYNAMIC: Vendor Address value]</td><td>[DYNAMIC: Vendor Address confidence score]</td></tr>
   <tr><td>Customer Name</td><td>[DYNAMIC: Customer Name value]</td><td>[DYNAMIC: Customer Name confidence score]</td></tr>
   <tr><td>Subtotal</td><td>[DYNAMIC: Subtotal value]</td><td>[DYNAMIC: Subtotal confidence score]</td></tr>
   <tr><td>Total Tax</td><td>[DYNAMIC: Total Tax value]</td><td>[DYNAMIC: Total Tax confidence score]</td></tr>
   <tr><td>Invoice Total</td><td>[DYNAMIC: Invoice Total value]</td><td>[DYNAMIC: Invoice Total confidence score]</td></tr>
   <tr><td>Amount Due</td><td>[DYNAMIC: Amount Due value]</td><td>[DYNAMIC: Amount Due confidence score]</td></tr>
   </table>
   ```

4. To insert each dynamic value:
   - Place your cursor where the `[DYNAMIC: ...]` placeholder is
   - Delete the placeholder text
   - Click in the dynamic content panel on the right
   - Scroll through the fields from **Extract information from invoices**. You will see pairs like "Invoice Id value" and "Invoice Id confidence score"
   - Click the correct one


> **Gotcha:** The dynamic content panel can be overwhelming. The invoice model returns a LOT of fields. Use the search box at the top of the panel and type the field name (for example, "Invoice Id") to filter. The fields you want are under the "Extract information from invoices" section header. If you accidentally switch to the Expression tab, click back to the Dynamic content tab.

### Step 5: Add "Get my profile (V2)" action

You need your email address to send the results to yourself.

1. Click **+** below the Compose action, then **Add an action**.

2. Search for `Get my profile` and select **Get my profile (V2)** from the **Office 365 Users** connector.

3. No configuration needed. It grabs your profile. Done.

### Step 6: Add "Send an email (V2)" action

1. Click **+** below the Get my profile action, then **Add an action**.

2. Search for `Send an email` and select **Send an email (V2)** from the **Office 365 Outlook** connector.

3. Configure the email:
   - **To:** Click the field, open dynamic content, select **Mail** from the "Get my profile (V2)" step
   - **Subject:** Type `Invoice Processing Results - Lab 1`
   - **Body:** Click the field, open dynamic content, select **Outputs** from the "Compose" step (this is your HTML table)

4. Attach the original invoice. Click **Show advanced options** at the bottom of the email action.

5. Under **Attachments**, fill in:
   - **Attachments Name - 1:** From dynamic content, select **File name** from "Manually trigger a flow"
   - **Attachments Content - 1:** From dynamic content, select **File Content** from "Manually trigger a flow"

### Step 7: Save the flow

1. Click **Save** in the top-right corner of the designer.

2. If you get warnings about connections, click the affected step, sign in with your Microsoft 365 credentials, and save again.

Your flow is built. Five steps. Let's test it.

---

## Part B: Run It and Verify (5 minutes)

### Run the test

1. In the flow designer, click **Test** in the top-right corner.

2. Select **Manually** and click **Test**.


3. The flow runs and waits for your input. Click the **File** input, browse to your `sample-invoice-contoso.pdf` file, and click **Run flow**.


4. Wait 15 to 30 seconds. Each step lights up green as it completes.

5. Check your email. You should have a new message with subject "Invoice Processing Results - Lab 1" containing:
   - An HTML table with each field, its extracted value, and its confidence score
   - The original PDF attached

### What to look for

- **Confidence scores.** Most fields should be 0.85 or higher on a clean, typed invoice. Handwritten invoices or poor scans score lower.
- **Extracted values.** Compare them to the actual invoice. Is the Invoice Total correct? Did it get the right vendor name?
- **Line items.** We did not extract line items in our Compose step (to keep it simple), but the AI Builder action DID extract them. Click on the AI Builder step in the run history to see the full output, including line items with descriptions, quantities, and amounts.

---

## Part C: Explore Document Intelligence Studio (15 minutes)

Now that you have seen the prebuilt model in action through Power Automate, let's look under the hood. Document Intelligence Studio is where you test models directly, see exactly what they extract, and understand the boundaries of what is possible.

### Step 1: Open Document Intelligence Studio

1. Open a new browser tab and go to [https://contentunderstanding.ai.azure.com/](https://contentunderstanding.ai.azure.com/).

2. You will see the Document Intelligence Studio landing page with a grid of prebuilt models and custom model options.


### Step 2: Test the Invoice model

1. Click the **Invoice** tile under Prebuilt models.

2. You land on the invoice analysis page. It may have a sample invoice already loaded.

3. Click **Browse for files** (or drag and drop) and upload your `sample-invoice-contoso.pdf`.

4. Click **Run analysis** (or it runs automatically).

5. Watch what happens. On the left, the invoice appears with colored bounding boxes around detected fields. On the right, the extracted data appears in a structured panel.

6. Explore the output:
   - Click on different fields in the right panel. The corresponding area on the invoice highlights
   - Open the **Key-value pairs** tab to see everything the model found
   - Open the **Tables** tab. If your invoice has a line items table, it appears here as structured rows and columns
   - Check the **confidence** values next to each field

### Step 3: Try different documents

This is the fun part. Upload different documents and see what the prebuilt models can handle.

1. **Try a receipt.** Go back to the Studio home, click the **Receipt** tile, and upload a photo of a restaurant or store receipt from your phone. See how it extracts merchant name, date, items, total, tip.

2. **Try an ID document.** Click the **ID Document** tile. Upload a photo of your driver's license or passport, or use a provided sample. See how it pulls out name, date of birth, document number, expiration date.

3. **Try the Layout model.** This is the general-purpose one. Upload a document that ISN'T an invoice. A contract, a report, a menu. The Layout model extracts text, tables, and structure from anything.

### What you should take away

- The prebuilt models are very good at the document types they were trained for.
- They struggle with handwritten text, poor scans, unusual layouts, and documents in languages they were not trained on.
- For documents that don't fit the prebuilt models, you would build a **custom model**. That is a different workshop.
- Everything you see in this studio maps directly to the API that Power Automate calls behind the scenes.

---

## Gotchas

> **Confidence scores below 0.8.** Do not automatically trust the extracted value. In a production flow, add a condition: if confidence is below 0.8, route to a human reviewer instead of auto-processing. One wrong decimal in an invoice total can ruin your week.

> **Regional model availability.** Not all AI Builder models are available in all regions. If you run a GCC or sovereign cloud environment, check the [AI Builder availability docs](https://learn.microsoft.com/en-us/ai-builder/availability-region) before promising your boss this will work.

> **Credit consumption.** Each invoice processed costs AI Builder credits. The trial gives you a limited pool. In production, you need AI Builder capacity add-ons. Talk to your licensing team BEFORE you deploy to 500 users.

> **File size limits.** The invoice model processes one invoice per call (typically 1 to 2 pages). For batch processing, loop through invoices in your flow. Max file size is 50 MB. For a multi-invoice PDF, split it into individual invoices first, then loop.

---

## What you built

In 45 minutes, you:

1. Built a complete Power Automate flow that processes invoices using AI Builder's prebuilt model. No training, no custom models, no code.
2. Tested it with a sample invoice and received a formatted email with extracted fields and confidence scores.
3. Explored Document Intelligence Studio to understand what prebuilt models can and cannot do, and tested with different document types.
4. Learned about confidence scores, which tell you when to trust the AI and when to flag for human review.

This is production-capable. Add an email or SharePoint trigger, add a condition on confidence scores, connect to your accounting system's API, and you have AP automation.

---

## Stretch goal: Save to a SharePoint list instead of email

Got extra time? Replace the email step with a SharePoint step.

1. Create a SharePoint list called "Processed Invoices" with these columns:
   - InvoiceID (Single line of text)
   - InvoiceDate (Date)
   - VendorName (Single line of text)
   - InvoiceTotal (Currency)
   - Confidence (Number)
   - Status (Choice: "Auto-Approved" or "Needs Review")

2. In your flow, delete the "Get my profile" and "Send an email" actions.

3. Add a **Condition** action after the Compose step:
   - Condition: The lowest confidence score is greater than 0.8
   - If yes: Add a **Create item** action (SharePoint) that writes to your list with Status = "Auto-Approved"
   - If no: Same Create item action, but with Status = "Needs Review"

4. Test again with the sample invoice.

This is closer to what a real production flow looks like. Centralized data, automated triage, auditable trail.

---

*Lab 1 complete. Take a breath. Grab a coffee. In Lab 2 we are going to break things on purpose.*
