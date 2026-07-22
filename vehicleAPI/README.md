# Vehicle Service

## Overview

The **Vehicle Service** is one of the core microservices in the **Smart Toll Plaza Automation System**. It is responsible for storing and managing vehicle registration details along with FASTag information.

Before a toll transaction is completed, the Toll Service communicates with this module to verify the vehicle and obtain the corresponding FASTag ID. This ensures that only registered vehicles are allowed to proceed with the toll payment process.

---

# Main Functions

The Vehicle Service provides the following capabilities:

* Register a new vehicle
* View vehicle information
* Update existing vehicle records
* Delete vehicle records
* Store FASTag details
* Ensure unique vehicle numbers
* Ensure unique FASTag IDs

---

# System Design

```text
                Client
                   │
                   ▼
          Vehicle Controller
                   │
                   ▼
            Vehicle Service
                   │
                   ▼
         Vehicle Repository
                   │
                   ▼
              H2 Database
```

---

# Project Organization

```text
vehicle-service
│
├── controller
│     └── VehicleController
│
├── service
│     └── VehicleService
│
├── repository
│     └── VehicleRepository
│
├── entity
│     └── Vehicle
│
├── dto
│     └── VehicleDTO
│
├── enums
│     └── VehicleType
│
├── advice
│     ├── ErrorResponse
│     └── GlobalExceptionHandler
│
├── exception
│     ├── DuplicateVehicleException
│     ├── FastTagNotFoundException
│     └── VehicleNotFoundException
│
└── VehicleApiApplication
```

---

# Vehicle Entity

The Vehicle entity stores all the required information related to a registered vehicle.

| Field         | Data Type | Description                  |
| ------------- | --------- | ---------------------------- |
| id            | Integer   | Unique identifier            |
| vehicleNumber | String    | Registration number          |
| ownerName     | String    | Name of the vehicle owner    |
| vehicleType   | Enum      | Type of vehicle              |
| fastagId      | String    | FASTag identification number |

---

# Data Transfer Object

## VehicleDTO

The DTO is used to transfer request data from the client to the application.

```text
vehicleNumber
ownerName
vehicleType
fastagId
```

### Validation Rules

* Vehicle number must follow the required format.
* Owner name is mandatory.
* Vehicle type must be valid.
* FASTag ID cannot be empty.

---

# Supported Vehicle Categories

```text
CAR

BUS

TRUCK

BIKE
```

---

# Registration Process

```text
Receive Vehicle Details
        │
        ▼
Validate Input Data
        │
        ▼
Check Vehicle Number
        │
        ▼
Already Exists?
   │           │
 Yes          No
   │           │
Return Error   ▼
          Verify FASTag ID
               │
               ▼
        FASTag Exists?
          │          │
        Yes         No
          │          │
   Return Error   Save Record
                     │
                     ▼
            Send Success Response
```

---

# REST Endpoints

## Register Vehicle

```http
POST /api/v1/vehicles
```

### Sample Request

```json
{
  "vehicleNumber":"TN38AB1234",
  "ownerName":"Pavithira",
  "vehicleType":"CAR",
  "fastagId":"FT1001"
}
```

### Response

```
HTTP 201 Created
```

---

## Retrieve Vehicle

```http
GET /api/v1/vehicles/{vehicleNumber}
```

---

## Retrieve All Vehicles

```http
GET /api/v1/vehicles
```

---

## Update Vehicle

```http
PUT /api/v1/vehicles/{id}
```

---

## Remove Vehicle

```http
DELETE /api/v1/vehicles/{id}
```

---

# Input Validation

| Field          | Validation Rule                |
| -------------- | ------------------------------ |
| Vehicle Number | Required with valid format     |
| Owner Name     | Minimum length of 3 characters |
| Vehicle Type   | Accepted enum value            |
| FASTag ID      | Mandatory field                |

Example:

```text
TN42IK2007
```

---

# Exception Management

The application uses a centralized exception handler implemented with:

```java
@RestControllerAdvice
```

The following exceptions are managed:

* DuplicateVehicleException
* VehicleNotFoundException
* FastTagNotFoundException
* MethodArgumentNotValidException

---

# Standard Error Format

Whenever an error occurs, the application returns a structured response.

```json
{
  "statusCode":400,
  "errorType":"Validation Failed",
  "errorMessage":"Vehicle already exists",
  "timestamp":"2026-07-18T12:00:00"
}
```

---

# Repository Layer

The repository is built using:

```java
JpaRepository<Vehicle, Integer>
```

### Repository Methods

```java
existsByVehicleNumber()

existsByFastagId()

findByVehicleNumber()
```

---

# Logging

Application events are recorded using **SLF4J**.

Common log entries include:

* Vehicle registration
* Vehicle retrieval
* Vehicle modification
* Vehicle deletion
* Duplicate registration attempts
* Vehicle lookup failures

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

```bash
mvn spring-boot:run
```

Default Server Port:

```text
8081
```

---

# API Testing

The REST endpoints can be tested using:

* Postman
* IntelliJ HTTP Client
* curl

---

# Integration

The Vehicle Service is primarily consumed by the **Toll Service** to:

* Verify registered vehicles
* Obtain FASTag information before processing toll payments

---

# Planned Enhancements

* Pagination support
* Advanced vehicle search
* Swagger/OpenAPI integration
* MySQL database support
* Unit and integration testing
* Docker containerization
* OpenFeign communication
* Eureka Service Discovery

---

# Author

**PAVITHIRA V**

B.E. Computer Science and Engineering

Saveetha Engineering College

GitHub: https://github.com/pavivengat787-sys/SMART-TOLL-PLASA-PROJECT
