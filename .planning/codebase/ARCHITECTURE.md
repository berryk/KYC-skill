# Architecture

## System Overview

The KYC Officer Skill implements a **state machine architecture** defined in Markdown, executed by Claude AI at runtime. This is a prompt engineering project, not a traditional software system.

## Architectural Pattern

### State Machine Design

```
┌──────────────────────────────────────────────────────────────────────┐
│                    KYC Workflow State Machine                         │
│                         (23 States)                                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  ┌───────────────────────────────────────────────────────────────┐   │
│  │                    INTAKE PHASE                                │   │
│  │  ┌─────────────────────┐    ┌─────────────────────────────┐   │   │
│  │  │InitialInformation   │───▶│CustomerTypeClassification   │   │   │
│  │  │Gathering            │    │(INDIVIDUAL/CORPORATE)       │   │   │
│  │  └─────────────────────┘    └─────────────────────────────┘   │   │
│  └───────────────────────────────────────────────────────────────┘   │
│                              │                                        │
│              ┌───────────────┴───────────────┐                       │
│              ▼                               ▼                        │
│  ┌─────────────────────┐        ┌─────────────────────┐              │
│  │   IndividualKYC     │        │   EntityUnwrapping  │              │
│  └─────────────────────┘        └─────────────────────┘              │
│              │                               │                        │
│              └───────────────┬───────────────┘                       │
│                              ▼                                        │
│  ┌───────────────────────────────────────────────────────────────┐   │
│  │                 SCREENING PHASE (Parallel)                     │   │
│  │  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌───────────┐│   │
│  │  │ Sanctions   │ │ PEP         │ │ Adverse     │ │Enforcement││   │
│  │  │ Check       │ │ Check       │ │ Media       │ │ Check     ││   │
│  │  └─────────────┘ └─────────────┘ └─────────────┘ └───────────┘│   │
│  └───────────────────────────────────────────────────────────────┘   │
│                              │                                        │
│                              ▼                                        │
│  ┌───────────────────────────────────────────────────────────────┐   │
│  │                  ANALYSIS PHASE                                │   │
│  │  ┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐  │   │
│  │  │ Screening       │ │ Jurisdictional  │ │ Business        │  │   │
│  │  │ Results         │ │ Analysis        │ │ Activity        │  │   │
│  │  └─────────────────┘ └─────────────────┘ └─────────────────┘  │   │
│  └───────────────────────────────────────────────────────────────┘   │
│                              │                                        │
│                              ▼                                        │
│  ┌───────────────────────────────────────────────────────────────┐   │
│  │                  DECISION PHASE                                │   │
│  │  ┌─────────────────┐    ┌─────────────────────────────────┐   │   │
│  │  │Risk Assessment  │───▶│Risk Decision Logic              │   │   │
│  │  │Calculation      │    │(Route by Risk Rating)           │   │   │
│  │  └─────────────────┘    └─────────────────────────────────┘   │   │
│  └───────────────────────────────────────────────────────────────┘   │
│                              │                                        │
│         ┌────────────────────┼────────────────────┐                  │
│         ▼                    ▼                    ▼                   │
│  ┌─────────────┐    ┌─────────────────┐    ┌─────────────┐          │
│  │ LOW Risk    │    │ MEDIUM/HIGH     │    │ PROHIBITED  │          │
│  │ Path        │    │ Risk Path       │    │ Path        │          │
│  └─────────────┘    └─────────────────┘    └─────────────┘          │
│         │                    │                    │                   │
│         ▼                    ▼                    ▼                   │
│  ┌─────────────┐    ┌─────────────────┐    ┌─────────────┐          │
│  │Documentation│    │Enhanced Due     │    │ AutoReject  │          │
│  │Review       │    │Diligence (EDD)  │    │             │          │
│  └─────────────┘    └─────────────────┘    └─────────────┘          │
│         │                    │                                        │
│         ▼                    ▼                                        │
│  ┌─────────────┐    ┌─────────────────┐                              │
│  │ AutoApprove │    │ApproveWith      │                              │
│  │             │    │Monitoring       │                              │
│  └─────────────┘    └─────────────────┘                              │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

## State Types

### 1. Task States
Execute specific actions and store results:

| Action | Purpose | Example State |
|--------|---------|---------------|
| GATHER | Collect information from user | InitialInformationGathering |
| SEARCH | Query databases/lists | SanctionsCheck, PEPCheck |
| ANALYZE | Evaluate collected data | JurisdictionalAnalysis |
| COMPUTE | Calculate scores/ratings | RiskAssessmentCalculation |
| REVIEW | Verify documentation | DocumentationReview |

### 2. Choice States
Branch based on conditions:

```
CustomerTypeClassification:
  - IF entity_type == INDIVIDUAL → IndividualKYC
  - IF entity_type == CORPORATE  → EntityUnwrapping

RiskDecisionLogic:
  - IF risk_rating == PROHIBITED → AutoReject
  - IF risk_rating == LOW        → DocumentationReview
  - IF risk_rating == MEDIUM     → EnhancedDueDiligence
  - IF risk_rating == HIGH       → EnhancedDueDiligence
```

### 3. Parallel States
Execute multiple branches simultaneously:

```
ComprehensiveScreening:
  ├── SanctionsCheck      → sanctions_results
  ├── PEPCheck            → pep_results
  ├── AdverseMediaCheck   → media_results
  └── EnforcementCheck    → enforcement_results
```

### 4. HumanIntervention States
Pause for user decisions:

| State | Trigger | User Action Required |
|-------|---------|---------------------|
| RequestAdditionalDocuments | Incomplete docs | Specify missing items |
| EnhancedDueDiligence | MEDIUM/HIGH risk | Approve/Reject/Request more |
| RequestAdditionalEDD | More info needed | Specify requirements |

### 5. Wait States
Pause for external events:

| State | Timeout | On Completion |
|-------|---------|---------------|
| AwaitDocumentSubmission | 10 business days | → DocumentationReview |
| AwaitEDDSubmission | 15 business days | → EnhancedDueDiligence |

### 6. Terminal States
End workflow with decision:

| State | Type | Action |
|-------|------|--------|
| AutoApprove | Success | CREATE_ACCOUNT_STANDARD |
| ApproveWithMonitoring | Success | CREATE_ACCOUNT_WITH_ENHANCED_MONITORING |
| AutoReject | Failure | REJECT_AND_FILE_SAR_IF_REQUIRED |
| RejectWithReason | Failure | REJECT_WITH_DOCUMENTATION |

## Data Flow

### State Variables

```
┌─────────────────────────────────────────────────────────────────┐
│                    State Variables                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  INPUT VARIABLES (collected from user):                         │
│  ├── customer_basic_info                                        │
│  ├── individual_kyc_data (if INDIVIDUAL)                        │
│  └── entity_structure_list (if CORPORATE)                       │
│                                                                  │
│  SCREENING RESULTS (from parallel screening):                   │
│  ├── sanctions_results                                          │
│  ├── pep_results                                                │
│  ├── media_results                                              │
│  └── enforcement_results                                        │
│                                                                  │
│  ANALYSIS OUTPUTS:                                              │
│  ├── screening_analysis                                         │
│  ├── jurisdictional_risk                                        │
│  ├── business_risk                                              │
│  └── final_risk_assessment                                      │
│                                                                  │
│  DECISION VARIABLES:                                            │
│  ├── documentation_status                                       │
│  ├── edd_analysis                                               │
│  ├── approval_details                                           │
│  └── rejection_details                                          │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## Execution Model

### Claude as Runtime Engine

```
┌─────────────────────────────────────────────────────────────────┐
│                    Execution Flow                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  1. USER activates skill                                        │
│                │                                                 │
│                ▼                                                 │
│  2. CLAUDE loads skill.md + workflow.md                         │
│                │                                                 │
│                ▼                                                 │
│  3. CLAUDE starts at InitialInformationGathering               │
│                │                                                 │
│                ▼                                                 │
│  4. For each state:                                             │
│     ├── Read state definition                                   │
│     ├── Execute instruction                                     │
│     ├── Store output_variable                                   │
│     ├── Evaluate next state                                     │
│     └── Transition                                              │
│                │                                                 │
│                ▼                                                 │
│  5. At HumanIntervention:                                       │
│     ├── Present information                                     │
│     ├── PAUSE for user input                                    │
│     └── Resume on user response                                 │
│                │                                                 │
│                ▼                                                 │
│  6. At Terminal state:                                          │
│     ├── Execute final_action                                    │
│     ├── Generate decision report                                │
│     └── END workflow                                            │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## Component Relationships

```
skill.json ──────────────┐
(configuration)          │
                         ▼
skill.md ───────────────────────────▶ CLAUDE EXECUTION
(persona, instructions)              (runtime engine)
                         ▲
workflow.md ─────────────┘
(state machine)
```

## Key Architectural Decisions

### 1. Markdown-Based Workflow
- **Why**: Human-readable, version-controlled, easy to modify
- **Tradeoff**: No type safety, relies on Claude's interpretation

### 2. Deterministic State Machine
- **Why**: Regulatory compliance requires reproducible, auditable processes
- **Tradeoff**: Less flexibility, requires explicit state definitions

### 3. Parallel Screening
- **Why**: Efficiency - run all checks simultaneously
- **Tradeoff**: Must handle aggregation of results

### 4. Human Intervention Points
- **Why**: Compliance requires human judgment for elevated risk
- **Tradeoff**: Workflow pauses, requires user engagement

### 5. Risk-Based Routing
- **Why**: FATF-compliant approach - different treatment based on risk
- **Tradeoff**: More complex workflow with multiple paths
