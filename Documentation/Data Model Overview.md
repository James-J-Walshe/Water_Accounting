# Data Model Overview

## Document Information
- **Version**: 1.0
- **Last Updated**: December 2025
- **Status**: Draft
- **Owner**: Data Architecture Team

## Overview

This document describes the high-level data model for the Water Accounting solution. The model is designed to support complex water sharing rules, maintain accurate ledger balances, and provide comprehensive audit trails while integrating with surrounding systems.

## Data Architecture Principles

### 1. Ledger-Based Design
Water accounting fundamentally follows double-entry accounting principles:
- Every transaction has equal and opposite entries
- Total system water always balances
- Accounts organized hierarchically
- Historical integrity preserved

### 2. Temporal Design
All data includes temporal attributes:
- Effective dates for configuration changes
- Transaction timestamps for audit
- Point-in-time balance reconstruction
- Historical reporting capability

### 3. Audit and Compliance
Complete traceability of all data:
- Who created/modified (user tracking)
- When changed (timestamp)
- What changed (before/after values)
- Why changed (reason codes)

### 4. Referential Integrity
Strong data relationships:
- Foreign key constraints
- Cascade rules defined
- No orphaned records
- Data quality validation

---

## Core Domain Model

### Entity Relationship Overview

```
┌──────────────┐
│    Scheme    │
└──────┬───────┘
       │ 1:N
       │
       ├─────────────────────────────────┐
       │                                 │
       ▼                                 ▼
┌────────────────┐              ┌──────────────┐
│  Water         │              │   Product    │
│  Allocation    │              │              │
│  Title         │              └──────┬───────┘
└──────┬─────────┘                     │
       │ 1:N                           │ M:N
       │                               │
       ▼                               ▼
┌────────────────┐              ┌──────────────┐
│  Water         │──────────────│  Customer    │
│  Account       │   1:1        │              │
└──────┬─────────┘              └──────────────┘
       │ 1:N
       │
       ▼
┌────────────────┐
│  Transaction   │
│                │
└────────────────┘
       │ 1:N
       ▼
┌────────────────┐
│  Ledger        │
│  Entry         │
└────────────────┘
```

---

## Core Entities

### 1. Scheme

**Purpose**: Represents a Water Supply Scheme with its configuration and rules

**Key Attributes**:
```
Scheme
├── SchemeID (PK)
├── SchemeName
├── SchemeCode
├── WaterPlanReference
├── ROLReference
├── OperationsManualReference
├── WaterSharingRule (Announced/Continuous/Share)
├── WaterYearStart (e.g., 1-July, 1-April, 1-October)
├── WaterYearEnd
├── Status (Active/Suspended/Closed)
├── EffectiveFrom
├── EffectiveTo
└── Attributes (JSON for scheme-specific config)
```

**Relationships**:
- 1:N with WaterAllocationTitles
- 1:N with Products
- 1:N with StorageLocations
- 1:N with SchemeRules
- 1:N with SchemeZones

**Business Rules**:
- Scheme cannot be deleted if active allocations exist
- Water year dates must not overlap
- ROL reference required for regulatory schemes

---

### 2. Water Allocation Title

**Purpose**: Represents a legal entitlement to water within a scheme

**Key Attributes**:
```
WaterAllocationTitle
├── AllocationTitleID (PK)
├── SchemeID (FK)
├── AllocationNumber (unique within scheme)
├── Priority (High/Medium/Low)
├── VolumeML (entitlement volume)
├── ProductType
├── Zone (geographic zone within scheme)
├── OwnerCustomerID (FK)
├── Status (Active/Suspended/Cancelled)
├── IssueDate
├── ExpiryDate (if applicable)
└── Attributes (JSON for title-specific properties)
```

**Relationships**:
- N:1 with Scheme
- 1:1 with WaterAccount
- 1:N with PropertyTransfers
- N:1 with Customer (owner)

**Business Rules**:
- Allocation number unique within scheme
- Volume cannot be changed without authority
- Priority determines allocation order
- Active allocations must have valid owner

---

### 3. Water Account

**Purpose**: Ledger account structure for tracking water balances

**Key Attributes**:
```
WaterAccount
├── WaterAccountID (PK)
├── AccountNumber (unique)
├── AllocationTitleID (FK - if customer account)
├── AccountType (Customer/Storage/Environmental/Reserve)
├── SchemeID (FK)
├── Status (Active/Inactive/Closed)
├── OpenedDate
├── ClosedDate
└── Attributes
```

**Sub-Ledger Accounts** (1:N):
```
SubLedgerAccount
├── SubLedgerID (PK)
├── WaterAccountID (FK)
├── LedgerType (Allocation/Available/Pending/Carryover/Usage)
├── ProductType (if applicable)
├── Status
└── Attributes
```

**Current Balances** (calculated from transactions):
```
AccountBalance (View/Materialized)
├── WaterAccountID
├── SubLedgerID
├── BalanceType (Actual/Pending/Forecast)
├── BalanceML
├── AsOfDate
└── LastTransactionID
```

**Relationships**:
- 1:1 with WaterAllocationTitle (for customer accounts)
- 1:N with SubLedgerAccounts
- 1:N with Transactions (as debit or credit account)
- N:1 with Scheme

**Business Rules**:
- Account number globally unique
- Customer accounts linked to allocation title
- Storage/Environmental accounts not customer-linked
- Balance calculated from transaction history
- Negative balances trigger breach process

---

### 4. Transaction

**Purpose**: Records all water movements and adjustments

**Key Attributes**:
```
Transaction
├── TransactionID (PK)
├── TransactionNumber (unique)
├── TransactionType (Allocation/Transfer/Order/Consumption/Adjustment/etc.)
├── TransactionDate
├── EffectiveDate
├── Status (Pending/Posted/Cancelled/Reversed)
├── Description
├── SourceSystemID (where transaction originated)
├── SourceTransactionRef
├── ReversalOfTransactionID (FK - if reversal)
├── ReasonCode
├── CreatedBy
├── CreatedDateTime
├── PostedBy
├── PostedDateTime
└── Attributes (JSON for transaction-specific data)
```

**Transaction Details** (Line Items):
```
TransactionLine
├── TransactionLineID (PK)
├── TransactionID (FK)
├── LineNumber
├── DebitAccountID (FK to WaterAccount)
├── CreditAccountID (FK to WaterAccount)
├── VolumeML
├── ProductType
├── UnitPrice (if applicable)
├── Amount (if financial)
└── Attributes
```

**Relationships**:
- 1:N with TransactionLines (double-entry postings)
- 1:N with LedgerEntries (posted transactions)
- N:1 with WaterOrder (if order-related)
- N:1 with Transfer (if transfer-related)
- N:1 with MeterRead (if consumption-related)

**Business Rules**:
- Every transaction must balance (sum of debits = sum of credits)
- Posted transactions cannot be modified (immutable)
- Reversals create new offsetting transactions
- Effective date determines accounting period
- Transaction date records when transaction created

---

### 5. Ledger Entry

**Purpose**: Individual postings to water account ledgers (immutable audit trail)

**Key Attributes**:
```
LedgerEntry
├── LedgerEntryID (PK)
├── TransactionID (FK)
├── TransactionLineID (FK)
├── WaterAccountID (FK)
├── SubLedgerID (FK)
├── EntryType (Debit/Credit)
├── VolumeML
├── EffectiveDate
├── PostedDate
├── RunningBalanceML (denormalized for performance)
└── Attributes
```

**Relationships**:
- N:1 with Transaction
- N:1 with WaterAccount
- N:1 with SubLedgerAccount

**Business Rules**:
- Ledger entries are immutable (insert only)
- Running balance calculated on insert
- Effective date determines when balance affected
- Posted date records when entry made

---

### 6. Product

**Purpose**: Defines types of water available within schemes

**Key Attributes**:
```
Product
├── ProductID (PK)
├── ProductCode (unique)
├── ProductName
├── Description
├── ProductCategory (Allocation/Supplementary/Environmental)
├── Priority (High/Medium/Low)
├── CarryoverEligible (Y/N)
├── TradingEligible (Y/N)
├── Status (Active/Inactive)
├── EffectiveFrom
├── EffectiveTo
└── Attributes
```

**Scheme-Product Assignment**:
```
SchemeProduct
├── SchemeProductID (PK)
├── SchemeID (FK)
├── ProductID (FK)
├── AvailabilityRules (JSON)
├── OrderingRules (JSON)
├── PricingTariffID (FK)
├── EffectiveFrom
├── EffectiveTo
└── Status
```

**Relationships**:
- M:N with Schemes (via SchemeProduct)
- 1:N with Transactions (product type)
- 1:N with PricingTariffs

**Business Rules**:
- Product code unique across system
- Active products must be assigned to at least one scheme
- Effective dating for version control

---

### 7. Customer

**Purpose**: Customer master data (synchronized from Salesforce)

**Key Attributes**:
```
Customer
├── CustomerID (PK)
├── SalesforceAccountID (unique - external key)
├── CustomerNumber
├── CustomerName
├── CustomerType (Individual/Organization)
├── ABN
├── ContactDetails
├── Status (Active/Inactive)
└── Attributes
```

**Relationships**:
- 1:N with WaterAllocationTitles (ownership)
- 1:N with Contracts
- 1:N with WaterOrders

**Business Rules**:
- Master data owned by Salesforce
- CustomerID internal to Water Accounting
- SalesforceAccountID used for integration
- Updates synchronized from Salesforce

---

### 8. Water Order

**Purpose**: Customer requests for water delivery

**Key Attributes**:
```
WaterOrder
├── WaterOrderID (PK)
├── OrderNumber (unique)
├── CustomerID (FK)
├── WaterAccountID (FK)
├── SchemeID (FK)
├── ProductType
├── OrderedVolumeML
├── DeliveryLocationID (FK)
├── RequestedDeliveryDate
├── ScheduledDeliveryDate
├── ActualDeliveryDate
├── OrderStatus (Requested/Approved/Scheduled/InProgress/Fulfilled/Cancelled)
├── CreatedBy
├── CreatedDate
├── ApprovedBy
├── ApprovedDate
└── Attributes
```

**Order Fulfillment**:
```
OrderFulfillment
├── FulfillmentID (PK)
├── WaterOrderID (FK)
├── ReleaseDate
├── ReleaseVolumeML
├── SourceStorageID (FK)
├── ReleaseTransactionID (FK)
└── Attributes
```

**Relationships**:
- N:1 with Customer
- N:1 with WaterAccount
- 1:1 with Transaction (reservation)
- 1:N with OrderFulfillments
- N:1 with DeliveryLocation (infrastructure)

**Business Rules**:
- Order volume cannot exceed available water
- Delivery location must be active infrastructure
- Order status workflow enforced
- Fulfilled orders create consumption transactions

---

### 9. Transfer

**Purpose**: Water transfers between customers (temporary or permanent)

**Key Attributes**:
```
Transfer
├── TransferID (PK)
├── TransferNumber (unique)
├── TransferType (Temporary/Permanent)
├── FromCustomerID (FK)
├── ToCustomerID (FK)
├── FromAccountID (FK)
├── ToAccountID (FK)
├── VolumeML
├── TransferDate
├── WaterYear (for temporary)
├── ConsiderationAmount (if sale)
├── Status (Pending/Approved/Completed/Cancelled)
├── ApprovedBy
├── ApprovedDate
├── CompletedDate
├── TransactionID (FK)
└── Attributes
```

**Relationships**:
- N:1 with Customer (from/to)
- N:1 with WaterAccount (from/to)
- 1:1 with Transaction (transfer posting)

**Business Rules**:
- Temporary transfers expire at end of water year
- Permanent transfers change allocation ownership
- Both parties must be in compatible zones
- Transfer volume cannot exceed available water
- Some schemes restrict transfers

---

### 10. Meter Read

**Purpose**: Records water consumption measurements

**Key Attributes**:
```
MeterRead
├── MeterReadID (PK)
├── MeterID (FK)
├── ReadingDate
├── ReadingValue
├── PreviousReadingID (FK)
├── ReadingType (Actual/Estimated)
├── ReadingMethod (Manual/Automated)
├── ReadBy
├── Status (Pending/Validated/Posted/Rejected)
├── ValidationErrors (JSON)
├── ConsumptionML (calculated)
├── TransactionID (FK - if posted)
└── Attributes
```

**Meter Master**:
```
Meter
├── MeterID (PK)
├── MeterNumber (unique)
├── MeterType
├── Location
├── WaterAccountID (FK)
├── SchemeID (FK)
├── InstallDate
├── Status (Active/Inactive/Decommissioned)
├── CalibrationDate
├── CalibrationDue
└── Attributes
```

**Relationships**:
- N:1 with Meter
- 1:1 with Transaction (consumption posting)
- N:1 with WaterAccount

**Business Rules**:
- Reading value must be >= previous reading
- Consumption = Current - Previous reading
- Failed validations require investigation
- Automated readings may require confirmation
- Posted readings create consumption transactions

---

## Supporting Entities

### Scheme Configuration

#### Storage Location
```
StorageLocation
├── StorageLocationID (PK)
├── SchemeID (FK)
├── LocationName
├── LocationType (Dam/Weir/Barrage)
├── CapacityML
├── DeadStorageML
├── UseableStorageML
├── Status
└── Attributes
```

#### Scheme Rule
```
SchemeRule
├── SchemeRuleID (PK)
├── SchemeID (FK)
├── RuleType (Allocation/Distribution/CutOff/Carryover)
├── RuleFormula (calculation expression)
├── Parameters (JSON)
├── TriggerConditions (JSON)
├── EffectiveFrom
├── EffectiveTo
└── Description
```

#### Scheme Zone
```
SchemeZone
├── SchemeZoneID (PK)
├── SchemeID (FK)
├── ZoneCode
├── ZoneName
├── ZoneBoundary (geographic)
├── TradingRestrictions (JSON)
└── Attributes
```

---

### Pricing and Tariffs

#### Pricing Tariff
```
PricingTariff
├── TariffID (PK)
├── TariffCode
├── TariffName
├── SchemeProductID (FK)
├── PriceType (Allocation/Volumetric/Fixed)
├── UnitPrice
├── Currency (AUD)
├── EffectiveFrom
├── EffectiveTo
└── Attributes
```

---

### Operational Data

#### Infrastructure
```
Infrastructure
├── InfrastructureID (PK)
├── InfrastructureType (Offtake/Pump/Channel)
├── SchemeID (FK)
├── LocationID
├── Capacity
├── Status (Operational/Maintenance/Failed)
├── WaterAccountID (FK - if customer-owned)
└── Attributes
```

#### Release
```
Release
├── ReleaseID (PK)
├── SchemeID (FK)
├── SourceStorageID (FK)
├── ReleaseDate
├── ReleaseVolumeML
├── ReleaseType (Order/Environmental/Losses)
├── TransactionID (FK)
└── Attributes
```

---

## Data Volumes (Estimated)

### Master Data (relatively static)
| Entity | Estimated Rows | Growth Rate |
|--------|----------------|-------------|
| Schemes | 30 | 1-2 per year |
| Products | 50 | 2-3 per year |
| Customers | 5,000 | 50-100 per year |
| Allocation Titles | 8,000 | 100-150 per year |
| Water Accounts | 8,000 | Matches Allocation Titles |
| Meters | 3,000 | 50-100 per year |
| Infrastructure | 2,000 | 20-50 per year |

### Transactional Data (high volume)
| Entity | Daily Volume | Annual Volume |
|--------|--------------|---------------|
| Transactions | 500-1,000 | 250,000 |
| Ledger Entries | 1,000-2,000 | 500,000 |
| Meter Reads | 100-200 | 50,000 |
| Water Orders | 50-100 | 25,000 |
| Transfers | 5-10 | 2,500 |

### Retention Requirements
- **Current Water Year**: Online, high-performance
- **Previous 2 Years**: Online, standard performance
- **3-7 Years**: Archived, reporting access
- **7+ Years**: Compliance archive (read-only)

---

## Data Quality Rules

### Validation Rules

#### Water Account Balances
- Running balance must reconcile to sum of ledger entries
- Negative balances only allowed with breach process
- Pending + Available cannot exceed Allocation

#### Transactions
- Transaction must balance (debits = credits)
- Effective date cannot be future
- Posted transactions immutable
- Transaction amount must be > 0

#### Meter Reads
- Reading date cannot be future
- Reading value >= previous reading
- Consumption within expected range (0-max daily usage)
- Gaps in reading sequence flagged

#### Allocations
- Total allocated ≤ Scheme capacity
- Priority allocations respect precedence
- Geographic zones enforced

---

## Data Synchronization

### Master Data Sync

#### From Salesforce
- **Entity**: Customer
- **Frequency**: Real-time (event-driven)
- **Trigger**: Customer create/update in Salesforce
- **Data**: Customer details, contacts, addresses

#### From SAP
- **Entity**: Infrastructure, Meter
- **Frequency**: Daily batch
- **Trigger**: Scheduled job
- **Data**: Asset configuration, status, location

#### To Gentrack
- **Entity**: Consumption Transaction
- **Frequency**: Real-time (event-driven)
- **Trigger**: Meter read posted
- **Data**: Consumption volume, customer, product, date

---

## Data Model Evolution

### Version Control
- All schema changes version controlled
- Migration scripts for each version
- Backward compatibility maintained
- Deprecation warnings for breaking changes

### Extension Mechanism
- **Attributes** (JSON columns) for scheme-specific data
- Avoids excessive customization
- Queryable using JSON functions
- Documented schema for JSON structures

---

## Related Documents

- [Ledger Structure](./ledger-structure.md) - Detailed account hierarchy
- [Transaction Model](./transaction-model.md) - Transaction types and processing
- [Master Data](./master-data.md) - Master data management approach

---

**Version History**

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | Dec 2025 | Data Architecture | Initial data model overview |

