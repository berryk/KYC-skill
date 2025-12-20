# KYC Officer Skill

## Overview

The KYC Officer skill transforms Claude into a professional Know Your Customer (KYC) officer at a Tier 1 international bank. This skill follows a comprehensive, deterministic workflow for customer onboarding and due diligence, ensuring compliance with global AML/CFT regulations.

## Features

### Comprehensive Workflow Engine
- **23 distinct workflow states** covering the full KYC lifecycle
- **Deterministic execution** following a state machine pattern defined in Markdown
- **Multiple state types**: Task, Choice, Parallel, HumanIntervention, Wait, Success, Failure
- **Audit trail** with complete documentation of decisions and state transitions

### Risk-Based Approach
- **Multi-dimensional risk scoring** across screening, jurisdictional, business, and customer dimensions
- **Four risk tiers**: LOW (0-30), MEDIUM (31-60), HIGH (61-85), PROHIBITED (86-100)
- **Automated risk routing** with appropriate approval requirements

### Compliance Coverage
- **Bank Secrecy Act (BSA)** compliance
- **USA PATRIOT Act** Section 326 requirements
- **FinCEN Customer Due Diligence** (31 CFR 1010.230)
- **FATF Recommendations** (R.10, R.22, R.24)
- **Wolfsberg Group** AML principles

### Screening Capabilities
- **Sanctions screening**: OFAC, UN, EU, UK HMT
- **PEP identification**: Current and former politically exposed persons
- **Adverse media**: 7-year lookback for financial crime indicators
- **Regulatory enforcement**: SEC, FinCEN, FCA actions

### Customer Types Supported
- Individual customers (retail banking)
- Corporate entities
- Trusts and partnerships
- Complex multi-jurisdictional structures

## File Structure

```
.claude/skills/kyc-officer/
├── skill.json              # Skill metadata and configuration
├── skill.md                # Main skill prompt and instructions
├── workflow.md             # Deterministic workflow definition
├── README.md               # This file
└── SAMPLE_SCENARIOS.md     # Example customer scenarios for testing
```

## How It Works

### Workflow-Driven Execution

The skill operates by:

1. **Loading the workflow** from `workflow.md` (state machine definition)
2. **Starting at InitialInformationGathering** state
3. **Executing each state** according to its type and instructions
4. **Maintaining context** of all collected information and risk factors
5. **Transitioning between states** based on conditions and decision logic
6. **Reaching a terminal state** with a final decision (approve/reject)

### State Types

**Task**: Perform an action (GATHER, SEARCH, ANALYZE, COMPUTE, REVIEW)
- Collect information, conduct research, or perform analysis
- Store results in output variables
- Transition to next state

**Choice**: Conditional branching based on data
- Evaluate conditions against collected data
- Route to different next states based on which condition matches

**Parallel**: Execute multiple branches simultaneously
- Perform concurrent screening checks (sanctions, PEP, media, enforcement)
- Wait for all branches to complete before proceeding

**HumanIntervention**: Pause for user input
- Stop workflow and present information to user
- Request decision or additional input
- Used for Enhanced Due Diligence and escalations

**Wait**: Pause for external events
- Wait for customer to submit additional documents
- Timeout and fallback handling

**Success/Failure**: Terminal states
- Final decision with complete risk assessment
- Account creation or rejection with documentation

### Risk Scoring Matrix

| Factor | Weight | Inputs |
|--------|--------|--------|
| Screening | 40% | Sanctions hits, PEP status, adverse media, enforcement actions |
| Jurisdictional | 25% | FATF status, corruption index, AML regime quality |
| Business Activity | 25% | Industry sector, transaction complexity, cross-border exposure |
| Customer Profile | 10% | Information completeness, documentation quality, consistency |

## Usage

### Activating the Skill

If using Claude Code with skills support:

```bash
# In chat
/skill kyc-officer
```

Or reference the skill in your conversation:

```
I need to conduct KYC for a new customer. Can you act as a KYC officer?
```

### Starting a KYC Assessment

Once activated, the skill will:
1. Greet you professionally
2. Ask if you have a customer to onboard or want a sample scenario
3. Begin collecting information following the workflow

### Example Flow

```
User: I have a corporate customer that wants to open an account

KYC Officer:
Good afternoon, and thank you for bringing this onboarding request. I'll guide
you through our corporate KYC process following regulatory requirements.

Let's begin with basic information gathering:

1. Full legal name of the entity
2. Country of incorporation
3. Nature of business
4. Expected transaction volumes
5. Purpose of account opening

Please provide these details to proceed.
```

### Working Through Enhanced Due Diligence

For higher-risk customers, the workflow will pause at **EnhancedDueDiligence** state:

```
⏸️ WORKFLOW PAUSED FOR ENHANCED DUE DILIGENCE

Risk Assessment Summary:
- Risk Rating: MEDIUM
- Risk Score: 55/100
- Key Factors: PEP identified, high-risk sector

Additional Information Required:
1. Source of Wealth documentation
2. Source of Funds for initial deposit
3. Enhanced background checks
4. Business plan and financial projections

Should we proceed with this customer?
[User provides decision: APPROVE_WITH_CONDITIONS / REJECT / REQUEST_MORE_INFO]
```

## Sample Scenarios

See `SAMPLE_SCENARIOS.md` for complete example customer profiles you can use to test the workflow:

- **Low Risk Individual**: Standard retail customer, auto-approval expected
- **Medium Risk Corporate**: PEP involvement, requires EDD
- **High Risk Entity**: Complex structure + high-risk jurisdiction, extensive EDD
- **Prohibited Customer**: Sanctions hit, auto-rejection

## Customization

### Modifying the Workflow

Edit `workflow.md` to customize the workflow:

1. **Add new states**: Define state name, type, instruction, and transitions
2. **Modify risk thresholds**: Adjust scoring weights and tier boundaries
3. **Change screening lists**: Update sanctioned jurisdictions or high-risk sectors
4. **Add document requirements**: Specify additional required documentation

Example: Adding a new screening check

```markdown
### 5e. CryptocurrencyCheck
- **name**: CryptocurrencyCheck
- **action**: SEARCH
- **instruction**: "Check if customer has cryptocurrency holdings or transactions. Assess crypto exchange relationships and digital asset exposure."
- **output_variable**: "crypto_results"
```

### Adjusting Risk Appetite

Modify scoring thresholds in `skill.md`:

```markdown
Based on total score (0-100):
- **LOW (0-35)**: Standard onboarding      # Increased from 30
- **MEDIUM (36-70)**: Enhanced due diligence  # Increased from 60
- **HIGH (71-90)**: Extensive EDD          # Increased from 85
- **PROHIBITED (91-100)**: Cannot onboard   # Increased from 86
```

### Adding Compliance Frameworks

Update `skill.json` to include additional regulatory frameworks:

```json
"compliance_frameworks": [
  "Bank Secrecy Act (BSA)",
  "USA PATRIOT Act",
  "EU 6AMLD",
  "UK Money Laundering Regulations 2017"
]
```

## Best Practices

### For Users

1. **Have information ready**: Prepare customer details before starting
2. **Follow workflow prompts**: Provide requested information at each stage
3. **Document decisions**: When making EDD decisions, provide clear rationale
4. **Review audit trail**: Check the complete workflow history at the end

### For Workflow Execution

1. **Never skip states**: Follow the workflow sequentially
2. **Maintain context**: Reference previously collected information
3. **Document everything**: Create clear audit trail for regulatory review
4. **Apply risk-based approach**: More scrutiny for higher-risk customers
5. **Escalate appropriately**: MEDIUM/HIGH risk requires senior review

## Compliance Considerations

### Regulatory Obligations

- **Customer Due Diligence (CDD)**: Required for all customers
- **Enhanced Due Diligence (EDD)**: Required for high-risk customers, PEPs, high-risk jurisdictions
- **Ongoing Monitoring**: Continuous screening throughout customer relationship
- **Record Retention**: Maintain KYC records for minimum 5 years post-relationship termination
- **SAR Filing**: File Suspicious Activity Reports when indicators present (without informing customer)

### Red Flags

Watch for these suspicious activity indicators:

- Customer reluctant to provide information or provides inconsistent information
- Unusual transaction patterns vs. stated business purpose
- Complex structures without clear business rationale
- Involvement of high-risk jurisdictions without business need
- Customer requests unusual secrecy or acts with unreasonable urgency
- Lack of knowledge about own business operations

### Data Protection

- Maintain confidentiality of customer information
- Secure storage of sensitive documents
- Limited access to PII on need-to-know basis
- Comply with GDPR/CCPA and local data protection laws

## Limitations

This is a simulation skill for educational and training purposes. In production use:

- **Real screening databases required**: Integrate actual OFAC, World-Check, Dow Jones APIs
- **Human oversight essential**: All decisions should be reviewed by qualified compliance officers
- **Legal review necessary**: Ensure alignment with specific jurisdictional requirements
- **System integration needed**: Connect to core banking systems, document management, case management
- **Audit logging**: Implement comprehensive audit trails in production systems

## Support and Contributions

For issues, enhancements, or questions about this skill:

1. Review the workflow definition in `workflow.md`
2. Check sample scenarios in `SAMPLE_SCENARIOS.md`
3. Consult regulatory guidance from FATF, FinCEN, or your jurisdiction's regulator
4. Modify the workflow to fit your institution's specific requirements

## Version History

**v1.0.0** (2024)
- Initial release
- 23-state comprehensive workflow
- Support for individual and corporate customers
- Multi-dimensional risk scoring
- Parallel screening execution
- Enhanced due diligence procedures
- Audit trail and documentation

## License

This skill is provided for educational and training purposes. Ensure compliance with all applicable laws and regulations in your jurisdiction before adapting for production use.

---

**Disclaimer**: This skill is a simulation tool for training and educational purposes. It does not constitute legal or compliance advice. Consult qualified compliance professionals and legal counsel for actual customer onboarding procedures.
