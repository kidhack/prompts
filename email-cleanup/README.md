# Email Cleanup

Use this prompt with a coding agent to help clean up your IMAP inbox. The goal is to clear out spam, newsletters, marketing, political, and social emails while keeping personal and useful service mail safe. It should unsubscribe from non-whitelisted bulk senders and create a resubscribe summary at the end.

## Summary

At the end, you will get a summary with categories, counts, and resubscribe links so you can undo anything later if you want. **[Example summary](https://kidhack.github.io/prompts/email-cleanup/newsletter_summary_example.html)** — or open `newsletter_summary_example.html` from this folder locally.

<img src="email-cleanup-summary-example.png" alt="Example summary with stats, category filters, and newsletter cards" width="700" />

## How to run

Paste the prompt below into a coding agent that can run scripts, access IMAP/SMTP, and generate files.

It was tested with **Claude Cowork**. It likely also works with **Claude Code** and other coding agents such as:

- Cursor Agent
- OpenAI Codex / ChatGPT agent with terminal access
- Aider
- Cline / Roo Code
- Windsurf
- Gemini CLI / Gemini Code Assist

---

## Before you send the prompt

Follow these steps before you send it:

1. **For security:** temporarily change your account password before sending this prompt, then change it again right after.
2. Replace the IMAP server, email address, password, and SMTP server with your account details.
3. Add whitelist addresses or domains.
4. Remove or add cleanup bullets.
5. Add any extra safety rules.

If you are unsure about a sender, whitelist it first. It is much easier to delete something later than to recover an important message that was removed by mistake.

## The Prompt

```
I need help cleaning up an IMAP email account. Here are the details:

**Account credentials:**
- IMAP server: [e.g. imap.sonic.net / imap.gmail.com / outlook.office365.com]
- Email address: [email@example.com]
- Password / app password: [password]
- SMTP server (for sending unsubscribes): [e.g. smtp.sonic.net]

**What I want cleaned up:**
- Delete and unsubscribe from newsletters that are never read
- Delete and unsubscribe from all political fundraising/campaign emails
- Delete and unsubscribe from all marketing and promotional emails
- Delete and unsubscribe from all social media notification emails
- Remove spam

**Whitelist (comma-separated sender addresses or domains to always keep):**
[e.g. no-reply@meetflo.com, chase.com, noreply@kaiserpermanente.org, updates.netflix.com]

**Rules to follow — please be conservative:**
- If you're not sure about an email, move it to a new "Review" folder rather than deleting it
- Keep all personal emails from friends and family untouched
- Keep any email that has already been read
- Move read emails in the inbox that are older than 1 month to a new "Older" folder
- For important service emails (utilities, banks, shipping, medical, smart home alerts), keep them in the inbox or ask me before deleting

**After the cleanup:**
1. Unsubscribe from every sender that was deleted using all available methods:
   - Click through any List-Unsubscribe URLs in a browser to complete web-based unsubscribe flows
   - Send unsubscribe requests via List-Unsubscribe email headers where no URL is available
2. Scan the Deleted Messages folder and flag anything that looks important so I can review it
3. Create an HTML summary of every newsletter/subscription that was unsubscribed, organized by category, with a resubscribe button/link for each one
4. Email the HTML summary directly to the email address above with the subject: "→ → READ THIS - Email cleanup summary"

Please use IMAP directly via Python or another scripting environment rather than a browser.
```

---

## Setup notes and defaults

**App passwords:** If your account uses Gmail or Outlook with 2-factor authentication, your normal password usually will not work here. You will need an app-specific password:
- Gmail: myaccount.google.com → Security → App passwords
- Outlook: account.microsoft.com → Security → Advanced security → App passwords
- Yahoo: account.security.yahoo.com → Generate app password

**Common IMAP/SMTP servers:** If you are not sure what to enter, these are the usual defaults:

| Provider | IMAP server | SMTP server |
|---|---|---|
| Gmail | imap.gmail.com | smtp.gmail.com (port 587) |
| Outlook / Hotmail | outlook.office365.com | smtp.office365.com (port 587) |
| Yahoo | imap.mail.yahoo.com | smtp.mail.yahoo.com (port 587) |
| Apple iCloud | imap.mail.me.com | smtp.mail.me.com (port 587) |
| Sonic.net | imap.sonic.net | smtp.sonic.net (port 587) |
| Comcast/Xfinity | imap.comcast.net | smtp.comcast.net (port 587) |

**What gets deleted vs. moved:** This is the default behavior the prompt is aiming for as currently written. If you change the prompt, expect this behavior to change too.

| Type | Action |
|---|---|
| Personal emails from friends and family | Kept |
| Important service emails (utilities, banks, shipping, medical, smart home alerts) | Kept in inbox or reviewed before deletion |
| Unsure emails | Moved to Review |
| Read emails older than 1 month | Moved to Older |
| Marketing/promotional emails | Deleted + unsubscribed |
| Newsletters that are never read | Deleted + unsubscribed |
| Political fundraising/campaign emails | Deleted + unsubscribed |
| Social media notifications (Facebook, Instagram, LinkedIn, etc.) | Deleted + unsubscribed |
| Spam | Removed |
