# ✈️ SkyWings Flight Booking System — Centralized Configuration

Centralized configuration repository powered by **Spring Cloud Config Server** for the distributed SkyWings Flight Booking System microservices architecture.

> **Repository:** [https://github.com/moulimds/flight-booking-config](https://github.com/moulimds/flight-booking-config)  
> **Maintained & Committed By:** [Sindhu-0-7](https://github.com/Sindhu-0-7)

---

## 🏛️ Architecture Overview

The Spring Cloud Config Server (`port 8888`) externalizes and centralizes configuration properties across all microservices. When a microservice starts or refreshes its configuration context, it contacts the Config Server, which serves:
1. `application.properties`: Global shared properties (database dialect, JPA settings, RabbitMQ broker, Eureka discovery client, JWT security, Actuator telemetry).
2. `{service-name}.properties`: Service-specific properties (service port, isolated database URL, route definitions, mail credentials, event topics).

---

## 📋 Microservices Configuration Matrix

| # | Service Name | Configuration File | Port | Database Name | Description |
|---|--------------|-------------------|------|---------------|-------------|
| **0** | **Global Shared** | `application.properties` | — | — | Shared JPA, RabbitMQ, Eureka, JWT, Actuator |
| **1** | `API-GATEWAY` | `api-gateway.properties` | `8080` | — | Reactive Gateway, Route Predicates, CORS & Deduplication |
| **2** | `AUTH-SERVICE` | `auth-service.properties` | `8081` | `flight_auth_db` | User Auth, JWT Issuance, Admin Seeding |
| **3** | `FLIGHT-SERVICE` | `flight-service.properties` | `8082` | `flight_db` | Flight Inventory, Schedules, Aircraft Registry |
| **4** | `SEARCH-SERVICE` | `search-service.properties` | `8083` | `flight_search_db` | Origin/Destination Search, Caching & Filters |
| **5** | `BOOKING-SERVICE` | `booking-service.properties` | `8084` | `flight_booking_db` | PNR Generation, Passenger Bookings, Refund Workflows |
| **6** | `FARE-SERVICE` | `fare-service.properties` | `8085` | `flight_fare_db` | Dynamic Tier Pricing & Class Calculations |
| **7** | `SEAT-SERVICE` | `seat-service.properties` | `8086` | `flight_seat_db` | Aircraft Cabin Layouts, Seat Allocation & Locking |
| **8** | `PAYMENT-SERVICE` | `payment-service.properties` | `8087` | `flight_payment_db` | Gateway Integration, Settlement, Refund Dispenser |
| **9** | `INVOICE-SERVICE` | `invoice-service.properties` | `8088` | `flight_invoice_db` | Tax Computation, Invoice PDFs, Accounting Records |
| **10** | `NOTIFICATION-SERVICE` | `notification-service.properties` | `8089` | `flight_notification_db` | SMTP Email Alerts, PDF Ticket Dispatch, SMS |
| **11** | `CHECKIN-SERVICE` | `checkin-service.properties` | `8090` | `flight_checkin_db` | Web Check-in, Boarding Passes, Baggage Tags |
| **12** | `PROFILE-SERVICE` | `profile-service.properties` | `8091` | `flight_profile_db` | Customer Profiles, Miles Ledger, Preferences |
| **13** | `FLIGHT-TRACKING-SERVICE` | `flight-tracking-service.properties` | `8092` | `flight_tracking_db` | Radar Telemetry, Geo Coordinates, Flight Status Events |

---

## 🔍 Config Server HTTP Verification Endpoints

Once Spring Cloud Config Server is running on `http://localhost:8888`, each service configuration can be queried directly:

```http
# Global shared configuration
GET http://localhost:8888/application/default

# Microservice specific configurations
GET http://localhost:8888/api-gateway/default
GET http://localhost:8888/auth-service/default
GET http://localhost:8888/flight-service/default
GET http://localhost:8888/search-service/default
GET http://localhost:8888/booking-service/default
GET http://localhost:8888/fare-service/default
GET http://localhost:8888/seat-service/default
GET http://localhost:8888/payment-service/default
GET http://localhost:8888/invoice-service/default
GET http://localhost:8888/notification-service/default
GET http://localhost:8888/checkin-service/default
GET http://localhost:8888/profile-service/default
GET http://localhost:8888/flight-tracking-service/default
```

---

## 🚀 Client Microservice Integration

To bind a Spring Boot client service to the centralized config server:

### 1. Maven Dependency (`pom.xml`):
```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```

### 2. Microservice `application.properties`:
```properties
spring.application.name=booking-service
spring.config.import=optional:configserver:http://localhost:8888
```

---

## 🔄 Dynamic Refresh
To reload properties at runtime without restarting the service instance, annotate beans with `@RefreshScope` and trigger:

```bash
curl -X POST http://localhost:<port>/actuator/refresh
```
