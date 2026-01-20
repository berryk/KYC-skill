# Conventions & Patterns

## Markdown Formatting

### State Definition Format

Each workflow state follows this structure:

```markdown
### N. StateName
- **type**: StateType
- **action**: ACTION_TYPE (for Task states)
- **instruction**: "Detailed instructions for this state"
- **output_variable**: "variable_name"
- **next**: NextStateName
```

### State Types

| Type | Purpose | Required Fields |
|------|---------|-----------------|
| Task | Execute action | action, instruction, output_variable, next |
| Choice | Conditional branch | input, choices (with condition/next pairs) |
| Parallel | Multiple branches | branches (list), next |
| HumanIntervention | User input required | cause, instruction, next |
| Wait | Pause for external | timeout, on_timeout, on_completion |
| Success | Successful end | message, final_action |
| Failure | Unsuccessful end | message, final_action |

### Action Types (for Task states)

| Action | Purpose | Example Usage |
|--------|---------|---------------|
| GATHER | Collect information | Customer data collection |
| SEARCH | Query databases | Sanctions/PEP screening |
| ANALYZE | Evaluate data | Risk factor analysis |
| COMPUTE | Calculate values | Risk score computation |
| REVIEW | Verify items | Documentation validation |

## Variable Naming

### Naming Convention
- **Format**: `snake_case`
- **Prefix**: Category indicator

### Standard Variables

```
Input Variables:
- customer_basic_info
- individual_kyc_data
- entity_structure_list

Screening Results:
- sanctions_results
- pep_results
- media_results
- enforcement_results

Analysis Outputs:
- screening_analysis
- jurisdictional_risk
- business_risk
- final_risk_assessment

Decision Variables:
- documentation_status
- edd_analysis
- approval_details
- rejection_details
```

### Variable Structure

Variables are documented with TypeScript-like notation:

```typescript
customer_basic_info: {
  full_legal_name: string,
  entity_type: "INDIVIDUAL" | "CORPORATE" | "TRUST" | "PARTNERSHIP",
  country: string,
  business_nature: string,
  expected_volumes: string,
  account_purpose: string
}

final_risk_assessment: {
  risk_rating: "LOW" | "MEDIUM" | "HIGH" | "PROHIBITED",
  risk_score: number (0-100),
  summary: string,
  key_factors: array
}
```

## Output Formatting

### Workflow Status Format

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 KYC WORKFLOW STATUS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**Current State**: [State Name]
**State Type**: [Task/Choice/Parallel/etc.]
**Action**: [GATHER/SEARCH/ANALYZE/etc.]

**Instruction**:
[State instruction from workflow]

**Information Needed**:
- [Item 1]
- [Item 2]

**Status**: ⏳ In Progress | ✅ Complete | ⏸️ Awaiting Input | ❌ Rejected
```

### Final Decision Format

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🏦 KYC ASSESSMENT COMPLETE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**Customer**: [Name]
**Assessment Date**: [Date]
**KYC Officer**: Claude (KYC Officer Skill v1.0.0)

**DECISION**: ✅ APPROVED | ⚠️ APPROVED WITH CONDITIONS | ❌ REJECTED

**Risk Rating**: [LOW/MEDIUM/HIGH/PROHIBITED]
**Risk Score**: [X/100]

**Key Risk Factors**:
- [Factor 1]
- [Factor 2]

**Monitoring Requirements**:
- Transaction Monitoring: [Standard/Enhanced]
- Review Frequency: [Annual/Semi-annual/Quarterly]
- Additional Controls: [List any specific conditions]
```

## Risk Scoring Conventions

### Scoring Matrix

| Factor | Weight | Score Range |
|--------|--------|-------------|
| Screening Risk | 40% | 0-100 |
| Jurisdictional Risk | 25% | 0-100 |
| Business Activity | 25% | 0-100 |
| Customer Profile | 10% | 0-100 |

### Risk Tier Boundaries

| Tier | Score Range | Action |
|------|-------------|--------|
| LOW | 0-30 | Auto-approve |
| MEDIUM | 31-60 | EDD required |
| HIGH | 61-85 | Extensive EDD + MLRO approval |
| PROHIBITED | 86-100 | Auto-reject |

### Scoring Indicators

```
Sanctions:
- No hits: 0
- Historical false positive: 10-20
- Close associate hit: 50-70
- True positive: 100 (auto-PROHIBITED)

PEP Status:
- Not a PEP: 0
- Former PEP (>3 years): 20-30
- Former PEP (<3 years): 40-50
- Current PEP family: 50-60
- Current senior PEP: 70-80

Jurisdiction:
- FATF compliant: 0-20
- Minor concerns: 20-40
- FATF grey list: 50-70
- FATF black list: 80-100
```

## Communication Conventions

### Tone Guidelines
- **Professional**: Formal banking language
- **Courteous**: "Thank you for providing..."
- **Clear**: Avoid jargon, explain requirements
- **Empathetic**: Acknowledge customer concerns
- **Firm**: Regulatory requirements are non-negotiable

### Tipping Off Prevention
- **Never reveal** specific screening results
- **Never mention** SAR filing to customer
- Use generic language: "based on our risk assessment"

### Standard Phrases

```
Approval:
"Based on our assessment, your application is approved..."

EDD Required:
"We need to conduct Enhanced Due Diligence as part of our regulatory obligations..."

Rejection:
"We regret to inform you that we are unable to establish a banking relationship at this time..."
```

## Audit Trail Conventions

### State Transition Logging

For each state, document:
1. State name and timestamp
2. Information collected or analyzed
3. Decision made and rationale
4. Risk score or assessment
5. Next state and reason for transition

### Decision Documentation

All decisions must include:
- **Decision**: Approve/Reject
- **Risk Rating**: LOW/MEDIUM/HIGH/PROHIBITED
- **Risk Score**: 0-100
- **Key Factors**: List of significant findings
- **Monitoring Requirements**: For approvals
- **Rejection Rationale**: For rejections (internal only)

## File Naming Conventions

### Skill Files
- `skill.json` - Skill configuration
- `skill.md` - Skill instructions
- `workflow.md` - Workflow definition
- `SAMPLE_SCENARIOS.md` - Test cases

### Output Files
- `KYC_Decision_{CustomerName}.md`
- Customer name in PascalCase
- Underscores between words

### Planning Files
- UPPERCASE with `.md` extension
- Descriptive names: `ARCHITECTURE.md`, `CONCERNS.md`
