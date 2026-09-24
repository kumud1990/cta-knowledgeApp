# United Bank — CTA Mock Judgment (Attempt 1)

- **Date:** 2026-09-23
- **Scope:** First 35 minutes of the recorded session (full presentation, ending 28:43, + Q&A through the Apex-trigger question ~34:28). Post-35-min panel feedback (file storage, skinny tables, data skew, nearest-branch, FF-vs-RR) **excluded**.
- **Judge:** cta-mock-judge
- **Verdict:** Developing
- **Exam-style score:** 54% (passing bar 67%) → **FAIL**

## Exam scorecard (weighted)

| Knowledge area | Weight | Rubric (1–5) | Area % | Contribution |
|---|---|---|---|---|
| System Landscape & Architecture | 15% | 3 | 60% | 9.0 |
| Data Model & Management | 20% | 3 | 60% | 12.0 |
| Integration & Identity | 20% | 3 | 60% | 12.0 |
| Security & Sharing | 15% | 3 | 60% | 9.0 |
| Dev Lifecycle / Governance (ALM) | 10% | 1 | 20% | 2.0 |
| Risk & Trade-off Analysis | 10% | 2 | 40% | 4.0 |
| Delivery (Communication + Adaptability) | 10% | 3 | 60% | 6.0 |
| **Total** | **100%** | | | **54%** |

## Domain scores (1–5)

| Domain | Score | Justification |
|---|---|---|
| system_landscape | 3 | Clouds + ETL/ESB + OKTA/SAML presented; single-org asserted but not justified; IDP-vs-SP defended well under follow-up. |
| data_model | 3 | Person Accounts, ACR, external objects, LDV flags all presented cleanly; not yet probed in-scope. |
| integration_identity | 3 | Good pattern vocabulary + OKTA/SCIM; Marketing Cloud justification weak in Q&A ("cleans leads"). |
| security_sharing | 3 | Full walkthrough; SSN trap resolved well (separate last-4 searchable field + encryption + FLS). |
| alm | 1 | Only internationalization (Translation Workbench, custom labels); no sandbox/deployment/testing/governance. |
| risk_tradeoffs | 2 | A few trade-offs stated (case load, external objects, single-org) but not defended with alternatives. |

## Meta scores (1–5)

| Dimension | Score | Justification |
|---|---|---|
| breadth | 4 | Every required area covered within the time box. |
| depth | 3 | Feature-specific, but MC and Apex justifications thin. |
| communication | 3 | Fluent, well-paced end-to-end walkthrough. |
| adaptability | 3 | Defended IDP-vs-SP and SSN searchability under follow-up. |

## Strengths
- SSN searchability trap handled correctly: separate custom field holding only the last 4 digits, made searchable, with the encrypted field behind it.
- IDP-vs-SP SSO defended credibly (OKTA app-tile launch for IDP-initiated; acknowledged SP-initiated also works).
- Full-breadth solution delivered inside the 30-minute presentation box.

## Gaps
- ALM/governance essentially absent beyond internationalization — largest single scoring drag.
- Risk/trade-offs reactive: decisions stated, not defended with rejected alternatives and risks up front.
- Two shaky Q&A justifications: Marketing Cloud "cleans leads" (not a real MC capability) and Apex-for-5-applicants ("more scalable than flow" — trivial volume).

## What moves this to a pass (67%+)
1. Build a real ALM/governance story (sandbox strategy, CI/CD, testing, release management).
2. Volunteer trade-offs proactively — state the rejected alternative and why for every major decision.
3. Fix the two shaky justifications: cut/re-justify Marketing Cloud; justify Apex by cross-record validation logic, not scale.
