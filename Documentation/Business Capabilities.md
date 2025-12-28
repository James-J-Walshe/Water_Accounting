# Business Capabilities

## Document Information
- **Version**: 1.0
- **Last Updated**: December 2025
- **Status**: Draft
- **Owner**: Business Analysis Team

## Overview

This document defines the core business capabilities that the Water Accounting solution must support. These capabilities represent the fundamental business functions required to manage water allocation, distribution, and compliance across Sunwater's Water Supply Schemes.

## Business Capability Model

The Water Accounting function comprises four primary capabilities:

```
Water Accounting
├── Administer Scheme
├── Transact Water
├── Manage Scheme
└── Manage Exceptions
```

---

## 1. Administer Scheme

**Purpose**: Transform regulatory frameworks (Water Plans, ROL, Operations Manuals) into operational scheme configurations.

### 1.1 Create and Update Scheme (AS001)

**Description**: Establish or modify scheme configurations including operational systems, ROL zones, ordering parameters, and water balance rules.

**Inputs**:
- Water Plan documentation
- Resource Operations License (ROL)
- Operations Manual
- Infrastructure configuration

**Process**:
1. Define scheme boundaries and zones
2. Configure water sharing rules
3. Establish allocation title structures
4. Set operational parameters (ordering, releases, restrictions)
5. Define environmental flow requirements
6. Configure monitoring and reporting obligations

**Outputs**:
- Operational scheme configuration
- Water allocation title definitions
- Scheme rules and parameters
- Monitoring requirements

**Frequency**: Ad-hoc (scheme creation, regulatory changes)

**Business Rules**:
- Scheme configuration must comply with approved Water Plan
- ROL conditions must be enforced
- Environmental flows cannot be compromised
- All scheme changes require regulatory approval

---

### 1.2 Manage Pricing (AS002)

**Description**: Define and maintain pricing structures for water entitlements and products within each scheme.

**Inputs**:
- Queensland Competition Authority (QCA) pricing determinations
- DRDMW pricing guidelines
- Scheme product definitions
- Cost allocation models

**Process**:
1. Receive pricing determinations
2. Map prices to scheme products
3. Configure tariff structures
4. Set billing cycles and rules
5. Establish discounts and concessions
6. Version and effective-date pricing

**Outputs**:
- Product pricing catalog
- Tariff schedules
- Billing configurations

**Frequency**: Annual (with ad-hoc adjustments)

**Business Rules**:
- Non-commercial customer pricing must follow QCA determinations
- Commercial customer pricing follows negotiated agreements
- Price changes require customer notification (60-90 days)
- Historical pricing must be maintained for billing accuracy

---

### 1.3 Manage Products (AS003)

**Description**: Control the availability and characteristics of water products within each scheme based on rules, geography, infrastructure, and contracts.

**Inputs**:
- Scheme configuration
- Infrastructure capacity
- Customer contracts
- Regulatory restrictions

**Process**:
1. Define product types (High Priority, Medium Priority, priority groups)
2. Set product availability rules
3. Configure geographic restrictions
4. Define seasonal availability
5. Establish ordering and delivery rules
6. Set usage restrictions and conditions

**Outputs**:
- Product catalog by scheme
- Availability rules
- Usage conditions
- Ordering parameters

**Frequency**: Continuous (with seasonal variations)

**Business Rules**:
- Product availability must respect infrastructure capacity
- Geographic restrictions enforce water zone boundaries
- Seasonal rules reflect Water Plan allocations
- Contract terms override default product rules

---

## 2. Transact Water

**Purpose**: Enable and record all water-related transactions including ordering, consumption, trading, and transfers.

### 2.1 Manage Breach Remedy (TW001)

**Description**: Identify, manage, and resolve customer overconsumption situations.

**Inputs**:
- Customer water account balances (negative)
- Meter readings showing overconsumption
- Customer contracts and entitlements

**Process**:
1. Identify negative balance condition
2. Notify customer of breach
3. Determine remedy options:
   - Emergency temporary transfer
   - Balance adjustment with penalties
   - Supply restriction
4. Execute remedy transaction
5. Report to DRDMW if required
6. Update customer account

**Outputs**:
- Breach notifications
- Remedy transactions
- Regulatory reports
- Updated account balances

**Frequency**: As-needed (event-driven)

**Business Rules**:
- Negative balances trigger immediate breach process
- DRDMW must be notified within 5 business days
- Penalties apply per ROL conditions
- Multiple breaches may result in allocation suspension

---

### 2.2 Manage Meter Read (TW002)

**Description**: Record, validate, and process meter readings to calculate consumption and update account balances.

**Inputs**:
- Meter readings (manual or automated)
- Previous meter readings
- Meter metadata (location, type, customer)
- Validation rules

**Process**:
1. Receive meter reading
2. Validate reading:
   - Range check (vs. previous reading)
   - Rate of change validation
   - Negative flow detection
3. Calculate consumption (current - previous)
4. Determine product allocation
5. Create consumption transaction
6. Post to customer water account
7. Trigger billing process

**Outputs**:
- Validated meter readings
- Consumption transactions
- Updated account balances
- Billing triggers

**Frequency**: 
- Manual: Monthly to quarterly (scheme-dependent)
- Automated: Daily to real-time (Smart Schemes)

**Business Rules**:
- Meter readings must be sequential (no backdating)
- Failed validations require investigation before posting
- Consumption reduces available water balance
- Zero or minimal usage may indicate infrastructure issues
- Manual consumption entry allowed for non-metered accounts

---

### 2.3 Property Transfer (TW003)

**Description**: Permanently transfer water allocation titles and/or infrastructure between parties.

**Inputs**:
- Transfer application
- Current owner details
- New owner details
- Allocation title details
- Infrastructure details (if applicable)
- Transfer fees

**Process**:
1. Receive and validate transfer application
2. Verify current ownership
3. Check transfer eligibility and restrictions
4. Calculate transfer volumes and fees
5. Execute transfer:
   - Update allocation title ownership
   - Transfer available water balance (optional)
   - Reassign infrastructure
6. Update customer accounts
7. Generate transfer documentation
8. Notify DRDMW (if required)

**Outputs**:
- Updated ownership records
- Transfer transactions
- Legal documentation
- Updated accounts
- Regulatory notifications

**Frequency**: Ad-hoc (customer-initiated)

**Business Rules**:
- Transfers must comply with Water Plan trading rules
- Some zones/products may restrict transfers
- Permanent transfers require DRDMW notification
- Infrastructure transfer may have separate approvals
- Transfer fees apply per tariff schedule

---

### 2.4 Temporary Transfer (TW004)

**Description**: Trade available water between customers for the remainder of the current water year.

**Inputs**:
- Transfer request (seller/buyer)
- Available water balances
- Scheme trading rules
- Transfer price (if commercial)

**Process**:
1. Validate transfer eligibility:
   - Seller has sufficient available water
   - Both parties in same scheme/zone
   - Transfer allowed under scheme rules
2. Calculate transfer volume
3. Create transfer transactions:
   - Debit seller available water
   - Credit buyer available water
4. Update both customer accounts
5. Record transfer details (price, parties, volume)
6. Generate confirmation

**Outputs**:
- Transfer transactions (debit/credit pair)
- Updated account balances
- Transfer confirmation
- Trading record

**Frequency**: Continuous during water year

**Business Rules**:
- Transfers expire at end of water year
- Only available water can be transferred (not allocations)
- Allocation ownership unchanged
- Maximum transfer volumes may apply
- Some schemes restrict temporary transfers
- Environmental water cannot be transferred

---

### 2.5 Water Orders (TW005)

**Description**: Process customer requests for water delivery to their infrastructure.

**Inputs**:
- Customer order request
- Delivery location and volume
- Requested delivery date/period
- Available water balance
- Infrastructure capacity

**Process**:
1. Receive and validate order:
   - Customer has sufficient available water
   - Infrastructure exists and operational
   - Delivery feasible (scheme rules, capacity)
2. Reserve water (update pending balance)
3. Create order record
4. Notify operations team
5. Schedule release/delivery
6. Update estimated consumption
7. Track order fulfillment

**Outputs**:
- Water order
- Pending balance adjustment
- Operations work order
- Delivery schedule

**Frequency**: Continuous (demand-driven)

**Business Rules**:
- Order volume cannot exceed available water
- Minimum notice periods apply (scheme-specific)
- Some schemes require orders, others allow direct usage
- Infrastructure must be operational
- Orders may be modified/cancelled (within notice period)
- Unfulfilled orders release reserved water

---

## 3. Manage Scheme

**Purpose**: Manage scheme-wide processes including annual allocations, carryover, continuous sharing, and year-end procedures.

### 3.1 Apply Announced Allocation (MS001)

**Description**: Calculate and distribute water allocation as a percentage of entitlement based on storage levels and scheme rules.

**Inputs**:
- Storage levels (all scheme storages)
- Customer allocation titles (HP, MP)
- Total scheme allocations (HPA, MPA)
- Announced Allocation percentage decision
- Water year start date

**Process**:
1. Monitor storage levels and inflows
2. Determine allocation percentage (0-100%)
3. Calculate individual customer allocations:
   - `Customer Allocation = Title Volume × AA%`
4. Create allocation transactions for all customers
5. Post to customer available water accounts
6. Notify customers
7. Notify DRDMW
8. Publish announcement

**Outputs**:
- Announced Allocation percentage (HP and MP)
- Individual customer allocation transactions
- Customer notifications
- Public announcement
- DRDMW report

**Frequency**: 
- Initial: Start of water year (typically 1 July)
- Updates: As storage allows (may reach 100% or reduce)

**Business Rules**:
- HP allocation typically 100% (unless severe drought)
- MP allocation varies based on storage (0-100%)
- Cannot reduce previously announced allocation (except specific drought provisions)
- Different formulas apply to different schemes
- Nogoa Mackenzie example: Complex formula based on storage zones

**Example (Nogoa Mackenzie - simplified)**:
```
If Fairbairn Dam > 250,000 ML: HP=100%, MP=100%
If Fairbairn Dam 200,000-250,000 ML: HP=100%, MP=70-99%
If Fairbairn Dam < 200,000 ML: HP=100%, MP<70%
```

---

### 3.2 Carryover (MS002)

**Description**: Allow eligible customers to carry unused water from ending water year into the next water year.

**Inputs**:
- End of water year balances
- Carryover eligibility rules (by scheme/product)
- Maximum carryover limits
- Carryover loss factors

**Process**:
1. Calculate unused water at year end
2. Determine eligible carryover amount:
   - Apply maximum carryover cap
   - Apply loss factors (if any)
3. Create carryover transactions:
   - Transfer from current year
   - Credit to next year carryover account
4. Track carryover separately from allocation
5. Apply carryover degradation (if applicable):
   - Monthly losses during next year
   - Cancellation triggers (low storage)

**Outputs**:
- Carryover transactions
- Next year account balances
- Carryover tracking records

**Frequency**: Annual (end of water year)

**Business Rules**:
- Not all schemes allow carryover
- Maximum carryover limits apply (typically 50-100% of entitlement)
- Carryover may degrade during year (evaporation losses)
- Carryover used before new allocation
- Low storage triggers may cancel carryover

**Example (Nogoa Mackenzie)**:
- Carryover allowed for MP allocations
- Current carryover: 134,207 ML (as of 1 May 2025)
- No carryover diversions in current period

---

### 3.3 Continuous Accounting Distribution (MS003)

**Description**: Distribute additional water to medium priority customers as reserves become available throughout the year.

**Inputs**:
- DRDMW determination of available reserves
- Customer MP allocation titles
- Current account balances
- Distribution rules

**Process**:
1. Receive notification of available reserves
2. Calculate distribution based on:
   - Customer share of total MP allocations
   - Customer storage limits
   - Priority rankings
3. Create distribution transactions
4. Post to customer available water accounts
5. Update scheme reserve balance
6. Notify customers

**Outputs**:
- Distribution transactions
- Updated customer balances
- Updated reserve balance
- Customer notifications

**Frequency**: 
- Monthly (scheduled distributions)
- Ad-hoc (following significant inflows)

**Business Rules**:
- Only applies to schemes with Continuous Accounting
- Distribution proportional to allocation holding
- Cannot exceed customer storage limits
- Reserves distributed in priority order
- Distributions can be positive or negative (adjustments)

---

### 3.4 Continuous Share Daily Processing (MS004)

**Description**: Calculate daily water availability and losses for customers in Continuous Share schemes based on storage levels and individual share sizes.

**Inputs**:
- Daily storage levels
- Customer share sizes (from allocation titles)
- Current account balances
- Evaporation and seepage rates
- Release volumes

**Process**:
1. Retrieve end-of-day storage level
2. For each customer:
   - Calculate share of storage: `Storage × (Customer Share / Total Shares)`
   - Calculate expected balance
   - Determine variance from actual balance
   - Calculate losses (evaporation, seepage): `Loss = Balance × Loss Rate`
3. Create daily transactions:
   - Storage level adjustment
   - Loss deductions
4. Post to customer accounts
5. Generate daily balancing report

**Outputs**:
- Daily adjustment transactions
- Loss transactions
- Updated customer balances
- Daily reconciliation report

**Frequency**: Daily (automated)

**Business Rules**:
- Share size fixed (from allocation title)
- Losses calculated on actual balance
- Storage changes allocated proportionally
- Releases debited to ordering customers first
- Daily processing must complete before next day

---

### 3.5 Continuous Share Reconciliation (MS005)

**Description**: Reconcile calculated customer balances against actual storage levels monthly or as required.

**Inputs**:
- Audited storage level at reconciliation date
- Sum of all customer account balances
- Daily processing history since last reconciliation
- Unaccounted gains/losses

**Process**:
1. Audit actual storage level
2. Sum all customer balances
3. Calculate variance: `Variance = Actual Storage - Sum(Customer Balances)`
4. Investigate significant variances
5. Distribute variance proportionally:
   - `Customer Adjustment = Variance × (Customer Balance / Total Balances)`
6. Create reconciliation transactions
7. Post to customer accounts
8. Document reconciliation

**Outputs**:
- Reconciliation transactions
- Variance analysis report
- Updated customer balances
- Reconciliation documentation

**Frequency**: 
- Monthly (standard)
- Ad-hoc (as required for accuracy)

**Business Rules**:
- All transactions must be posted before reconciliation
- Variance threshold triggers investigation
- Large variances require management approval
- Reconciliation overrides daily processing balances
- Documentation required for audit trail

---

### 3.6 Continuous Share Reversal (MS006)

**Description**: Reverse daily processing or reconciliation for a specific date or period.

**Inputs**:
- Date(s) or period to reverse
- Original transactions
- Reason for reversal
- Authorization

**Process**:
1. Identify transactions to reverse
2. Validate reversal eligibility:
   - No dependent transactions posted
   - Within reversal window
   - Authorized appropriately
3. Create offsetting reversal transactions
4. Post reversals
5. Update customer balances
6. Document reversal reason
7. Re-run processing if required

**Outputs**:
- Reversal transactions
- Updated balances (pre-reversal state)
- Reversal documentation
- Audit trail

**Frequency**: Exception-based

**Business Rules**:
- Reversals require management authorization
- Cannot reverse if subsequent invoices posted
- Must document reason for audit
- May trigger re-processing of period
- Customer notification required if balances affected

---

### 3.7 End of Water Year Audit (MS007)

**Description**: Comprehensive audit and reconciliation of scheme and customer accounts before finalizing the water year.

**Inputs**:
- All scheme transactions for water year
- Final storage levels
- Customer account balances
- Infrastructure status
- Outstanding orders
- Billing status

**Process**:
1. Verify all transactions posted:
   - All meter reads received and processed
   - All orders fulfilled or cancelled
   - All transfers completed
   - All payments allocated
2. Reconcile scheme water balance:
   - Beginning storage + Inflows - Releases - Losses = Ending Storage
   - Verify against actual storage
3. Reconcile customer accounts:
   - Beginning balance + Allocations + Transactions = Ending Balance
   - Verify no negative balances (or breach processes complete)
4. Verify environmental flows met
5. Check ROL compliance
6. Generate audit report
7. Obtain management sign-off

**Outputs**:
- Water year audit report
- Variance analysis
- Compliance confirmation
- Management approval
- Readiness for rollover

**Frequency**: Annual (end of water year)

**Business Rules**:
- All transactions must be finalized
- All billing must be complete
- Variances must be investigated and resolved
- Environmental compliance must be verified
- Rollover cannot proceed without clean audit

---

### 3.8 End of Water Year Rollover (MS008)

**Description**: Finalize the ending water year and initialize the new water year, including balance resets and carryover processing.

**Inputs**:
- Completed audit approval
- Final customer balances
- Carryover eligibility and calculations
- New year scheme configuration
- New year allocations (if predetermined)

**Process**:
1. Verify audit complete and approved
2. Process carryover (see MS002)
3. Archive ending water year:
   - Close water year accounting period
   - Archive transactions
   - Generate final reports
4. Initialize new water year:
   - Create new accounting period
   - Reset annual usage balances
   - Apply carryover balances
   - Reset environmental flow counters
5. Apply initial allocation (if applicable)
6. Notify customers of new year balances
7. Generate rollover report

**Outputs**:
- Archived water year records
- New water year initialization
- Customer opening balances
- Rollover report
- Customer notifications

**Frequency**: Annual (typically 30 June - 1 July)

**Business Rules**:
- Rollover date is scheme-specific:
  - Most schemes: 30 June → 1 July
  - Eton: 31 March → 1 April
  - Dawson: 30 September → 1 October
- Annual allocations reset to zero
- Carryover calculated and transferred
- Cannot reverse after rollover complete
- New year must be clean (no outstanding issues)

---

### 3.9 Manage Cut-off (MS009)

**Description**: Apply restrictions to water availability when storage levels trigger cut-off thresholds.

**Inputs**:
- Current storage level
- Cut-off trigger thresholds (from ROL)
- Affected products and customers
- Restriction rules

**Process**:
1. Monitor storage levels against thresholds
2. When threshold crossed:
   - Determine affected products/customers
   - Calculate restricted availability
   - Notify operations team
   - Notify affected customers
   - Apply restrictions in system
   - Update ordering rules
3. Monitor for threshold recovery
4. When restriction lifted:
   - Notify customers
   - Remove restrictions
   - Restore normal operations

**Outputs**:
- Cut-off activation/deactivation
- Customer notifications
- Updated availability calculations
- Operations alerts
- Regulatory notifications

**Frequency**: Continuous monitoring, event-driven activation

**Business Rules**:
- Cut-off thresholds defined in ROL
- Typically apply to lower priority products first
- May completely stop allocations for some products
- Environmental flows protected during cut-off
- DRDMW notification required
- Customers must be notified before restriction

**Example (Nogoa Mackenzie - Critical Water Sharing)**:
- **Stage 1**: Fairbairn < 250,000 ML, MP AA < 30%
- **Stage 2**: HP AA < MP AA in Zones Mackenzie B-D
- **Stage 3**: Fairbairn < 12,300 ML

---

## 4. Manage Exceptions

**Purpose**: Handle unusual circumstances, corrections, and service disruptions.

### 4.1 Reverse Announced Allocation (ME001)

**Description**: Reverse an incorrectly applied announced allocation and restore prior balances.

**Inputs**:
- Announced allocation to reverse
- Original allocation transactions
- Customer balances post-allocation
- Reversal reason and authorization

**Process**:
1. Validate reversal request:
   - Identify error or issue
   - Obtain management authorization
   - Verify no dependent transactions (orders, consumption)
2. Create reversal transactions:
   - Offset original allocation transactions
3. Post reversals to customer accounts
4. Restore prior balances
5. Notify customers
6. Notify DRDMW
7. Document reversal
8. Re-apply correct allocation if needed

**Outputs**:
- Reversal transactions
- Restored customer balances
- Customer notifications
- DRDMW notification
- Reversal documentation
- Corrected allocation (if applicable)

**Frequency**: Rare exception

**Business Rules**:
- Requires executive authorization
- Cannot reverse if significant consumption occurred
- May require customer agreement
- DRDMW must be notified immediately
- Public announcement may be required
- Full audit trail mandatory

---

### 4.2 Service Interruptions (ME002)

**Description**: Manage and communicate infrastructure maintenance or failures that impact customer water access.

**Inputs**:
- Planned or unplanned outage details
- Affected infrastructure
- Affected customers
- Estimated duration
- Alternative supply options

**Process**:
1. Identify service interruption
2. Determine impact:
   - Affected customers
   - Affected orders
   - Duration estimate
3. Notify affected customers:
   - Interruption notice
   - Expected duration
   - Alternative arrangements
4. Update system:
   - Block new orders for affected infrastructure
   - Suspend/modify existing orders
5. Monitor restoration
6. When resolved:
   - Notify customers
   - Re-enable ordering
   - Process any make-good arrangements

**Outputs**:
- Interruption notifications
- Updated order status
- Infrastructure availability status
- Restoration notifications
- Service level reports

**Frequency**: As required

**Business Rules**:
- Planned outages require minimum notice (typically 7-14 days)
- Emergency outages notified ASAP
- Cannot charge for water not delivered
- May trigger service credits
- Critical customers may require alternative supply
- DRDMW notification if environmental flows affected

---

## Business Capability Dependencies

```
┌─────────────────────────────────────────────────────────┐
│                    Administer Scheme                     │
│  (Foundational - must exist before other capabilities)   │
└────────────────────┬────────────────────────────────────┘
                     │
         ┌───────────┼───────────┐
         │           │           │
         ▼           ▼           ▼
┌────────────┐ ┌────────────┐ ┌──────────────┐
│  Transact  │ │   Manage   │ │    Manage    │
│   Water    │ │   Scheme   │ │  Exceptions  │
└────────────┘ └────────────┘ └──────────────┘
         │           │                │
         └───────────┴────────────────┘
                     │
                     ▼
          ┌──────────────────┐
          │  Reporting &     │
          │  Compliance      │
          └──────────────────┘
```

## Capability Maturity Assessment

| Capability | Current State (Orion) | Target State |
|------------|----------------------|--------------|
| Administer Scheme | Manual, fragmented | Automated configuration |
| Transact Water | Semi-automated | Fully automated with validation |
| Manage Scheme | Spreadsheet-based | System-driven with oversight |
| Manage Exceptions | Manual intervention | Workflow-driven with audit |

---

## Success Criteria

The Water Accounting solution successfully delivers these capabilities when:

1. **Accuracy**: All allocations and transactions calculated correctly per scheme rules
2. **Timeliness**: Daily/monthly processing completes on schedule
3. **Compliance**: 100% ROL compliance with complete audit trails
4. **Transparency**: Customers can view accurate balances in real-time
5. **Efficiency**: Minimal manual intervention required for standard processes
6. **Resilience**: System handles exceptions without data corruption
7. **Scalability**: Supports all 26 schemes with varying complexity

---

## Related Documents

- [Water Sharing Rules](./water-sharing-rules.md) - Detailed allocation formulas
- [Use Cases](./use-cases.md) - Specific user scenarios
- [Regulatory Framework](./regulatory-framework.md) - Compliance requirements

---

**Version History**

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | Dec 2025 | Solution Team | Initial high-level capability definition |

