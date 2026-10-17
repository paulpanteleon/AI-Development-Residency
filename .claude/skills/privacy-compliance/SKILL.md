---
name: privacy-compliance
description: Produce a privacy and data-protection evidence pack showing how software handles personal data (PII), GDPR requirements, and HIPAA-protected health information (PHI) — data inventory, data-flow map, controls matrix with code-level evidence, gaps, and GDPR/HIPAA records. Use when the user asks to prove or document that software handles data securely and lawfully, prepare for a client or auditor review, or assess PII, GDPR, HIPAA, PHI, DPIA, ROPA, or BAA readiness.
---

# Privacy Compliance Evidence Pack

Build an evidence pack that shows, requirement by requirement, how the software handles personal data — and where it falls short. Every claim must point to something checkable: a file and line, a config value, a test, or a named organizational document.

The controls catalog is in [controls.md](controls.md). Load it before step 4.

## Ground rules

- **Not legal advice, not a certification.** There is no official HIPAA certification, and GDPR compliance depends on facts outside the code. Say "controls observed" and "gaps found" — never "compliant" or "certified". Put this disclaimer at the top of every pack.
- **Never copy real personal data into the evidence.** Refer to fields by name (`patients.date_of_birth`), never by value. If you encounter real PII/PHI in code, fixtures, logs, or seed data, report the location and redact it in anything you write.
- **Separate what code can prove from what it can't.** Training, signed BAAs/DPAs, policies, and risk-management decisions are organizational evidence. Mark them `Org evidence required` with exactly what document is needed — don't mark them met or not met from the code alone.
- **Pin the evidence to a version.** Record the date and git commit SHA reviewed; evidence is only valid for that version.
- **Review only.** Don't change application code while building the pack. Propose fixes in the gaps file; implement them only if the user asks.

## 1. Establish scope and applicability

Determine from the code, docs, and the user (ask only what can't be inferred):

- **Data subjects and data types**: customers, patients, employees, children? General PII, special category data (GDPR Art. 9: health, biometrics, etc.), or PHI?
- **Roles**: GDPR controller or processor? HIPAA covered entity, business associate, or neither?
- **Jurisdictions**: EU/UK users (GDPR / UK GDPR)? US health data (HIPAA)?
- **Deployment**: where data is hosted, which cloud and third-party services are used.

Then decide which control sets apply: the general PII set (`P-`) always applies; GDPR (`G-`) if EU/UK personal data is processed; HIPAA (`H-`) if the software creates, receives, stores, or transmits PHI for a covered entity or business associate. Record the reasoning for any set marked not applicable.

## 2. Build the data inventory

Find every place personal data is defined, stored, or sent:

- Database schemas, migrations, ORM models, GraphQL/OpenAPI specs, form fields, API payloads
- Files and object storage (uploads, exports, backups)
- Logs, error tracking, analytics, and session replay tools
- Third-party SDKs and outbound API calls (email, payments, AI/LLM APIs, CRMs)
- Caches, queues, search indexes, and data warehouses

Search for likely field names (`name`, `email`, `phone`, `address`, `dob|birth`, `ssn`, `ip`, `location`, `diagnosis`, `mrn`, `insurance`, `medication`, `patient`) and the 18 HIPAA identifiers listed in controls.md (H-13).

For each data element record: field/location, category (PII / special category / PHI), purpose, source, where stored, encrypted at rest (yes/no/unknown), retention, and who/what can access it.

## 3. Map the data flows

Draw a Mermaid flowchart showing sources → services → stores → third parties, marking trust boundaries, encryption in transit, and any transfer outside the EU/UK. Every third party in the diagram must appear in the subprocessor list (P-11).

## 4. Assess each control

Work through every applicable control in [controls.md](controls.md). For each one record:

| Field | Content |
| --- | --- |
| ID | e.g. `H-08` |
| Requirement | One line, with the legal citation |
| Status | `Met` · `Partial` · `Not met` · `Not applicable` · `Org evidence required` |
| Evidence | `file:line`, config key, test name, or document name — specific enough for someone else to verify |
| Gap | What's missing, if anything |
| Fix | Concrete code, config, or document change |

Verify rather than assume: read the encryption config, confirm the audit log actually records reads of sensitive records, check that the delete endpoint removes data from backups' retention schedule and third parties, not just the main table. Where a passing automated test exists, cite it — it is the strongest evidence. Where a control could be proven by a test that doesn't exist yet, say so in the Fix column.

Rate each gap: **Critical** (unlawful processing or exposed PHI/PII now), **High** (likely breach or regulatory finding), **Medium**, **Low**.

## 5. Write the evidence pack (markdown in the repo)

Write to `docs/compliance/` in the project being reviewed:

| File | Contents |
| --- | --- |
| `README.md` | Disclaimer, scope and applicability decisions, date, commit SHA, summary counts by status and gap severity, top 5 gaps |
| `data-inventory.md` | The table from step 2 |
| `data-flows.md` | The Mermaid diagram from step 3 plus the third-party/subprocessor list |
| `controls-matrix.md` | Every control from step 4, grouped by P / G / H |
| `gaps-and-remediation.md` | Every gap sorted by severity, with fix and suggested owner |
| `gdpr-ropa.md` | GDPR only: Article 30 record of processing — purposes, lawful basis per purpose, categories of data and subjects, recipients, transfers, retention, security measures |
| `gdpr-dpia.md` | GDPR only, when processing is likely high risk (health data, large scale, monitoring, profiling): a DPIA draft — description, necessity and proportionality, risks, mitigations |
| `hipaa-risk-analysis.md` | HIPAA only: a §164.308(a)(1)(ii)(A) risk analysis draft — assets holding ePHI, threats, vulnerabilities, likelihood, impact, risk level, planned safeguards |
| `org-evidence-checklist.md` | Every `Org evidence required` item as a `- [ ]` task naming the document needed (e.g. signed BAA with the hosting provider, workforce training records) |

If `docs/compliance/` already exists, update it: keep the previous commit SHA in a "History" section of the README and note what changed.

## 6. Create the shareable summary doc

After the markdown pack is written, create a shareable doc for clients or auditors (use the Claude Docs artifact type if available; otherwise skip this step and say so):

- Title: `Privacy & Data Protection Evidence: <project name>`
- Lead: the disclaimer, scope, commit SHA, and a one-sentence overall assessment
- A gaps tracker table with Severity and Status dropdown columns (To do / In progress / Done / Accepted risk)
- A controls summary table: control ID, requirement, status, evidence (short)
- The org-evidence checklist as tasks
- No real personal data, secrets, internal hostnames, or credentials in the doc — it may be shared outside the team

Remind the user that the doc must be shared from its Share menu before others can open it.

## 7. Report back

In chat, give: the applicable control sets, counts by status, the top gaps with severity, the path to `docs/compliance/`, and the summary doc link. Offer to write the missing automated tests or implement the fixes.
