# KYC Onboarding Workflow

**name**: KYC_Onboarding_Workflow
**version**: 1.0.0
**startAt**: InitialInformationGathering
**description**: Tier 1 Bank KYC/AML Onboarding Workflow for Corporate and Individual Customers

---

## Workflow States

### 1. InitialInformationGathering
- **type**: Task
- **action**: GATHER
- **instruction**: "Greet the customer professionally. Collect basic information: Full legal name (individual or entity), Country of incorporation/residence, Nature of business/occupation, Expected transaction volumes, Purpose of account opening."
- **output_variable**: "customer_basic_info"
- **validation**: "Ensure all required fields are collected before proceeding"
- **next**: CustomerTypeClassification

---

### 2. CustomerTypeClassification
- **type**: Choice
- **input**: "customer_basic_info.entity_type"
- **description**: "Determine if customer is Individual or Corporate Entity"
- **choices**:
  - **condition**: "entity_type == INDIVIDUAL"
    - **next**: IndividualKYC
  - **condition**: "entity_type == CORPORATE || entity_type == TRUST || entity_type == PARTNERSHIP"
    - **next**: EntityUnwrapping

---

### 3. IndividualKYC
- **type**: Task
- **action**: GATHER
- **instruction**: "For individual customers, collect: Date of birth, Residential address, Occupation and employer, Source of wealth and funds, Government-issued ID (passport/driver's license), Proof of address (utility bill, bank statement)."
- **output_variable**: "individual_kyc_data"
- **next**: ComprehensiveScreening

---

### 4. EntityUnwrapping
- **type**: Task
- **action**: SEARCH
- **instruction**: "For corporate entities, identify and document: Target company full legal name and registration number, Country of incorporation, Immediate parent companies (if any), Ultimate Beneficial Owners (UBOs) - individuals with 25%+ ownership or control, Board of Directors and C-Suite executives, Authorized signatories, Corporate structure diagram if complex."
- **output_variable**: "entity_structure_list"
- **required_documents**:
  - "Certificate of Incorporation"
  - "Shareholder Register"
  - "Organizational Chart"
  - "Board Resolution authorizing account opening"
- **next**: ComprehensiveScreening

---

### 5. ComprehensiveScreening
- **type**: Parallel
- **description**: "Screen all identified parties (individuals, UBOs, directors, authorized signatories) simultaneously against multiple watchlists and databases"
- **timeout**: "120 seconds"
- **next**: ScreeningResultsAnalysis
- **branches**:

  #### 5a. SanctionsCheck
  - **name**: SanctionsCheck
  - **action**: SEARCH
  - **instruction**: "Check all names against: OFAC SDN List (US), UK HMT Sanctions List, EU Consolidated Sanctions List, UN Security Council Sanctions, Local jurisdiction sanctions. Report any exact or fuzzy matches with match score."
  - **output_variable**: "sanctions_results"

  #### 5b. PEPCheck
  - **name**: PEPCheck
  - **action**: SEARCH
  - **instruction**: "Screen for Politically Exposed Persons (PEPs): Current or former senior government officials, Politicians and party officials, Senior executives of state-owned enterprises, Close family members or associates of PEPs. Check World-Check, Dow Jones, or similar PEP databases."
  - **output_variable**: "pep_results"

  #### 5c. AdverseMediaCheck
  - **name**: AdverseMediaCheck
  - **action**: SEARCH
  - **instruction**: "Search for adverse media coverage in past 7 years: Financial crimes (fraud, embezzlement, money laundering), Terrorism or terrorist financing, Corruption or bribery, Regulatory sanctions or enforcement actions, Significant civil litigation, Bankruptcy or insolvency. Use news aggregators and specialized databases."
  - **output_variable**: "media_results"

  #### 5d. EnforcementCheck
  - **name**: EnforcementCheck
  - **action**: SEARCH
  - **instruction**: "Check regulatory enforcement databases: SEC enforcement actions, FinCEN actions, FCA final notices, Other financial regulator sanctions, Criminal convictions related to financial crimes."
  - **output_variable**: "enforcement_results"

---

### 6. ScreeningResultsAnalysis
- **type**: Task
- **action**: ANALYZE
- **instruction**: "Review all screening results. For each hit: Assess if it's a true positive or false positive (fuzzy match), Document the nature and severity of the finding, Calculate a screening risk score (0-100). If there are TRUE POSITIVE hits on sanctions lists, immediately flag for rejection."
- **output_variable**: "screening_analysis"
- **next**: SanctionsBlockCheck

---

### 7. SanctionsBlockCheck
- **type**: Choice
- **input**: "screening_analysis.sanctions_true_positive"
- **description**: "Automatic rejection for sanctioned entities"
- **choices**:
  - **condition**: "sanctions_true_positive == TRUE"
    - **next**: AutoReject
  - **condition**: "sanctions_true_positive == FALSE"
    - **next**: JurisdictionalAnalysis

---

### 8. JurisdictionalAnalysis
- **type**: Task
- **action**: ANALYZE
- **instruction**: "Assess jurisdictional risks: Check if country is on FATF Grey or Black List, Review country corruption index (Transparency International), Assess quality of AML/CFT regime, Identify if it's a tax haven or high-risk jurisdiction, Review any specific country-based restrictions or enhanced measures required by the bank."
- **output_variable**: "jurisdictional_risk"
- **risk_factors**:
  - "FATF Black List: HIGH RISK"
  - "FATF Grey List: ELEVATED RISK"
  - "Transparency International CPI < 40: HIGH RISK"
  - "Secrecy jurisdictions: ELEVATED RISK"
- **next**: BusinessActivityAnalysis

---

### 9. BusinessActivityAnalysis
- **type**: Task
- **action**: ANALYZE
- **instruction**: "Evaluate the nature of business activities: Industry sector risk (high-risk industries: MSBs, crypto, gaming, arms, precious metals), Transaction patterns and expected volumes, Cross-border payment exposure, Cash-intensive business indicator, Complexity and transparency of business model."
- **output_variable**: "business_risk"
- **high_risk_sectors**:
  - "Money Service Businesses (MSBs)"
  - "Cryptocurrency/Virtual Assets"
  - "Online Gaming/Gambling"
  - "Arms/Defense"
  - "Precious Metals/Stones"
  - "Shell companies/Inactive entities"
- **next**: RiskAssessmentCalculation

---

### 10. RiskAssessmentCalculation
- **type**: Task
- **action**: COMPUTE
- **instruction**: "Synthesize all risk factors into a final risk rating: Screening hits (sanctions, PEP, adverse media): Weight 40%, Jurisdictional risk: Weight 25%, Business activity risk: Weight 25%, Customer profile completeness and transparency: Weight 10%. Calculate final risk score (0-100) and assign risk rating: LOW (0-30), MEDIUM (31-60), HIGH (61-85), PROHIBITED (86-100)."
- **output_variable**: "final_risk_assessment"
- **scoring_matrix**:
  - "Sanctions hit (true positive): Auto PROHIBITED"
  - "PEP + High-risk jurisdiction + High-risk business: HIGH"
  - "PEP + Low-risk jurisdiction + Transparent business: MEDIUM"
  - "No hits + Low-risk jurisdiction + Standard business: LOW"
- **next**: RiskDecisionLogic

---

### 11. RiskDecisionLogic
- **type**: Choice
- **input**: "final_risk_assessment.risk_rating"
- **description**: "Route customer based on risk rating"
- **choices**:
  - **condition**: "risk_rating == PROHIBITED"
    - **next**: AutoReject
  - **condition**: "risk_rating == LOW"
    - **next**: DocumentationReview
  - **condition**: "risk_rating == MEDIUM"
    - **next**: EnhancedDueDiligence
  - **condition**: "risk_rating == HIGH"
    - **next**: EnhancedDueDiligence

---

### 12. DocumentationReview
- **type**: Task
- **action**: REVIEW
- **instruction**: "Verify all required documentation is complete and valid: Identity documents are government-issued and not expired, Proof of address is recent (within 3 months), Corporate documents are certified and apostilled if required, UBO declarations are signed and dated, All information is consistent across documents."
- **output_variable**: "documentation_status"
- **next**: DocumentationDecision

---

### 13. DocumentationDecision
- **type**: Choice
- **input**: "documentation_status.complete"
- **description**: "Check if documentation is complete"
- **choices**:
  - **condition**: "complete == TRUE"
    - **next**: AutoApprove
  - **condition**: "complete == FALSE"
    - **next**: RequestAdditionalDocuments

---

### 14. RequestAdditionalDocuments
- **type**: HumanIntervention
- **cause**: "Incomplete Documentation"
- **instruction**: "PAUSE workflow. List the missing or inadequate documents. Request the customer to provide: [Specific list of missing items]. Explain why each document is required for regulatory compliance. Set a deadline for submission (typically 10 business days)."
- **output_variable**: "additional_doc_request"
- **next**: AwaitDocumentSubmission

---

### 15. AwaitDocumentSubmission
- **type**: Wait
- **instruction**: "Wait for customer to submit additional documents. Once received, return to DocumentationReview state."
- **timeout**: "10 business days"
- **on_timeout**: "Reject"
- **on_completion**: "DocumentationReview"

---

### 16. EnhancedDueDiligence
- **type**: HumanIntervention
- **cause**: "Elevated Risk Detected - MEDIUM or HIGH rating"
- **instruction**: "STOP for Enhanced Due Diligence (EDD). Present comprehensive risk summary to user: List all risk factors identified (PEP status, adverse media, jurisdictional concerns, business risks), Show the risk score breakdown, Request additional information: Detailed Source of Wealth (SOW) documentation, Detailed Source of Funds (SOF) for initial deposit, Business plan and financial projections, References from other financial institutions, Enhanced background checks on UBOs and directors, Explanation for any adverse findings. Ask user: Should we proceed with this customer? Are the risk factors acceptable with additional controls?"
- **output_variable**: "edd_analysis"
- **required_for_edd**:
  - "Source of Wealth narrative and supporting documents"
  - "Source of Funds for initial transactions"
  - "Enhanced background reports"
  - "Rationale for accepting elevated risk"
- **next**: EDDDecision

---

### 17. EDDDecision
- **type**: Choice
- **input**: "edd_analysis.user_decision"
- **description**: "Compliance officer decision after EDD review"
- **choices**:
  - **condition**: "user_decision == APPROVE_WITH_CONDITIONS"
    - **next**: ApproveWithMonitoring
  - **condition**: "user_decision == REJECT"
    - **next**: RejectWithReason
  - **condition**: "user_decision == REQUEST_MORE_INFO"
    - **next**: RequestAdditionalEDD

---

### 18. RequestAdditionalEDD
- **type**: HumanIntervention
- **cause**: "Additional EDD Information Required"
- **instruction**: "Specify exactly what additional information or clarification is needed. Contact customer and request supplementary EDD materials. Document the request in the case file."
- **next**: AwaitEDDSubmission

---

### 19. AwaitEDDSubmission
- **type**: Wait
- **instruction**: "Wait for customer to submit additional EDD materials. Once received, return to EnhancedDueDiligence state for re-review."
- **timeout**: "15 business days"
- **on_timeout**: "Reject"
- **on_completion**: "EnhancedDueDiligence"

---

### 20. ApproveWithMonitoring
- **type**: Success
- **message**: "KYC APPROVED - Enhanced Monitoring Required"
- **instruction**: "Customer is approved subject to enhanced monitoring conditions: Transaction monitoring: Set lower thresholds for alerts, Periodic review: Schedule next review in 6 months (vs. standard 12 months), Ongoing screening: Monthly re-screening against watchlists, Senior management notification: Inform MLRO of high-risk acceptance, Document rationale: Record why risk is acceptable and what mitigating controls are in place. Generate approval letter with account opening terms and conditions."
- **output_variable**: "approval_details"
- **final_action**: "CREATE_ACCOUNT_WITH_ENHANCED_MONITORING"

---

### 21. AutoApprove
- **type**: Success
- **message**: "KYC APPROVED - Standard Risk"
- **instruction**: "Customer is approved for onboarding: Risk rating: LOW, Monitoring: Standard transaction monitoring, Review cycle: Annual review, No additional restrictions required. Generate approval letter and proceed with account opening. Welcome the customer to the bank and explain next steps for account activation."
- **output_variable**: "approval_details"
- **final_action**: "CREATE_ACCOUNT_STANDARD"

---

### 22. AutoReject
- **type**: Failure
- **message**: "KYC REJECTED - Prohibited Customer"
- **instruction**: "Customer cannot be onboarded due to: Sanctions list match (true positive), Prohibited risk rating, Regulatory restrictions. Send formal rejection notice (without disclosing specific reason per FATF guidance to avoid tipping off). File Suspicious Activity Report (SAR) if required. Document rejection in case management system. Politely inform customer that we cannot proceed with the relationship at this time."
- **output_variable**: "rejection_details"
- **final_action**: "REJECT_AND_FILE_SAR_IF_REQUIRED"

---

### 23. RejectWithReason
- **type**: Failure
- **message**: "KYC REJECTED - Risk Appetite Exceeded"
- **instruction**: "Customer rejected after EDD review: Document specific reasons for rejection, File SAR if suspicious activity indicators present, Send professional rejection letter, Preserve case file for regulatory audit trail. Inform customer professionally that we cannot establish a banking relationship based on our risk assessment."
- **output_variable**: "rejection_details"
- **final_action**: "REJECT_WITH_DOCUMENTATION"

---

## Workflow Metadata

**Compliance Framework**:
- Bank Secrecy Act (BSA)
- USA PATRIOT Act Section 326
- FinCEN Customer Due Diligence Requirements (31 CFR 1010.230)
- FATF Recommendations (especially R.10, R.22, R.24)
- Wolfsberg Group AML Principles

**Document Retention**: All KYC records must be retained for minimum 5 years after relationship termination

**Escalation**: Any MEDIUM or HIGH risk customer requires MLRO (Money Laundering Reporting Officer) approval

**Audit Trail**: Every state transition must be logged with timestamp, user, and decision rationale

---

## Risk Scoring Reference

| Factor | Low Risk (1-30) | Medium Risk (31-60) | High Risk (61-85) | Prohibited (86-100) |
|--------|----------------|-------------------|------------------|-------------------|
| Sanctions | No hits | Historical false positives | Close associate hit | True positive match |
| PEP Status | Not a PEP | Former PEP (>3 years) | Current PEP family member | Current senior PEP |
| Jurisdiction | FATF compliant | Minor concerns | FATF grey list | FATF black list |
| Business | Transparent, standard | Some complexity | High-risk sector | MSB/Crypto unlicensed |
| Adverse Media | None | Minor civil issues | Regulatory fine | Criminal conviction |

