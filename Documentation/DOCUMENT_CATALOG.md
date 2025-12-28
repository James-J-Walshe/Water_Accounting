# Document Catalog and Status

## Overview

This catalog tracks all design documents for the Water Accounting Solution. Use this to navigate the documentation library and track document completion status.

**Last Updated**: December 2025

---

## Document Status Legend

- ✅ **Complete**: Reviewed and approved
- 🟡 **Draft**: Initial version complete, pending review
- 🔴 **Planned**: Not yet started
- 📝 **In Progress**: Currently being written

---

## 01 - Business Context

| Document | Status | Priority | Owner | Last Updated |
|----------|--------|----------|-------|--------------|
| [Business Capabilities](./01-business-context/business-capabilities.md) | 🟡 Draft | High | Business Analysis | Dec 2025 |
| [Water Sharing Rules](./01-business-context/water-sharing-rules.md) | 🔴 Planned | High | Business Analysis | - |
| [Regulatory Framework](./01-business-context/regulatory-framework.md) | 🔴 Planned | High | Legal/Compliance | - |
| [Use Cases](./01-business-context/use-cases.md) | 🔴 Planned | Medium | Business Analysis | - |

### Notes
- Business Capabilities provides comprehensive process definitions
- Water Sharing Rules needs detailed formula documentation from ROL/Operations Manuals
- Regulatory Framework should reference all applicable legislation

---

## 02 - Solution Architecture

| Document | Status | Priority | Owner | Last Updated |
|----------|--------|----------|-------|--------------|
| [Architecture Overview](./02-solution-architecture/architecture-overview.md) | 🟡 Draft | Critical | Solution Architect | Dec 2025 |
| [Solution Components](./02-solution-architecture/solution-components.md) | 🔴 Planned | High | Solution Architect | - |
| [Integration Architecture](./02-solution-architecture/integration-architecture.md) | 🔴 Planned | High | Integration Architect | - |
| [Technology Stack](./02-solution-architecture/technology-stack.md) | 🔴 Planned | High | Technical Lead | - |
| [Deployment Architecture](./02-solution-architecture/deployment-architecture.md) | 🔴 Planned | Medium | DevOps Lead | - |

### Notes
- Architecture Overview establishes high-level design principles
- Component detail needed for development team
- Integration specs critical for Phase 1 delivery

---

## 03 - Data Architecture

| Document | Status | Priority | Owner | Last Updated |
|----------|--------|----------|-------|--------------|
| [Data Model Overview](./03-data-architecture/data-model-overview.md) | 🟡 Draft | Critical | Data Architect | Dec 2025 |
| [Ledger Structure](./03-data-architecture/ledger-structure.md) | 🔴 Planned | Critical | Data Architect | - |
| [Transaction Model](./03-data-architecture/transaction-model.md) | 🔴 Planned | Critical | Data Architect | - |
| [Master Data](./03-data-architecture/master-data.md) | 🔴 Planned | High | Data Architect | - |

### Notes
- Data Model provides entity overview
- Ledger Structure needs detailed account hierarchy and chart of accounts
- Transaction Model should document all transaction types with examples

---

## 04 - Functional Design

| Document | Status | Priority | Owner | Last Updated |
|----------|--------|----------|-------|--------------|
| [Water Accounting Core](./04-functional-design/water-accounting-core.md) | 🔴 Planned | High | Lead Developer | - |
| [Calculation Services](./04-functional-design/calculation-services.md) | 🔴 Planned | High | Lead Developer | - |
| [Transaction Processing](./04-functional-design/transaction-processing.md) | 🔴 Planned | High | Lead Developer | - |
| [Allocation & Balancing](./04-functional-design/allocation-balancing.md) | 🔴 Planned | High | Lead Developer | - |
| [Reporting Services](./04-functional-design/reporting-services.md) | 🔴 Planned | Medium | BI Developer | - |

### Notes
- Functional specs needed before development starts (Phase 1)
- Calculation Services most complex - needs early focus
- Consider breaking Calculation Services by water sharing rule type

---

## 05 - Integration Specifications

| Document | Status | Priority | Owner | Last Updated |
|----------|--------|----------|-------|--------------|
| [Salesforce Integration](./05-integration-specs/salesforce-integration.md) | 🔴 Planned | Critical | Integration Lead | - |
| [Gentrack Integration](./05-integration-specs/gentrack-integration.md) | 🔴 Planned | High | Integration Lead | - |
| [Operational Systems Integration](./05-integration-specs/operational-systems-integration.md) | 🔴 Planned | High | Integration Lead | - |
| [SAP Integration](./05-integration-specs/sap-integration.md) | 🔴 Planned | Medium | Integration Lead | - |
| [API Specifications](./05-integration-specs/api-specifications.md) | 🔴 Planned | High | API Developer | - |

### Notes
- Salesforce integration critical for Phase 1
- API specs should follow OpenAPI 3.0 standard
- Each integration needs: data mapping, frequency, error handling, monitoring

---

## 06 - Non-Functional Requirements

| Document | Status | Priority | Owner | Last Updated |
|----------|--------|----------|-------|--------------|
| [Performance Requirements](./06-nfr/performance-requirements.md) | 🔴 Planned | High | Technical Lead | - |
| [Security & Access Control](./06-nfr/security-access-control.md) | 🔴 Planned | Critical | Security Architect | - |
| [Audit & Compliance](./06-nfr/audit-compliance.md) | 🔴 Planned | Critical | Compliance Lead | - |
| [Availability & Resilience](./06-nfr/availability-resilience.md) | 🔴 Planned | High | DevOps Lead | - |
| [Scalability](./06-nfr/scalability.md) | 🔴 Planned | Medium | Technical Lead | - |

### Notes
- Performance requirements drive architecture decisions
- Security review needed before Phase 1 deployment
- Audit requirements must be validated against ROL obligations

---

## 07 - Implementation Approach

| Document | Status | Priority | Owner | Last Updated |
|----------|--------|----------|-------|--------------|
| [Phased Delivery](./07-implementation/phased-delivery.md) | 🟡 Draft | Critical | Project Manager | Dec 2025 |
| [Migration Strategy](./07-implementation/migration-strategy.md) | 🔴 Planned | Critical | Data Migration Lead | - |
| [Testing Strategy](./07-implementation/testing-strategy.md) | 🔴 Planned | High | QA Lead | - |
| [Change Management](./07-implementation/change-management.md) | 🔴 Planned | High | Change Manager | - |

### Notes
- Phased Delivery provides high-level roadmap
- Migration Strategy critical - needs Orion data analysis
- Testing Strategy should cover: unit, integration, UAT, performance, security

---

## 08 - Operations & Support

| Document | Status | Priority | Owner | Last Updated |
|----------|--------|----------|-------|--------------|
| [Operational Procedures](./08-operations/operational-procedures.md) | 🔴 Planned | High | Operations Manager | - |
| [Monitoring & Alerting](./08-operations/monitoring-alerting.md) | 🔴 Planned | High | DevOps Lead | - |
| [Support Model](./08-operations/support-model.md) | 🔴 Planned | Medium | Service Delivery | - |
| [Maintenance & Updates](./08-operations/maintenance-updates.md) | 🔴 Planned | Medium | DevOps Lead | - |

### Notes
- Operational docs needed before Phase 2 go-live
- Define support tiers and escalation paths
- Include runbooks for common scenarios

---

## 09 - Appendices

| Document | Status | Priority | Owner | Last Updated |
|----------|--------|----------|-------|--------------|
| [Glossary](./09-appendices/glossary.md) | 🔴 Planned | Medium | Technical Writer | - |
| [References](./09-appendices/references.md) | 🔴 Planned | Low | Technical Writer | - |
| [Decision Log](./09-appendices/decision-log.md) | 🔴 Planned | High | Solution Architect | - |
| [Open Issues](./09-appendices/open-issues.md) | 🔴 Planned | Medium | Project Manager | - |

### Notes
- Decision Log should track all major architectural decisions
- Open Issues to track items needing resolution
- Glossary critical for stakeholder understanding

---

## Document Dependencies

```
Business Context
    ↓
Solution Architecture ← Data Architecture
    ↓                       ↓
Functional Design    ← Integration Specs
    ↓                       ↓
Implementation Approach     NFRs
    ↓                       ↓
Operations & Support ← Security & Compliance
```

### Critical Path Documents

Priority order for completion:

1. **Foundation** (Phase 0 - Months 1-3)
   - Business Capabilities ✅
   - Architecture Overview ✅
   - Data Model Overview ✅
   - Phased Delivery ✅
   - Decision Log
   - Water Sharing Rules

2. **Design** (Phase 0 completion - Month 3)
   - Solution Components
   - Ledger Structure
   - Transaction Model
   - Integration Architecture
   - Security & Access Control
   - Performance Requirements

3. **Build Prep** (Start of Phase 1 - Month 4)
   - Functional Design (all)
   - API Specifications
   - Salesforce Integration
   - Testing Strategy
   - Migration Strategy

4. **Pre-Production** (Before Phase 2 - Month 9)
   - Operational Procedures
   - Monitoring & Alerting
   - Support Model
   - Change Management

---

## Review Schedule

### Monthly Architecture Review
- **First Tuesday of Month**
- Review recent design decisions
- Validate alignment with principles
- Address open issues

### Weekly Documentation Standup
- **Every Friday 10am**
- Status of in-progress documents
- Blockers and dependencies
- Next week priorities

### Quarterly Governance Review
- **End of each quarter**
- Overall documentation completeness
- Alignment with project phases
- Quality assessment

---

## Document Templates

Standard templates available:

- **Architecture Document Template**: [Link to template]
- **Design Specification Template**: [Link to template]
- **Integration Spec Template**: [Link to template]
- **Test Plan Template**: [Link to template]
- **Operational Procedure Template**: [Link to template]

---

## Quality Standards

All documents must meet:

- [ ] Clear purpose and scope defined
- [ ] Target audience identified
- [ ] Version control maintained
- [ ] Review and approval documented
- [ ] Related documents linked
- [ ] Diagrams use standard notation
- [ ] Technical terms defined or linked to glossary
- [ ] Examples provided where appropriate
- [ ] Change history maintained

---

## Contribution Guidelines

### Creating New Documents

1. Use appropriate template
2. Follow naming convention: `lowercase-with-hyphens.md`
3. Add to relevant section folder
4. Update this catalog
5. Create pull request for review

### Updating Existing Documents

1. Update version number
2. Add entry to version history table
3. Update "Last Updated" date
4. Note changes in commit message
5. Update status in this catalog if moving from Draft to Complete

### Review Process

1. Author completes draft
2. Peer review (technical accuracy)
3. Stakeholder review (business alignment)
4. Approval by document owner
5. Status updated to Complete

---

## Document Metrics

### Completion Status

| Section | Total Docs | Complete | Draft | Planned | % Complete |
|---------|-----------|----------|-------|---------|-----------|
| 01 - Business Context | 4 | 0 | 1 | 3 | 25% |
| 02 - Solution Architecture | 5 | 0 | 1 | 4 | 20% |
| 03 - Data Architecture | 4 | 0 | 1 | 3 | 25% |
| 04 - Functional Design | 5 | 0 | 0 | 5 | 0% |
| 05 - Integration Specs | 5 | 0 | 0 | 5 | 0% |
| 06 - NFR | 5 | 0 | 0 | 5 | 0% |
| 07 - Implementation | 4 | 0 | 1 | 3 | 25% |
| 08 - Operations | 4 | 0 | 0 | 4 | 0% |
| 09 - Appendices | 4 | 0 | 0 | 4 | 0% |
| **TOTAL** | **40** | **0** | **4** | **36** | **10%** |

### Target Milestones

| Milestone | Target Date | Target Completion |
|-----------|------------|-------------------|
| Phase 0 Foundation | End Month 3 | 50% |
| Phase 1 Build Start | Month 4 | 70% |
| Phase 2 Go-Live | Month 12 | 90% |
| Project Close | Month 24 | 100% |

---

## Contact Information

### Document Owners

| Role | Name | Email | Responsible For |
|------|------|-------|----------------|
| Solution Architect | [TBD] | [email] | Architecture, decisions |
| Data Architect | [TBD] | [email] | Data model, integration |
| Business Analyst | [TBD] | [email] | Business context, requirements |
| Project Manager | Jamie Walshe | [email] | Implementation, coordination |
| Technical Writer | [TBD] | [email] | Documentation quality, templates |

### Documentation Governance

**Steering Committee**: Reviews major design decisions monthly
**Technical Review Board**: Reviews technical specifications weekly
**Documentation Lead**: [TBD] - Overall documentation coordination

---

**Last Updated**: December 2025  
**Maintained By**: Project Documentation Team  
**Next Review**: January 2026

