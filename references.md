# References & Tools

Sources and tools referenced in this playbook. Verify current documentation before use;
tool availability and features change.

## Frameworks & standards
- **MITRE ATT&CK — T1566 Phishing**: https://attack.mitre.org/techniques/T1566/
  (sub-techniques .001 attachment, .002 link, .003 via service)
- **NIST SP 800-61 — Computer Security Incident Handling Guide**:
  https://csrc.nist.gov/pubs/sp/800/61/r2/final
- **SANS Incident Handling (PICERL)**: https://www.sans.org/
- **CISA — Phishing guidance**: https://www.cisa.gov/

## Email authentication
- **SPF — RFC 7208**: https://datatracker.ietf.org/doc/html/rfc7208
- **DKIM — RFC 6376**: https://datatracker.ietf.org/doc/html/rfc6376
- **DMARC — RFC 7489**: https://datatracker.ietf.org/doc/html/rfc7489
- **M3AAWG** best practices: https://www.m3aawg.org/

## URL / domain analysis
- **VirusTotal**: https://www.virustotal.com/
- **URLScan.io**: https://urlscan.io/
- **Cisco Talos Intelligence**: https://talosintelligence.com/
- **urlvoid**: https://www.urlvoid.com/
- WHOIS / registrar lookups (domain age)

## Attachment / malware sandboxes
- **Any.Run**: https://any.run/
- **Joe Sandbox**: https://www.joesandbox.com/
- **Hybrid Analysis**: https://www.hybrid-analysis.com/
- **Cuckoo Sandbox**: https://cuckoosandbox.org/

## Static analysis utilities
- **oletools (`olevba`)** — Office macro analysis: https://github.com/decalage2/oletools
- **pdfid / pdf-parser** (Didier Stevens): https://blog.didierstevens.com/programs/pdf-tools/
- `sha256sum`, `strings`, `file` (standard \*nix utilities)

## Threat-intel sharing
- **MISP**: https://www.misp-project.org/
- **STIX/TAXII (OASIS)**: https://oasis-open.github.io/cti-documentation/

---
*Compiled by Osman Onur Uyar as a defensive, lab-based learning reference. External trademarks
and tools belong to their respective owners; links are provided for reference only.*
