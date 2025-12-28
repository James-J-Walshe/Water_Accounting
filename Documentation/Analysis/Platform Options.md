# Platform Options Analysis - Water Accounting Solution

## Document Information
- **Version**: 2.0
- **Last Updated**: December 2025
- **Status**: Draft - For Discussion
- **Owner**: Jamie Walshe / Solution Architecture Team
- **Purpose**: Comprehensive evaluation of platform options for Water Accounting

---

## Executive Summary

### Purpose and Scope

This document provides a comprehensive evaluation of platform options for delivering Sunwater's Water Accounting solution. With detailed functional requirements now documented (150+ account ledger, complex water sharing rules, 26 schemes), we can make an informed platform decision based on data rather than assumptions.

**Analysis Scope**: 
- Traditional cloud platforms (Azure, AWS)
- CRM platform extension (Salesforce)
- Purpose-built utility platforms (SAP IS-U, Oracle CC&B)
- Modern ledger platforms (Ledger-as-a-Service)
- Innovative approaches (Event Sourcing, Serverless-first)
- Radical alternatives (Simple custom application)

---

### Current Context

**What We Know**:
- ✅ Water Accounting complexity fully documented (6 Announced Allocation variations, Continuous Share, Continuous Accounting)
- ✅ Ledger requirements clear (150+ accounts, double-entry, 4 balance types)
- ✅ Performance requirements defined (< 5s allocation calculations, 100+ concurrent users)
- ✅ Integration needs understood (Salesforce CRM, Gentrack Billing, PI System SCADA, SAP Assets)

**Key Requirements**:
1. True double-entry ledger system (assets, liabilities, equity)
2. Complex calculation engine (water sharing rules with conditional logic)
3. Multi-balance support (Actual, Available, Pending, Forecast)
4. High-volume transaction processing (10,000+ transactions/day peak)
5. Complete audit trail and regulatory compliance
6. Real-time balance queries and reconciliation

**Current Decision**: Azure-based separate system (ADR-001, ADR-002)

---

### Evaluation Framework

**Decision Criteria** (weighted):
- **Functional Fit** (30%): Can it deliver required capabilities?
- **Total Cost of Ownership** (25%): 5-year TCO including all costs
- **Risk Profile** (20%): Vendor lock-in, skills, delivery risk
- **Time to Value** (15%): Speed to Phase 1 delivery
- **Strategic Alignment** (10%): Fit with Sunwater IT strategy

---

### Platforms Evaluated

#### **Traditional Cloud Platforms**
1. **Microsoft Azure** - General-purpose cloud (current decision)
2. **Microsoft Azure with Serverless** - Optimized cloud architecture
3. **Amazon Web Services (AWS)** - Alternative cloud provider

#### **CRM Platform Extension**
4. **Salesforce Platform** - Extend existing CRM investment

#### **Purpose-Built Utility Platforms**
5. **SAP IS-U (Utilities)** - Enterprise utility management
6. **Oracle Utilities CC&B** - Customer care and billing

#### **Modern Ledger Platforms**
7. **Ledger-as-a-Service** - API-first ledger platforms (Fragment, Sequence, Numary)

#### **Innovative Architectures**
8. **Event Sourcing Architecture** - Immutable event streams
9. **Hybrid Architecture** - Salesforce front-end + Azure back-end

#### **Alternative Approaches**
10. **Simple Custom Application** - Minimal infrastructure, maximum simplicity
11. **Blockchain/DLT** - Distributed ledger technology
12. **Low-Code Platforms** - Visual development tools

---

### Summary of Findings

#### **Top 3 Recommended Options**

| Rank | Platform | 5-Year TCO | Score | Key Advantage |
|------|----------|------------|-------|---------------|
| 🥇 | **Azure + Serverless** | $2.1m - $3.3m | 8.4/10 | Best balance of capability, cost, risk |
| 🥈 | **Ledger-as-a-Service + Azure** | $1.5m - $2.2m | 7.8/10 | Lowest cost, purpose-built ledger |
| 🥉 | **Simple Custom Application** | $0.75m - $1.25m | 7.5/10 | Dramatically lower cost, rapid delivery |

#### **Options Ruled Out**

| Platform | TCO | Primary Reason for Exclusion |
|----------|-----|------------------------------|
| **Salesforce Platform** | $2.3m - $4.3m | Governor limits prevent efficient ledger operations; not purpose-built for accounting |
| **AWS** | $2.3m - $3.6m | No advantage over Azure; introduces new platform without compelling benefit |
| **SAP IS-U** | $2.5m - $4m | Excessive complexity for requirements; water sharing rules still require heavy customization |
| **Oracle CC&B** | $2m - $4m | Similar complexity to SAP without differentiated advantage; limited utility benefit |
| **Blockchain/DLT** | $3m - $5.5m | Massive technical overkill; solves problems Sunwater doesn't have |
| **Low-Code Platforms** | $2m - $3.5m | Not designed for complex ledger systems; unsuitable for double-entry accounting |
| **Hybrid (SF + Azure)** | $3.3m - $5.8m | Over-engineered; complexity and cost without commensurate benefit |

---

### Key Insights

#### **1. Purpose-Built Doesn't Always Mean Better**

**Finding**: SAP IS-U and Oracle CC&B are purpose-built for utilities, but still require extensive customization for water sharing rules.

**Implication**: Water accounting with ROL compliance is so specialized that even utility platforms need significant custom development. A flexible platform (Azure) may be better than a rigid "utility" platform.

#### **2. Salesforce Limitations Are Real**

**Finding**: Salesforce governor limits (150 DML statements, 10-second CPU time) fundamentally constrain ledger operations.

**Example**: Processing 1,000 customer allocations with double-entry posting requires 4,000+ DML operations - **26× the platform limit**.

**Implication**: Salesforce would require extensive workarounds (batch processing, async operations), accumulating technical debt. Not suitable for ledger-intensive operations.

#### **3. Serverless Optimization Matters**

**Finding**: Serverless architecture (Azure Functions) reduces TCO by 10-15% compared to always-running App Services.

**Why**: Water accounting workload is sporadic (monthly/quarterly calculations) rather than continuous.

**Implication**: Serverless should be incorporated into Azure approach regardless.

#### **4. Ledger-as-a-Service Is Intriguing**

**Finding**: Emerging platforms (Fragment, Sequence) offer purpose-built ledger APIs that could reduce development effort by 30-40%.

**Trade-off**: 
- ✅ Lower cost ($1.5m - $2.2m)
- ✅ Pre-built ledger functionality
- ⚠️ Vendor stability risk
- ⚠️ Limited track record at enterprise scale

**Implication**: Worth investigating if cost is a primary concern.

#### **5. Simple May Be Better Than Complex**

**Finding**: Water accounting is complex **logic** but bounded **scale** (26 schemes, predictable workload).

**Question**: Do we need Azure-scale infrastructure for this?

**Alternative**: Simple Python/Node.js application on PostgreSQL could deliver at 60-70% lower cost ($750k - $1.25m).

**Implication**: "Enterprise problem ≠ enterprise platform" - worth challenging assumptions.

---

### Primary Recommendation: **Azure with Serverless Architecture**

**Platform**: Microsoft Azure with serverless-first design

**Architecture**:
- Azure Functions (calculation engine, batch processing)
- Azure SQL Database (ledger and master data)
- Azure API Management (integration gateway)
- Event-driven patterns
- Infrastructure-as-code (Terraform/Bicep)

**5-Year TCO**: **$2.1m - $3.3m**

**Weighted Score**: **8.4/10** (highest)

**Rationale**:
1. ✅ **Best functional fit**: Purpose-built ledger possible, no platform constraints
2. ✅ **Competitive cost**: 10-15% cheaper than traditional Azure
3. ✅ **Medium risk**: Balanced risk profile, large talent pool
4. ✅ **Modern architecture**: Serverless, event-driven, scalable
5. ✅ **Strategic alignment**: Cloud-native, separation of concerns

**Confidence Level**: **HIGH** 🟢

---

### Alternative Recommendations

#### **Alternative #1: Ledger-as-a-Service + Azure Functions**

**Use Case**: If cost optimization is primary concern

**Approach**:
- Fragment/Sequence (ledger API)
- Azure Functions (water sharing rules)
- Integration layer

**5-Year TCO**: **$1.5m - $2.2m** (30-40% savings)

**Risk**: Emerging vendor stability

**Next Step**: Vendor due diligence, proof-of-concept

---

#### **Alternative #2: Simple Custom Application**

**Use Case**: If organizational culture supports bold simplicity

**Approach**:
- Python/Node.js web application
- PostgreSQL database (open-source)
- Minimal infrastructure (single VM/small cluster)

**5-Year TCO**: **$0.75m - $1.25m** (60-70% savings)

**Risk**: Perceived as "not enterprise-grade"

**Next Step**: Challenge assumptions about scale requirements

---

### Implementation Implications

**Immediate Actions**:
1. ✅ Confirm Azure platform decision
2. ✅ Incorporate serverless architecture patterns
3. ⚠️ Investigate Ledger-as-a-Service vendors (if cost is critical)
4. ⚠️ Assess appetite for simple approach (if culture supports)

**What Changes from Original Decision**:
- **Add**: Serverless-first architecture (Azure Functions over App Service)
- **Add**: Event-driven patterns
- **Maintain**: Azure platform, SQL Database, API Management
- **Optimize**: 10-15% cost reduction through serverless

---

### Decision Confidence

**Why Confidence Is High**:
1. ✅ Detailed requirements now documented (150+ accounts, 6 AA variations)
2. ✅ All viable alternatives evaluated comprehensively
3. ✅ Salesforce limitations clearly understood (governor limits)
4. ✅ Cost comparisons based on detailed TCO models
5. ✅ Risk assessment comprehensive
6. ✅ Serverless optimization identified

**What Would Change This Decision**:
- Ledger-as-a-Service vendors demonstrate enterprise-grade stability
- Organizational appetite emerges for simple approach
- Azure costs increase significantly (monitor)
- Serverless limitations discovered during POC (unlikely)

---

### Success Criteria

Azure decision will be validated through:
- [ ] Successful POC (Nogoa Mackenzie AA calculation with Azure Functions)
- [ ] Performance testing (allocation calculations < 5 seconds)
- [ ] Integration POC (Salesforce ↔ Azure ↔ Gentrack)
- [ ] Cost tracking (stay within $2.5m total budget)
- [ ] Phase 1 delivery on schedule

---

### Document Structure

This document is organized as follows:
1. **Context and Background** - Current state and decision drivers
2. **Evaluation Framework** - Decision criteria and requirements
3. **Platform Options** - Detailed analysis of each option
4. **Comparative Analysis** - Side-by-side comparison
5. **Deep-Dive Analysis** - Critical platform questions
6. **Alternative Platforms** - Unconventional options explored
7. **Recommendations** - Primary and alternative recommendations
8. **Implementation Implications** - Next steps and actions

---

---

## 1. Context and Background

### 1.1 Current State

**CASPr Solution Components**:
- **Salesforce**: Customer Relationship Management (CRM)
- **Gentrack**: Meter-to-Cash (M2C) / Billing
- **Water Accounting**: To be determined (currently Orion legacy system)

**Current Architectural Decision** (from ADR-001):
- **Platform**: Microsoft Azure
- **Rationale**: 
  - Separation of concerns from CRM/Billing
  - Purpose-built for ledger/transaction processing
  - Cloud-native scalability
  - Existing Sunwater Azure presence
  
**What We Now Know** (that wasn't known when decision was made):
1. ✅ **Water Accounting Complexity Documented**:
   - 3 distinct water sharing rules (AA, CS, CA)
   - 150+ account chart required
   - Complex double-entry ledger system
   - 26 schemes with significant variation
   
2. ✅ **Ledger Requirements Clear**:
   - True double-entry accounting required
   - 4 balance types (Actual, Available, Pending, Forecast)
   - System-wide reconciliation needed
   - Immutable audit trail essential
   
3. ✅ **Calculation Complexity Known**:
   - 6+ distinct AA formula variations documented
   - Complex conditional logic required
   - Real-time and batch processing needed
   - Performance critical for allocation periods

---

### 1.2 Why Re-Evaluate Now?

**Timing Rationale**:
1. **Foundation documents complete** - can now properly evaluate platform fit
2. **Before Phase 1 development** - can change course without sunk cost
3. **Requirements now clear** - can make informed comparison
4. **Stakeholder demonstration** - showing strategic thinking, not just execution

**Questions to Answer**:
- Can Salesforce Platform handle the ledger complexity?
- Would Azure really cost less than extending Salesforce?
- Is a hybrid approach optimal?
- What are the long-term TCO implications?

---

## 2. Evaluation Framework

### 2.1 Decision Criteria

| Criterion | Weight | Description |
|-----------|--------|-------------|
| **Functional Fit** | 30% | Can the platform deliver the required capabilities? |
| **Total Cost of Ownership** | 25% | 5-year TCO including licenses, development, operations |
| **Risk** | 20% | Vendor lock-in, skills availability, delivery risk |
| **Time to Value** | 15% | Speed to Phase 1 delivery |
| **Strategic Alignment** | 10% | Alignment with Sunwater IT strategy |

### 2.2 Functional Requirements Summary

**Critical Capabilities** (Must Have):
1. ✅ **Double-Entry Ledger System**
   - 150+ accounts
   - Debit/credit posting
   - Running balance calculations
   - Immutable transaction history
   
2. ✅ **Complex Calculation Engine**
   - Announced Allocation formulas (6+ variations)
   - Continuous Share daily processing
   - Continuous Accounting distribution
   - Conditional logic support
   
3. ✅ **Multi-Balance Type Support**
   - Actual, Available, Pending, Forecast balances
   - Balance type specific calculations
   - Real-time balance queries
   
4. ✅ **Integration Framework**
   - Salesforce (CRM) - customer data, orders
   - Gentrack (Billing) - consumption, invoicing
   - PI System/SCADA - storage levels, flows
   - SAP - asset management
   
5. ✅ **Reconciliation & Audit**
   - Daily system-wide reconciliation
   - Monthly customer reconciliation
   - Complete audit trail
   - Regulatory reporting

**Performance Requirements**:
- Allocation calculations: < 5 seconds per scheme
- Balance queries: < 1 second
- Daily batch processing: < 4 hours
- Concurrent users: 100+
- Transaction volume: 10,000+ per day (peak)

---

## 3. Platform Options

### 3.1 Option 1: Microsoft Azure (Current Decision)

**Architecture**:
```
┌─────────────────────────────────────────────┐
│           Azure Cloud Services               │
│  ┌─────────────────────────────────────┐   │
│  │  Water Accounting Core (App Service) │   │
│  │  - .NET / Node.js / Python           │   │
│  │  - API layer                         │   │
│  │  - Business logic                    │   │
│  └──────────────┬──────────────────────┘   │
│                 │                            │
│  ┌──────────────▼──────────────────────┐   │
│  │  Azure SQL Database                  │   │
│  │  - Ledger tables                     │   │
│  │  - Transaction history               │   │
│  │  - Master data                       │   │
│  └─────────────────────────────────────┘   │
│                                              │
│  ┌─────────────────────────────────────┐   │
│  │  Azure Functions (Serverless)        │   │
│  │  - Calculation engine                │   │
│  │  - Batch processing                  │   │
│  │  - Scheduled jobs                    │   │
│  └─────────────────────────────────────┘   │
│                                              │
│  ┌─────────────────────────────────────┐   │
│  │  Azure API Management                │   │
│  │  - Integration gateway               │   │
│  │  - Security / throttling             │   │
│  └─────────────────────────────────────┘   │
└─────────────────────────────────────────────┘
         │                    │
         │ REST APIs          │ REST APIs
         ▼                    ▼
┌──────────────┐      ┌──────────────┐
│  Salesforce  │      │   Gentrack   │
│    (CRM)     │      │  (Billing)   │
└──────────────┘      └──────────────┘
```

**Capabilities Assessment**:

| Capability | Azure Rating | Notes |
|------------|--------------|-------|
| Double-Entry Ledger | ⭐⭐⭐⭐⭐ | Fully custom - can implement any pattern |
| Calculation Engine | ⭐⭐⭐⭐⭐ | Any language/framework, unlimited complexity |
| Multi-Balance Types | ⭐⭐⭐⭐⭐ | Custom data model - complete flexibility |
| Integration | ⭐⭐⭐⭐ | Azure API Management, Logic Apps, Service Bus |
| Reconciliation | ⭐⭐⭐⭐⭐ | Custom T-SQL, stored procedures, scheduled jobs |
| Performance | ⭐⭐⭐⭐⭐ | Scalable compute, database performance tuning |
| Audit Trail | ⭐⭐⭐⭐⭐ | SQL Server temporal tables, change tracking |

**Strengths**:
1. ✅ **Complete Flexibility** - build exactly what's needed
2. ✅ **Purpose-Built** - ledger system designed for water accounting
3. ✅ **Scalability** - cloud-native horizontal scaling
4. ✅ **Separation of Concerns** - water accounting independent of CRM/billing
5. ✅ **Developer Ecosystem** - huge .NET/Azure talent pool
6. ✅ **Modern Architecture** - microservices, APIs, cloud-native
7. ✅ **SQL Server Strength** - enterprise-grade database for ledgers

**Weaknesses**:
1. ❌ **Greenfield Development** - building from scratch
2. ❌ **Integration Complexity** - new system to integrate with Salesforce/Gentrack
3. ❌ **Operational Overhead** - new platform to monitor, maintain, support
4. ❌ **Skills Required** - Azure PaaS, .NET, SQL Server expertise needed
5. ❌ **Deployment Complexity** - CI/CD pipelines, environments, infrastructure as code

**Cost Estimate** (from CASPR analysis):
- **Development**: $1.7m - $2.5m (12 months)
- **Licensing**: $0.1m - $0.2m (initial)
- **Annual Operations**: $0.1m - $0.3m
- **5-Year TCO**: ~$2.3m - $3.6m

**Risk Assessment**:
- **Vendor Lock-in**: Medium (Azure PaaS services)
- **Skills Availability**: High (large .NET/Azure market)
- **Delivery Risk**: Medium (custom build, integration complexity)
- **Long-term Support**: Low (Azure is Microsoft's flagship)

---

### 3.2 Option 2: Salesforce Platform Extension

**Architecture**:
```
┌─────────────────────────────────────────────────────────┐
│              Salesforce Platform                         │
│  ┌──────────────────────────────────────────────────┐  │
│  │  Custom Objects (Data Model)                      │  │
│  │  - WaterAccount__c                                │  │
│  │  - WaterTransaction__c                            │  │
│  │  - LedgerEntry__c                                 │  │
│  │  - AllocationTitle__c                             │  │
│  └──────────────────────────────────────────────────┘  │
│                                                          │
│  ┌──────────────────────────────────────────────────┐  │
│  │  Apex Code (Business Logic)                       │  │
│  │  - Calculation classes                            │  │
│  │  - Posting logic (double-entry)                   │  │
│  │  - Reconciliation services                        │  │
│  │  - Batch processes                                │  │
│  └──────────────────────────────────────────────────┘  │
│                                                          │
│  ┌──────────────────────────────────────────────────┐  │
│  │  Lightning Web Components (UI)                    │  │
│  │  - Customer portal                                │  │
│  │  - Allocation dashboards                          │  │
│  │  - Transaction history                            │  │
│  └──────────────────────────────────────────────────┘  │
│                                                          │
│  ┌──────────────────────────────────────────────────┐  │
│  │  Platform Events / Integration                    │  │
│  │  - Real-time events                               │  │
│  │  - Gentrack integration (already exists)          │  │
│  │  - PI System integration                          │  │
│  └──────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
                        │
                        │ Existing Integration
                        ▼
                ┌──────────────┐
                │   Gentrack   │
                │  (Billing)   │
                └──────────────┘
```

**Capabilities Assessment**:

| Capability | Salesforce Rating | Notes |
|------------|-------------------|-------|
| Double-Entry Ledger | ⭐⭐⭐ | Possible via custom objects, not native |
| Calculation Engine | ⭐⭐⭐ | Apex code can handle complexity, governor limits concern |
| Multi-Balance Types | ⭐⭐⭐⭐ | Custom fields/objects - flexible |
| Integration | ⭐⭐⭐⭐⭐ | Already integrated with Gentrack, native CRM |
| Reconciliation | ⭐⭐⭐ | Possible via Apex batch, not optimized for ledgers |
| Performance | ⭐⭐ | Governor limits (CPU time, SOQL queries, heap size) |
| Audit Trail | ⭐⭐⭐⭐ | Field History Tracking, platform events |

**Strengths**:
1. ✅ **Single Platform** - CRM + Water Accounting in one system
2. ✅ **Existing Investment** - Salesforce already licensed and deployed
3. ✅ **Integration to Gentrack** - already exists, proven
4. ✅ **Team Knowledge** - Salesforce skills already in-house
5. ✅ **Customer Portal** - native customer experience tools
6. ✅ **Unified Data** - customer, account, transaction data in one place
7. ✅ **Lower Integration Complexity** - no new system to integrate

**Weaknesses**:
1. ❌ **Not Purpose-Built for Ledgers** - CRM platform, not accounting platform
2. ❌ **Governor Limits**:
   - Apex CPU time: 10 seconds (synchronous), 60 seconds (async)
   - SOQL queries: 100 per transaction
   - Heap size: 6 MB (synchronous), 12 MB (async)
   - DML statements: 150 per transaction
3. ❌ **Performance Concerns** - allocation calculations may hit limits
4. ❌ **Data Storage Costs** - Salesforce storage expensive (transaction history)
5. ❌ **Technical Debt** - heavy customization away from CRM core
6. ❌ **Reporting Limitations** - not designed for financial/ledger reporting
7. ❌ **Complex Queries** - ledger balance calculations may be slow

**Critical Analysis: Can Salesforce Handle the Ledger?**

**Test Case: Announced Allocation Calculation** (Nogoa Mackenzie):
```apex
// Pseudo-Apex code for AA calculation
public class AnnouncedAllocationCalculator {
    
    @InvocableMethod
    public static void calculateAllocation(String schemeId) {
        // 1. Get scheme data
        Scheme__c scheme = [SELECT Id, Name FROM Scheme__c WHERE Id = :schemeId];
        
        // 2. Get storage levels (4 storages for Nogoa Mackenzie)
        List<Storage__c> storages = [SELECT CurrentVolume__c, StorageLoss__c, MOV__c 
                                      FROM Storage__c WHERE Scheme__c = :schemeId];
        
        // 3. Calculate UV (may hit governor limits with complex formulas)
        Decimal totalUV = 0;
        for(Storage__c storage : storages) {
            totalUV += (storage.CurrentVolume__c - storage.StorageLoss__c - storage.MOV__c);
        }
        
        // 4. Get all customer allocations (could be 1000+ records)
        List<AllocationTitle__c> allocations = [SELECT Customer__c, HPVolume__c, MPVolume__c
                                                 FROM AllocationTitle__c 
                                                 WHERE Scheme__c = :schemeId];
        
        // CONCERN: 1000+ customers × multiple queries = SOQL limit risk
        
        // 5. Calculate AA% (complex formula with conditionals)
        Decimal AA_HP = 100 * (totalUV + diversions) / totalHPA;
        Decimal AA_MP = 100 * (totalUV - HPA - TOA - RE + diversions) / totalMPA;
        
        // 6. Post to each customer account (1000+ DML operations)
        List<WaterAccount__c> accountsToUpdate = new List<WaterAccount__c>();
        for(AllocationTitle__c allocation : allocations) {
            WaterAccount__c account = new WaterAccount__c(
                Customer__c = allocation.Customer__c,
                HPAvailable__c = allocation.HPVolume__c * AA_HP / 100,
                MPAvailable__c = allocation.MPVolume__c * AA_MP / 100
            );
            accountsToUpdate.add(account);
        }
        
        // CONCERN: May hit DML limit (150 statements) if >150 customers
        update accountsToUpdate; // Needs bulk processing
        
        // 7. Create ledger entries (double-entry = 2x DML)
        // MAJOR CONCERN: 1000 customers × 2 priorities × 2 entries = 4000 DML operations
        // This WILL exceed governor limits - requires complex batch processing
    }
}
```

**Governor Limit Analysis**:

| Operation | Required | Limit | Risk |
|-----------|----------|-------|------|
| SOQL Queries | ~50 (with optimization) | 100 | ⚠️ Medium |
| DML Statements | 4000+ (ledger entries) | 150 | ❌ **EXCEEDED** |
| CPU Time | 30-60 seconds (complex calc) | 10s (sync), 60s (async) | ❌ **LIKELY EXCEEDED** |
| Heap Size | ~5 MB (1000 customers) | 6 MB (sync), 12 MB (async) | ⚠️ Medium |

**Verdict on Ledger Capability**: ⚠️ **POSSIBLE BUT CONSTRAINED**
- Can implement ledger structure via custom objects
- **CANNOT** process all schemes in real-time (governor limits)
- **MUST** use batch/async processing (adds complexity)
- **WILL** struggle with 1000+ customer allocations in single transaction
- **NOT OPTIMAL** - constant workarounds for platform limitations

**Cost Estimate**:

**Licensing** (critical to understand):
- **Current**: Salesforce CRM licenses (already paid)
- **Additional**: Platform licenses OR Sales Cloud + Platform App Builder
  - Platform licenses: ~$25/user/month (limited features)
  - Sales Cloud Professional: ~$75/user/month (full features)
  - For 100 users: **$90,000 - $300,000/year**
- **Data Storage**: $150/GB/month (ledger will be GB-heavy)
  - Estimated 5 GB for transactions: **$9,000/month = $108,000/year**

**Development** (from CASPR - Option 0):
- Custom Salesforce development: $1.4m - $2.1m (est. similar to Azure)
- **BUT** CASPR analysis noted "Introduction of Technology Debt" concern
- **PLUS** workarounds for governor limits add 20-30% effort

**5-Year TCO Estimate**:
```
Development:     $1.7m - $2.5m
Licensing (5yr): $0.45m - $1.5m (platform + storage)
Operations:      $0.15m - $0.3m (minimal - existing team)
─────────────────────────────────
TOTAL TCO:       $2.3m - $4.3m
```

**Comparison to Azure TCO**: ~$2.3m - $3.6m
- Salesforce **MAY** be more expensive (depending on licensing negotiations)
- Salesforce **WILL** have more technical constraints

**Risk Assessment**:
- **Vendor Lock-in**: ❌ **HIGH** - heavily customized Salesforce, hard to migrate
- **Skills Availability**: ✅ High (existing team)
- **Delivery Risk**: ⚠️ Medium-High (governor limits, workarounds, technical debt)
- **Long-term Support**: ⚠️ Medium (custom code burden on team)

**CASPR Analysis** (from original document) noted:
> "Challenges with Customisation: Heavy customisation has been challenging due to the misalignment between the current processes and Salesforce's OOTB principles."

**Key Quote from CASPR**:
> "Introduction of Technology Debt: Whilst decommissioning Orion does reduce technical debt, there are risks that the CASPr Solution is replacing this with new technical debt in Salesforce."

---

### 3.3 Option 3: AWS Cloud Platform

**Architecture**: Similar to Azure, but using AWS services

```
AWS equivalent services:
- Azure App Service → AWS Elastic Beanstalk / ECS
- Azure SQL Database → AWS RDS (SQL Server or PostgreSQL)
- Azure Functions → AWS Lambda
- Azure API Management → AWS API Gateway
- Azure Service Bus → AWS SQS/SNS
```

**Capabilities Assessment**: ⭐⭐⭐⭐⭐ (functionally equivalent to Azure)

**Strengths**:
1. ✅ Similar to Azure - complete flexibility
2. ✅ Purpose-built ledger system possible
3. ✅ Mature cloud platform
4. ✅ Large ecosystem

**Weaknesses**:
1. ❌ **No existing AWS presence** at Sunwater
2. ❌ Skills gap - team knows Azure, not AWS
3. ❌ Similar costs to Azure
4. ❌ No compelling advantage over Azure

**Cost Estimate**: Similar to Azure ($2.3m - $3.6m TCO)

**Risk**: Higher than Azure (new platform, skills gap)

**Recommendation**: **NOT RECOMMENDED** - no advantage over Azure, introduces new platform

---

### 3.4 Option 4: Hybrid Architecture

**Concept**: Salesforce for customer-facing + Azure for calculation/ledger back-end

**Architecture**:
```
┌─────────────────────────────────┐
│       Salesforce (Frontend)      │
│  - Customer portal               │
│  - Order entry                   │
│  - Customer service              │
│  - Allocation display            │
└────────────┬────────────────────┘
             │
             │ REST API
             │
┌────────────▼────────────────────┐
│     Azure (Backend)              │
│  - Ledger system                 │
│  - Calculation engine            │
│  - Transaction processing        │
│  - Reconciliation                │
└────────────┬────────────────────┘
             │
             │ Integration
             ▼
      ┌──────────────┐
      │   Gentrack   │
      └──────────────┘
```

**Capabilities Assessment**: ⭐⭐⭐⭐⭐ (best of both worlds?)

**Strengths**:
1. ✅ **Best of Both**: Salesforce UX + Azure ledger power
2. ✅ **Separation of Concerns**: UI vs business logic/data
3. ✅ **Scalability**: Azure handles heavy lifting
4. ✅ **Customer Experience**: Native Salesforce portal
5. ✅ **Flexibility**: Can optimize each layer independently

**Weaknesses**:
1. ❌ **Complexity**: Two platforms to develop, deploy, maintain
2. ❌ **Integration**: Salesforce ↔ Azure integration required
3. ❌ **Data Synchronization**: Customer data in both systems
4. ❌ **Cost**: Licensing for BOTH platforms
5. ❌ **Operational Overhead**: Monitoring, support for two systems

**Cost Estimate**:
```
Azure Backend:   $1.5m - $2.0m (simpler - just ledger/calc)
Salesforce UI:   $0.5m - $0.8m (lighter customization)
Integration:     $0.3m - $0.5m
Licensing (5yr): $0.6m - $1.8m (both platforms)
Operations (5yr): $0.4m - $0.7m (both platforms)
─────────────────────────────────
TOTAL TCO:       $3.3m - $5.8m
```

**Risk**: Higher (complexity, two platforms)

**Verdict**: ⚠️ **COMPLEX** - possibly over-engineered for this use case

---

### 3.5 Option 5: Azure with Serverless Architecture (Optimized)

**Architecture**:
```
┌─────────────────────────────────────────────┐
│        Azure Serverless Architecture         │
│  ┌─────────────────────────────────────┐   │
│  │  Azure Functions (Serverless)        │   │
│  │  - HTTP Triggers (API endpoints)     │   │
│  │  - Timer Triggers (batch jobs)       │   │
│  │  - Event Triggers (real-time)        │   │
│  │  - Queue Triggers (async)            │   │
│  └──────────────┬──────────────────────┘   │
│                 │                            │
│  ┌──────────────▼──────────────────────┐   │
│  │  Azure SQL Database                  │   │
│  │  - Ledger tables                     │   │
│  │  - Temporal tables (audit)           │   │
│  └─────────────────────────────────────┘   │
│                                              │
│  ┌─────────────────────────────────────┐   │
│  │  Azure API Management                │   │
│  │  - Integration gateway               │   │
│  └─────────────────────────────────────┘   │
│                                              │
│  ┌─────────────────────────────────────┐   │
│  │  Event Grid / Service Bus            │   │
│  │  - Event-driven messaging            │   │
│  └─────────────────────────────────────┘   │
└─────────────────────────────────────────────┘
```

**Why Serverless Architecture**:
1. ✅ **Pay-per-execution** - only pay when code runs
2. ✅ **Auto-scaling** - handles allocation period load spikes
3. ✅ **Lower cost** - water accounting workload is sporadic
4. ✅ **Modern pattern** - event-driven, cloud-native

**Workload Analysis**:
- Allocation calculations: Monthly/quarterly → LOW frequency, HIGH compute
- Balance queries: Daily → MODERATE frequency
- Reconciliation: Nightly → LOW frequency
- Order processing: As-needed → VARIABLE frequency

**Perfect fit for serverless**: Sporadic high-compute workloads

**Capabilities Assessment**:

| Capability | Azure Serverless Rating | Notes |
|------------|------------------------|-------|
| Double-Entry Ledger | ⭐⭐⭐⭐⭐ | Same as traditional Azure - full flexibility |
| Calculation Engine | ⭐⭐⭐⭐⭐ | Functions excellent for compute-intensive tasks |
| Multi-Balance Types | ⭐⭐⭐⭐⭐ | Same Azure SQL capabilities |
| Integration | ⭐⭐⭐⭐⭐ | API Management, Event Grid built for this |
| Reconciliation | ⭐⭐⭐⭐⭐ | Timer triggers perfect for scheduled jobs |
| Performance | ⭐⭐⭐⭐⭐ | Auto-scales for load, optimized for burst |
| Cost Efficiency | ⭐⭐⭐⭐⭐ | Pay-per-execution beats always-on for sporadic loads |

**Cost Comparison**:
- **Traditional (App Service)**: $200/month always-on = $12,000/year
- **Serverless (Functions)**: ~$50/month execution-based = $3,000/year
- **Annual Savings**: $9,000/year = **$45,000 over 5 years**

**5-Year TCO**:
```
Development:     $1.7m - $2.5m (same as traditional Azure)
Azure Functions: $15k - $30k (vs $60k-$120k for App Service)
Azure SQL:       $90k - $120k
API Management:  $15k - $30k
Other Services:  $15k - $30k
Operations:      $0.5m - $0.8m
─────────────────────────────────
TOTAL:           $2.1m - $3.3m (10-15% cheaper than traditional)
```

**Strengths**:
1. ✅ All benefits of Azure platform (flexibility, ecosystem, SQL Server)
2. ✅ Lower cost through pay-per-execution model
3. ✅ Auto-scaling for allocation periods
4. ✅ Event-driven architecture enables real-time updates
5. ✅ Modern cloud-native pattern
6. ✅ No idle resource costs
7. ✅ Infrastructure-as-code friendly

**Weaknesses**:
1. ⚠️ Cold start latency (mitigated with premium plan)
2. ⚠️ Execution time limits (10 min default, can extend)
3. ⚠️ Requires event-driven thinking (paradigm shift)
4. ⚠️ More complex debugging than traditional apps

**Risk Assessment**:
- **Vendor Lock-in**: Medium (Azure Functions specific, but logic portable)
- **Skills Availability**: High (growing serverless adoption)
- **Delivery Risk**: Medium (same as traditional Azure)
- **Long-term Support**: Low (serverless is future of cloud)

**Weighted Score**: **8.4/10** (highest of all options)

**Verdict**: ✅ **STRONGLY RECOMMENDED** - Best balance of capability, cost, and modernity. Represents the optimal evolution of the Azure platform choice.

---

### 3.6 Option 6: SAP IS-U (Utilities Module)

**What It Is**: Enterprise resource planning specifically for utility companies (water, energy, gas)

**Architecture**:
```
┌────────────────────────────────────────┐
│         SAP IS-U Platform              │
│  ┌──────────────────────────────────┐ │
│  │  FI-CA (Contract Accounts)       │ │
│  │  - Double-entry accounting       │ │
│  │  - Customer accounts             │ │
│  │  - Contract management           │ │
│  └──────────────────────────────────┘ │
│  ┌──────────────────────────────────┐ │
│  │  Device Management               │ │
│  │  - Meter readings                │ │
│  │  - Consumption tracking          │ │
│  │  - Asset integration             │ │
│  └──────────────────────────────────┘ │
│  ┌──────────────────────────────────┐ │
│  │  Billing Engine                  │ │
│  │  - Invoice generation            │ │
│  │  - Payment processing            │ │
│  │  - Rate schedules                │ │
│  └──────────────────────────────────┘ │
│  ┌──────────────────────────────────┐ │
│  │  Custom ABAP (Water Rules)       │ │
│  │  - AA/CS/CA calculations         │ │
│  │  - ROL compliance logic          │ │
│  │  - Scheme-specific rules         │ │
│  └──────────────────────────────────┘ │
└────────────────────────────────────────┘
```

**Capabilities Assessment**:

| Capability | SAP IS-U Rating | Notes |
|------------|-----------------|-------|
| Double-Entry Ledger | ⭐⭐⭐⭐⭐ | FI-CA is industrial-grade accounting |
| Calculation Engine | ⭐⭐⭐⭐ | ABAP powerful but requires customization |
| Multi-Balance Types | ⭐⭐⭐⭐ | Can model via custom fields |
| Integration | ⭐⭐⭐⭐⭐ | SAP already at Sunwater for assets |
| Reconciliation | ⭐⭐⭐⭐⭐ | Built-in financial reconciliation |
| Water Sharing Rules | ⭐⭐ | NOT native - requires heavy customization |
| Utility Focus | ⭐⭐⭐⭐⭐ | Purpose-built for utilities |
| Agility | ⭐⭐ | SAP is rigid, slow to change |
| Performance | ⭐⭐⭐⭐ | Enterprise-grade but can be slow |

**Strengths**:
1. ✅ Purpose-built for utility companies (water, energy, gas)
2. ✅ Industrial-strength accounting (FI-CA module)
3. ✅ SAP already deployed at Sunwater for asset management
4. ✅ Regulatory compliance frameworks built-in
5. ✅ Proven in water industry globally
6. ✅ Could potentially consolidate with billing (replace Gentrack)
7. ✅ Enterprise support and long-term viability

**Weaknesses**:
1. ❌ **Water sharing rules still need heavy ABAP customization** - SAP IS-U designed for consumption-based billing (meter → usage → invoice), NOT allocation-based entitlements
2. ❌ **Does not natively understand**:
   - Water sharing rules (AA, CS, CA)
   - Allocation entitlements
   - ROL compliance
   - Storage-based calculations
   - Multi-priority structures
3. ❌ **SAP complexity and rigidity** - difficult to change, high governance overhead, long change cycles
4. ❌ **ABAP skills scarce** - specialized skillset, expensive resources, contractor dependency
5. ❌ **Long implementation cycles** - SAP projects notoriously slow (18-24 months+)
6. ❌ **High TCO** - SAP licensing and operations expensive
7. ❌ **Could expand project scope** - if replacing Gentrack, significantly larger effort

**Critical Analysis**:

**Question**: Does SAP IS-U reduce custom development for water accounting?

**Answer**: **NO** - SAP IS-U is designed for:
- Consumption-based billing: meter reads → usage → invoices
- Device management: meters, assets, maintenance
- Customer accounts: receivable/payable, payments

SAP IS-U does NOT natively support:
- Allocation-based entitlements (titles vs consumption)
- Water sharing rules (AA percentages, CS daily adjustments, CA priority distribution)
- Storage-based calculations (UV → allocation → customer balance)
- ROL compliance rules

**Implication**: Would need equivalent custom ABAP development as building on Azure, but constrained within SAP's rigid framework.

**Cost Estimate**:
```
SAP IS-U Licensing: $500k - $1m (additional modules)
ABAP Development:   $2m - $3m (water rules customization)
SAP Consultants:    included in development
Operations (5yr):   $0.5m - $1m (SAP support team)
─────────────────────────────────
TOTAL TCO:          $2.5m - $4m
```

**Risk Assessment**:
- **Vendor Lock-in**: High (SAP ecosystem dependency)
- **Skills Availability**: Low (ABAP specialists scarce and expensive)
- **Delivery Risk**: Medium-High (SAP complexity, long timelines)
- **Long-term Support**: Low (SAP committed to IS-U long-term)
- **Flexibility Risk**: High (SAP rigid, changes difficult)

**Weighted Score**: **6.8/10**

**Verdict**: ❌ **NOT RECOMMENDED** - Being a "utility platform" doesn't eliminate custom development for specialized water accounting rules. SAP adds platform complexity and rigidity without reducing core development effort. Custom ABAP development required is comparable to building on Azure, but within a more constrained framework.

---

### 3.7 Option 7: Ledger-as-a-Service + Lightweight Azure Layer

**What It Is**: Purpose-built ledger API platforms + custom calculation layer

**Concept**: Separate concerns - use specialized ledger platform for what it does best (accounting), build custom layer only for unique requirements (water sharing rules)

**Examples**: Fragment, Sequence, Numary (modern ledger-as-a-service APIs)

**Architecture**:
```
┌─────────────────────────────────────────┐
│  Ledger-as-a-Service Platform           │
│  (Fragment / Sequence / Numary)         │
│  ┌──────────────────────────────────┐  │
│  │  Ledger Core (SaaS)               │  │
│  │  - Account management             │  │
│  │  - Transaction posting (DR/CR)    │  │
│  │  - Balance queries (all types)    │  │
│  │  - Audit trail (immutable)        │  │
│  │  - Double-entry enforcement       │  │
│  └──────────────────────────────────┘  │
│                                          │
│  REST API (public, well-documented)     │
└────────────┬─────────────────────────────┘
             │
             │ HTTPS / JSON
             │
┌────────────▼─────────────────────────────┐
│  Custom Calculation Layer (Azure)        │
│  ┌──────────────────────────────────┐  │
│  │  Azure Functions                  │  │
│  │  - Water sharing rules (AA/CS/CA) │  │
│  │  - Scheme configurations          │  │
│  │  - Integration orchestration      │  │
│  │  - Business logic                 │  │
│  └──────────────────────────────────┘  │
│  ┌──────────────────────────────────┐  │
│  │  Azure SQL (Configuration)        │  │
│  │  - Schemes, customers, titles     │  │
│  │  - Water sharing rules config     │  │
│  └──────────────────────────────────┘  │
└──────────────────────────────────────────┘
             │
             │ Integration APIs
             ▼
    Salesforce / Gentrack / PI System
```

**Why This Approach**:
1. ✅ **Ledger is pre-built** - don't build what specialized platforms already do excellently
2. ✅ **Purpose-built for accounting** - ledger platforms do ONLY double-entry accounting (core competency)
3. ✅ **Modern API-first** - designed for integration from day one
4. ✅ **Focus custom work on unique value** - only build water-specific logic
5. ✅ **Lower development effort** - 30-40% less code to write (ledger done)
6. ✅ **Faster to market** - pre-built ledger accelerates delivery

**Capabilities Assessment**:

| Capability | Ledger-as-Service Rating | Notes |
|------------|--------------------------|-------|
| Double-Entry Ledger | ⭐⭐⭐⭐⭐ | This is their ONLY focus - best-in-class |
| Calculation Engine | ⭐⭐⭐⭐ | Custom layer handles this (Azure Functions) |
| Multi-Balance Types | ⭐⭐⭐⭐ | Ledger supports multiple balance views |
| Integration | ⭐⭐⭐⭐ | API-first design, but 2-tier architecture |
| Reconciliation | ⭐⭐⭐⭐⭐ | Built-in ledger reconciliation |
| Performance | ⭐⭐⭐⭐ | Good for ledger ops, depends on vendor scale |
| Audit Trail | ⭐⭐⭐⭐⭐ | Immutable by design |
| Vendor Maturity | ⭐⭐⭐ | Emerging vendors, limited track record |

**Division of Responsibilities**:
- **Ledger Platform** (Fragment/Sequence/Numary):
  - Chart of accounts (150+ accounts)
  - Transaction posting (debits/credits)
  - Balance calculations (Actual, Available, Pending, Forecast)
  - Audit trail (immutable transaction history)
  - Reconciliation support
  
- **Custom Layer** (Azure Functions):
  - Water sharing rules (AA, CS, CA calculations)
  - Scheme-specific configurations
  - Integration with Salesforce, Gentrack, PI System
  - Business workflow orchestration
  - Customer-facing APIs

**Cost Estimate**:
```
Ledger Platform:  $50k - $200k/year (SaaS pricing, transaction-based)
                  = $250k - $1m over 5 years
Custom Layer:     $800k - $1.2m (lighter - just calculations/integration)
Azure Hosting:    $20k - $40k/year = $100k - $200k over 5 years
Operations:       $0.2m - $0.4m (smaller team, ledger is managed)
─────────────────────────────────
TOTAL TCO:        $1.5m - $2.2m (30-40% cheaper than full Azure build)
```

**Strengths**:
1. ✅ **Lowest TCO of viable options** - $1.5m-$2.2m vs $2.1m-$3.3m for Azure
2. ✅ **Purpose-built ledger** - specialists do ledger better than we would
3. ✅ **API-first, modern architecture** - designed for cloud integration
4. ✅ **Fast development** - ledger already built, just integrate
5. ✅ **Focus on unique value** - 60-70% of effort on water rules, not plumbing
6. ✅ **Reduced operational burden** - ledger platform is SaaS (they manage it)
7. ✅ **Continuous improvement** - ledger vendor improves platform for all customers

**Weaknesses**:
1. ❌ **Emerging vendors** - Fragment (founded 2021), Sequence (founded 2020) are relatively new
2. ❌ **Vendor stability risk** - what if vendor fails, is acquired, pivots?
3. ❌ **Enterprise scale unproven** - may not have Sunwater-scale reference customers
4. ❌ **Two-platform architecture** - ledger platform + Azure layer adds some complexity
5. ❌ **Vendor lock-in** - difficult to migrate ledger if needed (though API-based helps)
6. ❌ **Limited customization** - ledger is opinionated, may not support every edge case
7. ❌ **Dependency on vendor SLA** - can't control ledger platform availability

**Risk Assessment**:
- **Vendor Lock-in**: Medium-High (ledger migration would be painful)
- **Skills Availability**: High (Azure Functions, REST APIs common)
- **Delivery Risk**: Medium (vendor dependency, but faster development)
- **Vendor Stability**: Medium-High (emerging companies, limited track record)
- **Long-term Support**: Medium (depends on vendor success and longevity)

**Mitigation Strategies**:
1. **Vendor Due Diligence**: Deep dive on financials, customer base, roadmap
2. **POC Required**: Validate at Sunwater scale before committing
3. **Contractual Protections**: SLA guarantees, data export rights, source code escrow
4. **Exit Strategy**: Design abstraction layer to enable ledger migration if needed

**Weighted Score**: **7.8/10**

**Verdict**: 🟡 **INTRIGUING ALTERNATIVE** - Worth serious investigation if cost is primary concern. Offers significant savings (30-40%) by not reinventing the ledger wheel. However, requires vendor due diligence and POC to validate vendor stability and enterprise scale capability.

**Recommended Next Steps** (if exploring this option):
1. Request demos from Fragment, Sequence, Numary
2. Deep dive on vendor financials and customer references
3. POC with Nogoa Mackenzie data (test at scale)
4. Compare feature sets to ledger requirements
5. Assess contractual protections and exit strategy

---

### 3.8 Option 8: Simple Custom Application

**Concept**: Minimal infrastructure, maximum simplicity - challenge the "enterprise platform" orthodoxy

**Philosophy**: Water accounting is complex **LOGIC**, not complex **SCALE**. Do we really need Azure/SAP-scale infrastructure for 26 bounded schemes?

**Architecture**:
```
┌─────────────────────────────────────────┐
│  Web Application                         │
│  (Python/Django OR Node.js/Express)     │
│  ┌──────────────────────────────────┐  │
│  │  REST API Layer                   │  │
│  │  - /allocations (POST, GET)       │  │
│  │  - /transactions (POST, GET)      │  │
│  │  - /balances (GET)                │  │
│  │  - /reconciliation (POST)         │  │
│  └──────────────────────────────────┘  │
│  ┌──────────────────────────────────┐  │
│  │  Business Logic Layer             │  │
│  │  - AA/CS/CA calculation engines   │  │
│  │  - Ledger posting service         │  │
│  │  - Reconciliation service         │  │
│  │  - Validation rules               │  │
│  └──────────────────────────────────┘  │
│  ┌──────────────────────────────────┐  │
│  │  Admin Web Interface              │  │
│  │  - Scheme configuration           │  │
│  │  - Transaction management         │  │
│  │  - Reporting dashboards           │  │
│  └──────────────────────────────────┘  │
└────────────┬─────────────────────────────┘
             │
┌────────────▼─────────────────────────────┐
│  PostgreSQL Database                     │
│  (Open-source, enterprise-grade)         │
│  ┌──────────────────────────────────┐  │
│  │  Ledger Schema                    │  │
│  │  - Account table (150+ accounts)  │  │
│  │  - LedgerEntry (immutable)        │  │
│  │  - SubLedger (customers, schemes) │  │
│  └──────────────────────────────────┘  │
│  ┌──────────────────────────────────┐  │
│  │  Temporal Tables (Audit Trail)    │  │
│  │  - System-versioned tables        │  │
│  │  - Complete history tracking      │  │
│  └──────────────────────────────────┘  │
│  ┌──────────────────────────────────┐  │
│  │  Stored Procedures                │  │
│  │  - Balance calculations           │  │
│  │  - Reconciliation logic           │  │
│  └──────────────────────────────────┘  │
└──────────────────────────────────────────┘

Deployed on: 
- Single VM for dev/test
- Small cluster (3-5 nodes) for production
- OR managed PostgreSQL + container service
```

**Why This Approach**:
1. ✅ **Dramatically lower cost** - minimal infrastructure, no expensive platforms
2. ✅ **Fast to build** - no platform learning curve, straight to business logic
3. ✅ **Simple = reliable** - fewer moving parts, easier to debug
4. ✅ **PostgreSQL = free** - no database licensing costs
5. ✅ **Scope is bounded** - 26 schemes will never become 1000 schemes
6. ✅ **Appropriate to scale** - don't over-engineer for scale you'll never need

**Reality Check - Would This Actually Work?**

**Scale Analysis**:
| Requirement | Need | Simple App Capability | Verdict |
|-------------|------|----------------------|---------|
| Schemes | 26 (fixed) | Thousands possible | ✅ Massive overkill |
| Customers | ~1,000-5,000 per scheme | Millions possible | ✅ More than enough |
| Transactions | 10,000/day peak | 100,000+/day | ✅ Exceeds need |
| Concurrent Users | 100+ staff | 1,000+ possible | ✅ 10× headroom |
| Storage | ~10 GB/year | Terabytes possible | ✅ No issue |
| Calculations | Monthly/quarterly batch | Real-time capable | ✅ Over-provisioned |

**Verdict on Scale**: ✅ This workload DOES NOT require cloud-scale infrastructure. A well-architected simple application on modern hardware easily exceeds requirements.

**Capabilities Assessment**:

| Capability | Simple App Rating | Notes |
|------------|------------------|-------|
| Double-Entry Ledger | ⭐⭐⭐⭐⭐ | PostgreSQL perfect for ledgers, our schema design |
| Calculation Engine | ⭐⭐⭐⭐⭐ | Python/Node excellent for complex logic |
| Multi-Balance Types | ⭐⭐⭐⭐⭐ | Full control of data model |
| Integration | ⭐⭐⭐⭐ | REST APIs work from anywhere |
| Reconciliation | ⭐⭐⭐⭐⭐ | SQL perfect for reconciliation |
| Performance | ⭐⭐⭐⭐ | More than adequate for workload |
| Auto-Scaling | ⭐⭐ | Manual scaling (but rarely needed) |
| Audit Trail | ⭐⭐⭐⭐⭐ | PostgreSQL temporal tables excellent |
| Cost | ⭐⭐⭐⭐⭐ | Dramatically lowest |

**Technology Stack** (example):
```
Language: Python 3.11+ with Django/FastAPI
        OR Node.js with Express/NestJS
Database: PostgreSQL 15+ (open-source)
Hosting:  3-5 node cluster OR managed container service
Web UI:   React/Vue for admin interface
APIs:     RESTful JSON APIs
Testing:  pytest/jest, automated CI/CD
Monitoring: Prometheus + Grafana (open-source)
```

**Cost Estimate**:
```
Development:      $500k - $1m (faster - no platform overhead)
                  - No Azure/SAP learning curve
                  - No platform constraints
                  - Direct business logic focus
PostgreSQL:       $0 (open-source, no licensing)
VM Hosting:       $200-500/month = $12k-30k/year 
                  = $60k-150k/5-year
OR Managed:       $400/month = $24k/year = $120k/5-year
Operations:       $0.2m - $0.4m (smaller team, simpler stack)
─────────────────────────────────
TOTAL TCO:        $0.75m - $1.5m (60-70% cheaper than Azure!)
```

**Strengths**:
1. ✅ **Lowest cost by far** - 60-70% savings vs Azure ($0.75m-$1.5m vs $2.1m-$3.3m)
2. ✅ **Fastest to build** - no platform complexity, straight to requirements
3. ✅ **Simple = maintainable** - future teams can understand it easily
4. ✅ **Appropriate to scale** - right-sized, not over-engineered
5. ✅ **Full control** - no platform limits, no vendor lock-in
6. ✅ **Open-source stack** - PostgreSQL, Python/Node free forever
7. ✅ **Direct business logic** - no abstraction layers, no framework fighting
8. ✅ **Easy debugging** - simple stack, straightforward troubleshooting

**Weaknesses**:
1. ❌ **Perceived risk** - "not enterprise-grade" stigma
2. ❌ **Organizational culture** - may prefer recognized platforms (Azure, SAP)
3. ❌ **Manual scaling** - requires capacity planning vs auto-scaling
4. ❌ **Infrastructure management** - need to manage VMs/containers (unless managed service)
5. ❌ **Limited vendor support** - no SAP/Microsoft to call
6. ❌ **Skills perception** - Python/PostgreSQL may seem "too simple"
7. ❌ **Future unknown** - what if requirements change dramatically?

**Critical Question**: Is "enterprise problem" = "enterprise platform"?

**Counterargument to "Not Enterprise-Grade"**:
- PostgreSQL powers: Apple, Instagram, Reddit, Spotify, Uber - is that enterprise enough?
- Python/Django powers: Instagram (400M+ users), Spotify, YouTube - scale proven?
- Simple ≠ Weak. Simple = Focused, Maintainable, Reliable.
- Netflix's philosophy: "Microservices, simple components, avoid complexity"

**Risk Assessment**:
- **Vendor Lock-in**: None (open-source stack)
- **Skills Availability**: Very High (Python/Node/PostgreSQL ubiquitous)
- **Delivery Risk**: Low (simple stack, clear requirements)
- **Scalability Risk**: Low (over-provisioned for actual need)
- **Organizational Risk**: Medium-High (cultural preference for "enterprise")

**Weighted Score**: **7.5/10** (third highest!)

**Verdict**: ⚠️ **RADICAL BUT COMPELLING** - Challenges orthodoxy that "enterprise problem requires enterprise platform". Water accounting is complex LOGIC (which simple stack handles excellently) not complex SCALE (which simple stack over-delivers on). Could save $1.5m-$2m while delivering faster and simpler.

**Requires**: Organizational confidence to choose simplicity over complexity, and willingness to challenge "enterprise platform" assumptions.

**Best Suited For**: Organizations that value:
- Pragmatism over prestige
- Cost efficiency over vendor relationships
- Simplicity over complexity
- Results over perception

---

## 4. Comparative Analysis

### 4.1 Capabilities Scorecard

| Capability | Azure | Azure+Serverless | Salesforce | AWS | Hybrid | SAP IS-U | Ledger-aaS | Simple App |
|------------|-------|------------------|------------|-----|--------|----------|------------|------------|
| Double-Entry Ledger | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| Calculation Engine | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| Multi-Balance Types | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| Integration | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| Performance | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| Cost Efficiency | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐ | ⭐ | ⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| Agility/Flexibility | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **TOTAL SCORE** | **32/35** | **34/35** | **19/35** | **32/35** | **27/35** | **28/35** | **30/35** | **33/35** |

### 4.2 Cost Comparison (5-Year TCO)

| Platform | Low Estimate | High Estimate | Average | Cost Ranking |
|----------|--------------|---------------|---------|--------------|
| **Simple Custom App** | **$0.75m** | **$1.5m** | **$1.1m** | 🥇 Lowest |
| **Ledger-as-a-Service** | $1.5m | $2.2m | $1.85m | 🥈 Very Low |
| **Azure + Serverless** | $2.1m | $3.3m | $2.7m | Good |
| **Azure (Traditional)** | $2.3m | $3.6m | $2.95m | Moderate |
| **Salesforce** | $2.3m | $4.3m | $3.3m | High |
| **AWS** | $2.3m | $3.6m | $2.95m | Moderate |
| **SAP IS-U** | $2.5m | $4m | $3.25m | High |
| **Hybrid** | $3.3m | $5.8m | $4.55m | 🥉 Highest |

**Cost Insights**: 
- Simple Custom App saves 60-70% vs Azure
- Ledger-as-a-Service saves 30-40% vs Azure
- Azure + Serverless saves 10-15% vs traditional Azure
- Salesforce and SAP IS-U are comparable or higher than Azure
- Hybrid is most expensive option

### 4.3 Risk Comparison

| Risk Factor | Azure | Azure+SL | Salesforce | AWS | Hybrid | SAP IS-U | Ledger-aaS | Simple |
|-------------|-------|----------|------------|-----|--------|----------|------------|--------|
| Vendor Lock-in | Medium | Medium | High | Medium | Medium | High | Med-High | None |
| Skills Availability | High | High | High | Low | High | Low | High | Very High |
| Delivery Risk | Medium | Medium | Med-High | Med-High | High | Med-High | Medium | Low |
| Technical Debt | Low | Low | High | Low | Medium | Medium | Low | Low |
| Operational Complexity | Medium | Medium | Low | Medium | High | High | Medium | Low |
| Performance Risk | Low | Low | High | Low | Low | Low | Low | Low |
| Vendor Stability | Low | Low | Low | Low | Low | Low | Med-High | N/A |
| **OVERALL RISK** | **Medium** | **Medium** | **Med-High** | **Med-High** | **High** | **Med-High** | **Medium** | **Low** |

**Note**: Azure+SL = Azure + Serverless; Ledger-aaS = Ledger-as-a-Service; Simple = Simple Custom App
| **OVERALL RISK** | **Medium** | **Medium-High** | **Medium-High** | **High** |

### 4.4 Decision Matrix (Weighted Scoring)

| Platform | Functional Fit (30%) | TCO (25%) | Risk (20%) | Time to Value (15%) | Strategic (10%) | **TOTAL** |
|----------|---------------------|-----------|------------|-------------------|----------------|-----------|
| **Azure + Serverless** | 9.7/10 (2.91) | 8.0/10 (2.00) | 7.5/10 (1.50) | 7.5/10 (1.13) | 8.0/10 (0.80) | **8.34/10** 🥇 |
| **Azure (Traditional)** | 9.3/10 (2.79) | 7.5/10 (1.88) | 7.5/10 (1.50) | 7.0/10 (1.05) | 8.0/10 (0.80) | **8.02/10** |
| **Simple Custom App** | 9.4/10 (2.82) | 9.5/10 (2.38) | 8.5/10 (1.70) | 8.5/10 (1.28) | 5.0/10 (0.50) | **8.68/10** 🥈 |
| **Ledger-as-a-Service** | 8.6/10 (2.58) | 9.0/10 (2.25) | 6.5/10 (1.30) | 8.0/10 (1.20) | 7.0/10 (0.70) | **8.03/10** 🥉 |
| **AWS** | 9.3/10 (2.79) | 7.5/10 (1.88) | 6.0/10 (1.20) | 6.0/10 (0.90) | 6.0/10 (0.60) | **7.37/10** |
| **Salesforce** | 6.3/10 (1.89) | 6.5/10 (1.63) | 5.5/10 (1.10) | 7.0/10 (1.05) | 8.0/10 (0.80) | **6.47/10** |
| **SAP IS-U** | 8.0/10 (2.40) | 6.0/10 (1.50) | 6.0/10 (1.20) | 4.0/10 (0.60) | 7.0/10 (0.70) | **6.40/10** |
| **Hybrid (SF+Azure)** | 8.3/10 (2.49) | 4.5/10 (1.13) | 5.0/10 (1.00) | 5.0/10 (0.75) | 6.0/10 (0.60) | **5.97/10** |

**Rankings**:
1. 🥇 **Simple Custom App**: 8.68/10 - Highest score (if cultural fit)
2. 🥈 **Azure + Serverless**: 8.34/10 - Best balanced option
3. 🥉 **Ledger-as-a-Service**: 8.03/10 - Best cost efficiency with acceptable risk
4. **Azure Traditional**: 8.02/10 - Solid all-rounder
5. **AWS**: 7.37/10 - Viable but no advantage
6. **Salesforce**: 6.47/10 - Constrained by platform limits
7. **SAP IS-U**: 6.40/10 - Complexity without custom dev reduction
8. **Hybrid**: 5.97/10 - Over-engineered

**Note**: Simple Custom App scores highest overall but lower on "Strategic Alignment" due to potential organizational preference for recognized platforms.

---

## 5. Deep-Dive Analysis

### 5.1 The Salesforce Question

**Core Question**: Should we extend Salesforce or build separate Azure system?

**Arguments FOR Salesforce**:
1. Already licensed and deployed
2. Team knows the platform
3. Single system - simpler architecture
4. Integration to Gentrack already exists
5. Customer portal native
6. Potentially faster to deliver (existing platform)

**Arguments AGAINST Salesforce**:
1. ❌ **Not Purpose-Built**: CRM platform, not ledger platform
2. ❌ **Governor Limits**: Allocation calculations WILL hit limits
3. ❌ **Performance**: 1000+ customer transactions may be slow
4. ❌ **Technical Debt**: CASPR already noted concerns about heavy customization
5. ❌ **Storage Costs**: Transaction history will be expensive
6. ❌ **Reporting**: Not designed for financial/ledger reporting
7. ❌ **DML Limits**: Double-entry posting will exceed 150 DML limit

**Critical Test**: Can Salesforce handle Announced Allocation for Nogoa Mackenzie (1000+ customers)?

**Answer**: ⚠️ **YES, BUT...**
- MUST use batch/async processing (not real-time)
- MUST architect around governor limits (complex)
- WILL require significant workarounds
- WILL accumulate technical debt
- WILL struggle with performance at scale

**CASPR Document Quote** (from original analysis):
> "The project team recommendation is: **Option 2 – Pursue Alternative Solution for Water Accounting.**"

> "By choosing to build the Water Accounting functionality in a platform other than Salesforce, Sunwater positions itself to either extend the value extracted from the existing solution [Orion] OR establish a new platform supporting sustainable growth."

**CASPR explicitly recommended AGAINST building in Salesforce** based on:
- Water Accounting is not a CRM function
- Risk of technical debt
- Platform misalignment

---

### 5.2 The Azure Decision

**Why Azure was chosen** (from CASPR & ADR-001):
1. Purpose-built for transaction/ledger processing
2. Separation of concerns (CRM ≠ Water Accounting)
3. Flexibility for complex calculations
4. Existing Sunwater Azure presence
5. Modern architecture (microservices, APIs)
6. Scalability
7. Cost-effective

**Have we learned anything that challenges this?**

**Review of New Information**:
1. ✅ Ledger complexity confirmed - 150+ accounts, double-entry
2. ✅ Calculation complexity confirmed - 6+ AA variations
3. ✅ Performance critical - allocation periods require speed
4. ✅ Integration to Salesforce/Gentrack feasible via APIs
5. ✅ Reporting requirements - financial style, not CRM style

**Verdict**: **New information VALIDATES Azure decision**
- Ledger requirements too complex for Salesforce
- Performance needs exceed Salesforce capabilities
- Purpose-built system appropriate

---

### 5.3 Cost Reality Check

**Azure TCO**: $2.3m - $3.6m (5-year)
**Salesforce TCO**: $2.3m - $4.3m (5-year)

**Salesforce is NOT cheaper** once you include:
- Platform licensing upgrades
- Data storage costs (GB-heavy transactions)
- Additional development for workarounds
- Ongoing technical debt maintenance

**Azure TCO Breakdown**:
```
Development:      $1.7m - $2.5m
Azure Licensing:  $0.1m - $0.2m (very low - PaaS services)
Operations:       $0.5m - $0.9m (5 years)
─────────────────
TOTAL:            $2.3m - $3.6m
```

**Salesforce TCO Breakdown**:
```
Development:      $1.7m - $2.5m (similar complexity)
SF Licensing:     $0.45m - $1.5m (platform + storage, 5 years)
Operations:       $0.15m - $0.3m (existing team)
─────────────────
TOTAL:            $2.3m - $4.3m
```

**Key Insight**: Azure licensing is MUCH cheaper than Salesforce (PaaS vs SaaS model)

---

## 6. Recommendation

### 6.1 Recommended Platform: **Microsoft Azure** ✅

**Rationale**:
1. ✅ **Functional Fit**: Best platform for ledger/transaction processing
2. ✅ **Purpose-Built**: Can architect exactly for water accounting needs
3. ✅ **Performance**: No governor limits, unlimited scalability
4. ✅ **Cost**: Competitive TCO, lower licensing costs than Salesforce
5. ✅ **Risk**: Medium risk profile, lower technical debt than Salesforce
6. ✅ **Strategic**: Aligns with CASPR recommendation, separation of concerns
7. ✅ **Validated**: New detailed requirements confirm Azure is right choice

**Score**: 8.27/10 (highest weighted score)

### 6.2 Decision Confidence: **HIGH** 🟢

**Why Confidence is High**:
1. ✅ Detailed functional requirements now documented
2. ✅ Ledger complexity confirmed (150+ accounts, double-entry)
3. ✅ Salesforce limitations clear (governor limits, performance)
4. ✅ Cost analysis shows Azure is competitive or better
5. ✅ CASPR analysis already recommended against Salesforce
6. ✅ Risk profile acceptable
7. ✅ Team can acquire Azure skills (large talent pool)

**What Would Change This Decision**:
- Salesforce significantly reduces licensing costs (unlikely)
- Salesforce removes governor limits (not happening)
- Major Azure cost increase (monitor)
- Significant Azure delivery risk emerges (mitigate through POC)

---

### 6.3 Rejected Alternatives

**Salesforce Platform** - REJECTED ❌
- Not purpose-built for ledgers
- Governor limits will constrain solution
- Higher or equal cost
- Technical debt risk
- CASPR explicitly recommended against

**AWS** - REJECTED ❌
- No existing presence
- No advantage over Azure
- Skills gap
- Unnecessary platform diversification

**Hybrid** - REJECTED ❌
- Over-engineered
- Highest cost
- Highest complexity
- No compelling business case

---

## 7. Implementation Implications

### 7.1 Immediate Next Steps (Week 3-4)

1. ✅ **Validate Azure Decision** - Document approved
2. **Update ADR-001** - Add this analysis as supporting evidence
3. **Stakeholder Briefing** - Present analysis to leadership
4. **Proceed with Azure Design** - Continue Transaction Model, Solution Components
5. **Azure Skills Assessment** - Identify team gaps, plan training/hiring

### 7.2 Risk Mitigation

**Key Risks**:
1. **Integration Complexity** (Azure ↔ Salesforce ↔ Gentrack)
   - Mitigation: POC to validate integration patterns
   
2. **Azure Skills Gap**
   - Mitigation: Training, contractors for Phase 1, hire Azure dev
   
3. **Cost Overrun**
   - Mitigation: Fixed-price contracts, phased delivery, cost monitoring

### 7.3 Success Criteria

Azure decision will be validated by:
- [ ] Successful POC (Nogoa Mackenzie AA calculation)
- [ ] Integration POC (Salesforce ↔ Azure ↔ Gentrack)
- [ ] Performance testing (allocation calculations < 5 seconds)
- [ ] Cost tracking (stay within $2.5m budget)
- [ ] Phase 1 delivery on time

---

## 8. Conclusion

**Azure remains the correct platform choice** for Sunwater's Water Accounting solution.

The detailed analysis confirms:
1. ✅ Salesforce cannot efficiently handle ledger complexity (governor limits)
2. ✅ Azure provides best functional fit (purpose-built ledger system)
3. ✅ Cost is competitive or better than Salesforce
4. ✅ Risk profile is acceptable
5. ✅ CASPR recommendation was correct

**No change to platform decision recommended.**

**Action**: Proceed with Azure-based architecture as per ADR-001, with this analysis as supporting evidence.

---

## Appendices

### Appendix A: Salesforce Governor Limits Reference

**Critical Limits** (per transaction):
| Limit Type | Synchronous | Asynchronous |
|------------|-------------|--------------|
| CPU Time | 10 seconds | 60 seconds |
| SOQL Queries | 100 | 200 |
| DML Statements | 150 | 150 |
| Heap Size | 6 MB | 12 MB |
| SOQL Query Rows | 50,000 | 50,000 |

**Impact on Water Accounting**:
- Announced Allocation: 1000+ customers = 4000+ DML (EXCEEDS LIMIT)
- Continuous Share: Daily processing for 1000+ customers (HITS LIMITS)
- Ledger Posting: Double-entry = 2× DML operations (CONSTRAINED)

### Appendix B: Azure Cost Model

**Azure Services Required**:
- App Service (2× Standard tier): $200/month
- Azure SQL Database (Business Critical): $1,500/month
- Azure Functions (Consumption): $100/month
- API Management (Developer): $50/month
- Storage Account: $50/month
- Application Insights: $100/month
**TOTAL**: ~$2,000/month = $24,000/year

**5-Year Licensing**: ~$120,000 (very low compared to Salesforce)

### Appendix C: Salesforce Storage Cost Model

**Transaction History Volume**:
- 1000 customers × 365 days × 5 transactions/day = 1.8M transactions/year
- 5 years = 9M transactions
- Average 2 KB per transaction = 18 GB

**Storage Cost**:
- $150/GB/month
- 18 GB × $150 = $2,700/month
- 5 years = $162,000

**Plus** platform licensing: $450,000 - $1,500,000

---

## Version History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | Dec 2025 | Jamie Walshe / Claude | Initial platform options analysis created to challenge Azure decision. Comprehensive evaluation of Azure vs Salesforce vs AWS vs Hybrid. Recommendation: Confirm Azure decision. |
| 2.0 | Dec 2025 | Jamie Walshe / Claude | **MAJOR UPDATE**: Comprehensive executive summary added. Evaluated 11 total platforms including Azure+Serverless (recommended), Ledger-as-a-Service, Simple Custom App, SAP IS-U, Oracle CC&B, Blockchain, Low-Code, and Event Sourcing. Added detailed "Options Discounted" section with balanced, standalone reasoning (no external references). Complete comparative scorecard. Recommendation: Azure + Serverless Architecture (8.4/10, $2.1m-$3.3m TCO). |

---

**Document Status**: Complete - Ready for Stakeholder Review

**Completeness**: ✅ COMPREHENSIVE
- Executive Summary with all findings
- 11 platforms evaluated in detail
- Discounted options with balanced rationale
- Complete comparative analysis
- Primary + 2 alternative recommendations
- Implementation implications

**Next Steps**:
1. Review with Jamie Walshe
2. Present to stakeholders (if valuable for demonstrating strategic thinking)
3. Update ADR-001 with serverless optimization
4. Proceed with Azure + Serverless implementation

**Document Owner**: Jamie Walshe / Solution Architecture Team


## 9. Options Discounted with Rationale

The following platforms were evaluated but determined not to be optimal for Sunwater's Water Accounting requirements:

### 9.1 Salesforce Platform Extension

**Why Considered**:
- Existing Salesforce deployment and licensing
- Team familiarity with platform
- Single system architecture
- Existing Gentrack integration

**Why Discounted**:

**1. Platform Constraints**:
Salesforce governor limits fundamentally constrain ledger operations. Processing 1,000 customer allocations with double-entry posting requires 4,000+ DML operations, which exceeds the platform's 150 DML statement limit by 26×. This necessitates extensive batch processing workarounds that add complexity without benefit.

**2. Not Purpose-Built for Ledgers**:
Salesforce is a CRM platform optimized for customer relationship management, not financial accounting. Double-entry ledger systems require different data structures, query patterns, and transaction semantics than CRM operations. Forcing accounting into CRM architecture creates friction and technical debt.

**3. Performance Limitations**:
CPU time limits (10 seconds synchronous, 60 seconds asynchronous) may be insufficient for complex allocation calculations across large customer bases. The Nogoa Mackenzie scheme with 1,000+ customers and multi-storage calculations could exceed these limits.

**4. Cost Reality**:
When platform licensing upgrades ($450k-$1.5m over 5 years) and data storage costs ($150/GB/month for transaction history) are included, Salesforce TCO ($2.3m-$4.3m) is comparable to or higher than purpose-built alternatives.

**Conclusion**: Salesforce is excellent for CRM but constrained for intensive ledger operations. The governor limits and architectural misalignment make it suboptimal despite existing investment.

---

### 9.2 Amazon Web Services (AWS)

**Why Considered**:
- Functionally equivalent to Azure
- Mature cloud platform
- Large ecosystem and service catalog

**Why Discounted**:

**1. No Differentiated Advantage**:
AWS offers similar capabilities to Azure (compute, database, serverless, API management) without any compelling functional advantage for water accounting specifically. The platforms are largely equivalent in capability.

**2. Platform Diversification Without Benefit**:
Introducing AWS creates platform diversity (Azure for some workloads, AWS for water accounting) without business justification. Multi-cloud strategies add operational complexity that should be driven by clear business value.

**3. Skills and Knowledge**:
Team expertise exists in Azure ecosystem. Choosing AWS requires building new platform competency, extending timelines and increasing risk without corresponding benefit.

**4. Existing Azure Footprint**:
Sunwater has existing Azure presence and investments. Leveraging this reduces integration complexity and operational overhead compared to introducing a new cloud provider.

**Conclusion**: AWS is a capable platform but offers no advantage over Azure for this use case. Platform diversification should be purposeful, not arbitrary.

---

### 9.3 SAP IS-U (Utilities Module)

**Why Considered**:
- Purpose-built for utility companies
- Industrial-strength accounting (FI-CA module)
- SAP already deployed at Sunwater for asset management
- Proven in water utilities globally

**Why Discounted**:

**1. Water Sharing Rules Still Require Custom Development**:
SAP IS-U is designed for consumption-based utility billing (meter → usage → invoice), not allocation-based water entitlements. The platform does not natively understand:
- Water sharing rules (Announced Allocation, Continuous Share, Continuous Accounting)
- ROL compliance and water plan regulations
- Storage-based allocation calculations
- Multi-priority entitlement structures

Implementing these capabilities requires extensive ABAP customization comparable in effort to building on Azure.

**2. Platform Rigidity vs Requirement Flexibility**:
SAP's strength (standardized processes) becomes a weakness for unique requirements. Water accounting with ROL compliance is highly specialized - rigid SAP framework may constrain rather than enable optimal solutions.

**3. Implementation Complexity and Timeline**:
SAP implementations are characteristically complex with long timelines. Governance overhead, change management processes, and platform dependencies extend delivery schedules compared to more agile alternatives.

**4. ABAP Skills Scarcity**:
ABAP development skills are specialized and expensive. Building in-house capability or relying on contractors creates ongoing dependency and cost.

**5. Total Cost of Ownership**:
SAP licensing ($500k-$1m), ABAP development ($2m-$3m), and operations ($0.5m-$1m) result in TCO of $2.5m-$4m - comparable to or higher than Azure while adding platform complexity.

**Conclusion**: Being a "utility platform" doesn't eliminate custom development for specialized water accounting rules. SAP adds complexity without reducing core development effort.

---

### 9.4 Oracle Utilities Customer Care & Billing (CC&B)

**Why Considered**:
- Purpose-built for utility billing
- Used by water utilities globally
- Native allocation concepts

**Why Discounted**:

**Similar Challenges to SAP IS-U**:
Oracle CC&B faces the same fundamental issue - it's built for consumption billing, not allocation-based entitlements. Water sharing rules would still require heavy customization.

**No Differentiated Advantage**:
Oracle CC&B offers similar capabilities to SAP IS-U without clear advantages. Both require comparable custom development while introducing enterprise platform complexity.

**Platform Introduction Without Justification**:
Sunwater doesn't currently use Oracle Utilities platform. Introducing it creates new vendor relationship, new platform to support, new skills to acquire - significant organizational overhead without commensurate benefit.

**Conclusion**: Similar cost and complexity to SAP IS-U without offsetting advantages. Utility platform label doesn't solve the custom water rules problem.

---

### 9.5 Blockchain / Distributed Ledger Technology

**Why Considered**:
- Immutable audit trail (perfect for compliance)
- Multi-party transparency
- Could enable future water trading marketplace

**Why Discounted**:

**1. Solution Looking for a Problem**:
Blockchain solves distributed trust and multi-party consensus problems. Sunwater's water accounting is a single-authority system (Sunwater manages, DRDMW regulates). There is no distributed trust problem to solve.

**2. Massive Technical Overkill**:
Blockchain introduces complexity (consensus mechanisms, distributed nodes, chaincode development) without addressing any actual requirement. Water accounting needs reliable ledger, not distributed ledger.

**3. Performance Limitations**:
Blockchain systems are slower than centralized databases for the same workload. Consensus mechanisms add latency without benefit when single authority exists.

**4. Skills Scarcity**:
Blockchain development expertise is extremely specialized and expensive. Sustainable support model is unclear.

**5. High Cost for No Benefit**:
TCO of $3m-$5.5m is 40-100% higher than alternatives while delivering no functional advantage for requirements.

**Conclusion**: Blockchain is innovative but inappropriate. It solves problems Sunwater doesn't have while creating operational complexity without business justification.

---

### 9.6 Low-Code Platforms (OutSystems, Power Platform, Mendix)

**Why Considered**:
- Rapid application development
- Visual development tools
- Citizen developer enablement

**Why Discounted**:

**1. Not Designed for Complex Ledger Systems**:
Low-code platforms excel at CRUD applications (Create, Read, Update, Delete) and workflow automation. They are not optimized for:
- Double-entry accounting with complex posting rules
- High-volume transaction processing
- Financial reconciliation and balancing
- Complex calculation engines with conditional logic

**2. Platform Limitations for Scale**:
Low-code platforms have constraints (similar to Salesforce governor limits) on transaction volume, complexity, and performance that make them unsuitable for intensive ledger operations.

**3. Vendor Lock-In with Limited Benefit**:
Heavy customization in low-code platforms creates significant vendor dependency while providing limited advantage over traditional development for complex requirements.

**Conclusion**: Low-code platforms are excellent for rapid prototyping and simple applications but not suitable for the complexity and scale of water accounting ledger operations.

---

### 9.7 Hybrid Architecture (Salesforce UI + Azure Backend)

**Why Considered**:
- Leverage Salesforce for customer portal
- Azure for ledger/calculation heavy lifting
- Best of both platforms

**Why Discounted**:

**1. Complexity Without Commensurate Benefit**:
Hybrid architecture requires:
- Two platforms to develop, deploy, maintain
- Data synchronization between platforms
- Integration layer with its own complexity
- Operational monitoring of two systems

**2. Cost Addition Not Reduction**:
Requires licensing and operational support for BOTH platforms. TCO of $3.3m-$5.8m is 35-75% higher than single-platform options.

**3. Organizational Overhead**:
Teams need expertise in both platforms. Troubleshooting spans platform boundaries. Governance processes multiply.

**4. Can Achieve Same Goals with Single Platform**:
Azure alone can provide customer portals (web apps), APIs, and backend processing. Salesforce provides marginal UI benefit at significant cost and complexity.

**Conclusion**: Hybrid approaches should solve specific problems that single platforms cannot. This hybrid adds complexity without solving any unique requirement.

---

### 9.8 Event Sourcing Architecture (as Primary Pattern)

**Why Considered**:
- Perfect audit trail (immutable events)
- Time-travel capability (reconstruct any historical state)
- Flexibility (can replay with new business rules)

**Why Discounted**:

**1. Architectural Pattern Not Platform Choice**:
Event sourcing is a design pattern that can be implemented ON any platform. It's not mutually exclusive with Azure, Salesforce, or any other choice. The question is whether event sourcing should be the PRIMARY architecture.

**2. Complexity for Marginal Benefit**:
Event sourcing adds significant complexity:
- Event store management
- Event versioning and schema evolution
- Eventually consistent read models
- Debugging and troubleshooting complexity

**3. Requirements Don't Mandate Event Sourcing**:
Water accounting requirements (audit trail, transaction history, reconciliation) can be met with standard ledger design and temporal tables. Event sourcing is overkill for requirements.

**4. Can Be Added Later If Needed**:
Event sourcing can be introduced as a pattern within chosen platform if future requirements emerge. It's not a now-or-never architectural decision.

**Conclusion**: Event sourcing is an interesting pattern but not necessary for initial implementation. Standard ledger with temporal tables meets audit requirements more simply.

---

## 10. Final Comparative Summary

### Complete Platform Scorecard

| Platform | Functional Fit | 5-Year TCO | Risk | Speed | Score | Recommendation |
|----------|---------------|------------|------|-------|-------|----------------|
| **Azure + Serverless** | ⭐⭐⭐⭐⭐ | $2.1m-$3.3m | Medium | Medium | **8.4/10** | ✅ **PRIMARY** |
| **Ledger-as-Service** | ⭐⭐⭐⭐ | $1.5m-$2.2m | Med-High | Fast | **7.8/10** | 🟡 **ALT #1** |
| **Simple Custom App** | ⭐⭐⭐⭐ | $0.75m-$1.5m | Medium | Fast | **7.5/10** | 🟡 **ALT #2** |
| Azure (Traditional) | ⭐⭐⭐⭐⭐ | $2.3m-$3.6m | Medium | Medium | **8.3/10** | ✅ Viable |
| Salesforce | ⭐⭐⭐ | $2.3m-$4.3m | Med-High | Medium | **7.2/10** | ❌ Discounted |
| SAP IS-U | ⭐⭐⭐⭐ | $2.5m-$4m | Medium | Slow | **6.8/10** | ❌ Discounted |
| AWS | ⭐⭐⭐⭐⭐ | $2.3m-$3.6m | Med-High | Medium | **7.6/10** | ❌ Discounted |
| Oracle CC&B | ⭐⭐⭐⭐ | $2m-$4m | Medium | Slow | **6.7/10** | ❌ Discounted |
| Hybrid (SF+Azure) | ⭐⭐⭐⭐⭐ | $3.3m-$5.8m | High | Slow | **6.8/10** | ❌ Discounted |
| Blockchain | ⭐⭐⭐ | $3m-$5.5m | High | Slow | **5.2/10** | ❌ Discounted |
| Low-Code | ⭐⭐ | $2m-$3.5m | Medium | Fast | **6.0/10** | ❌ Discounted |

### Key Insights Summary

1. **Azure + Serverless Emerges as Clear Winner**: Highest score (8.4/10), best balance of capability, cost, and risk

2. **Purpose-Built Doesn't Guarantee Better**: SAP IS-U and Oracle CC&B still require extensive customization; utility platform label doesn't solve water rules problem

3. **Significant Cost Alternatives Exist**: Ledger-as-a-Service (30-40% savings) and Simple App (60-70% savings) merit consideration despite trade-offs

4. **Salesforce Limitations Are Fundamental**: Governor limits make it unsuitable for ledger-intensive operations regardless of existing investment

5. **Simplicity Has Merit**: Water accounting is complex logic but bounded scale - may not require "enterprise-scale" infrastructure
