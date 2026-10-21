# IOC Extraction & Response

## Indicators of Compromise (IOCs) to extract
| Type | Examples |
|---|---|
| Email | Sender address, Return-Path, Reply-To, `Message-ID`, originating IP, subject line |
| Network | Domains, full URLs, IPs, C2 endpoints (from sandbox) |
| File | File names, SHA-256 (and MD5/SHA-1 if needed), macro/script artifacts |
| Host | Dropped files, registry/persistence, processes (from detonation) |

Record IOCs **defanged** (see [`url-attachment-analysis.md`](url-attachment-analysis.md)).
Where possible, map and share them using a standard such as **STIX/TAXII** or **MISP**.

## Response (align to your IR process, e.g. NIST SP 800-61 / SANS PICERL)
1. **Contain**
   - Block sender, domains, URLs, and file hashes at the **email gateway**, **proxy/firewall**,
     and **EDR**.
   - **Quarantine / purge** the message from all mailboxes (e.g. mail-gateway search-and-remove).
2. **Identify impact**
   - Who received it? Who **clicked / entered credentials / opened the attachment**?
   - Pull sign-in logs, proxy logs, and EDR telemetry for those users/hosts.
3. **Eradicate & recover**
   - Credential phishing → **reset passwords**, **revoke sessions/tokens**, verify MFA,
     review mailbox rules the attacker may have added.
   - Malware → **isolate** host, run EDR scan, reimage if needed, restore from known-good.
4. **Notify**
   - Inform affected users; escalate per policy; notify any required parties.
5. **Detect & improve**
   - Add IOCs to blocklists / watchlists and **SIEM detections**.
   - Create/boost a correlation rule (e.g. "user clicked newly blocked URL").
   - Send a short **awareness note** if the lure was convincing.

## Example IOC table (template)
```
# Case: <id>   Date: <yyyy-mm-dd>   Analyst: <name>   Verdict: <phishing/malware>
Type    | Indicator                | Notes / source
--------|--------------------------|----------------------------
sender  | bad[at]evil[.]com        | From != Return-Path
domain  | evil[.]com               | reg. 3 days ago; VT 12/90
url     | hxxps://evil[.]com/login | M365 credential harvest (URLScan)
sha256  | <hash>                   | macro dropper (Any.Run)
ip      | 1[.]2[.]3[.]4            | C2 from detonation
```

## Metrics worth tracking
Time-to-triage, time-to-contain, number of recipients vs clickers, repeat senders/themes.
These justify tuning email controls and awareness training.

> This mirrors a standard SOC flow; adapt to your org's IR policy and tooling.
> See [`references.md`](references.md).
