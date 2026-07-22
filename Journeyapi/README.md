# Journey Service

## Overview

The **Journey Service** is a microservice within the **Smart Toll Plaza Automation System** that maintains the history of completed toll transactions. Each time a vehicle successfully passes through a toll plaza, the Toll Service sends the journey details to this module for permanent storage.

The stored information can later be used for tracking vehicle movements, reviewing travel history, and generating reports.

---

# Primary Responsibilities

* Store completed journey records
* Maintain toll transaction history
* Retrieve saved journey information
* Search journeys using vehicle number
* Record journey start and end times
* Save payment status for every transaction

---

# Service Architecture

```text id="3m9w7x"
                Client
                   │
                   ▼
         Journey Controller
                   │
                   ▼
          Journey Service
                   │
                   ▼
       Journey Repository
                   │
                   ▼
            H2 Database
```

---

# Project Structure

```text id="q5v8bn"
journey-service
│
├── controller
│     └── JourneyController
│
├── service
│     └── JourneyService
│
├── repository
│     └── JourneyRepository
│
├── entity
│     └── Journey
│
├── dto
│     └── JourneyDTO
│
├── advice
│     ├── ErrorResponse
│     └── GlobalExceptionHandler
│
├── exception
│     └── JourneyNotFoundException
│
└── JourneyApiApplication
```

---

# Journey Entity

The Journey entity stores the details of every successful toll transaction.

| Field         | Data Type     | Description               |
| ------------- | ------------- | ------------------------- |
| id            | Integer       | Unique identifier         |
| vehicleNumber | String        | Registered vehicle number |
| startTime     | LocalDateTime | Journey start time        |
| endTime       | LocalDateTime | Journey end time          |
| plaza         | String        | Toll plaza name           |
| amount        | Double        | Toll fee collected        |
| paymentStatus | String        | Transaction status        |

---

# Data Transfer Object

## JourneyDTO

The JourneyDTO carries journey information received from the Toll Service.

```text id="x2g7pj"
vehicleNumber

startTime

endTime

plaza

amount

paymentStatus
```

---

# Journey Processing Flow

```text id="n8r6dy"
Receive Journey Request
          │
          ▼
Validate Request
          │
          ▼
Create Journey Object
          │
          ▼
Store in Database
          │
          ▼
Return Success Response
```

---

# REST API Endpoints

## Save Journey

```http id="z3k9bt"
POST /api/v1/journeys
```

### Sample Request

```json id="m5q2vc"
{
  "vehicleNumber":"TN38AB1234",
  "startTime":"2026-07-18T10:30:00",
  "endTime":"2026-07-18T10:35:00",
  "plaza":"Chennai Toll Plaza",
  "amount":150,
  "paymentStatus":"SUCCESS"
}
```

### Response

```text id="w8f4na"
HTTP 201 Created
```

---

## Retrieve All Journeys

```http id="t6h1er"
GET /api/v1/journeys
```

---

## Retrieve Journey by Vehicle Number

```http id="k7p5xs"
GET /api/v1/journeys/{vehicleNumber}
```

---

# Journey Lifecycle

```text id="v4m2je"
Vehicle Reaches Toll Plaza
          │
          ▼
Payment Completed
          │
          ▼
Toll Service
          │
          ▼
Journey Service
          │
          ▼
Journey Stored
          │
          ▼
History Available for Retrieval
```

---

# Request Validation

Before storing a journey record, the service validates the incoming request.

Validation checks include:

* Vehicle number is required
* Toll plaza name is mandatory
* Toll amount must be provided
* Payment status is required
* Journey start time is mandatory
* Journey end time is mandatory

---

# Payment Status Values

Supported transaction states:

```text id="y9l4cf"
SUCCESS

FAILED

PENDING
```

---

# Exception Handling

Centralized exception handling is implemented using:

```java id="c2w7ha"
@RestControllerAdvice
```

Handled exceptions include:

* JourneyNotFoundException
* MethodArgumentNotValidException

---

# Standard Error Response

```json id="j6x8pe"
{
  "statusCode":404,
  "errorType":"Journey Not Found",
  "errorMessage":"Journey not found for vehicle TN38AB1234",
  "timestamp":"2026-07-18T18:20:00"
}
```

---

# Repository Layer

The repository extends:

```java id="b3n5rt"
JpaRepository<Journey, Integer>
```

### Custom Repository Methods

```java id="h8q1yk"
findByVehicleNumber()

findAll()
```

---

# Integration

The Journey Service is mainly used by the **Toll Service** to:

* Store completed toll transactions
* Maintain travel history
* Preserve journey records for future reference

---

# Logging

The application uses **SLF4J** to record important events.

Typical log entries include:

* Journey creation
* Journey retrieval
* Vehicle-based search
* Validation failures
* Database operations

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

# Running the Service

Start the application using:

```bash id="g1m6za"
mvn spring-boot:run
```

Default Server Port:

```text id="r7d4lv"
8083
```

---

# API Testing

The service can be tested using:

* Postman
* IntelliJ HTTP Client
* curl

---

# Future Enhancements

* Journey pagination
* Search by date range
* Vehicle-wise journey reports
* Monthly analytics reports
* Excel export
* PDF export
* Dashboard visualization
* Swagger/OpenAPI integration
* Docker support
* MySQL or PostgreSQL migration

---

# Author

**PAVITHIRA V**

B.E. Computer Science and Engineering

Saveetha Engineering College

GitHub: https://github.com/pavivengat787-sys/SMART-TOLL-PLASA-PROJECT
