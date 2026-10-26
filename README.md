# Phishing Helpdesk

An n8n workflow that lets staff forward a suspicious email to one address and get
an AI-written verdict back by email within a minute.

## Workflow
![Workflow canvas](workflow-canvas.jpg)

## Example replies
![Example reply 1](reply-email.jpg)
![Example reply 2](reply-email2.jpg)

## How it works
1. A dedicated mailbox is watched over IMAP.
2. A sender allowlist (If node) makes sure only approved addresses get a reply.
3. An AI model (Gemini) analyzes the forwarded email and returns a verdict
   (PHISHING / SUSPICIOUS / LIKELY SAFE), a confidence level, red flags, and next steps.
4. The result is emailed back to the person who forwarded it.

## Safety design
- The AI is told to treat email content as untrusted data, never as instructions.
- It analyzes the original forwarded email, not the employee who forwarded it.
- Only allowlisted senders get a reply, which limits abuse of the mailbox.
- Tested with: a gift-card scam, a normal internal email (no false alarm), and an
  email containing a hidden instruction aimed at the AI.

## Setup
1. Import `phishing-helpdesk.json` into n8n.
2. Create IMAP and SMTP credentials for a dedicated mailbox (use an app password).
3. Add your own Gemini API key.
4. Put the allowed sender addresses in the If node.

## Limitations
- Forwarded emails usually lose their technical headers, so SPF/DKIM/DMARC results
  cannot be checked and confidence is capped.
- Sender addresses can be spoofed, so the allowlist reduces abuse but does not eliminate it.
- Email content is sent to an AI provider. Review the provider's data terms before using
  real client emails.
- AI verdicts can be wrong and should support, not replace, a human decision.

-    - Built and tested on a free AI tier with a low daily request limit; real use needs a paid plan or a local model.
