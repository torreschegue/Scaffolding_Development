# 🏦 Layered Architecture Overview

## 📌 Introduction

This project follows a **layered architecture pattern** to ensure clear separation of concerns, scalability, and maintainability. Each layer has a well-defined responsibility and interacts only with adjacent layers.

The main layers are:

* Presentation Layer
* Use Case Layer (Application Layer)
* Business Layer (Domain Layer)
* Infrastructure Layer

---

## 🧱 1. Presentation Layer

### 📍 Purpose

The Presentation Layer is responsible for handling **external communication** with clients (e.g., web, mobile, APIs).

### ⚙️ Responsibilities

* Expose REST endpoints (FastAPI routes)
* Receive HTTP requests
* Validate input data (via Pydantic schemas)
* Return HTTP responses
* Delegate processing to the Use Case Layer

### 🚫 What it should NOT do

* Contain business logic
* Directly access the database

### 🧪 Example

```python
@router.post("/transfer")
def transfer(request: TransferRequest):
    return transaction_service.transfer(request)
```

---

## ⚙️ 2. Use Case Layer (Application Layer)

### 📍 Purpose

This layer defines **application-specific workflows** and orchestrates the system behavior.

### ⚙️ Responsibilities

* Implement use cases (e.g., transfer money, withdraw cash)
* Coordinate interactions between domain and infrastructure
* Apply business rules through domain objects
* Handle transaction logic

### 🚫 What it should NOT do

* Contain persistence details
* Depend on frameworks like FastAPI

### 🧪 Example

```python
def transfer(self, source, target, amount):
    account = self.account_repo.find(source)
    account.debit(amount)
```

---

## 🧠 3. Business Layer (Domain Layer)

### 📍 Purpose

This is the **core of the system**, where all business rules and logic are defined.

### ⚙️ Responsibilities

* Define domain entities (e.g., Account, Customer, Movement)
* Enforce business rules (e.g., insufficient balance)
* Define repository interfaces (contracts)
* Represent the problem domain

### 🚫 What it should NOT do

* Depend on external frameworks
* Contain infrastructure logic

### 🧪 Example

```python
class Account:
    def withdraw(self, amount):
        if self.balance < amount:
            raise ValueError("Insufficient funds")
        self.balance -= amount
```

---

## 🏗️ 4. Infrastructure Layer

### 📍 Purpose

The Infrastructure Layer handles **technical details and external integrations**.

### ⚙️ Responsibilities

* Implement repository interfaces
* Handle database access (MySQL)
* Connect to external systems (MongoDB, APIs)
* Manage persistence and I/O operations

### 🚫 What it should NOT do

* Contain business rules

### 🧪 Example

```python
def find_by_account_number(self, db, account_number):
    return db.query(Account).filter(Account.account_number == account_number).first()
```

---

## 🔄 Layer Interaction Flow

```text
Client
  ↓
Presentation Layer (API)
  ↓
Use Case Layer (Services)
  ↓
Business Layer (Domain)
  ↓
Infrastructure Layer (Repositories / DB)
```

---

## 🎯 Benefits of This Architecture

* ✅ Clear separation of concerns
* ✅ Improved maintainability
* ✅ Easier testing and debugging
* ✅ Scalable for microservices
* ✅ Framework-independent business logic

---

## 🚀 Conclusion

By adopting this layered architecture, the system achieves a clean and modular design. Each layer focuses on a specific responsibility, making the application easier to extend, test, and maintain over time.

This approach is especially suitable for **microservices-based systems** and **enterprise-level applications**.
