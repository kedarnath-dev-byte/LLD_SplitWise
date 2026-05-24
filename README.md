# Splitwise Low-Level Design

A Spring Boot implementation of a Splitwise-style expense sharing system. The project models users, groups, expenses, per-user expense shares, and settlement transactions using a layered backend structure.

## Tech Stack

- Java 21
- Spring Boot 3.4
- Spring Web
- Spring Data JPA
- PostgreSQL
- Maven
- Lombok

## Features

- Create and manage users
- Create groups for shared expenses
- Add expenses with user-level paid/owed splits
- Track each user's contribution through `UserExpense`
- Generate settlement transactions through a strategy interface
- Persist users, groups, expenses, and transactions using JPA repositories
- Separate controller, service, repository, DTO, model, and exception layers

## Design Overview

```text
Controller Layer
  UserController
  GroupController
  ExpenseController

Service Layer
  UserService
  GroupService
  ExpenseService
  UserExpenseService
  SettleUpStrategy

Repository Layer
  UserRepository
  GroupRepository
  ExpenseRepository
  UserExpenseRepository
  TransactionRepository

Domain Model
  User
  Group
  Expense
  UserExpense
  Transaction
```

## Core Models

- `User`: Represents a person participating in expense sharing.
- `Group`: Represents a collection of users and their shared expenses.
- `Expense`: Represents a bill or payment event.
- `UserExpense`: Tracks how much a user paid or owes for an expense.
- `Transaction`: Represents the final settlement transfer between users.

## Settlement Strategy

The project uses a `SettleUpStrategy` abstraction so settlement logic can evolve without changing the controller or group service flow. `MinimumTransactionSettleUpStrategy` calculates each user's net balance from all group expenses and is the extension point for minimizing repayment transactions.

## How to Run

1. Create a PostgreSQL database named `splitwisedb`.
2. Update database credentials in `src/main/resources/application.properties`.
3. Start the application:

```bash
./mvnw spring-boot:run
```

On Windows:

```bash
mvnw.cmd spring-boot:run
```

## What This Project Demonstrates

- Low-level design of a real-world expense sharing workflow
- Clean separation between API, business logic, persistence, and domain models
- Strategy pattern usage for settlement calculation
- Spring Boot REST API structure
- JPA entity modeling for many-to-many style business relationships

## Future Improvements

- Complete minimum-transaction settlement output generation
- Add request validation and richer error responses
- Add unit tests for expense creation and settlement logic
- Add Swagger/OpenAPI documentation
- Add Docker Compose for PostgreSQL setup
