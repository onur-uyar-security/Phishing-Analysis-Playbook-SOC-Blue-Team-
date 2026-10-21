# Email Header Analysis

Headers tell you **who really sent** an email, regardless of the friendly `From` name.

## Key headers
| Header | What it tells you |
|---|---|
| `From` | The **displayed** sender — easily spoofed (display-name & domain). |
| `Return-Path` (envelope sender) | Where bounces go; often reveals the **real** sending domain. |
| `Reply-To` | Where replies go — a mismatch with `From` is a classic phishing tell. |
| `Received` | The hop-by-hop path. **Read bottom-up**: the lowest `Received` is the origin. |
| `Authentication-Results` | The receiver's **SPF / DKIM / DMARC** verdicts. |
| `Message-ID` | Unique ID; its domain can hint at the true sending system. |
| `X-*` headers | Vendor/gateway specifics (spam score, originating IP, etc.). |

## Email authentication (SPF / DKIM / DMARC)
- **SPF** (Sender Policy Framework): did the sending **IP** match the domain's allowed senders?
  Checked against the **envelope (Return-Path)** domain.
- **DKIM** (DomainKeys Identified Mail): is the message **cryptographically signed** by the
  domain, and does the signature validate (and cover the `From`)?
- **DMARC**: ties SPF/DKIM to the **`From`** domain (**alignment**) and states a policy
  (`none` / `quarantine` / `reject`). A **DMARC fail** on a brand's domain is a strong signal.

> Read these in `Authentication-Results`, e.g.
> `spf=fail dkim=fail dmarc=fail (p=reject)` → very likely spoofed.

## Spoofing tells (checklist)
- [ ] `From` domain ≠ `Return-Path` domain
- [ ] `Reply-To` points to a different / free-mail domain
- [ ] SPF / DKIM / **DMARC fail** for a domain that should enforce it
- [ ] `Received` origin IP/host inconsistent with the claimed brand
- [ ] **Lookalike** domain (e.g. `micros0ft.com`, `paypa1.com`) or subdomain trick
- [ ] Display name says "IT Helpdesk" but the address is unrelated

## How to view full headers
- **Outlook:** open message → File → Properties → *Internet headers*.
- **Gmail:** open message → ⋮ → *Show original* (shows SPF/DKIM/DMARC results).
- **Thunderbird:** View → Headers → All, or *More → View Source*.
- Or analyse the saved `.eml` with a header-analysis tool (see [`references.md`](references.md)).

## Caution
A **pass** on SPF/DKIM/DMARC does **not** guarantee the mail is safe (the attacker may own a
look-alike domain that passes its own auth). Combine header checks with content, URL and
attachment analysis.
