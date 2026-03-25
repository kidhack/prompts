# Email Cleanup Prompt (IMAP)

Use this prompt to clean up someone's IMAP email account — removing spam, newsletters, marketing, political, and social media emails, while safely archiving personal mail.

## Example summary

The cleanup step asks for an HTML summary with categories, counts, and resubscribe links. **[Open the example summary in your browser](newsletter_summary_example.html)** (file: `newsletter_summary_example.html` in this folder).

![Email Cleanup Summary — example dashboard with stats, category filters, and newsletter cards](email-cleanup-summary-example.png)

---

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

**Senders to always keep (whitelist) — never delete or unsubscribe these:**
- [e.g. no-reply@meetflo.com — smart home water alerts]
- [e.g. invoices@mybank.com — bank statements]
- [e.g. noreply@myinsurance.com — insurance updates]
- [Add any other senders that look like bulk mail but are actually important]

**Rules to follow — please be conservative:**
- If you're not sure about an email, move it to a new "Review" folder rather than deleting it
- Keep all personal emails from friends and family untouched
- Keep any email that has already been read
- Move read emails in the inbox that are older than 1 month to a new "Older" folder
- For important service emails (utilities, banks, shipping, medical, smart home alerts), keep them in the inbox or ask me before deleting

**After the cleanup:**
1. Send unsubscribe requests (via List-Unsubscribe headers) for every sender that was deleted
2. Scan the Deleted Messages folder and flag anything that looks important so I can review it
3. Create an HTML summary of every newsletter/subscription that was unsubscribed, organized by category, with a resubscribe button/link for each one
4. Email the HTML summary directly to [email@example.com] with the subject: "→ → READ THIS - Email cleanup summary"

Please use IMAP directly (Python via Desktop Commander) rather than a browser.
```

---

## Notes for the person running this

**App passwords:** If the account uses Gmail or Outlook with 2-factor authentication, a regular password won't work. You'll need an app-specific password:
- Gmail: myaccount.google.com → Security → App passwords
- Outlook: account.microsoft.com → Security → Advanced security → App passwords
- Yahoo: account.security.yahoo.com → Generate app password

**Common IMAP/SMTP servers:**

| Provider | IMAP server | SMTP server |
|---|---|---|
| Gmail | imap.gmail.com | smtp.gmail.com (port 587) |
| Outlook / Hotmail | outlook.office365.com | smtp.office365.com (port 587) |
| Yahoo | imap.mail.yahoo.com | smtp.mail.yahoo.com (port 587) |
| Apple iCloud | imap.mail.me.com | smtp.mail.me.com (port 587) |
| Sonic.net | imap.sonic.net | smtp.sonic.net (port 587) |
| Comcast/Xfinity | imap.comcast.net | smtp.comcast.net (port 587) |

**What gets deleted vs. moved:**

| Type | Action |
|---|---|
| Spam (X-Spam headers, spam patterns) | Deleted |
| Social media notifications (Facebook, Instagram, LinkedIn, etc.) | Deleted + unsubscribed |
| Political fundraising/campaign emails | Deleted + unsubscribed |
| Unread newsletters (List-Unsubscribe header present) | Deleted + unsubscribed |
| Unread marketing/promotional emails | Deleted + unsubscribed |
| Read newsletters/marketing | Moved to Review |
| Uncertain / unrecognized senders | Moved to Review |
| Personal emails (from free mail domains, no bulk headers) | Kept |
| Read emails older than 1 month | Moved to Older |

**Tip — fill in the whitelist before running:**
The whitelist section is the most important thing to get right. Bulk-mail headers are common on legitimate service emails — smart home devices, utilities, medical reminders, shipping notifications — and they'll get caught in the cleanup without a whitelist entry. Before running, think through:
- Smart home devices (Flo by Moen, Nest, Ring, etc.)
- Utilities (PG&E, water company, internet provider)
- Banks and credit cards
- Insurance companies
- Doctors, pharmacies, or medical portals
- Any subscription services you actually rely on (e.g. Rocket Money, DocuSign)

Add the sender address or domain for each one. For example:
> `- no-reply@meetflo.com — smart home water system alerts`
> `- statements@chase.com — bank statements`
> `- noreply@kaiserpermanente.org — medical appointments`
