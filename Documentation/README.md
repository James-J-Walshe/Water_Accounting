# Sunwater Water Accounting Solution - Design Documentation

## Overview

This repository contains the high-level solution design documentation for Sunwater's Water Accounting platform. The solution provides a specialized water accounting system to manage water allocation, distribution, and compliance across Sunwater's 26 Water Supply Schemes in Queensland.

## Document Status

- **Version**: 1.0 (Initial Design)
- **Date**: December 2025
- **Status**: Draft - High-Level Design
- **Next Review**: Q1 2026

## Purpose

The Water Accounting solution replaces the legacy Orion system with a modern, scalable platform that:
- Manages complex water sharing rules (Announced Allocation, Continuous Share, Continuous Accounting)
- Maintains accurate water ledgers for customers, storage locations, and environmental accounts
- Integrates with CRM (Salesforce), Billing (Gentrack), and operational systems
- Ensures regulatory compliance with Resource Operations Licenses (ROL)
- Provides real-time visibility into water availability and allocations

## Documentation Structure

### 1. Business Context
- [Business Capabilities](./01-business-context/business-capabilities.md) - Core business functions and processes
- [Water Sharing Rules](./01-business-context/water-sharing-rules.md) - Detailed explanation of allocation mechanisms
- [Regulatory Framework](./01-business-context/regulatory-framework.md) - Compliance requirements and obligations
- [Use Cases](./01-business-context/use-cases.md) - Key user scenarios and workflows

### 2. Solution Architecture
- [Architecture Overview](./02-solution-architecture/architecture-overview.md) - High-level solution design
- [Solution Components](./02-solution-architecture/solution-components.md) - Detailed component descriptions
- [Integration Architecture](./02-solution-architecture/integration-architecture.md) - System integrations and data flows
- [Technology Stack](./02-solution-architecture/technology-stack.md) - Platform and technology choices
- [Deployment Architecture](./02-solution-architecture/deployment-architecture.md) - Infrastructure and hosting

### 3. Data Architecture
- [Data Model Overview](./03-data-architecture/data-model-overview.md) - Core entities and relationships
- [Ledger Structure](./03-data-architecture/ledger-structure.md) - Account hierarchy and ledger design
- [Transaction Model](./03-data-architecture/transaction-model.md) - Transaction types and processing
- [Master Data](./03-data-architecture/master-data.md) - Schemes, products, customers, infrastructure

### 4. Functional Design
- [Water Accounting Core](./04-functional-design/water-accounting-core.md) - Core accounting engine
- [Calculation Services](./04-functional-design/calculation-services.md) - Rule engines and formulas
- [Transaction Processing](./04-functional-design/transaction-processing.md) - Transaction lifecycle
- [Allocation & Balancing](./04-functional-design/allocation-balancing.md) - Automated allocation processing
- [Reporting Services](./04-functional-design/reporting-services.md) - Reporting and analytics

### 5. Integration Specifications
- [Salesforce Integration](./05-integration-specs/salesforce-integration.md) - CRM integration
- [Gentrack Integration](./05-integration-specs/gentrack-integration.md) - Billing integration
- [Operational Systems Integration](./05-integration-specs/operational-systems-integration.md) - PI, SCADA, Smart Schemes
- [SAP Integration](./05-integration-specs/sap-integration.md) - Asset and network data
- [API Specifications](./05-integration-specs/api-specifications.md) - API design standards

### 6. Non-Functional Requirements
- [Performance Requirements](./06-nfr/performance-requirements.md) - Response times, throughput
- [Security & Access Control](./06-nfr/security-access-control.md) - Authentication, authorization
- [Audit & Compliance](./06-nfr/audit-compliance.md) - Audit trails, regulatory compliance
- [Availability & Resilience](./06-nfr/availability-resilience.md) - Uptime, disaster recovery
- [Scalability](./06-nfr/scalability.md) - Growth and expansion considerations

### 7. Implementation Approach
- [Phased Delivery](./07-implementation/phased-delivery.md) - Implementation roadmap
- [Migration Strategy](./07-implementation/migration-strategy.md) - Orion to new platform migration
- [Testing Strategy](./07-implementation/testing-strategy.md) - Test approach and scenarios
- [Change Management](./07-implementation/change-management.md) - User adoption and training

### 8. Operations & Support
- [Operational Procedures](./08-operations/operational-procedures.md) - Day-to-day operations
- [Monitoring & Alerting](./08-operations/monitoring-alerting.md) - System health monitoring
- [Support Model](./08-operations/support-model.md) - Support tiers and responsibilities
- [Maintenance & Updates](./08-operations/maintenance-updates.md) - Ongoing maintenance approach

### 9. Appendices
- [Glossary](./09-appendices/glossary.md) - Terms and definitions
- [References](./09-appendices/references.md) - Source documents and standards
- [Decision Log](./09-appendices/decision-log.md) - Key architectural decisions
- [Open Issues](./09-appendices/open-issues.md) - Items requiring resolution

## How to Use This Documentation

### For Architects
Start with Section 2 (Solution Architecture) to understand the overall design approach, then dive into specific areas of interest.

### For Developers
Review Section 2 (Solution Architecture) and Section 4 (Functional Design) for implementation guidance. Section 5 provides integration specifications.

### For Business Analysts
Begin with Section 1 (Business Context) to understand capabilities and requirements, then review Section 4 for functional specifications.

### For Project Managers
Focus on Section 7 (Implementation Approach) for delivery planning and phasing.

### For Operations Teams
Review Section 8 (Operations & Support) for operational procedures and support requirements.

## Document Maintenance

This is a living design document that will evolve through:
- **Design Phase**: High-level design refinement
- **Build Phase**: Detailed specifications and technical design
- **Test Phase**: Updated based on testing outcomes
- **Deploy Phase**: As-built documentation
- **Operate Phase**: Operational refinements and enhancements

### Version Control
All documents are version controlled with change history maintained in Git. Major design decisions are captured in the Decision Log.

### Review Cycle
- **Monthly**: Technical review during design/build phases
- **Quarterly**: Governance review post-implementation
- **Ad-hoc**: As significant changes occur

## Key Stakeholders

- **Business Owner**: Sunwater Commercial Customers Manager
- **Solution Architect**: [To be assigned]
- **Enterprise Architect**: Lance Altena
- **Project Manager**: Jamie Walshe
- **Technical Lead**: [To be assigned]

## Related Projects

- **CASPr**: Customer and Stakeholder Project (Salesforce CRM, Gentrack M2C)
- **Smart Schemes**: IoT and telemetry infrastructure
- **Orion Decommissioning**: Legacy system retirement

## Contact

For questions or clarifications regarding this design documentation:
- Email: caspr-team@sunwater.com.au
- Project Site: [SharePoint link]

---

**Document Classification**: Internal Use Only  
**Last Updated**: December 2025  
**Next Review**: Q1 2026
