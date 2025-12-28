# Architecture Overview

## Document Information
- **Version**: 1.0
- **Last Updated**: December 2025
- **Status**: Draft
- **Owner**: Solution Architecture Team

## Executive Summary

The Sunwater Water Accounting solution is a purpose-built platform designed to manage complex water allocation, distribution, and compliance processes across 26 Water Supply Schemes. This document provides a high-level overview of the solution architecture, component design, and integration approach.

## Architectural Principles

### 1. Separation of Concerns
- **Water Accounting** is distinct from CRM and Billing
- Each system maintains its own domain data
- Integration through well-defined APIs
- No tight coupling between systems

### 2. Fit for Purpose
- Water Accounting built on ledger/transaction platform
- CRM (Salesforce) handles customer relationships
- Billing (Gentrack) manages meter-to-cash
- Each platform optimized for its function

### 3. Regulatory Compliance by Design
- Full audit trails for all transactions
- Immutable transaction history
- Compliance reporting embedded
- ROL rules enforced systematically

### 4. Scalability and Flexibility
- Support for varying scheme complexity
- Configurable water sharing rules
- Extensible for new schemes and products
- Cloud-native for elastic scaling

### 5. Data Integrity
- Double-entry ledger accounting
- Transaction atomicity (all-or-nothing)
- Reconciliation as core capability
- No orphaned or inconsistent data

---

## Solution Context

### Problem Statement

Sunwater's legacy Water Accounting system (Orion) is:
- End-of-life with no vendor support
- Fragmented across multiple spreadsheets
- Manually intensive with high error risk
- Unable to integrate with modern systems
- Insufficient audit trails for compliance
- Cannot scale to meet future needs

### Solution Vision

A modern, integrated Water Accounting platform that:
- Automates allocation and distribution calculations
- Maintains accurate, real-time water balances
- Integrates seamlessly with CRM and billing
- Ensures regulatory compliance
- Provides transparency to customers and regulators
- Reduces manual effort and operational risk

---

## High-Level Architecture

### Conceptual Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        Presentation Layer                    │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐       │
│  │   Customer   │  │  Operations  │  │  Regulatory  │       │
│  │    Portal    │  │   Dashboard  │  │   Reporting  │       │
│  └──────────────┘  └──────────────┘  └──────────────┘       │
└────────────┬────────────────┬────────────────┬──────────────┘
             │                │                │
┌────────────┴────────────────┴────────────────┴──────────────┐
│                     Integration Layer                        │
│                  (API Gateway / ESB)                         │
└──┬────────────┬────────────────┬────────────────┬──────────┬┘
   │            │                │                │          │
   ▼            ▼                ▼                ▼          ▼
┌──────┐  ┌───────────┐  ┌─────────────┐  ┌──────┐  ┌──────┐
│ CRM  │  │   Water   │  │   Billing   │  │ SAP  │  │ Smart│
│Sales │  │Accounting │  │  (M2C)      │  │      │  │Schemes│
│force │  │  Core     │  │  Gentrack   │  │      │  │      │
└──────┘  └───────────┘  └─────────────┘  └──────┘  └──────┘
             │
    ┌────────┴────────┐
    ▼                 ▼
┌──────────┐    ┌──────────────┐
│  Ledger  │    │ Calculation  │
│ Services │    │   Engine     │
└──────────┘    └──────────────┘
```

### Solution Components

The Water Accounting solution comprises three primary components:

#### 1. Water Accounting Core
- **Purpose**: Central orchestration and business logic
- **Responsibilities**:
  - Transaction management
  - Business process workflows
  - Validation and business rules
  - Integration coordination
  - Reporting services

#### 2. Water Ledger Services
- **Purpose**: Maintain accurate water account balances
- **Responsibilities**:
  - Account structure management
  - Transaction posting (double-entry)
  - Balance calculation and tracking
  - Historical transaction storage
  - Reconciliation services

#### 3. Calculation and Balancing Services
- **Purpose**: Execute water sharing rules and allocation calculations
- **Responsibilities**:
  - Announced Allocation calculations
  - Continuous Share daily processing
  - Continuous Accounting distributions
  - Carryover calculations
  - Network balancing (storage-to-accounts)
  - Loss calculations

---

## Logical Architecture

### Component View

```
┌─────────────────────────────────────────────────────────────┐
│               Water Accounting Core                          │
│  ┌──────────────────────────────────────────────────────┐   │
│  │          Transaction Processing Engine               │   │
│  │  • Create/Modify/Cancel transactions                 │   │
│  │  • Validate business rules                           │   │
│  │  • Orchestrate posting to ledgers                    │   │
│  └──────────────────────────────────────────────────────┘   │
│  ┌──────────────────────────────────────────────────────┐   │
│  │          Business Process Management                 │   │
│  │  • Water orders                                      │   │
│  │  • Transfers (temporary & permanent)                 │   │
│  │  • Meter read processing                             │   │
│  │  • Breach management                                 │   │
│  └──────────────────────────────────────────────────────┘   │
│  ┌──────────────────────────────────────────────────────┐   │
│  │          Integration Services                        │   │
│  │  • CRM integration (customer/orders)                 │   │
│  │  • Billing integration (consumption)                 │   │
│  │  • Operational data (storage levels, flows)          │   │
│  └──────────────────────────────────────────────────────┘   │
│  ┌──────────────────────────────────────────────────────┐   │
│  │          Reporting & Analytics                       │   │
│  │  • Customer balance inquiries                        │   │
│  │  • Regulatory reports                                │   │
│  │  • Operational dashboards                            │   │
│  │  • Compliance monitoring                             │   │
│  └──────────────────────────────────────────────────────┘   │
└────────────────────┬─────────────────────────────────────────┘
                     │
       ┌─────────────┴─────────────┐
       │                           │
       ▼                           ▼
┌────────────────────┐      ┌──────────────────────┐
│  Water Ledger      │      │  Calculation &       │
│  Services          │      │  Balancing Services  │
│                    │      │                      │
│ • Account Mgmt     │      │ • Allocation Calc    │
│ • Posting Engine   │      │ • Share Processing   │
│ • Balance Calc     │      │ • Network Balance    │
│ • Reconciliation   │      │ • Rule Engine        │
│ • History/Audit    │      │ • Formula Library    │
└────────────────────┘      └──────────────────────┘
```

### Data Flow Architecture

```
┌──────────┐                                    ┌──────────┐
│ Customer │──── Order Water ─────────────────▶│   CRM    │
└──────────┘                                    │Salesforce│
                                                └────┬─────┘
                                                     │
                                        API: Create Water Order
                                                     │
                                                     ▼
┌──────────┐                            ┌──────────────────┐
│Operations│◀──── Work Order ───────────│  Water Acct Core │
└──────────┘                            └────┬─────────────┘
     │                                       │
     │ Release Water                         │ Reserve Water
     │                                       ▼
     │                              ┌──────────────────┐
     │                              │  Ledger Services │
     │                              │  • Debit Avail   │
     │                              │  • Credit Pending│
     │                              └──────────────────┘
     │
     └─── Water Released ──────────────────▶
                                            │
                             Update Order Status
                                            │
                                            ▼
┌──────────┐                       ┌──────────────────┐
│  Meter   │──── Reading ────────▶│  Water Acct Core │
│  System  │                       └────┬─────────────┘
└──────────┘                            │
                                        │ Calculate Consumption
                                        ▼
                               ┌──────────────────┐
                               │  Ledger Services │
                               │  • Debit Pending │
                               │  • Record Usage  │
                               └────┬─────────────┘
                                    │
                      Consumption Transaction
                                    │
                                    ▼
                            ┌──────────────┐
                            │   Gentrack   │
                            │   (Billing)  │
                            └──────────────┘
```

---

## Technology Architecture

### Platform Selection: Microsoft Azure

**Rationale**:
- Cloud-native scalability and resilience
- Strong integration capabilities (Azure Integration Services)
- Rich data services (Azure SQL, Cosmos DB)
- Comprehensive security and compliance
- Existing Sunwater Azure investment
- Extensive partner ecosystem

### Core Technologies

#### Application Platform
- **Azure App Services**: Host core application components
- **Azure Functions**: Event-driven processing (daily calculations, triggers)
- **Azure API Management**: API gateway and management
- **Azure Logic Apps**: Integration workflows and orchestration

#### Data Platform
- **Azure SQL Database**: Primary transactional database
  - Water accounts and ledgers
  - Transaction history
  - Master data (schemes, products, customers)
- **Azure Cosmos DB**: High-performance lookups (optional)
  - Current balances (fast read access)
  - Customer account summaries
- **Azure Synapse Analytics**: Reporting and analytics
  - Historical analysis
  - Regulatory reporting
  - Business intelligence

#### Integration Platform
- **Azure API Management**: Centralized API gateway
- **Azure Service Bus**: Asynchronous messaging
- **Azure Event Grid**: Event distribution
- **Azure Data Factory**: Batch data integration

#### Security & Monitoring
- **Azure Active Directory**: Authentication and authorization
- **Azure Key Vault**: Secrets and certificate management
- **Azure Monitor**: Application monitoring and alerting
- **Application Insights**: Performance monitoring and diagnostics

---

## Integration Architecture

### Integration Patterns

#### 1. Synchronous (Request/Response)
**Use for**: Real-time inquiries and critical transactions
- Customer balance inquiries
- Water order creation
- Transaction validation
- Account lookups

**Technology**: REST APIs via Azure API Management

#### 2. Asynchronous (Event-Driven)
**Use for**: Non-urgent updates and batch processing
- Meter read processing
- Daily allocation calculations
- Reconciliation processing
- Reporting data updates

**Technology**: Azure Service Bus, Event Grid

#### 3. Batch (Scheduled)
**Use for**: Periodic bulk processing
- End of day reconciliation
- Monthly reporting
- Data archival
- Master data synchronization

**Technology**: Azure Data Factory, Azure Functions (timer-triggered)

### Key Integrations

#### Salesforce (CRM)
- **Direction**: Bidirectional
- **Pattern**: Synchronous + Event-driven
- **Data Exchanged**:
  - Customer master data → Water Accounting
  - Water orders → Water Accounting
  - Account balances ← Water Accounting
  - Allocation notifications ← Water Accounting

#### Gentrack (Billing)
- **Direction**: Water Accounting → Gentrack
- **Pattern**: Event-driven
- **Data Exchanged**:
  - Consumption transactions → Gentrack
  - Account balances (for validation) → Gentrack

#### SAP
- **Direction**: SAP → Water Accounting
- **Pattern**: Batch
- **Data Exchanged**:
  - Infrastructure configuration → Water Accounting
  - Network topology → Water Accounting

#### PI System / SCADA
- **Direction**: PI/SCADA → Water Accounting
- **Pattern**: Event-driven (near real-time)
- **Data Exchanged**:
  - Storage levels → Water Accounting
  - Flow rates → Water Accounting
  - Release volumes → Water Accounting

#### Smart Schemes
- **Direction**: Bidirectional
- **Pattern**: Event-driven
- **Data Exchanged**:
  - Meter readings → Water Accounting
  - Water orders → Smart Schemes
  - Account status → Smart Schemes

---

## Deployment Architecture

### Environment Strategy

```
┌──────────────────────────────────────────────────────────┐
│                    Production (PROD)                      │
│  • Live customer transactions                            │
│  • 24/7 availability                                     │
│  • Full redundancy (multi-AZ)                            │
│  • Automated backups (daily)                             │
└──────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────┐
│              Pre-Production (UAT/STAGING)                 │
│  • Final testing before production                       │
│  • Production-like configuration                         │
│  • Customer acceptance testing                           │
│  • Performance testing                                   │
└──────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────┐
│                      Test (TEST)                          │
│  • Integration testing                                   │
│  • Regression testing                                    │
│  • Refreshed from production data (sanitized)            │
└──────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────┐
│                Development (DEV)                          │
│  • Active development                                    │
│  • Unit testing                                          │
│  • Synthetic test data                                  │
└──────────────────────────────────────────────────────────┘
```

### High Availability Design

- **Multi-Zone Deployment**: Azure Availability Zones for redundancy
- **Load Balancing**: Azure Load Balancer for traffic distribution
- **Database Replication**: Geo-redundant storage and read replicas
- **Automated Failover**: Health probes and automatic failover
- **Backup & Recovery**: Daily backups with point-in-time recovery

### Scalability Design

- **Horizontal Scaling**: Auto-scaling App Services based on load
- **Elastic Database**: Azure SQL elastic pools for variable workload
- **Caching**: Redis cache for frequently accessed data
- **CDN**: Azure CDN for static content delivery
- **Queue Buffering**: Service Bus queues to handle traffic spikes

---

## Security Architecture

### Security Layers

```
┌─────────────────────────────────────────────────────┐
│          Network Security                            │
│  • Azure Firewall                                   │
│  • Network Security Groups (NSG)                    │
│  • Private Endpoints                                │
│  • DDoS Protection                                  │
└─────────────────────────────────────────────────────┘
                      ▼
┌─────────────────────────────────────────────────────┐
│          Identity & Access                           │
│  • Azure AD authentication                          │
│  • Multi-factor authentication (MFA)                │
│  • Role-based access control (RBAC)                 │
│  • Conditional access policies                      │
└─────────────────────────────────────────────────────┘
                      ▼
┌─────────────────────────────────────────────────────┐
│          Application Security                        │
│  • API authentication (OAuth 2.0)                   │
│  • Input validation                                 │
│  • Output encoding                                  │
│  • SQL injection prevention                         │
└─────────────────────────────────────────────────────┘
                      ▼
┌─────────────────────────────────────────────────────┐
│          Data Security                               │
│  • Encryption at rest (TDE)                         │
│  • Encryption in transit (TLS 1.2+)                 │
│  • Data classification & masking                    │
│  • Key management (Azure Key Vault)                 │
└─────────────────────────────────────────────────────┘
                      ▼
┌─────────────────────────────────────────────────────┐
│          Monitoring & Audit                          │
│  • Activity logging (Azure Monitor)                 │
│  • Security alerts (Security Center)                │
│  • Audit trails (immutable logs)                    │
│  • Compliance reporting                             │
└─────────────────────────────────────────────────────┘
```

### Access Control Model

**Role-Based Access Control (RBAC)**:

| Role | Access Level | Typical Actions |
|------|--------------|-----------------|
| **Customer** | Read own data | View balances, order water, view history |
| **Customer Service** | Read all, Write limited | Inquiries, order support, customer assistance |
| **Operations** | Read all, Write operational | Water releases, infrastructure status, meter reads |
| **Finance** | Read all, Write financial | Pricing, billing validation, financial reports |
| **Scheme Manager** | Read all, Write scheme config | Allocation decisions, scheme parameters, exceptions |
| **System Admin** | Full access | System configuration, user management, troubleshooting |
| **Auditor** | Read all (audit logs) | Compliance review, transaction audits, reporting |

---

## Data Architecture Principles

### 1. Single Source of Truth
- Each entity has one authoritative source
- Master data clearly defined
- Reference data synchronized

### 2. Temporal Accuracy
- Historical data preserved
- Effective dating for all changes
- Point-in-time queries supported

### 3. Audit Trail
- Complete transaction history
- Immutable logs
- Who/when/what/why captured

### 4. Data Quality
- Validation at entry
- Reconciliation processes
- Data quality monitoring
- Exception handling

---

## Architectural Decisions

### Key Decisions Made

| Decision | Rationale | Alternatives Considered |
|----------|-----------|------------------------|
| **Azure Platform** | Existing investment, comprehensive services, strong integration | AWS, Google Cloud |
| **Separate Water Accounting System** | Fit-for-purpose, reduces tech debt, modular architecture | Extend Salesforce, Custom development on-premise |
| **Ledger-based Design** | Double-entry accounting integrity, audit compliance | Document-based, Event-sourcing |
| **API-First Integration** | Loose coupling, scalability, standard interfaces | Direct database access, File-based |
| **Azure SQL Database** | ACID compliance, familiar technology, strong tooling | Cosmos DB, PostgreSQL |
| **Event-Driven for Non-Critical** | Resilience, scalability, decoupling | Synchronous all, Batch all |

### Decisions Pending

| Decision | Options | Target Date |
|----------|---------|-------------|
| Caching Strategy | Redis vs. Cosmos DB vs. In-memory | Design phase |
| Reporting Tool | Power BI vs. Custom dashboards | Design phase |
| Mobile Access | Native app vs. Responsive web | Requirements phase |
| Archive Strategy | Blob storage vs. Synapse | Design phase |

---

## Design Constraints

### Technical Constraints
- Must integrate with existing Salesforce and Gentrack
- Must support existing PI System data feeds
- Azure platform mandated
- Must support 26 diverse schemes
- Must handle daily processing within nightly batch window

### Business Constraints
- Cannot disrupt customer service during migration
- Must maintain Orion in parallel during transition
- Regulatory reporting cannot be interrupted
- Peak water year processing: July-August
- Budget constraints limit greenfield development

### Regulatory Constraints
- ROL compliance mandatory
- DRDMW reporting requirements
- Audit trail retention (7 years minimum)
- Privacy and security requirements
- Environmental flow monitoring

---

## Architecture Validation

### Validation Against Requirements

| Requirement | Architectural Response |
|-------------|----------------------|
| **Accurate allocation calculations** | Dedicated calculation engine with configurable rules |
| **Real-time balance visibility** | API-based access to ledger services, cached for performance |
| **Regulatory compliance** | Immutable audit trails, compliance reporting embedded |
| **System integration** | API-first with multiple integration patterns |
| **Scalability** | Cloud-native, auto-scaling, distributed architecture |
| **Resilience** | Multi-zone deployment, redundancy, automated recovery |
| **Data integrity** | Double-entry ledgers, ACID transactions, reconciliation |
| **User experience** | Separation of concerns allows optimized UIs per role |

---

## Success Metrics

The architecture will be considered successful when:

1. **Performance**:
   - Balance inquiry < 2 seconds (95th percentile)
   - Daily processing completes within 4-hour window
   - API response times < 500ms (95th percentile)

2. **Reliability**:
   - System availability > 99.5% during business hours
   - Zero data loss
   - Recovery Time Objective (RTO) < 4 hours
   - Recovery Point Objective (RPO) < 1 hour

3. **Scalability**:
   - Supports all 26 schemes without performance degradation
   - Handles 10x transaction volume (future growth)
   - Accommodates new schemes with configuration only

4. **Compliance**:
   - 100% ROL compliance
   - Complete audit trails for all transactions
   - Regulatory reports generated automatically
   - Zero compliance findings in audits

5. **Integration**:
   - Real-time data sync with CRM and billing
   - Automated data feeds from operational systems
   - API availability > 99.9%

---

## Next Steps

1. **Detailed Design**: Component-level specifications
2. **Data Model**: Complete entity-relationship design
3. **API Specifications**: Detailed interface contracts
4. **Security Design**: Threat modeling and controls
5. **Performance Design**: Load testing scenarios
6. **Migration Design**: Orion to new platform strategy

---

## Related Documents

- [Solution Components](./solution-components.md) - Detailed component design
- [Integration Architecture](./integration-architecture.md) - Integration patterns and specs
- [Technology Stack](./technology-stack.md) - Detailed technology choices
- [Deployment Architecture](./deployment-architecture.md) - Infrastructure design

---

**Version History**

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | Dec 2025 | Solution Architecture | Initial high-level architecture |

