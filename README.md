<div align="center">

# Smart Toll Plaza Automation System

### Toll Collection Platform Using Spring Boot Microservices

A distributed toll management application developed with **Spring Boot**, **REST APIs**, **FASTag**, and **Microservices Architecture** to provide a seamless and automated toll payment experience.

![Java](https://img.shields.io/badge/Java-21-orange?style=for-the-badge)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.x-6DB33F?style=for-the-badge\&logo=springboot)
![Spring Cloud Gateway](https://img.shields.io/badge/API-Gateway-blue?style=for-the-badge)
![H2 Database](https://img.shields.io/badge/H2-Database-blue?style=for-the-badge)
![REST API](https://img.shields.io/badge/REST-API-success?style=for-the-badge)
![Microservices](https://img.shields.io/badge/Architecture-Microservices-red?style=for-the-badge)

</div>

---

# Introduction

The **Smart Toll Plaza Automation System** is a microservices-based application developed using Spring Boot to modernize the toll collection process. The application minimizes manual intervention by automatically validating vehicles, identifying their FASTag accounts, deducting toll charges, and maintaining journey records.

Each service is designed independently following a layered architecture, making the application scalable, maintainable, and easy to extend. Communication between services is achieved through REST APIs, while an API Gateway serves as the single entry point for all client requests.

---

# Project Goals

* Digitize toll collection
* Maintain vehicle registration information
* Manage FASTag wallet transactions
* Record journey details
* Enable communication among microservices
* Provide centralized request routing using an API Gateway

---

# System Architecture

```text
                    Client
                       │
                       ▼
               API Gateway (8080)
                       │
      ┌────────────────┼────────────────┐
      │                │                │
      ▼                ▼                ▼
 Vehicle Service   Wallet Service   Journey Service
      │
      ▼
 Toll Service
```

---

# Microservices Overview

| Service         | Purpose                                                      |
| --------------- | ------------------------------------------------------------ |
| Vehicle Service | Stores vehicle registration and FASTag information           |
| Wallet Service  | Maintains wallet balance, recharge, and deduction operations |
| Journey Service | Saves and retrieves journey records                          |
| Toll Service    | Handles the complete toll payment process                    |
| API Gateway     | Routes all incoming requests to the appropriate service      |

---

# Toll Processing Workflow

```text
Vehicle Number
      │
      ▼
Vehicle Verification
      │
      ▼
Retrieve FASTag Details
      │
      ▼
Verify Wallet Balance
      │
      ▼
Deduct Toll Amount
      │
      ▼
Save Journey Record
      │
      ▼
Return Payment Status
```

---

# Repository Layout

```text
smart-toll-plaza-automation
│
├── api-gateway
├── vehicle-service
├── wallet-service
├── journey-service
├── toll-service
└── README.md
```

---

# Core Features

## Vehicle Service

* Register new vehicles
* Modify vehicle information
* Remove registered vehicles
* View vehicle details
* Generate unique vehicle numbers
* Maintain unique FASTag IDs

## Wallet Service

* Create FASTag wallets
* Recharge wallet balance
* Deduct toll charges
* Check available balance
* Prevent insufficient balance transactions

## Journey Service

* Save journey information
* Display travel history
* Search journeys using vehicle number

## Toll Service

* Validate registered vehicles
* Retrieve FASTag details
* Process wallet deduction
* Create journey records
* Generate payment responses

## API Gateway

* Centralized API routing
* Single client access point
* Simplified request forwarding

---

# Technology Stack

## Backend

* Java 21
* Spring Boot
* Spring Web
* Spring Data JPA
* Spring Validation
* Spring Cloud Gateway

## Database

* H2 Database

## Service Communication

* REST APIs
* RestTemplate

## Development Tools

* IntelliJ IDEA
* Maven
* Postman
* Git
* GitHub

---

# Project Architecture

Every microservice follows a layered structure.

```text
service-name
│
├── controller
├── service
├── repository
├── entity
├── dto
├── advice
├── exception
└── config
```

---

# Running the Application

Start the services in the following order:

```text
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

# Default Service Ports

| Service         | Port |
| --------------- | ---- |
| API Gateway     | 8080 |
| Vehicle Service | 8081 |
| Wallet Service  | 8082 |
| Journey Service | 8083 |
| Toll Service    | 8084 |

---

# API Gateway Endpoints

| Route        | Destination     |
| ------------ | --------------- |
| /vehicles/** | Vehicle Service |
| /wallet/**   | Wallet Service  |
| /journeys/** | Journey Service |
| /toll/**     | Toll Service    |

---

# Complete Workflow

1. Register a vehicle.
2. Create a FASTag wallet.
3. Recharge the wallet.
4. Submit a toll payment request.
5. Verify vehicle details.
6. Deduct the toll amount.
7. Store journey information.
8. Return the payment confirmation.

---

# Concepts Implemented

* Spring Boot
* Microservices Architecture
* RESTful Web Services
* Layered Architecture
* DTO Pattern
* Bean Validation
* Global Exception Handling
* API Gateway
* RestTemplate
* CRUD Operations
* Logging
* H2 Database

---

# Future Scope

* Eureka Service Discovery
* OpenFeign Client
* MySQL or PostgreSQL Integration
* Docker Support
* Spring Security
* JWT Authentication
* Redis Cache
* Apache Kafka
* Swagger/OpenAPI Documentation
* Kubernetes Deployment

---

# Author

**PAVITHIRA V**

B.E. Computer Science and Engineering

Saveetha Engineering College

GitHub: https://github.com/pavivengat787-sys/SMART-TOLL-PLASA-PROJECT

---
