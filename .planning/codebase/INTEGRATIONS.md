# Integrations

## Overview

This Claude Skill is designed for **educational/training purposes** and does not have real integrations. However, it simulates interactions with the following external systems that would be required in production.

## Simulated Screening Databases

### Sanctions Lists

| List | Provider | Purpose | Simulation Status |
|------|----------|---------|-------------------|
| **OFAC SDN** | US Treasury | Specially Designated Nationals | ⚠️ Simulated |
| **UN Sanctions** | United Nations | Security Council sanctions | ⚠️ Simulated |
| **EU Consolidated** | European Union | EU sanctions list | ⚠️ Simulated |
| **UK HMT** | HM Treasury | UK financial sanctions | ⚠️ Simulated |

### PEP Databases

| Database | Provider | Purpose | Simulation Status |
|----------|----------|---------|-------------------|
| **World-Check** | Refinitiv | PEP screening, adverse media | ⚠️ Simulated |
| **Dow Jones Risk & Compliance** | Dow Jones | PEP, sanctions, adverse media | ⚠️ Simulated |
| **LexisNexis WorldCompliance** | LexisNexis | Entity verification, PEP | ⚠️ Simulated |

### Corporate Registries

| Registry | Jurisdiction | Purpose | Simulation Status |
|----------|--------------|---------|-------------------|
| **UK Companies House** | United Kingdom | Company verification, filings | ⚠️ Simulated |
| **SEC EDGAR** | United States | Public company filings | ⚠️ Simulated |

## Required Production Integrations

For real-world deployment, the following integrations would be required:

### 1. Sanctions Screening API
```
Vendors:
- Refinitiv World-Check One
- Dow Jones Risk & Compliance
- LexisNexis WorldCompliance
- ComplyAdvantage

Required Capabilities:
- Real-time name matching
- Fuzzy matching algorithms
- Match score thresholds
- False positive management
```

### 2. PEP Database API
```
Required Fields:
- PEP status (current/former)
- Position held
- Jurisdiction
- Date range
- Family members
- Close associates
```

### 3. Adverse Media API
```
Required Capabilities:
- News aggregation
- Sentiment analysis
- Financial crime keywords
- 7-year lookback
- Source credibility scoring
```

### 4. Corporate Registry API
```
Required Data:
- Company registration details
- Director information
- Shareholder register
- PSC (Persons with Significant Control)
- Filing history
- Accounts status
```

### 5. Core Banking System
```
Integration Points:
- Account creation
- Customer profile
- Transaction monitoring rules
- Alert routing
- Case management
```

### 6. Document Management System
```
Required Capabilities:
- Document storage
- Retention management (5+ years)
- Audit trail
- Access control
- Version history
```

## Compliance Framework References

The skill references these regulatory frameworks (not integrated, used for guidance):

| Framework | Source | Usage |
|-----------|--------|-------|
| **BSA** | FinCEN | AML foundation, SAR requirements |
| **USA PATRIOT Act** | US Congress | CIP requirements (Section 326) |
| **FinCEN CDD Rule** | 31 CFR 1010.230 | Beneficial ownership |
| **FATF Recommendations** | FATF | Risk-based approach (R.10, R.22, R.24) |
| **Wolfsberg Principles** | Wolfsberg Group | AML standards |

## Data Flow (Simulated)

```
┌─────────────────┐     ┌──────────────────┐     ┌─────────────────┐
│  User Input     │────▶│  Claude Skill    │────▶│  Decision       │
│  (Customer Data)│     │  (KYC Workflow)  │     │  (Approve/Reject)│
└─────────────────┘     └──────────────────┘     └─────────────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │  Simulated Screening │
                    │  - Sanctions ⚠️      │
                    │  - PEP ⚠️            │
                    │  - Media ⚠️          │
                    │  - Enforcement ⚠️    │
                    └──────────────────────┘
```

## Production Architecture (Recommended)

```
┌─────────────────────────────────────────────────────────────────┐
│                        Production System                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────────────┐  │
│  │ Onboarding  │───▶│ Workflow    │───▶│ Core Banking        │  │
│  │ UI          │    │ Engine      │    │ System              │  │
│  └─────────────┘    └─────────────┘    └─────────────────────┘  │
│                            │                                     │
│                            ▼                                     │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                 Integration Layer                            ││
│  ├─────────────┬─────────────┬─────────────┬───────────────────┤│
│  │ World-Check │ Companies   │ Adverse     │ Case             ││
│  │ API         │ House API   │ Media API   │ Management       ││
│  └─────────────┴─────────────┴─────────────┴───────────────────┘│
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## Limitations

> ⚠️ **Critical**: This skill simulates all external integrations

**What's Missing for Production**:
1. Real API connections to screening databases
2. Authentication/authorization to data providers
3. Database persistence for customer records
4. Case management system integration
5. SAR filing system connection
6. Core banking system integration
7. Document management system
8. Audit logging infrastructure
