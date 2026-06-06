# FlipFit GymKaro — Gym Management Platform

A gym booking and management platform with two backend implementations (monolithic REST API and microservices) plus an Angular frontend.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Angular 17, Angular Material, Chart.js, SSR |
| REST API | Spring Boot 3.3, Java 17, Spring Data JPA, MySQL |
| Microservices | Spring Boot 4, Eureka Discovery, API Gateway, OpenFeign |
| API Docs | SpringDoc OpenAPI (Swagger UI) |

---

## Project Structure

```
FlipFit-GymKaro/
├── GYMKARO-FLIPFIT-UI-UX/                  # Angular 17 frontend
├── GYMKARO-SPRING-REST-FLIPFIT-API-JPA/    # Monolithic Spring Boot REST API
├── GYMKARO-SPRING-MICROSERVICE-FLIPFIT/    # Microservices architecture
│   ├── GYMKARO-EUREKA-SERVER-DISCOVERY/    # Service registry (port 8761)
│   ├── GYMKARO-SPRING-CLOUD-API-GATEWAY/   # API gateway
│   ├── GYMKARO-SPRING-MICROSERVICE-FLIPFIT-USER-PRODUCER/
│   ├── GYMKARO-SPRING-MICROSERVICE-FLIPFIT-ADMIN-PRODUCER/
│   ├── GYMKARO-SPRING-MICROSERVICE-FLIPFIT-OWNER-PRODUCER/
│   ├── GYMKARO-SPRING-MICROSERVICE-FLIPFIT-CUSTOMER-PRODUCER/
│   ├── GYMKARO-SPRING-MICROSERVICE-FLIPFIT-BOOKING-PRODUCER/
│   └── GYMKARO-SPRING-MICROSERVICE-FLIPFIT-NOTIFICATION-PRODUCER/
├── GYMKARO-SPRING-REST-FLIPFIT-API-COLLECTION/  # Postman collection
└── GYMKARO-UML-ARTIFACTS/                  # Class and use case diagrams
```

---

## Features

- **User / Customer** — register, login, browse gyms, book slots
- **Gym Owner** — register gym centers, manage slots and schedules
- **Admin** — approve/reject gym owners, manage platform users
- **Bookings** — slot booking with waitlist support and cancellation rules
- **Payments** — payment recording with multiple payment modes
- **Notifications** — booking confirmations and updates
- **Scheduler** — manage gym time slots and capacity

---

## Running Locally

### Prerequisites

- Java 17+, Maven
- Node.js 18+, Angular CLI (`npm install -g @angular/cli`)
- MySQL on port `3306`

---

### Option A — Monolithic REST API (simpler)

```bash
cd GYMKARO-SPRING-REST-FLIPFIT-API-JPA

# Edit src/main/resources/application.properties:
# spring.datasource.url=jdbc:mysql://localhost:3306/flipfit_schema
# spring.datasource.username=root
# spring.datasource.password=your_password
# spring.jpa.hibernate.ddl-auto=create   ← use 'update' after first run

mvn spring-boot:run
# Runs on http://localhost:8080
# Swagger UI: http://localhost:8080/swagger-ui.html
```

---

### Option B — Microservices

Start services in this order:

```bash
# 1. Eureka Discovery Server (port 8761)
cd GYMKARO-SPRING-MICROSERVICE-FLIPFIT/GYMKARO-EUREKA-SERVER-DISCOVERY
mvn spring-boot:run

# 2. API Gateway
cd ../GYMKARO-SPRING-CLOUD-API-GATEWAY
mvn spring-boot:run

# 3. Each domain microservice (in any order after gateway is up)
cd ../GYMKARO-SPRING-MICROSERVICE-FLIPFIT-USER-PRODUCER && mvn spring-boot:run
cd ../GYMKARO-SPRING-MICROSERVICE-FLIPFIT-BOOKING-PRODUCER && mvn spring-boot:run
# ... repeat for other producers
```

Eureka dashboard: `http://localhost:8761`

---

### Frontend

```bash
cd GYMKARO-FLIPFIT-UI-UX
npm install
ng serve
# Runs on http://localhost:4200
```

---

## API Overview (REST API)

All paths are documented in Swagger. Key controller groups:

| Controller | Base Path | Responsibility |
|---|---|---|
| User | `/user/**` | Registration, login |
| Admin | `/admin/**` | Platform administration |
| Owner | `/owner/**` | Gym owner management |
| Customer | `/customer/**` | Customer profile |
| Gym Center | `/gym-center/**` | Gym CRUD |
| Booking | `/booking/**` | Slot booking and cancellation |
| Scheduler | `/scheduler/**` | Slot scheduling |
| Payment | `/payment/**` | Payment recording |
| Notification | `/notification/**` | Notifications |
| Waitlist | Embedded in booking | Waitlist management |

Full docs: `http://localhost:8080/swagger-ui.html`

---

## Author

**Aditya Anand Mishra**
