# Controls catalog

Each control lists what to look for in the software. "Org" marks controls that usually need organizational evidence in addition to (or instead of) code.

## P — Personal data (always applies)

| ID | Requirement | What to check |
| --- | --- | --- |
| P-01 | Data inventory exists and is current | The inventory from step 2 covers every store and flow found in the code |
| P-02 | Data minimization | Fields collected are needed for a stated purpose; no "just in case" fields; optional fields are optional |
| P-03 | Encryption in transit | TLS 1.2+ for all external and internal service traffic; HSTS; no plaintext DB/queue connections |
| P-04 | Encryption at rest | Database, object storage, backups, and disks encrypted; field-level encryption for the most sensitive fields; key management (KMS) rather than keys in code |
| P-05 | Least-privilege access | Role-based access checks on every endpoint returning personal data; users can only reach their own records; admin functions restricted server-side |
| P-06 | Strong authentication | Password hashing (bcrypt/scrypt/argon2); MFA for admin and staff access; session expiry |
| P-07 | Audit logging | Access to and changes of personal data are logged with who, what, when; logs are tamper-resistant and retained |
| P-08 | No personal data leakage | No PII/PHI in application logs, error trackers, analytics events, URLs/query strings, or client-side storage |
| P-09 | Retention and deletion | Defined retention per data type; automated deletion or anonymization; deletion propagates to backups (by expiry), caches, search indexes, and third parties |
| P-10 | Secrets management | No credentials in source or git history; secrets from a vault or environment; rotation possible |
| P-11 | Third-party inventory | Every service receiving personal data is listed with purpose, data sent, and location (Org: contracts) |
| P-12 | Backup and recovery | Backups encrypted, access-restricted, and restore-tested (Org: test records) |
| P-13 | Secure development | Dependency vulnerability scanning, code review, and security tests in CI |
| P-14 | Incident detection and response | Alerting on suspicious access; documented breach response procedure (Org) |
| P-15 | Test and dev data | No real personal data in fixtures, seeds, or non-production environments |

## G — GDPR / UK GDPR (EU or UK personal data)

| ID | Requirement | What to check |
| --- | --- | --- |
| G-01 | Lawful basis (Art. 6) and special-category condition (Art. 9) | Each processing purpose has a documented basis; health or biometric data has an Art. 9 condition |
| G-02 | Valid consent (Art. 7) where consent is the basis | Consent captured unbundled and unticked by default; stored with timestamp, version, and scope; withdrawal as easy as giving it |
| G-03 | Transparency (Art. 13–14) | Privacy notice exists and matches actual processing found in the inventory |
| G-04 | Data subject rights (Art. 15–21) | Working mechanisms for access and portable export (15, 20), rectification (16), erasure (17), restriction and objection (18, 21), with identity verification and a one-month response process |
| G-05 | Privacy by design and default (Art. 25) | Most protective settings by default; pseudonymization where feasible |
| G-06 | Processor contracts (Art. 28) | DPAs with every processor in P-11 (Org) |
| G-07 | Records of processing (Art. 30) | ROPA exists — the skill drafts it |
| G-08 | Security of processing (Art. 32) | Covered by P-03 to P-07, P-12, P-13 |
| G-09 | Breach notification (Art. 33–34) | Ability to detect, assess, and notify the supervisory authority within 72 hours and affected people without undue delay (Org: procedure) |
| G-10 | DPIA (Art. 35) | Required for likely high-risk processing (large-scale health data, monitoring, profiling) — the skill drafts it |
| G-11 | International transfers (Ch. V) | Transfers outside the EU/UK identified; adequacy decision, SCCs, or other safeguard in place (Org) |
| G-12 | Cookies and trackers (ePrivacy) | No non-essential cookies, analytics, or pixels load before consent; consent choices respected |
| G-13 | Automated decisions (Art. 22) | Solely automated decisions with legal or similarly significant effects have human review and an explanation |

## H — HIPAA (PHI for a covered entity or business associate)

Citations are to 45 CFR Part 164. "Addressable" specifications must be implemented or the reason for an equivalent alternative documented — they are not optional.

| ID | Requirement | What to check |
| --- | --- | --- |
| H-01 | Risk analysis — §164.308(a)(1)(ii)(A) | An accurate, thorough assessment of risks to ePHI — the skill drafts it (Org: approval) |
| H-02 | Risk management — §164.308(a)(1)(ii)(B) | Gaps from H-01 have assigned remediation (Org) |
| H-03 | Information access management — §164.308(a)(4) | Access to ePHI granted by role and approved; access reviews (Org: process) |
| H-04 | Security awareness training — §164.308(a)(5) | Workforce training records (Org) |
| H-05 | Contingency plan — §164.308(a)(7) | Data backup, disaster recovery, and emergency-mode operation; tested |
| H-06 | Business associate agreements — §164.308(b), §164.314 | Signed BAA with every vendor that touches PHI (hosting, email, logging, AI APIs) (Org) |
| H-07 | Access control — §164.312(a) | Unique user IDs (required), emergency access procedure (required), automatic logoff (addressable), encryption/decryption of ePHI at rest (addressable) |
| H-08 | Audit controls — §164.312(b) | Activity in systems containing ePHI is recorded and reviewable, including reads |
| H-09 | Integrity — §164.312(c) | Protection against improper alteration or destruction; checksums, versioning, or immutability where appropriate |
| H-10 | Person or entity authentication — §164.312(d) | Users and systems verified before ePHI access; MFA recommended |
| H-11 | Transmission security — §164.312(e) | Integrity controls and encryption for ePHI in transit (addressable, expected in practice) |
| H-12 | Minimum necessary — §164.502(b), §164.514(d) | Endpoints, exports, and roles expose only the PHI needed for the purpose |
| H-13 | De-identification — §164.514(b) | Data treated as de-identified meets Safe Harbor (all 18 identifiers removed: names; geographic units smaller than a state, except the first 3 ZIP digits under conditions; all date elements except year related to an individual, and ages over 89; phone; fax; email; SSN; medical record number; health plan beneficiary number; account number; certificate/license number; vehicle identifiers; device identifiers; URLs; IP addresses; biometric identifiers; full-face photos; any other unique identifying code) or Expert Determination |
| H-14 | Breach notification — §164.400–414 | Ability to identify affected individuals and notify within 60 days of discovery; business associates notify the covered entity (Org: procedure) |
| H-15 | Right of access — §164.524 | Patients can obtain their PHI, in electronic form if requested, within 30 days |
| H-16 | Documentation retention — §164.316 | Policies and required documentation kept for 6 years (Org) |
| H-17 | Device and media controls — §164.310(d) | Secure disposal and re-use of media holding ePHI; storage volumes wiped or crypto-shredded (Org + infrastructure config) |
