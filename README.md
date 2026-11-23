# LinkedIn Post Approval

An n8n workflow that drafts a LinkedIn post from a form topic, reviews it with a separate AI agent, and emails it for approval. Only an approved post is appended to Google Sheets; rejection and a one-day timeout do not write a row.

## Setup

1. Import `LinkedIn Post Approval.json` into an n8n instance with the form trigger, AI Agent, OpenAI Chat Model, Gmail, and Google Sheets nodes. The workflow is inactive by default.
2. Create OpenAI, Gmail OAuth2, and Google Sheets OAuth2 credentials in n8n's credential store. Select the OpenAI credential on both chat model nodes and the corresponding Google credentials on the Gmail and Google Sheets nodes. Enable the Google Sheets API in the Google Cloud project used for the Sheets credential; ensure the connected account can edit the spreadsheet.
3. Create a spreadsheet with a header row containing exactly `Date`, `Topic`, `Final Post`, and `Status`. In **Save Approved Post**, select the spreadsheet and tab, and confirm the column mappings if n8n refreshes them.
4. In **Email for Approval**, replace `YOUR_EMAIL_ADDRESS` with your own single email address. Configure n8n's public webhook URL so the form and emailed approval links are reachable. Approval links authorize the decision, so do not forward the email.
5. Test the form trigger by submitting a topic, then run the workflow and approve the email. Verify the sheet contains the submitted topic and the exact reviewed post. Run again and reject; verify that no row is added. Also verify the one-day timeout in your environment. Activate the workflow and use its production form URL for ongoing submissions.

The date is the approval date in the workflow's configured timezone. Sheet values use RAW input to avoid interpreting a topic or post as a formula. The prompts request a 120-150 word post, one call to action, and at most three hashtags; AI output is not guaranteed to meet these limits, so review it before approving. This workflow does not publish to LinkedIn.

For the assignment submission, capture screenshots of the configured canvas, approval email, and approved sheet row after a real run. Share the repository link in WhatsApp; this repository is private, so recipients need access.
