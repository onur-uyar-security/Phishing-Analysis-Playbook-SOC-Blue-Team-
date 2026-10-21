# Phishing Triage Workflow

A repeatable workflow for a single reported phishing email.

## Step 0 — Preserve & scope
- Obtain the **original message** (`.eml`/`.msg`) with full headers — not a screenshot or forward.
- Record: reporter, received time, mailbox, whether links/attachments were **clicked/opened**.
- If clicked/opened → raise priority; prepare for possible credential/host compromise.

## Step 1 — Sender & authentication (headers)
- Compare **`From`** vs **`Return-Path`** vs **`Reply-To`** — mismatches are a red flag.
- Check **`Authentication-Results`**: **SPF**, **DKIM**, **DMARC** = `pass` / `fail`?
- Walk the **`Received`** chain (bottom = origin) — does the true origin match the claimed sender?
- Details: [`email-header-analysis.md`](email-header-analysis.md).

## Step 2 — Content & social engineering
- Look for urgency, authority, fear, reward, or a request to log in / pay / open a file.
- Note brand impersonation, lookalike domains, and display-name spoofing.

## Step 3 — URLs (safely)
- **Defang** and extract all links; expand shorteners **without visiting** in a browser.
- Check **reputation** (VirusTotal, URLscan, urlvoid) and **domain age**; look for homoglyphs.
- Detonate in a **sandbox** if needed. Details: [`url-attachment-analysis.md`](url-attachment-analysis.md).

## Step 4 — Attachments (safely)
- **Hash** the file (SHA-256) and check reputation; inspect type vs. extension.
- **Detonate** in a sandbox (Any.Run, Joe Sandbox, Hybrid Analysis) — never open on a prod host.

## Step 5 — Verdict & severity
| Verdict | Example | Typical action |
|---|---|---|
| Benign | Legitimate mail misreported | Educate reporter, close |
| Spam | Bulk marketing | Filter/blocklist, close |
| Phishing (credential) | Fake login page | Block URL/sender, reset creds if clicked |
| Malware | Malicious attachment/link | Block hash/URL, isolate host, IR |

## Step 6 — IOCs & response
- Extract IOCs (sender, domains, URLs, hashes, subject) → see [`response-and-iocs.md`](response-and-iocs.md).
- Contain (block, quarantine org-wide), eradicate, notify affected users, and feed detections.

## Step 7 — Close & improve
- Document the case (timeline, IOCs, actions).
- Tune detections / email controls; share an awareness note if the lure was effective.

> Keep notes **defanged** and store samples in a controlled location.
