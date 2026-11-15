# Construction Variation Order Automation (n8n)

An n8n workflow that automates the variation order (VO) process for construction projects: from the contractor's email to a multi-level approval chain, an automatic tracker, and a formal determination notice.

> Built with sample data only. All company names, project names, contract references, amounts and emails are fictional.

## The problem

Variation orders move through email chains: someone reads the contractor's letter, compares it against the contract, forwards it for approvals, and updates the tracker by hand. It is slow, easy to get wrong, and hard to audit.

## What this workflow does

1. **Intake:** takes the contractor's variation letter and the signed contract baseline.
2. **AI commercial review:** Gemini cross-checks the claim against the contract terms and returns a recommendation, an audited VO amount and justification notes.
3. **Financials:** calculates the VO impact on contract value and budget.
4. **Multi-level approvals by email:** commercial, project management, procurement, projects, finance and executive stages. Each stage waits for a response before moving on.
5. **Tracker:** every stage writes its result to a Google Sheets tracker automatically.
6. **PDF notice:** generates a formal determination notice (HTML to PDF with Gotenberg), stores it in Google Drive and emails it.

## Tech stack

n8n, Google Gemini, Gmail, Google Drive, Google Sheets, Gotenberg, JavaScript.

## How to run it

1. Import `vo-automation-demo.json` into n8n (**Workflows, Import from file**).
2. Add your own credentials: Gmail OAuth2, Google Drive OAuth2, Google Sheets OAuth2 and Google Gemini API.
3. Create a Google Sheet for the tracker and replace the `YOUR_SPREADSHEET_ID` placeholders. Do the same for `YOUR_DRIVE_FOLDER_ID` and `YOUR_DRIVE_FILE_ID`.
4. Replace the sample emails (`@example.com`) with your own test addresses.
5. Start Gotenberg for PDF generation:
   ```
   docker run --rm -p 3000:3000 gotenberg/gotenberg:8
   ```
   If n8n runs outside Docker, change the Gotenberg URL in the workflow to `http://localhost:3000`.
6. The workflow includes an HTTP node that calls an audit webhook. Point it to your own endpoint or disable it.
7. Execute the workflow manually with the sample input.

## Screenshots

## Workflow overview

![Workflow overview](workflow-flow.png)

## Workflow in n8n

**1. Intake and analysis**

![Intake and analysis](workflow-1-analysis.png)

**2. Approval chain**

![Approval chain](workflow-2-approvals.png)

**3. Final approvals and notice**

![Final approvals](workflow-3-final.png)

## Results

In my own use of this approach on a real project, the variation order processing time dropped by around 80%.

## Author

Ehab Elessawi, construction workflow automation.
LinkedIn: https://www.linkedin.com/in/ehab-elessawi-0b8497180
