# Sample KYC Scenarios

This document provides realistic customer scenarios for testing the KYC Officer skill. Each scenario includes complete customer information and the expected workflow outcome.

---

## Scenario 1: Low-Risk Individual Customer ✅

**Expected Outcome**: Auto-Approve (Standard Risk)

### Customer Information

**Personal Details**:
- Full Name: Sarah Johnson
- Date of Birth: March 15, 1985
- Nationality: United States
- Country of Residence: United States
- Residential Address: 123 Oak Street, Austin, TX 78701

**Employment & Financial**:
- Occupation: Software Engineer
- Employer: TechCorp Inc. (publicly traded company)
- Annual Income: $150,000
- Source of Wealth: Employment income over 10 years
- Source of Funds: Salary deposits, savings

**Account Details**:
- Account Purpose: Salary deposits and personal savings
- Expected Monthly Deposits: $12,000 (monthly salary)
- Expected Monthly Withdrawals: $8,000 (living expenses)
- Expected Transaction Volume: 15-20 transactions/month

**Documents Provided**:
- Valid US Passport
- Utility bill (electricity) dated within last 2 months
- Employment verification letter from TechCorp Inc.

**Screening Results**:
- Sanctions: No hits
- PEP: Not a PEP
- Adverse Media: No findings
- Enforcement: No regulatory actions

**Jurisdictional Assessment**:
- Country: United States (Low Risk, FATF compliant)
- Corruption Index: 67/100 (Transparent)

**Business Activity**:
- Nature: Personal banking (salary/savings)
- Risk Sector: N/A (individual)

### Expected Workflow Path

```
InitialInformationGathering
→ CustomerTypeClassification [INDIVIDUAL]
→ IndividualKYC
→ ComprehensiveScreening (Parallel: Sanctions, PEP, Media, Enforcement)
→ ScreeningResultsAnalysis [No significant hits]
→ SanctionsBlockCheck [FALSE]
→ JurisdictionalAnalysis [Low Risk]
→ BusinessActivityAnalysis [Low Risk]
→ RiskAssessmentCalculation [Score: 15/100, Rating: LOW]
→ RiskDecisionLogic [LOW → DocumentationReview]
→ DocumentationReview [Complete: TRUE]
→ DocumentationDecision [Complete]
→ AutoApprove ✅
```

**Final Risk Assessment**:
- Risk Rating: LOW
- Risk Score: 15/100
- Monitoring: Standard
- Review Frequency: Annual

---

## Scenario 2: Medium-Risk Corporate Customer ⚠️

**Expected Outcome**: Approve with Enhanced Monitoring (after EDD)

### Customer Information

**Corporate Details**:
- Legal Name: Global Trade Partners Ltd.
- Registration Number: UK-12345678
- Country of Incorporation: United Kingdom
- Date of Incorporation: January 2019
- Business Nature: Import/Export of consumer electronics
- Industry Sector: International Trade

**Corporate Structure**:
- **Ultimate Beneficial Owners**:
  - Michael Chen (60% ownership) - Former Deputy Minister of Commerce, Thailand (2015-2018)
  - David Williams (40% ownership) - British national, entrepreneur

- **Board of Directors**:
  - Michael Chen (Chairman)
  - David Williams (CEO)
  - Amanda Foster (CFO)

- **Authorized Signatories**: David Williams, Amanda Foster

**Financial Information**:
- Annual Revenue: £5 million
- Expected Monthly Transactions: £500,000
- Expected Transaction Volume: 40-60 transactions/month
- Primary Markets: Thailand, UK, EU countries
- Main Products: Consumer electronics, smartphones, tablets

**Account Purpose**:
- Receiving payments from European customers
- Making payments to Asian suppliers
- Foreign exchange transactions

**Documents Provided**:
- Certificate of Incorporation (UK)
- Shareholder Register
- Board Resolution authorizing account opening
- Audited Financial Statements (2 years)
- Company organizational chart
- Passports for all UBOs and directors
- Proof of address for all UBOs

**Screening Results**:
- **Sanctions**: No hits
- **PEP**: Michael Chen - Former PEP (Deputy Minister, Thailand, exited 2018)
- **Adverse Media**:
  - Michael Chen: One article mentioning his ministerial role, no negative content
  - David Williams: No findings
- **Enforcement**: No regulatory actions

**Jurisdictional Assessment**:
- UK: Low Risk (FATF compliant)
- Thailand: Medium Risk (Some AML concerns but not FATF listed)

**Business Activity**:
- Sector: International trade (Medium Risk - cross-border exposure)
- Cash Intensity: Low (bank transfers)
- Transaction Complexity: Medium (multi-currency, multi-jurisdiction)

### Expected Workflow Path

```
InitialInformationGathering
→ CustomerTypeClassification [CORPORATE]
→ EntityUnwrapping [UBO: Michael Chen (Former PEP), David Williams]
→ ComprehensiveScreening (Parallel branches)
  → SanctionsCheck [No hits]
  → PEPCheck [Former PEP identified: Michael Chen]
  → AdverseMediaCheck [Minor findings]
  → EnforcementCheck [No findings]
→ ScreeningResultsAnalysis [PEP flag raised]
→ SanctionsBlockCheck [FALSE]
→ JurisdictionalAnalysis [UK: Low, Thailand exposure: Medium]
→ BusinessActivityAnalysis [International trade: Medium]
→ RiskAssessmentCalculation [Score: 48/100, Rating: MEDIUM]
→ RiskDecisionLogic [MEDIUM → EnhancedDueDiligence]
→ EnhancedDueDiligence ⏸️
```

**EDD Information to Request**:
1. **Source of Wealth - Michael Chen**:
   - Post-ministerial employment history
   - Explanation of wealth accumulation
   - Tax returns (3 years)
   - Documentation of initial investment in company

2. **Source of Funds**:
   - Origin of startup capital for Global Trade Partners
   - Bank statements showing fund flows

3. **Business Information**:
   - Detailed business plan
   - List of major customers and suppliers
   - Sample contracts and invoices
   - Trade finance arrangements

4. **Enhanced Background**:
   - References from other banks
   - Explanation of business relationship between Chen and Williams
   - Rationale for Thailand focus given Chen's former government role

**User Decision Required**:
After reviewing EDD materials, user should choose: **APPROVE_WITH_CONDITIONS**

### Expected Final Outcome

```
→ EDDDecision [APPROVE_WITH_CONDITIONS]
→ ApproveWithMonitoring ✅
```

**Final Risk Assessment**:
- Risk Rating: MEDIUM
- Risk Score: 48/100
- Monitoring: Enhanced (lower alert thresholds)
- Review Frequency: Semi-annual (every 6 months)
- Special Conditions:
  - Monthly re-screening against PEP/sanctions lists
  - Transaction monitoring with 30% lower thresholds
  - Annual refresh of source of wealth documentation
  - MLRO notification of approval

---

## Scenario 3: High-Risk Entity ⚠️⚠️

**Expected Outcome**: Approve with Extensive EDD and Conditions OR Reject based on risk appetite

### Customer Information

**Corporate Details**:
- Legal Name: Caribbean Investment Holdings S.A.
- Registration Number: PA-98765432
- Country of Incorporation: Panama
- Date of Incorporation: June 2022
- Business Nature: Investment holding company
- Industry Sector: Financial services / Holdings

**Corporate Structure** (Complex):
- **Immediate Parent**: Tortola Investments Ltd. (British Virgin Islands)
- **Ultimate Parent**: Undisclosed discretionary trust (Jersey)
- **Ultimate Beneficial Owners** (per declaration):
  - Ahmed Al-Mansouri (35%) - UAE national, real estate developer
  - Maria Santos (30%) - Brazilian national, businesswoman
  - Nominee shareholder (35%) - Representing the Jersey trust

- **Board of Directors**:
  - Registered agent directors (2) - Panama
  - Ahmed Al-Mansouri
  - Maria Santos

**Financial Information**:
- Expected Initial Deposit: $5 million
- Expected Monthly Transactions: $2-3 million
- Transaction Types: Investments, dividends, fund transfers
- Primary Geographic Focus: Latin America, Middle East, Caribbean

**Account Purpose**:
- Investment portfolio management
- Receiving dividends from subsidiary companies
- Making investments in real estate and private equity

**Documents Provided** (Initial Submission):
- Certificate of Incorporation (Panama)
- Shareholder Register (incomplete - trust beneficiaries not disclosed)
- Board Resolution
- Passports for Al-Mansouri and Santos
- Corporate structure chart (partial)
- Bank reference letter from small regional bank

**Screening Results**:
- **Sanctions**:
  - No direct hits
  - Fuzzy match: "Ahmed Mansour" on OFAC list (different middle name, different DOB) - assessed as FALSE POSITIVE after investigation

- **PEP**:
  - Maria Santos: Close associate of former Brazilian state governor (business partner 2016-2019)

- **Adverse Media**:
  - Ahmed Al-Mansouri: Article from 2020 about failed real estate development, civil litigation ongoing
  - Maria Santos: Named in 2021 investigation into government contracts in Brazil (no charges filed)

- **Enforcement**:
  - No direct regulatory actions against individuals
  - Tortola Investments Ltd: Parent company mentioned in Panama Papers leak (no wrongdoing proven)

**Jurisdictional Assessment**:
- **Panama**: High Risk (Secrecy jurisdiction, FATF concerns historically)
- **BVI**: High Risk (Offshore financial center, limited transparency)
- **Jersey**: Medium Risk (Reputable but used for tax planning)
- **UAE**: Medium Risk (Good AML framework but regional risks)
- **Brazil**: Medium Risk (Corruption concerns)

**Business Activity**:
- Sector: Investment holding / Offshore structure (High Risk)
- Complexity: Very High (multi-layered, cross-border, opaque ownership)
- Cash Intensity: Low (wire transfers)
- Transparency: Low (undisclosed trust beneficiaries)

### Expected Workflow Path

```
InitialInformationGathering
→ CustomerTypeClassification [CORPORATE]
→ EntityUnwrapping [Complex structure, trust involved, multiple high-risk jurisdictions]
→ ComprehensiveScreening (Parallel branches)
  → SanctionsCheck [Fuzzy match - requires investigation → FALSE POSITIVE]
  → PEPCheck [Maria Santos - PEP Associate]
  → AdverseMediaCheck [Multiple findings - civil litigation, investigation]
  → EnforcementCheck [Panama Papers mention]
→ ScreeningResultsAnalysis [Multiple risk flags]
→ SanctionsBlockCheck [FALSE - fuzzy match cleared]
→ JurisdictionalAnalysis [Multiple high-risk jurisdictions: Panama, BVI]
→ BusinessActivityAnalysis [Offshore structure, investment holding: High Risk]
→ RiskAssessmentCalculation [Score: 74/100, Rating: HIGH]
→ RiskDecisionLogic [HIGH → EnhancedDueDiligence]
→ EnhancedDueDiligence ⏸️
```

**Extensive EDD Information to Request**:

1. **Complete Ownership Transparency**:
   - Full disclosure of trust beneficiaries (all individuals with >25% beneficial interest)
   - Trust deed and letter of wishes
   - Trustee certification
   - Explanation for use of offshore structure
   - Economic substance documentation for each entity in chain

2. **Source of Wealth - All UBOs**:
   - Ahmed Al-Mansouri:
     - Complete history of real estate business
     - Financial statements for operating companies
     - Explanation of ongoing civil litigation and expected outcome
     - Tax returns (5 years)

   - Maria Santos:
     - Business history and income sources
     - Detailed explanation of relationship with former governor
     - Documentation clearing her from the 2021 investigation
     - Tax returns (5 years)

   - Trust Beneficiaries:
     - Identity documents for all beneficiaries
     - Source of wealth for trust settlor
     - Purpose of trust structure

3. **Source of Funds**:
   - Detailed origin of $5M initial deposit
   - Bank statements tracing fund movement
   - Documentation of sale transactions or dividend distributions creating the funds
   - Independent verification of fund origins

4. **Business Justification**:
   - Detailed business plan for next 3 years
   - List of all subsidiary investments
   - Expected counterparties and transaction patterns
   - Legitimate business reason for Panama/BVI structure vs. onshore alternative
   - Economic substance evidence (office, employees, local operations)

5. **Enhanced Background**:
   - Independent background investigation reports for all UBOs
   - Litigation search results
   - Media monitoring report
   - Reference letters from at least 2 Tier 1 banks (if any existing relationships)
   - Professional references (lawyers, accountants)

6. **Adverse Media Explanations**:
   - Written explanation of Al-Mansouri litigation with supporting documents
   - Written explanation of Santos investigation with evidence of clearance
   - Explanation of Panama Papers mention for parent company

7. **Mitigation Measures**:
   - Proposed transaction monitoring parameters
   - Commitment to periodic (quarterly) updates
   - Agreement to enhanced transparency requirements

**User Decision Required**:
After extensive EDD review, user must decide based on bank's risk appetite.

**Option A - APPROVE_WITH_CONDITIONS** (if EDD satisfactory):
```
→ EDDDecision [APPROVE_WITH_CONDITIONS]
→ ApproveWithMonitoring ✅
```

**Final Risk Assessment** (if approved):
- Risk Rating: HIGH
- Risk Score: 74/100
- Monitoring: Extensive Enhanced Monitoring
- Review Frequency: Quarterly
- Special Conditions:
  - MLRO approval required
  - Senior management (CCO/Board) notification
  - Transaction monitoring: 50% lower thresholds than standard
  - Real-time screening for every transaction >$100K
  - Monthly re-screening of all UBOs and related parties
  - Quarterly source of funds refresh
  - Annual full KYC refresh (vs. standard 3 years)
  - Restricted activities: No cash transactions, no third-party payments without pre-approval
  - Maximum relationship size: $10M (cap on deposits/assets)

**Option B - REJECT** (if risk too high):
```
→ EDDDecision [REJECT]
→ RejectWithReason ❌
```

**Rejection Rationale**:
- Risk exceeds bank's appetite for offshore structures
- Insufficient transparency in ownership (trust not fully disclosed)
- Multiple high-risk jurisdictions
- Adverse media findings not adequately explained
- Complexity of structure not justified by legitimate business need

---

## Scenario 4: Prohibited Customer ❌

**Expected Outcome**: Auto-Reject (Sanctions Match)

### Customer Information

**Corporate Details**:
- Legal Name: International Trade & Shipping LLC
- Registration Number: SY-44556677
- Country of Incorporation: Syria
- Date of Incorporation: March 2020
- Business Nature: Import/export, shipping logistics
- Industry Sector: Trade, logistics

**Corporate Structure**:
- **Ultimate Beneficial Owner**:
  - Hassan Mahmoud (100% ownership) - Syrian national

- **Board of Directors**:
  - Hassan Mahmoud (Managing Director)

**Financial Information**:
- Expected Monthly Transactions: $500,000
- Transaction Types: Trade finance, payments for goods
- Geographic Focus: Syria, Lebanon, Russia

**Account Purpose**:
- Facilitating international trade
- Payment processing for import/export

**Screening Results**:
- **Sanctions**:
  - **TRUE POSITIVE MATCH**: Syria is subject to comprehensive OFAC sanctions
  - **Entity located in sanctioned jurisdiction**
  - Hassan Mahmoud: Possible fuzzy match with OFAC SDN list (under investigation)

### Expected Workflow Path

```
InitialInformationGathering
→ CustomerTypeClassification [CORPORATE]
→ EntityUnwrapping [Syrian entity identified]
→ ComprehensiveScreening (Parallel branches)
  → SanctionsCheck [SANCTIONED JURISDICTION - Syria under OFAC sanctions]
  → [Other checks may run but irrelevant after sanctions hit]
→ ScreeningResultsAnalysis [TRUE POSITIVE: Sanctioned country]
→ SanctionsBlockCheck [TRUE]
→ AutoReject ❌ [PROHIBITED]
```

**Final Decision**:
- Decision: REJECTED
- Reason: Sanctions list match - Prohibited jurisdiction
- SAR Filing: Required (suspicious activity attempting to circumvent sanctions)
- Risk Rating: PROHIBITED
- Risk Score: 100/100

**Customer Communication**:
"After careful review, we regret to inform you that we are unable to establish a banking relationship at this time. This decision is based on our internal risk assessment policies and regulatory obligations."

(Note: Do NOT reveal that the rejection is due to sanctions to avoid "tipping off")

---

## Scenario 5: Individual PEP - High Risk ⚠️⚠️

**Expected Outcome**: Approve with Enhanced Monitoring (after extensive EDD) OR Reject

### Customer Information

**Personal Details**:
- Full Name: Dr. Emmanuel Okoro
- Date of Birth: June 8, 1968
- Nationality: Nigeria
- Country of Residence: United Kingdom (resident since 2023)
- Residential Address: 45 Kensington Park Road, London W11 2EU

**Employment & Financial**:
- Current Occupation: International business consultant
- Previous Position: Minister of Petroleum Resources, Nigeria (2018-2022)
- Annual Income: £500,000 (consulting fees)
- Source of Wealth: Government salary (ministerial position), pension, consulting income, family inheritance

**Account Details**:
- Account Purpose: Personal banking, receiving consulting fees, investments
- Expected Initial Deposit: £1,000,000
- Expected Monthly Deposits: £50,000 - £100,000
- Expected Monthly Withdrawals: £40,000
- Source of Funds for Initial Deposit: Sale of property in Nigeria

**Documents Provided** (Initial):
- Nigerian passport + UK residence permit
- UK utility bill (council tax)
- Consulting contract with international oil company
- Property sale agreement from Nigeria

**Screening Results**:
- **Sanctions**: No hits
- **PEP**: **Current Senior PEP** (former minister, within 3-year window)
- **Adverse Media**:
  - Multiple articles about petroleum sector corruption in Nigeria during his tenure (2018-2022)
  - One article alleging contract irregularities (no charges filed)
  - Article about his resignation in 2022 (political reasons cited)
- **Enforcement**: No personal regulatory actions

**Jurisdictional Assessment**:
- Nigeria: High Risk (FATF grey list historically, high corruption, petroleum sector risks)
- UK: Low Risk (current residence)

**Business Activity**:
- Sector: Oil & Gas consulting (High Risk sector)
- Previous Government Role: Petroleum ministry (High Risk - corruption concerns)

### Expected Workflow Path

```
InitialInformationGathering
→ CustomerTypeClassification [INDIVIDUAL]
→ IndividualKYC
→ ComprehensiveScreening (Parallel branches)
  → SanctionsCheck [No hits]
  → PEPCheck [Senior PEP - Former Minister (within 3 years)]
  → AdverseMediaCheck [Multiple findings - corruption allegations in petroleum sector]
  → EnforcementCheck [No personal actions but sector concerns]
→ ScreeningResultsAnalysis [PEP + Adverse Media + High-risk sector]
→ SanctionsBlockCheck [FALSE]
→ JurisdictionalAnalysis [Nigeria: High Risk]
→ BusinessActivityAnalysis [Oil & Gas: High Risk]
→ RiskAssessmentCalculation [Score: 78/100, Rating: HIGH]
→ RiskDecisionLogic [HIGH → EnhancedDueDiligence]
→ EnhancedDueDiligence ⏸️
```

**Extensive EDD Information to Request**:

1. **Source of Wealth**:
   - Complete employment history with salary documentation
   - Ministerial salary records (2018-2022)
   - Government pension documentation
   - Family inheritance: Will/probate documents, valuation
   - Consulting income: Tax returns (5 years)
   - Asset register: All properties, investments, business interests

2. **Source of Funds - £1M Initial Deposit**:
   - Property sale documentation:
     - Sale contract
     - Property deed
     - Sales proceeds bank statement
     - Property purchase history (how acquired originally)
     - Property valuation report
     - Buyer identity
   - Fund tracing: Complete trail from Nigeria to UK bank

3. **Enhanced Background**:
   - Independent background investigation report
   - Verification of ministerial role and resignation circumstances
   - Response to adverse media allegations:
     - Detailed written explanation of corruption allegations
     - Evidence of clearance (if investigated)
     - Legal opinion if needed
   - Professional references from reputable sources

4. **PEP Relationship Details**:
   - Declaration of family members and close associates in government
   - List of business relationships with other PEPs or government officials
   - Confirmation of no ongoing government role or influence

5. **Current Business Activities**:
   - Consulting firm details (if applicable)
   - Client list for consulting work
   - Explanation of expertise and why hired by oil companies
   - Expected future income and transaction patterns

6. **Tax Compliance**:
   - Tax clearance certificate from Nigeria
   - UK tax registration and compliance documentation
   - Explanation of tax residency status

**Red Flags to Address**:
- Former minister in high-corruption sector (petroleum in Nigeria)
- Significant wealth (£1M deposit) vs. government salary
- Adverse media about corruption (even without charges)
- Transition from government to private sector consulting in same industry
- Property sale in Nigeria (potential for questionable origin of assets)

**User Decision Required**:

**If EDD is satisfactory** (clear explanations, verified legitimate sources):
```
→ EDDDecision [APPROVE_WITH_CONDITIONS]
→ ApproveWithMonitoring ✅
```

**Final Risk Assessment** (if approved):
- Risk Rating: HIGH
- Risk Score: 78/100 (reduced from initial 82 after satisfactory EDD)
- Monitoring: Extensive Enhanced Monitoring
- Review Frequency: Quarterly for first 2 years, then semi-annual
- Special Conditions:
  - Board/Senior Management approval required
  - MLRO personal review
  - Transaction monitoring: 60% lower thresholds
  - Real-time screening for transactions >£50K
  - Monthly PEP re-screening
  - Semi-annual source of wealth refresh
  - No cash transactions permitted
  - Third-party payment restrictions
  - Geographic restrictions: Extra scrutiny on Nigeria transactions
  - Annual full KYC refresh
  - Mandatory exit date: PEP status expires 3 years after leaving office (2025), then reassess

**If EDD is not satisfactory** (unexplained wealth, unresolved adverse media):
```
→ EDDDecision [REJECT]
→ RejectWithReason ❌
```

**Rejection Rationale**:
- Cannot satisfactorily explain source of wealth
- Adverse media allegations not adequately addressed
- Risk of proceeds of corruption
- Reputation risk too high for bank

---

## Using These Scenarios

### Testing the Workflow

1. **Activate the KYC Officer skill**
2. **Present the scenario**: "I have a customer to onboard..." and provide information gradually as prompted
3. **Follow the workflow**: Respond to requests for information by providing details from the scenario
4. **Make decisions at HumanIntervention states**: When EDD is required, decide whether to approve/reject based on scenario guidance
5. **Verify the outcome**: Check that the final decision matches the expected outcome

### Example Dialogue

```
User: I'd like to activate the KYC skill for a new customer onboarding

KYC Officer: [Activates and greets]

User: I have a corporate customer - Global Trade Partners Ltd from the UK

KYC Officer: [Begins InitialInformationGathering state, asks for basic information]

User: [Provides details from Scenario 2]

KYC Officer: [Progresses through workflow...]

... [Eventually reaches EnhancedDueDiligence state] ...

KYC Officer: ⏸️ WORKFLOW PAUSED FOR EDD
Risk Rating: MEDIUM (48/100)
Key Factor: Former PEP identified (Michael Chen)
[Requests additional documentation]

User: The customer has provided source of wealth documentation showing post-government consulting income and legitimate business investment. No concerns found in enhanced background checks. I believe we should approve with enhanced monitoring.

KYC Officer: [Processes decision: APPROVE_WITH_CONDITIONS]
[Proceeds to ApproveWithMonitoring]
[Outputs final decision with monitoring requirements]
```

### Customizing Scenarios

Feel free to modify these scenarios or create new ones:

- Change risk factors (add/remove PEP status, change jurisdictions, etc.)
- Adjust complexity (simplify or add more layers)
- Mix characteristics (combine elements from multiple scenarios)
- Create edge cases (borderline risk scores, unusual structures)
- Test specific compliance issues (sanctions, FATF grey-list countries, high-risk sectors)

---

## Scenario Summary Matrix

| Scenario | Customer Type | Risk Rating | Key Factors | Expected Outcome |
|----------|---------------|-------------|-------------|------------------|
| 1. Sarah Johnson | Individual | LOW (15/100) | Standard profile, no hits | Auto-Approve |
| 2. Global Trade Partners | Corporate | MEDIUM (48/100) | Former PEP, international trade | Approve w/ Enhanced Monitoring |
| 3. Caribbean Holdings | Corporate | HIGH (74/100) | Offshore structure, multiple risk factors | Approve w/ Conditions OR Reject |
| 4. Syrian Entity | Corporate | PROHIBITED (100/100) | Sanctioned jurisdiction | Auto-Reject |
| 5. Dr. Okoro | Individual PEP | HIGH (78/100) | Senior PEP, adverse media, petroleum sector | Approve w/ Conditions OR Reject |

---

**Note**: These scenarios are for training and testing purposes only. Real KYC assessments should be conducted by qualified compliance professionals using actual screening databases and following institution-specific policies.
