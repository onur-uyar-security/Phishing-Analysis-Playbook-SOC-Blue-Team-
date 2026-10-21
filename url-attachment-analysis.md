# Safe URL & Attachment Analysis

> **Golden rule:** never open a suspicious link or file on a production host, from your own
> browser, or while signed in. Use an **isolated VM / sandbox** and **defang** everything in notes.

## Defanging
Rewrite indicators so they can't be clicked or auto-linked:
- `http://evil.com/login` → `hxxp://evil[.]com/login`
- `1.2.3.4` → `1[.]2[.]3[.]4`
- `bad@domain.com` → `bad[at]domain[.]com`

## URLs
1. **Extract** every URL from the body and headers; don't forget links hidden behind
   button text or `<a>` tags that differ from the visible text.
2. **Expand shorteners** (bit.ly, t.co, …) **without visiting** — use a URL-expander or a
   sandbox, not your live browser.
3. **Reputation / intel:** check the domain and full URL against VirusTotal, URLScan.io,
   urlvoid, Cisco Talos, etc.
4. **Domain age & registration:** very **new** domains are suspicious (WHOIS / registrar lookup).
5. **Lookalikes / homoglyphs:** `rn` vs `m`, `0` vs `o`, IDN/punycode (`xn--`), and
   deceptive subdomains (`paypal.com.secure-login[.]ru`).
6. **Detonate** the URL in a sandbox (URLScan, Any.Run, Joe Sandbox) to see redirects,
   the landing page, and whether it harvests credentials or drops a file.

### Credential-harvesting pages
- Landing page imitates a known login (Microsoft 365, bank, courier).
- Form posts to an unrelated domain; page is on a new/odd host or a compromised site.
- If a user **entered credentials** → treat as credential compromise: reset password,
  revoke sessions/tokens, enable/verify MFA, hunt for sign-ins from the attacker.

## Attachments
1. **Never double-click.** Work on a copy in an isolated VM.
2. **Hash it** (SHA-256) and check reputation — you may get a verdict without detonating:
   ```bash
   sha256sum suspicious_file
   ```
3. **Type vs extension:** confirm the real type; watch for **double extensions**
   (`invoice.pdf.exe`), archives hiding executables, and `.lnk`/`.iso`/`.img` droppers.
4. **High-risk types:** Office docs with **macros** (`.docm`, `.xlsm`), `.html`/`.htm`
   attachments (phishing pages), `.js`/`.vbs`/`.hta`, password-protected archives (password
   in the email body is a classic evasion), OneNote/`.one`, `.lnk`.
5. **Static triage (in VM):** inspect Office macros (`olevba`), PDFs (`pdfid`/`pdf-parser`),
   and strings — look for URLs, shell commands, encoded payloads.
6. **Detonate** in a malware sandbox (Any.Run, Joe Sandbox, Hybrid Analysis, Cuckoo) and
   capture dropped files, C2 domains/IPs, and behaviour.

## What to record
Sender, Return-Path, Reply-To; subject; all URLs (defanged) + verdicts; file names + SHA-256
+ verdicts; sandbox report links; final verdict and severity. Feed these into
[`response-and-iocs.md`](response-and-iocs.md).

See [`references.md`](references.md) for the specific tools named here.
