# Fraud FSM v1

## Purpose

The Fraud FSM (Finite State Machine) defines the allowed lifecycle of a FraudCase.

The ontology defines the objects, relationships, domains, and governance structures.

The FSM defines how a FraudCase is allowed to change over time.

This ensures deterministic behavior, auditability, governance, and policy enforcement.

---

# Core Object

FraudCase

A FraudCase represents an investigation into a potentially fraudulent activity.

---

# States

## Created

Initial state.

Entered when a FraudAlert creates a FraudCase.

Allowed transitions:

- Assigned

---

## Assigned

A FraudCase has been assigned to a FraudAnalyst.

Allowed transitions:

- Investigating

---

## Investigating

Evidence collection and analysis are underway.

Allowed transitions:

- AwaitingApproval
- Closed

---

## AwaitingApproval

The analyst recommends an action.

Human governance approval is required.

Allowed transitions:

- Approved
- Rejected

---

## Approved

A governance decision has been made.

Allowed transitions:

- ExecutingAction

---

## Rejected

Requested action was rejected.

Allowed transitions:

- Investigating
- Closed

---

## ExecutingAction

A TypedAction is being executed.

Examples:

- FreezeAccount
- BlockCard
- NotifyCustomer

Allowed transitions:

- Closed

---

## Closed

Terminal state.

No further transitions allowed.

---

# State Diagram

Created
→ Assigned
→ Investigating
→ AwaitingApproval
→ Approved
→ ExecutingAction
→ Closed

Alternative path:

AwaitingApproval
→ Rejected
→ Investigating

Alternative path:

Investigating
→ Closed

---

# Governance Rules

A FraudCase may never skip states.

A FraudCase may never move directly from Created to Approved.

A FraudCase may never execute a TypedAction without approval.

All transitions must be auditable.

All transitions must record:

- timestamp
- actor
- reason
- previous state
- new state

---

# Relationship to Ontology

FraudAlert
creates
FraudCase

FraudCase
has state
FraudCaseState

FraudAnalyst
initiates transitions

Policy
constrains transitions

Decision
authorizes transitions

TypedAction
changes enterprise state
