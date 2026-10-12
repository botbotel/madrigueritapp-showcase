# MadrigueritApp — Architecture Notes

This document describes the architecture of MadrigueritApp at a portfolio level.

It intentionally avoids publishing source code, production security rules or implementation details that would expose the private codebase.

---

## 1. Application layers

MadrigueritApp separates responsibilities into five main layers.

```text
Routing / Screens
      │
      ▼
Feature layer
      │
      ▼
Domain services
      │
      ▼
State & Firebase SDK
      │
      ▼
Auth / Firestore / Security Rules
```

### Routing / screens

Expo Router owns route composition, public vs protected navigation and feature entry points.

### Feature layer

Features contain product-specific hooks, schemas, components and domain types.

Examples include:

- authentication
- households
- tickets
- calendar
- events
- shopping
- expenses

### Domain services

Data access is isolated from screens.

Instead of calling Firestore directly throughout the UI, product operations are grouped by domain.

This makes application behavior easier to test, reason about and evolve.

### State

Global session state is intentionally limited.

User, profile, household and membership state are kept centrally where several product areas need them, while feature-specific state stays closer to its feature.

### Backend

Firebase provides:

- authentication
- real-time database access through Cloud Firestore
- hosting for the web build
- server-enforced access control through Firestore Security Rules

---

## 2. Tenant boundary

The main tenant is a household.

```text
User ── Membership ── Household
                        │
                        ├── Tickets
                        ├── Events
                        ├── Shopping items
                        └── Expenses
```

Membership is explicit instead of inferred.

This supports:

- multiple roles
- membership status
- controlled onboarding
- owner-only operations
- household-scoped queries
- clear authorization boundaries

---

## 3. Authorization model

Permissions are evaluated using both identity and household context.

At a high level:

```text
Is the user authenticated?
        │
        ▼
Is the user an active member?
        │
        ▼
Does the operation require a role?
        │
        ▼
Is the document valid for this tenant?
        │
        ▼
Allow / deny
```

UI restrictions improve user experience, but the backend rules remain the security boundary.

---

## 4. Real-time model

For collaborative domains, the application subscribes to household-scoped data.

Examples include:

- tickets
- events
- shopping items
- household members
- expenses

The UI reacts to backend changes rather than requiring explicit refresh cycles.

---

## 5. Calendar composition

Calendar is treated as a presentation domain rather than a duplicate source of truth.

Events already contain dates, and tickets may contain due dates.

The calendar feature transforms both into a shared calendar-entry model.

```text
Events ───────┐
              ├── CalendarEntry[] ── Calendar UI
Dated tickets ┘
```

This reduces duplicated data and avoids synchronization problems between independent collections.

---

## 6. Cross-platform boundary

The same product targets web and native environments.

The architecture therefore keeps platform differences isolated when practical, especially around:

- authentication persistence
- navigation
- confirmation interactions
- build and deployment flows

---

## 7. Current evolution

The architecture is intentionally prepared to grow toward:

- automated end-to-end testing
- richer observability
- notification flows
- stronger offline behavior
- accessibility validation
- CI/CD
- additional reporting and expense insights

---

## Why this architecture matters

The main goal is not architectural complexity.

The goal is to keep product concerns explicit enough that features can grow without turning screens into a mix of UI, authorization, state and database logic.

That balance is one of the core engineering ideas behind MadrigueritApp.
