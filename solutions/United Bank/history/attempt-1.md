# United Bank — Solution Snapshot (Attempt 1, judged 2026-09-23)

> The judged solution for this attempt is the candidate's set of artifacts in `solutions/United Bank/`,
> not a single `solution.md`. This snapshot records which artifacts were judged and their key contents
> at the time of judgment so future attempts can diff against it.

## Artifacts judged
- `solution united bank.docx` — requirement-by-requirement written solution.
- `Actors & License.xlsx` — actor→license mapping, role hierarchy, LDV volumetrics.
- `system landscape.png` — single-org landscape diagram.
- `united bank data model.png` / `Data model.pdf` — ERD.
- `united bank transcript.pdf` — recorded presentation + Q&A (scored: first 35 min only).

## Key design decisions at time of judgment
- Single Salesforce org; Service + Experience Cloud, SF Voice, Mobile Publisher + SF mobile (Web Server flow + PKCE).
- Marketing Cloud for customer engagement.
- Informatica (JWT + mTLS, Bulk API 2.0) to CRM1/2/3; Mulesoft ESB (JWT + mTLS) to mainframe + DWH.
- OKTA IdP, SAML 2.0 IDP-initiated SSO to LDAP + custom LDAP; SCIM provisioning.
- Person Accounts (RT business + contact), ACR, Lead→Customer Master, Question/QA Response objects (LDV), Document checklist + ContentDocument, Asset (LDV), Opportunity(bankAccount)/OCR/Opportunity Product, Case (LDV)/Knowledge, Transactional data as External Objects (OData), Employee Discipline custom object, Account Team.
- OWD Private; sharing sets, criteria-based sharing rules, partner role hierarchy.
- Encrypted SSN with a separate searchable last-4 custom field.
- Integration patterns: Request-Reply (submit), Fire-and-Forget (address validation), Platform Events / ESB, External Objects for transactions.
- Global address validation via AppExchange; DocuSign for PDFs; OCR + Apex validation for 5-applicant / under-18 rules.

## Licenses / volumetrics (from Actors & License.xlsx)
- Customer = Customer Community; branch staff/managers/executives = Sales; contact centre = Service; 3rd party = Partner.
- Account/Contact 30M→44M, Lead 3.65M/yr, Opportunity 730K/yr, QA Response 3.65M/yr, Cases 6.8M→15M, Asset 15M→18M.
