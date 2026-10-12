# MadrigueritApp

🇬🇧 **English** | 🇪🇸 [Español](docs/README.es.md)

**Cross-platform household management product built with React Native, Expo, TypeScript and Firebase.**

> **Portfolio showcase only.**  
> The production source code is intentionally kept private. This repository documents the product, architecture and engineering decisions without exposing reusable implementation code.

MadrigueritApp is a shared household management application designed for **iOS, Android and Web**. It combines daily coordination flows such as tasks, calendar events, shopping and expenses with a multi-tenant household model, role-based access and real-time synchronization.

---

## Product overview

The goal is simple: reduce the friction of coordinating a shared home without spreading information across chat messages, notes, calendars and spreadsheets.

The application brings together:

- shared tasks and tickets
- calendar and scheduled items
- real-time shopping lists
- household expenses
- household members and roles
- invite-by-code onboarding
- profile and access management

The project is built as a real product rather than a UI-only demo: authentication, authorization, data modeling, state, navigation, validation and backend rules are treated as first-class parts of the system.

---

## Core stack

| Area | Technology |
| --- | --- |
| Web & Mobile | React 19, React Native, Expo |
| Routing | Expo Router |
| Language | TypeScript |
| State | Zustand |
| Validation | Zod |
| Backend | Firebase Authentication, Cloud Firestore |
| Realtime | Firestore listeners |
| Security | Firestore Security Rules |
| Builds | EAS Build |
| Web deployment | Expo Web + Firebase Hosting |

---

## Main product areas

### Authentication and protected navigation

The app uses Firebase Authentication with protected routes and persistent sessions across native and web environments.

### Multi-tenant households

Users work inside an active household rather than in a single-user data model.

The product supports:

- household creation
- owner / member / guest roles
- invite codes
- household joining
- membership state
- household member management
- owner-restricted actions

### Tickets

Shared task management with:

- title and description
- priority
- status
- due date
- assignment
- creator information
- real-time updates

### Calendar

The calendar combines multiple product domains into a common view:

- household events
- tickets with due dates

This avoids duplicating the same information into a separate calendar data source.

### Shopping

A shared real-time shopping list with:

- item creation
- optional notes
- completed state
- selection and bulk actions
- live updates between household members

### Expenses

Household-scoped expense tracking with:

- categories
- amounts
- dates
- notes
- monthly grouping
- validation and edit flows

---

## Architecture

The application is organized around a clear separation between routing, product features, domain services, state and shared UI.

```text
Expo Router / React Native UI
            │
            ▼
     Feature hooks & logic
            │
            ▼
        Service layer
            │
      ┌─────┴─────┐
      ▼           ▼
Firebase Auth   Firestore
                  │
                  ▼
          Security Rules
```

A more detailed architecture breakdown is available in [docs/architecture.md](docs/architecture.md).

---

## Multi-tenant model

The central tenant boundary is the **household**.

Product data such as tickets, events, shopping items and expenses is associated with a household rather than directly with one user.

At a conceptual level:

```text
User
  │
  ▼
Household Membership
  │
  ├── role
  ├── status
  └── permissions
  │
  ▼
Household
  ├── Tickets
  ├── Events
  ├── Shopping
  └── Expenses
```

This design makes collaboration, permissions and future product growth easier to reason about.

---

## Security approach

Security is not implemented only in the UI.

Authorization is also enforced in the backend rules layer using concepts such as:

- authenticated user checks
- active household membership
- owner-restricted operations
- role validation
- tenant isolation
- immutable tenant relationships
- restricted field updates
- input validation
- deny-by-default behavior

The exact production rules are intentionally not published in this showcase repository.

---

## Real-time collaboration

Several product areas use Firestore listeners so changes are propagated without requiring a manual refresh.

This is particularly useful for:

- shopping lists
- tasks
- events
- household membership
- expenses

The result is a product that behaves naturally when multiple household members are interacting with the same data.

---

## Cross-platform product decisions

The application targets:

- **Android**
- **iOS**
- **Web**

Some flows require platform-aware handling, including navigation, confirmation interactions and authentication persistence.

The project also includes preview builds and production build configuration through EAS.

---

## Engineering decisions

### 1. Household as the tenant boundary

Shared product data belongs to a household, not to individual users. This keeps collaboration and authorization consistent.

### 2. Authorization beyond the interface

Disabling a button is not treated as security. Sensitive access rules are enforced at the data layer as well.

### 3. Domain-oriented services

Data access is separated by product domain instead of being embedded directly in screens.

This keeps UI code focused on interaction and presentation while backend-specific concerns remain isolated.

### 4. Atomic multi-document flows

Operations such as household onboarding involve several related records. These flows are designed to preserve consistency when multiple documents must change together.

### 5. Derived calendar data

Calendar entries are derived from existing event and task domains instead of maintaining a duplicated calendar collection.

---

## Product status

The current private codebase includes working foundations for:

- authentication
- protected routes
- household onboarding
- role-based membership
- household invites
- ticket management
- calendar and events
- shopping
- expenses
- profile settings
- web and mobile builds

The project remains under active development.

---

## Screenshots

Product screenshots will be added here progressively.

Planned showcase views:

1. Home / household dashboard
2. Tickets
3. Calendar
4. Shopping
5. Expenses
6. Household settings

---

## What this project demonstrates

From a portfolio perspective, MadrigueritApp demonstrates:

- React Native and Expo product development
- TypeScript application architecture
- cross-platform UI
- authentication and protected routing
- multi-tenant product modeling
- role-based authorization
- real-time data synchronization
- backend security thinking
- state management
- domain separation
- product-oriented engineering decisions

---

## Source code

The source code for MadrigueritApp is **private by design**.

This repository is a public engineering case study intended to show the product, architecture and technical decisions while preserving the private implementation and potential commercial value of the application.

---

## Author

**Alberto Botella Guirado**  
Frontend / Product Engineer

[LinkedIn](https://www.linkedin.com/in/alberto-botella-guirado/)
