# API Gateway

## Overview

The **API Gateway** acts as the centralized access point for the **Smart Toll Plaza Automation System**. Rather than connecting directly to individual microservices, clients send all requests to the gateway, which routes them to the appropriate backend service.

This approach simplifies communication, hides internal service endpoints, and provides a scalable architecture for managing distributed services.

---

# Key Responsibilities

* Provide a single entry point for all client requests
* Route requests to the appropriate microservice
* Hide internal service addresses from clients
* Simplify communication between clients and services
* Support centralized request management
* Improve scalability and maintainability

---

# System Architecture

```text id="g3r7mx"
                    Client
                       │
                       ▼
                 API Gateway
                       │
      ┌────────────────┼────────────────┐
      │                │                │
      ▼                ▼                ▼
 Vehicle API      Wallet API      Journey API
                       │
                       ▼
                   Toll API
```

---

# Project Structure

```text id="m8p4vw"
api-gateway
│
├── src
├── application.yml
├── ApiGatewayApplication
└── pom.xml
```

The API Gateway is intentionally lightweight. It contains only the configuration required for request routing and does not include controllers, services, repositories, or database components.

---

# Purpose of the API Gateway

## Without an API Gateway

```text id="n5q9tb"
Client

 ├── Vehicle Service

 ├── Wallet Service

 ├── Journey Service

 └── Toll Service
```

In this approach, the client must know the address and port number of every microservice.

---

## With an API Gateway

```text id="w6v8ck"
              Client
                 │
                 ▼
           API Gateway
                 │
      ┌──────────┼──────────┐
      ▼          ▼          ▼
 Vehicle     Wallet     Journey
 Service     Service     Service
                 │
                 ▼
            Toll Service
```

The client communicates with only one endpoint, while the gateway handles request routing internally.

---

# Default Port

```text id="r7k3yd"
8080
```

---

# Route Mapping

| Client Request | Destination Service |
| -------------- | ------------------- |
| `/vehicles/**` | Vehicle Service     |
| `/wallet/**`   | Wallet Service      |
| `/journeys/**` | Journey Service     |
| `/toll/**`     | Toll Service        |

---

# Request Routing Examples

## Vehicle Registration

```text id="y2f6jr"
Client
   │
POST /vehicles
   │
   ▼
API Gateway
   │
   ▼
Vehicle Service
   │
   ▼
Response
   │
   ▼
Client
```

---

## Wallet Recharge

```text id="u4c9hn"
Client
   │
PUT /wallet/recharge
   │
   ▼
API Gateway
   │
   ▼
Wallet Service
   │
   ▼
Response
```

---

## Toll Payment

```text id="x5v7mb"
Client
      │
POST /toll/pay
      │
      ▼
API Gateway
      │
      ▼
Toll Service
      │
      ▼
Vehicle Service
      │
      ▼
Wallet Service
      │
      ▼
Journey Service
      │
      ▼
Return Success Response
```

---

# Accessing Services Through the Gateway

Instead of sending requests directly to the Vehicle Service:

```text id="a1z8qy"
http://localhost:8081/api/v1/vehicles
```

Use:

```text id="f9p2rw"
http://localhost:8080/api/v1/vehicles
```

---

Instead of:

```text id="l3d6ke"
http://localhost:8082/api/v1/wallets
```

Use:

```text id="t7m4xc"
http://localhost:8080/api/v1/wallets
```

---

Instead of:

```text id="h2q8sv"
http://localhost:8083/api/v1/journeys
```

Use:

```text id="e5b1zn"
http://localhost:8080/api/v1/journeys
```

---

Instead of:

```text id="c6x4jp"
http://localhost:8084/api/v1/tolls
```

Use:

```text id="q9n5yr"
http://localhost:8080/api/v1/tolls
```

---

# Benefits

* Single endpoint for all APIs
* Simplified client-side configuration
* Centralized request routing
* Better scalability
* Loose coupling between services
* Easier maintenance and deployment
* Improved flexibility for future enhancements

---

# Connected Services

| Service         | Port |
| --------------- | ---- |
| Vehicle Service | 8081 |
| Wallet Service  | 8082 |
| Journey Service | 8083 |
| Toll Service    | 8084 |

---

# Running the Application

Start the microservices in the following sequence:

```text id="p4j7mb"
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

Run the API Gateway using:

```bash id="k8v3hx"
mvn spring-boot:run
```

---

# Technology Stack

* Java 21
* Spring Boot
* Spring Cloud Gateway
* Spring Web
* Maven

---

# Future Enhancements

* Eureka Service Discovery
* Spring Cloud Config Server
* Client-side Load Balancing
* JWT Authentication
* Spring Security
* API Rate Limiting
* API Versioning
* Circuit Breaker with Resilience4j
* Distributed Tracing
* Centralized Request Logging
* Monitoring using Prometheus and Grafana

---

# Related Microservices

* Vehicle Service
* Wallet Service
* Journey Service
* Toll Service

Together, these microservices provide a complete solution for automated toll collection using the Smart Toll Plaza Automation System.

---

# Author

**PAVITHIRA V**

B.E. Computer Science and Engineering

Saveetha Engineering College

GitHub: https://github.com/pavivengat787-sys/SMART-TOLL-PLASA-PROJECT

---

## Support

If you found this project useful, consider giving it a star and sharing your feedback.
