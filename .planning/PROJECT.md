# Project: Red Team Simulation Module

## What This Is

An **Interactive Adversarial Training Mode** for the KYC Officer Skill that transforms compliance education from passive reference to active simulation. Officers learn to spot sophisticated money laundering patterns, defend their decisions under regulatory scrutiny, and understand criminal methodology — all within a safe, educational environment grounded in real-world enforcement cases.

## Core Value

**The ONE thing that must work:** Officers complete a simulated scenario where Claude plays a sophisticated "bad actor" attempting to pass KYC, and receive actionable feedback with regulatory citations when they miss red flags.

## Problem Statement

Current compliance training teaches officers *how to follow rules* but not *how to think critically*:
- Static scenarios don't teach pattern recognition
- No practice defending decisions under audit pressure
- Officers don't understand how criminals actually operate
- Generic pass/fail scores without regulatory context
- No connection to real-world enforcement consequences

## Target Users

- **Primary**: Compliance officers at financial institutions (banks, fintechs, MSBs)
- **Secondary**: Compliance training programs and certification prep
- **Tertiary**: Compliance managers evaluating team readiness

## Vision

A "flight simulator for financial crime detection" — officers practice in realistic scenarios before encountering sophisticated schemes in production.

### Learning Outcomes

1. **Pattern Recognition**: Spot sophisticated money laundering patterns (shell companies, layering, nominee structures, trade-based laundering)
2. **Audit Defense**: Practice defending decisions under fair but rigorous regulatory examination
3. **Criminal Methodology**: Understand how bad actors think and attempt to bypass controls

### Difficulty Progression

| Level | Name | Patterns | Example |
|-------|------|----------|---------|
| 1 | **Obvious** | Direct sanctions hits, declared PEPs, blacklisted jurisdictions | Syrian entity with OFAC-listed owner |
| 2 | **Hidden** | Nominee directors, layered ownership, fuzzy name matches, stale adverse media | BVI holding with undisclosed trust beneficiaries |
| 3 | **Sophisticated** | Trade-based laundering, crypto layering, shell networks, professional enablers | Multi-jurisdiction invoice carousel with correspondent banking exposure |

### Two Operating Modes

1. **Standalone Training Mode**: Officer explicitly enters training, receives synthetic scenarios with hidden red flags, must discover through investigation
2. **Post-Decision Audit Mode**: After completing a real KYC workflow, officer can request "audit my decision" to have regulator persona challenge their reasoning

### Feedback System (All Three Combined)

1. **Immediate Inline**: Real-time hints when officer misses critical questions ("Per FATF R.10(b), you should verify SOW for PEP associates...")
2. **End-of-Scenario Scorecard**: Detection rate, missed red flags, regulatory citations, risk of regulatory action
3. **Expert Comparison**: "Here's how an experienced BSA Officer would have approached this case..."

### Regulator Persona

- **Style**: Fair examiner (like FCA or OCC auditor), not adversarial prosecutor
- **Goal**: Educational — help officer understand gaps, not trick them
- **Approach**: Cite specific regulations, ask clarifying questions, highlight consequences

### Scenario Design

- **Repeatable with Variations**: Same pattern structure, different jurisdictions/names/industries
- **Pattern Library**: Officers learn to recognize *structures* not memorize cases
- **Randomized Elements**: Prevents gaming through memorization

### Enforcement Case Library (v1)

Real-world grounding with curated enforcement cases:
- **Capital One** ($390M, 2021) — Willful SAR failures
- **Danske Bank Estonia** ($230B laundered) — Correspondent banking failures
- **TD Bank** ($3B, 2024) — Transaction monitoring gaps
- **Deutsche Bank** ($150M, 2023) — Epstein relationship failures
- Pattern mapping: "This scenario resembles [Case Name]..."

## Constraints

| Constraint | Implication |
|------------|-------------|
| **Claude Skill Architecture** | Must work within existing skill.md/workflow.md pattern |
| **No External APIs** | Scenarios are generated/simulated, not pulled from live databases |
| **Educational Focus** | Not production KYC — explicitly for training purposes |
| **Token Efficiency** | Scenarios should be concise enough to complete in single context window |

## Success Metrics

- Officer completes Level 1 scenario and receives actionable feedback
- Officer can replay scenario with variations and recognize same pattern
- Audit mode provides regulatory-grounded challenge of real decisions
- Enforcement case library covers major AML failure patterns

## Requirements

### Validated

- ✓ 23-state KYC workflow engine — existing
- ✓ Risk-based assessment methodology (BSA, FATF, FinCEN) — existing
- ✓ Sample scenarios with expected outcomes — existing
- ✓ Decision report generation — existing
- ✓ Customer type classification (Individual/Corporate) — existing
- ✓ Parallel screening simulation — existing

### Active

- [ ] Adversarial scenario generation (synthetic bad actors)
- [ ] Difficulty level system (3 tiers)
- [ ] Hidden red flag discovery mechanics
- [ ] Inline feedback with regulatory citations
- [ ] End-of-scenario scorecard generation
- [ ] Expert comparison output
- [ ] Standalone training mode entry point
- [ ] Post-decision audit mode integration
- [ ] Pattern library (reusable scheme templates)
- [ ] Scenario variation engine
- [ ] Enforcement case library (5-10 major cases)
- [ ] Pattern-to-case mapping
- [ ] Regulator persona definition
- [ ] Training state machine (parallel to main workflow)

### Out of Scope

- Live API integrations (remains simulated) — maintains educational focus
- Automated scoring/grading system — v2 enhancement
- Multi-user leaderboards — v2 enhancement
- Custom scenario authoring UI — v2 enhancement
- LMS/SCORM integration — v2 enhancement

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Fair regulator vs adversarial | Educational goal is learning, not frustration | Fair examiner style |
| All three feedback types | Maximum learning value, layered understanding | Inline + Scorecard + Expert |
| Both operating modes | Flexibility for different training contexts | Standalone + Integrated |
| Enforcement cases in v1 | Real-world grounding is core differentiator | Include curated library |
| Pattern variations | Prevent memorization, teach recognition | Randomized elements |

## Technical Approach

### File Structure (Proposed)

```
.claude/skills/kyc-officer/
├── skill.md                    # Existing (update for training mode)
├── workflow.md                 # Existing (add training states)
├── training/                   # NEW
│   ├── adversarial_mode.md     # Training mode instructions
│   ├── regulator_persona.md    # Audit mode persona
│   ├── patterns/               # Reusable scheme templates
│   │   ├── shell_company.md
│   │   ├── layered_ownership.md
│   │   ├── trade_based.md
│   │   └── crypto_layering.md
│   ├── enforcement/            # Real case studies
│   │   ├── capital_one_2021.md
│   │   ├── danske_bank.md
│   │   ├── td_bank_2024.md
│   │   └── case_index.md
│   ├── scenarios/              # Level-based scenarios
│   │   ├── level_1/
│   │   ├── level_2/
│   │   └── level_3/
│   └── scoring_rubric.md       # How to evaluate officer performance
```

### Integration Points

1. **Entry Point**: New command or state to enter training mode
2. **State Machine**: Training workflow parallel to main KYC workflow
3. **Feedback Loop**: Scoring and feedback after each scenario
4. **Audit Hook**: Post-decision trigger in main workflow terminal states

---
*Last updated: 2026-01-20 after initialization*
