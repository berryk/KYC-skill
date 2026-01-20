# Technology Stack

## Overview

This is a **Claude Skill** project, not a traditional software application. It uses prompt engineering and structured Markdown to create a deterministic workflow system for KYC (Know Your Customer) compliance.

## Core Technology

### Claude Skill Framework
- **Platform**: Claude AI (Anthropic)
- **Skill Version**: 1.0.0
- **Execution Model**: Prompt-based state machine execution

### Markup Languages
| Language | Purpose | Files |
|----------|---------|-------|
| **Markdown** | Workflow definitions, skill prompts, documentation | `*.md` |
| **JSON** | Skill configuration and metadata | `skill.json` |

## Skill Architecture Components

### 1. Workflow Engine (`workflow.md`)
- **Type**: Markdown-based state machine definition
- **States**: 23 workflow states
- **State Types**: Task, Choice, Parallel, HumanIntervention, Wait, Success, Failure
- **Pattern**: Deterministic execution following defined transitions

### 2. Skill Prompt (`skill.md`)
- **Purpose**: Comprehensive instructions for Claude to act as KYC Officer
- **Contents**: 
  - Role definition and persona
  - Workflow execution instructions
  - Risk assessment methodology
  - Customer interaction guidelines
  - Output format templates
  - State variable definitions

### 3. Configuration (`skill.json`)
- **Format**: JSON metadata
- **Fields**: name, version, description, capabilities, tags, compliance frameworks

## State Machine Pattern

```
State Definition Format:
### N. StateName
- **type**: Task | Choice | Parallel | HumanIntervention | Wait | Success | Failure
- **action**: GATHER | SEARCH | ANALYZE | COMPUTE | REVIEW (for Task states)
- **instruction**: "What to do in this state"
- **output_variable**: "variable_name"
- **next**: NextStateName
```

## Risk Scoring System

### Multi-Dimensional Scoring
| Factor | Weight | Purpose |
|--------|--------|---------|
| Screening | 40% | Sanctions, PEP, Adverse Media, Enforcement |
| Jurisdictional | 25% | FATF status, Corruption index, AML regime |
| Business Activity | 25% | Industry sector, Complexity, Transparency |
| Customer Profile | 10% | Documentation quality, Consistency |

### Risk Tiers
| Tier | Score Range | Action |
|------|-------------|--------|
| LOW | 0-30 | Auto-approve, standard monitoring, annual review |
| MEDIUM | 31-60 | Enhanced due diligence, semi-annual review |
| HIGH | 61-85 | Extensive EDD, MLRO approval, quarterly review |
| PROHIBITED | 86-100 | Auto-reject, file SAR if required |

## Development Environment

### DevContainer Configuration
- **Location**: `.devcontainer/devcontainer.json`
- **Purpose**: Consistent development environment for skill authoring
- **Primary Use**: Markdown editing, skill testing with Claude

## No Traditional Dependencies

This project has **no runtime dependencies** because:
- It's not executable code - it's prompt engineering
- Execution happens within Claude's context
- No package managers, build tools, or compilation required
- Files are plain Markdown/JSON read by Claude at runtime

## External Systems (Simulated)

The skill references but does not integrate with:
- **OFAC SDN List** (US sanctions)
- **UN Security Council Sanctions**
- **EU Consolidated Sanctions List**
- **UK HMT Sanctions List**
- **World-Check / Dow Jones** (PEP databases)
- **Companies House** (UK company registry)

> ⚠️ All screening is **simulated** - real production use requires API integrations

## Version Control

- **VCS**: Git
- **Repository**: https://github.com/berryk/KYC-skill
- **Branch Strategy**: main branch only (experimental project)
