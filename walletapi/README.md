# Wallet Service

## Overview

The **Wallet Service** is a dedicated microservice in the **Smart Toll Plaza Automation System** that manages all FASTag wallet operations. It is responsible for creating wallets, maintaining account balances, processing recharges, deducting toll charges, and validating available funds before a transaction is completed.

Whenever a toll payment is initiated, the Toll Service communicates with this module to verify the wallet and perform the required balance deduction.

---

# Key Responsibilities

* Create FASTag wallets
* Maintain wallet balances
* Recharge wallet accounts
* Deduct toll charges
* Retrieve wallet information
* Delete wallet records
* Ensure unique FASTag IDs
* Verify sufficient balance before deduction

---

# Service Architecture

```text id="r2md8k"
                Client
                   │
                   ▼
          Wallet Controller
                   │
                   ▼
            Wallet Service
                   │
                   ▼
         Wallet Repository
                   │
                   ▼
              H2 Database
```

---

# Project Structure

```text id="9hx3ew"
wallet-service
│
├── controller
│     └── WalletController
│
├── service
│     └── WalletService
│
├── repository
│     └── WalletRepository
│
├── entity
│     └── Wallet
│
├── dto
│     ├── WalletRequest
│     ├── RechargeRequest
│     └── DeductRequest
│
├── advice
│     ├── ErrorResponse
│     └── GlobalExceptionHandler
│
├── exception
│     ├── DuplicateFastTagException
│     ├── FastTagNotFoundException
│     ├── InsufficientBalanceException
│     └── InvalidAmountException
│
└── WalletApiApplication
```

---

# Wallet Entity

The Wallet entity stores FASTag wallet information.

| Field    | Data Type | Description            |
| -------- | --------- | ---------------------- |
| id       | Integer   | Unique identifier      |
| fastagId | String    | FASTag ID              |
| balance  | Double    | Current wallet balance |

---

# Request DTOs

## WalletRequest

Used when creating a new wallet.

```text id="8eq7zf"
fastagId
```

---

## RechargeRequest

```text id="jv3r4q"
fastagId

amount
```

---

## DeductRequest

```text id="kv7s9x"
fastagId

amount
```

---

# Business Process

## Wallet Creation

```text id="m6g3py"
Receive Request
      │
      ▼
Check FASTag ID
      │
      ▼
Already Exists?
   │           │
 Yes          No
   │           │
Return Error   ▼
        Create Wallet
             │
      Initial Balance = ₹0
             │
             ▼
      Return Success
```

---

## Wallet Recharge

```text id="tk4q1c"
Receive Recharge Request
          │
          ▼
Verify FASTag ID
          │
          ▼
Valid FASTag?
     │            │
    No           Yes
     │            │
Return Error     ▼
         Validate Amount
              │
              ▼
Amount > ₹100?
     │            │
    No           Yes
     │            │
Invalid Amount  ▼
         Update Balance
              │
              ▼
        Save Changes
              │
              ▼
      Return Success
```

---

## Wallet Deduction

```text id="c2j8wr"
Receive Deduction Request
          │
          ▼
Verify FASTag ID
          │
          ▼
Wallet Exists?
     │            │
    No           Yes
     │            │
Return Error     ▼
      Check Balance
          │
          ▼
Enough Balance?
     │            │
    No           Yes
     │            │
Return Error     ▼
     Deduct Amount
          │
          ▼
      Save Wallet
          │
          ▼
Return Updated Wallet
```

---

# REST API Endpoints

## Create Wallet

```http id="ph9w6k"
POST /api/v1/wallets
```

### Sample Request

```json id="wr4p8j"
{
  "fastagId":"FT1001"
}
```

### Response

```text id="d2mv5n"
HTTP 201 Created
```

---

## Retrieve Wallet

```http id="s9jg2q"
GET /api/v1/wallets/{fastagId}
```

---

## Retrieve All Wallets

```http id="yh7r2b"
GET /api/v1/wallets
```

---

## Recharge Wallet

```http id="pd6x5v"
PUT /api/v1/wallets/recharge
```

### Sample Request

```json id="rq8w3l"
{
  "fastagId":"FT1001",
  "amount":500
}
```

---

## Deduct Wallet Balance

```http id="gv5n4e"
PUT /api/v1/wallets/deduct
```

### Sample Request

```json id="cn8r1w"
{
  "fastagId":"FT1001",
  "amount":150
}
```

---

## Delete Wallet

```http id="x4mk6q"
DELETE /api/v1/wallets/{id}
```

---

# Business Rules

## Creating a Wallet

* Every FASTag ID must be unique.
* Newly created wallets begin with a balance of ₹0.

### Recharging a Wallet

* FASTag ID must already exist.
* Recharge amount must be greater than ₹100.

### Deducting Balance

* FASTag ID must be valid.
* Wallet must contain enough balance.
* Balance is updated immediately after deduction.

---

# Exception Handling

Global exception handling is implemented using:

```java id="y9qp5d"
@RestControllerAdvice
```

Handled exceptions include:

* DuplicateFastTagException
* FastTagNotFoundException
* InvalidAmountException
* InsufficientBalanceException
* MethodArgumentNotValidException

---

# Standard Error Response

```json id="v2n7ha"
{
  "statusCode":400,
  "errorType":"Validation Failed",
  "errorMessage":"Amount must be greater than 100",
  "timestamp":"2026-07-18T15:20:00"
}
```

---

# Repository Layer

The repository extends:

```java id="e3jc4v"
JpaRepository<Wallet, Integer>
```

### Custom Repository Methods

```java id="gh2q7s"
findByFastagId()

existsByFastagId()
```

---

# Logging

The service uses **SLF4J** for application logging.

Typical log entries include:

* Wallet creation
* Wallet recharge
* Balance deduction
* Duplicate FASTag detection
* Invalid recharge amount
* Insufficient balance
* Wallet deletion

---

# Integration

The Wallet Service is mainly used by the **Toll Service** to:

* Verify wallet balance
* Deduct toll charges

---

# Technology Stack

* Java 21
* Spring Boot
* Spring Web
* Spring Data JPA
* Bean Validation
* Lombok
* H2 Database
* Maven

---

# Running the Application

Start the service using:

```bash id="z7m2fc"
mvn spring-boot:run
```

Default Server Port:

```text id="m5v4kp"
8082
```

---

# API Testing

The APIs can be tested using:

* Postman
* IntelliJ HTTP Client
* curl

---

# Future Enhancements

* Wallet transaction history
* Recharge history
* Downloadable wallet statements
* Daily transaction limits
* Automatic wallet recharge
* JWT Authentication
* Swagger/OpenAPI Documentation
* Docker Support
* MySQL/PostgreSQL Integration
* OpenFeign Client

---

# Author

**PAVITHIRA V**

B.E. Computer Science and Engineering

Saveetha Engineering College

GitHub: https://github.com/pavivengat787-sys/SMART-TOLL-PLASA-PROJECT
