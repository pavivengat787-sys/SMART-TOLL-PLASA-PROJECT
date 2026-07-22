# Toll Service

## Overview

The **Toll Service** acts as the central coordinator in the **Smart Toll Plaza Automation System**. Instead of storing data locally, it manages the complete toll payment process by interacting with the Vehicle, Wallet, and Journey services.

Whenever a vehicle reaches the toll plaza, this service verifies the vehicle details, obtains the associated FASTag information, deducts the required amount from the wallet, records the journey, and returns the final transaction result to the client.

---

# Core Responsibilities

* Process toll payment requests
* Verify vehicle registration
* Retrieve FASTag details
* Deduct the toll amount from the wallet
* Store journey information
* Return transaction results
* Coordinate communication between multiple microservices

---

# Service Architecture

```text id="5kd13e"
                    Client
                       │
                       ▼
                Toll Controller
                       │
                       ▼
                  Toll Service
                       │
      ┌────────────────┼────────────────┐
      │                │                │
      ▼                ▼                ▼
 Vehicle Client   Wallet Client   Journey Client
      │                │                │
      ▼                ▼                ▼
 Vehicle API      Wallet API      Journey API
```

---

# Project Structure

```text id="g9hzx8"
toll-service
│
├── controller
│     └── TollController
│
├── service
│     └── TollService
│
├── client
│     ├── VehicleClient
│     ├── WalletClient
│     └── JourneyClient
│
├── dto
│     ├── TollRequest
│     ├── TollResponse
│     ├── JourneyDTO
│     ├── WalletDTO
│     └── VehicleDTO
│
├── config
│     └── RestTemplateConfig
│
└── TollApiApplication
```

---

# Database Design

The Toll Service does **not** maintain its own database because its primary responsibility is service orchestration rather than data storage.

Each microservice manages its own data independently.

| Service         | Primary Responsibility    |
| --------------- | ------------------------- |
| Vehicle Service | Vehicle verification      |
| Wallet Service  | Wallet balance management |
| Journey Service | Journey record storage    |

This design follows the **Database per Service** architecture, making the system modular and loosely coupled.

---

# Toll Transaction Flow

```text id="bf29h1"
Receive Toll Request
        │
        ▼
Validate Vehicle
        │
        ▼
Retrieve FASTag Details
        │
        ▼
Deduct Wallet Balance
        │
        ▼
Sufficient Balance?
     │          │
    No         Yes
     │          │
Return Error   ▼
         Save Journey
              │
              ▼
      Return Success
```

---

# Service Communication

```text id="nr6k1f"
                Toll Service
                      │
      ┌───────────────┼───────────────┐
      ▼               ▼               ▼
 Vehicle API     Wallet API     Journey API
      │               │               │
 Verify Vehicle   Deduct Amount   Store Journey
```

---

# REST Endpoint

## Pay Toll

```http id="o8fsx3"
POST /api/v1/tolls
```

### Sample Request

```json id="e8q72y"
{
  "vehicleNumber":"TN38AB1234",
  "plaza":"Chennai Toll Plaza",
  "amount":150
}
```

### Sample Response

```json id="4rwp9q"
{
  "message":"Toll Paid Successfully",
  "vehicleNumber":"TN38AB1234",
  "fastagId":"FT1001",
  "amount":150,
  "status":"SUCCESS"
}
```

---

# Processing Sequence

### Step 1

Receive the toll payment request from the client.

↓

### Step 2

Request vehicle information from the Vehicle Service.

```http id="x8ldf4"
GET /vehicles/{vehicleNumber}
```

↓

If the vehicle does not exist, an error response is returned.

↓

Otherwise, retrieve the associated FASTag ID.

↓

### Step 3

Request wallet deduction from the Wallet Service.

```http id="q51xwy"
PUT /wallet/deduct
```

↓

If the wallet balance is insufficient, the transaction is stopped.

↓

If the deduction succeeds, continue to the next step.

↓

### Step 4

Send journey details to the Journey Service.

```http id="8sv8y7"
POST /journeys
```

↓

The journey is stored successfully.

↓

### Step 5

Return the payment confirmation to the client.

---

# Request Model

## TollRequest

```text id="r2v3hj"
vehicleNumber

plaza

amount
```

---

# Response Model

## TollResponse

```text id="qt6g7m"
message

vehicleNumber

fastagId

amount

status
```

---

# External Clients

## VehicleClient

Functions:

* Verify vehicle details
* Retrieve FASTag information

### WalletClient

Functions:

* Deduct wallet balance
* Check wallet information

### JourneyClient

Functions:

* Save journey details
* Return journey status

---

# REST Communication

The Toll Service exchanges data with other microservices through **RestTemplate**.

A dedicated configuration class provides a reusable **RestTemplate Bean**, making outbound service communication simple and centralized.

---

# Error Management

The service handles several runtime failures, including:

* Vehicle not registered
* FASTag not found
* Insufficient wallet balance
* Wallet service unavailable
* Journey service unavailable
* Invalid client request
* Connection timeout

---

# Transaction Lifecycle

```text id="4dxfkg"
Vehicle Arrives
      │
      ▼
Vehicle Verification
      │
      ▼
Wallet Deduction
      │
      ▼
Journey Recording
      │
      ▼
Transaction Completed
```

---

# Service Dependencies

The Toll Service depends on the following services:

* Vehicle Service
* Wallet Service
* Journey Service

Ensure these services are running before starting the Toll Service.

---

# Technology Stack

* Java 21
* Spring Boot
* Spring Web
* RestTemplate
* Bean Validation
* Lombok
* Maven

---

# Running the Service

```bash id="n7vb4r"
mvn spring-boot:run
```

Default Port

```text id="4m2yqw"
8084
```

---

# Recommended Startup Order

```text id="d9h6sw"
Vehicle Service
        ↓
Wallet Service
        ↓
Journey Service
        ↓
Toll Service
        ↓
API Gateway
```

---

# Future Enhancements

* OpenFeign Client
* Resilience4j Circuit Breaker
* Automatic Retry Mechanism
* Distributed Tracing
* Kafka Messaging
* Eureka Service Discovery
* Docker Support
* Kubernetes Deployment
* JWT Authentication
* Swagger/OpenAPI Documentation

---

# Author

**PAVITHIRA V**

B.E. Computer Science and Engineering

Saveetha Engineering College

GitHub: https://github.com/pavivengat787-sys/SMART-TOLL-PLASA-PROJECT
