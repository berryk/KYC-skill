# Testing Approach

## Overview

This Claude Skill project uses **scenario-based testing** rather than automated unit tests. Testing is performed by running customer scenarios through the skill and verifying the workflow path and final decision match expectations.

## Test Scenarios

### Available Scenarios (`SAMPLE_SCENARIOS.md`)

| # | Scenario | Customer Type | Risk Rating | Expected Outcome |
|---|----------|---------------|-------------|------------------|
| 1 | Sarah Johnson | Individual | LOW (15/100) | ✅ Auto-Approve |
| 2 | Global Trade Partners Ltd | Corporate | MEDIUM (48/100) | ⚠️ Approve w/ Enhanced Monitoring |
| 3 | Caribbean Investment Holdings | Corporate | HIGH (74/100) | ⚠️ Approve w/ Conditions OR ❌ Reject |
| 4 | International Trade & Shipping LLC | Corporate | PROHIBITED (100/100) | ❌ Auto-Reject (Sanctions) |
| 5 | Dr. Emmanuel Okoro | Individual PEP | HIGH (78/100) | ⚠️ Approve w/ Conditions OR ❌ Reject |

## Testing Procedure

### Manual Testing Steps

1. **Activate the Skill**
   ```
   User: I'd like to conduct KYC onboarding for a new customer
   ```

2. **Provide Scenario Data**
   - Provide customer information as prompted by the workflow
   - Use data from the chosen scenario

3. **Verify Workflow Path**
   - Check that states execute in expected order
   - Verify parallel screening runs correctly
   - Confirm Choice states route appropriately

4. **Make EDD Decisions** (if applicable)
   - At HumanIntervention states, provide the expected decision
   - Verify correct routing based on decision

5. **Verify Final Outcome**
   - Compare decision against expected outcome
   - Verify risk score is within expected range
   - Check that monitoring requirements match scenario

### Expected Workflow Paths

#### Scenario 1: Low-Risk Individual (Sarah Johnson)
```
InitialInformationGathering
→ CustomerTypeClassification [INDIVIDUAL]
→ IndividualKYC
→ ComprehensiveScreening (Parallel)
→ ScreeningResultsAnalysis [No hits]
→ SanctionsBlockCheck [FALSE]
→ JurisdictionalAnalysis [Low Risk]
→ BusinessActivityAnalysis [Low Risk]
→ RiskAssessmentCalculation [Score: 15/100, Rating: LOW]
→ RiskDecisionLogic [LOW → DocumentationReview]
→ DocumentationReview [Complete: TRUE]
→ DocumentationDecision [Complete]
→ AutoApprove ✅
```

#### Scenario 2: Medium-Risk Corporate (Global Trade Partners)
```
InitialInformationGathering
→ CustomerTypeClassification [CORPORATE]
→ EntityUnwrapping [UBO: Michael Chen (Former PEP)]
→ ComprehensiveScreening (Parallel)
→ ScreeningResultsAnalysis [PEP flag raised]
→ SanctionsBlockCheck [FALSE]
→ JurisdictionalAnalysis [UK: Low, Thailand: Medium]
→ BusinessActivityAnalysis [International trade: Medium]
→ RiskAssessmentCalculation [Score: 48/100, Rating: MEDIUM]
→ RiskDecisionLogic [MEDIUM → EnhancedDueDiligence]
→ EnhancedDueDiligence ⏸️
→ EDDDecision [APPROVE_WITH_CONDITIONS]
→ ApproveWithMonitoring ✅
```

#### Scenario 4: Prohibited Customer (Syrian Entity)
```
InitialInformationGathering
→ CustomerTypeClassification [CORPORATE]
→ EntityUnwrapping [Syrian entity]
→ ComprehensiveScreening (Parallel)
→ ScreeningResultsAnalysis [TRUE POSITIVE: Sanctioned jurisdiction]
→ SanctionsBlockCheck [TRUE]
→ AutoReject ❌
```

## Test Coverage

### State Coverage

| State Type | States | Test Coverage |
|------------|--------|---------------|
| Task | 9 states | All covered via scenarios |
| Choice | 4 states | All conditions tested |
| Parallel | 1 state (4 branches) | All branches tested |
| HumanIntervention | 3 states | Tested in scenarios 2, 3, 5 |
| Wait | 2 states | Not directly tested (simulated) |
| Success | 2 states | Tested in scenarios 1-3, 5 |
| Failure | 2 states | Tested in scenarios 3-5 |

### Risk Path Coverage

| Risk Rating | Workflow Path | Test Scenario |
|-------------|---------------|---------------|
| LOW | → DocumentationReview → AutoApprove | Scenario 1 |
| MEDIUM | → EnhancedDueDiligence → ApproveWithMonitoring | Scenario 2 |
| HIGH | → EnhancedDueDiligence → ApproveWithMonitoring OR RejectWithReason | Scenarios 3, 5 |
| PROHIBITED | → AutoReject | Scenario 4 |

### Screening Coverage

| Check Type | Test Scenarios |
|------------|----------------|
| Sanctions (No Hit) | 1, 2, 3, 5 |
| Sanctions (True Positive) | 4 |
| PEP (No Hit) | 1 |
| PEP (Former PEP) | 2 |
| PEP (Current PEP) | 5 |
| PEP (Associate) | 3 |
| Adverse Media (No Findings) | 1 |
| Adverse Media (Minor) | 2 |
| Adverse Media (Multiple) | 3, 5 |
| Enforcement (No Actions) | 1, 2, 5 |
| Enforcement (Findings) | 3 |

## Validation Criteria

### For Each Scenario

- [ ] Correct customer type classification
- [ ] All screening branches execute
- [ ] Correct risk score calculation
- [ ] Appropriate risk rating assignment
- [ ] Correct workflow routing based on risk
- [ ] HumanIntervention states pause correctly
- [ ] Terminal state matches expected outcome
- [ ] Output format follows specification

### Risk Score Validation

```
Expected Score = (Screening × 0.40) + (Jurisdictional × 0.25) + 
                 (Business × 0.25) + (Profile × 0.10)

Verify:
- Individual component scores are reasonable
- Weights are applied correctly
- Total score matches expected range (±5 points tolerance)
- Risk rating boundary is correct
```

## Sample Output Validation

The repository includes 6 sample KYC decision documents:

| Document | Purpose |
|----------|---------|
| `KYC_Decision_Quantexa_Limited.md` | Example of LOW-risk approval |
| `KYC_Decision_Aria_Associates_UK.md` | Corporate customer example |
| `KYC_Decision_Kushner_Companies_LLC.md` | Complex ownership example |
| `KYC_Decision_Metalloinvest.md` | International corporate example |
| `KYC_Decision_Norway_Ogarsen_International_Biotechnology.md` | Suspicious entity example |
| `KYC_Decision_UAB_EM_SYSTEM.md` | Sanctions-related example |

### Validation Checklist for Outputs

- [ ] Executive summary present
- [ ] Customer information complete
- [ ] Entity unwrapping documented (for corporate)
- [ ] All screening results documented
- [ ] Jurisdictional analysis complete
- [ ] Business activity analysis complete
- [ ] Risk score breakdown shown
- [ ] Final decision clearly stated
- [ ] Monitoring requirements specified
- [ ] Audit trail included

## Edge Cases to Test

### Customer Classification
- Trust entities
- Partnerships
- Individual with business accounts
- Corporate with single shareholder

### Screening Edge Cases
- Fuzzy name matches (false positives)
- Historical PEP (>3 years)
- Family member of PEP
- Minor adverse media vs. major findings

### Risk Boundary Cases
- Score = 30 (LOW/MEDIUM boundary)
- Score = 60 (MEDIUM/HIGH boundary)
- Score = 85 (HIGH/PROHIBITED boundary)

### Workflow Edge Cases
- Incomplete documentation → request → timeout
- EDD request → more info → request → approve/reject
- Multiple rounds of document requests

## Regression Testing

When modifying the skill, test:

1. **All 5 scenarios** to verify no regression
2. **Boundary cases** for any changed risk thresholds
3. **Output format** for any changed templates
4. **State transitions** for any workflow changes

## Testing Limitations

> ⚠️ **Note**: This is prompt-based testing, not automated testing

**Limitations**:
- No automated test execution
- Results depend on Claude's interpretation
- Manual verification required
- No CI/CD integration possible
- Timing/consistency may vary

**Recommendations for Production**:
- Implement actual test harness
- Create automated scenario runners
- Add regression test suite
- Implement output validation scripts
