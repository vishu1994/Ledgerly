# Product Requirements Document (PRD)

## 1. Overview

### Product Name
Ledgerly - An AI powered Digital Ledger Management System (DLMS)

### Purpose
DLMS is an end-to-end system that converts unstructured bank account statement documents into a structured, auditable, double-entry digital ledger. The system aims to automate bookkeeping workflows, reduce manual accounting effort, and provide financial intelligence through analytics and AI-driven insights.

This product is designed as a **portfolio-grade, production-inspired system** demonstrating strong backend engineering, distributed systems design, and applied AI.

---

## 2. Goals & Non-Goals

### Goals
- Ingest bank statement documents (PDFs/images)
- Extract and normalize transactions into structured data
- Convert single-entry bank transactions into double-entry ledger records
- Provide accountant-friendly workflows for review, correction, and reconciliation
- Demonstrate robust backend engineering practices (async processing, observability, CI/CD)
- Integrate AI meaningfully in document processing and classification

### Non-Goals (Initial Scope)
- Real-time bank integrations (e.g., Open Banking APIs)
- Full GST/VAT compliance
- Payroll processing
- Multi-currency accounting (Phase 1)

---

## 3. Target Users

### Primary Users
- Accountants
- Bookkeepers
- Finance professionals

### Secondary Users
- Freelancers
- Small business owners
- Fintech engineers reviewing ledger data

---

## 4. User Problems

- Manual entry of bank statements into accounting systems is time-consuming
- High error rates in transaction categorization
- Lack of transparency on how transactions are classified
- Difficulty reconciling bank statements with books
- Poor audit trails and correction workflows

---

## 5. User Journeys

### Journey 1: Bank Statement Upload
1. User uploads a bank statement document
2. System validates file and metadata
3. Processing begins asynchronously
4. User sees processing status

### Journey 2: Ledger Review
1. User views extracted transactions
2. System auto-posts ledger entries
3. User reviews confidence scores
4. User corrects or approves entries

### Journey 3: Reconciliation
1. User selects a date range
2. System matches ledger entries with bank transactions
3. Exceptions are highlighted
4. User resolves discrepancies

---

## 6. Functional Requirements

### 6.1 Document Ingestion
- Support PDF and image formats
- Store raw documents immutably
- Generate unique document IDs

### 6.2 OCR & Extraction
- Extract tabular transaction data
- Handle multi-page statements
- Detect headers, footers, and totals

### 6.3 Transaction Normalization
- Standardize dates, amounts, and descriptions
- Detect debit vs credit
- Normalize merchant names

### 6.4 Classification & Enrichment
- Transaction category classification
- Transaction type detection (UPI, IMPS, Card, etc.)
- Recurring transaction detection
- Confidence scoring

### 6.5 Ledger Engine
- Double-entry bookkeeping
- Configurable chart of accounts
- Support split transactions
- Idempotent ledger posting

### 6.6 Review & Overrides
- Manual override of classifications
- Full audit trail of changes
- Versioning of ledger entries

### 6.7 Reconciliation
- Match ledger entries to bank transactions
- Highlight unmatched items
- Support manual resolution

### 6.8 Reporting
- Account balances
- Expense summaries
- Monthly and yearly views

---

## 7. Non-Functional Requirements

### 7.1 Performance
- Async processing for long-running tasks
- Horizontal scalability for workers

### 7.2 Reliability
- Retry mechanisms for transient failures
- Dead-letter queues
- Idempotent operations

### 7.3 Observability
- Centralized structured logging
- Distributed tracing with correlation IDs
- Metrics for queue depth, task latency, error rates

### 7.4 Security
- Secure file storage
- Role-based access control (basic)
- Encrypted secrets

---

## 8. System Architecture (High-Level)

- FastAPI: API layer
- RabbitMQ: Message broker
- Celery: Background task execution
- PostgreSQL: Ledger and metadata storage
- Object Storage: Raw documents

Pipeline:
1. Upload → API
2. Queue → OCR task
3. OCR → Extraction task
4. Extraction → Classification task
5. Classification → Ledger posting

---

## 9. Data Model (High-Level)

### Core Entities
- Document
- Transaction (normalized)
- LedgerEntry
- Account
- ClassificationDecision
- AuditLog

---

## 10. AI Integration Strategy

- OCR: Document text extraction
- NLP: Transaction description understanding
- ML Classifier: Category and account prediction
- Confidence-based routing to manual review
- Continuous learning from overrides

---

## 11. Error Handling Strategy

- Explicit retry policies per task
- Distinction between system vs data errors
- Graceful degradation (manual review fallback)
- Comprehensive error logging

---

## 12. CI/CD & DevOps

- Dockerized services
- Linting and type checks
- Unit tests for ledger logic
- Integration tests for pipelines
- Automated builds and deployments

---

## 13. Success Metrics

### Technical
- Extraction accuracy
- Classification confidence distribution
- Task failure rate
- Processing time per document

### User
- Reduction in manual entry time
- % auto-posted transactions
- Reconciliation completion rate

---

## 14. Risks & Mitigations

| Risk | Mitigation |
|----|----|
| Poor OCR accuracy | Human-in-the-loop review |
| Misclassification | Confidence thresholds + overrides |
| Pipeline failures | Idempotency + retries |
| Scope creep | Phased roadmap |

---

## 15. Future Enhancements

- Multi-bank account linking
- Tax computation support
- Advanced forecasting
- Multi-currency support
- External integrations (accounting software)

---

## 16. Open Questions

- Supported geographies and banking formats
- Chart of accounts standardization
- Long-term ML model retraining strategy

