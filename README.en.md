# hr-hiring-fairness-checker · AI hiring fairness & employment compliance domain

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Domain: hr](https://img.shields.io/badge/domain-hr-9cf.svg)](domain.manifest.json)
[![Status: RESERVED](https://img.shields.io/badge/status-RESERVED-orange.svg)](domain.manifest.json)

> **Status: RESERVED (placeholder · open for adoption)** — This repo scaffolds the
> industry-access four-file skeleton per the platform profile and awaits a qualified
> operator to claim it and fill in real rules.

Turns "how much does AI participate in hiring, and is there discrimination risk" into
verifiable attributes: the AI only surfaces prerequisites, it never makes a hiring decision.

**Keywords**: AI hiring fairness · algorithmic bias · protected attributes · disclosure ·
human review · employment compliance · labor contract · attribute verification · red lines ·
gate-map · privacy-by-default · bias audit · MCP checker

## What this domain covers

Fairness, disclosure and employment compliance of AI used in hiring. When AI screens resumes
or assesses interviews, discrimination risk and the candidate's right to know are central. The
checker surfaces disclosure, protected-feature exclusion and human-review prerequisites — who
gets hired is always decided by a human.

## Domain identity

| Item | Value |
|---|---|
| Domain ID | `hr` (globally unique, never reused) |
| Domain name | HR · hiring fairness & employment compliance |
| Profile version | `domain/1.0` |
| Status | `RESERVED` |
| Reserved since | 2026-09-12 |

## Attribute catalogue

| Key | Type | Meaning |
|---|---|---|
| `hr.ai_screening_disclosure` | boolean | AI screening/assessment disclosed to candidate |
| `hr.protected_attribute_exclusion` | enum | excluded/partially_excluded/not_excluded/unverified |
| `hr.human_review_available` | boolean | human review & appeal channel available |
| `hr.job_qualification` | enum | license_valid/license_missing/not_required |
| `hr.labor_contract_status` | enum | signed/pending/none/dispatched |

## Red lines (machine-readable)

1. Protected-attribute exclusion must be verified and logged by the employer; AI must not self-certify "no discrimination".
2. Never output hire/reject advice or ranking for a specific candidate.
3. Never output screening judgements based on gender, age, region, health, marital/child status or other protected features.

## The four-file access kit

| File | Role |
|---|---|
| `domain.manifest.json` | Domain declaration: attributes, issuer requirements, TTL, red lines |
| `gate-map.json` | Friction per action: silent / warn / confirm / block / require-owner |
| `privacy.json` | Privacy statement: off by default, minimization, withdrawable, erasable |
| `checker.io.json` | Checker MCP tool I/O contract — shows & verifies only, never judges |

## Quick Start

```bash
git clone https://github.com/SynomosAI/hr-hiring-fairness-checker.git
cd hr-hiring-fairness-checker

# Validate the four JSON files with the Python stdlib
python -m json.tool domain.manifest.json > /dev/null
python -m json.tool gate-map.json > /dev/null
python -m json.tool privacy.json > /dev/null
python -m json.tool checker.io.json > /dev/null
```

## Examples

```bash
# List declared attribute keys and types
python -c "import json;m=json.load(open('domain.manifest.json',encoding='utf-8'));[print(a['key'],a['type']) for a in m['attributes']]"
```

```json
{
  "subject_ref": "mpp:cn:9f2c1a7b",
  "attribute_keys": ["hr.ai_screening_disclosure", "hr.human_review_available"],
  "evidence": ["tsa:2026-09-12#sha256:3b1f..."],
  "request_id": "req-20260912-0001"
}
```

The checker answers only with `shown / missing / expired / unverifiable` plus the
provenance chain. It never returns "hire this one", rankings, or discrimination verdicts.

## Core principles

The platform is an arena, not a shelf. The checker answers only "is this claim verifiable,
and what prerequisite is missing" — never "is this compliant, should it be done".

## How to adopt

This domain is open to qualified operators: fork the repo, fill in `operator` and
`checker_endpoint`, align attributes and red lines with current rules, run the platform
self-checker (all five criteria must pass), then open a PR with credentials.

## License

Code and configuration are MIT-licensed. Documentation copyright remains with SynomosAI.
See [LICENSE](LICENSE).

© 2026 SynomosAI. All rights reserved.
