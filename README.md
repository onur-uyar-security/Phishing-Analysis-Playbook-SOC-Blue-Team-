# Phishing Analysis Playbook (SOC / Blue Team)

A practical, analyst-focused playbook for triaging a reported phishing email: how to read the
**headers**, analyse **URLs** and **attachments** safely, extract **IOCs**, and drive the
**response**. Built as a lab/learning reference — no real victim data is included.

- **MITRE ATT&CK:** [T1566 – Phishing](https://attack.mitre.org/techniques/T1566/)
  (.001 attachment · .002 link · .003 via service)
- **Audience:** SOC L1/L2 handling user-reported or alert-driven phishing

## Triage at a glance
1. **Preserve** the original email (`.eml` / `.msg`) — never forward it as a live link.
2. **Headers** → who really sent it? (SPF/DKIM/DMARC, Return-Path, Reply-To, Received chain)
3. **Content** → URLs and attachments, analysed **safely** (defang, reputation, sandbox).
4. **Verdict** → benign / spam / phishing / malware; assign severity.
5. **IOCs** → sender, domains, URLs, file hashes, subjects.
6. **Respond** → block, quarantine, reset credentials if clicked, notify, educate.

## Contents
| File | Topic |
|---|---|
| [`triage-workflow.md`](triage-workflow.md) | Full step-by-step triage workflow |
| [`email-header-analysis.md`](email-header-analysis.md) | Reading headers, SPF/DKIM/DMARC, spoofing tells |
| [`url-attachment-analysis.md`](url-attachment-analysis.md) | Safe URL and attachment analysis |
| [`response-and-iocs.md`](response-and-iocs.md) | IOC extraction, containment and response |
| [`references.md`](references.md) | Sources and tools |

## Safety first
- Analyse URLs/attachments in a **sandbox or isolated VM**, never on a production host.
- **Defang** indicators in notes/tickets (`hxxp://`, `evil[.]com`) so nobody clicks them.
- Treat every attachment as potentially malicious until proven otherwise.

---
*Author: Osman Onur Uyar — defensive, lab-based learning reference. See [`references.md`](references.md) for sources.*
