# ByteBites Restaurant Management System 🍽️

![System Architecture](docs/architecture-diagram.png)

A comprehensive restaurant management platform with microservices architecture, handling orders, inventory, notifications, and more.

## 🌟 Features

- **Order Management** - Real-time order processing
- **Inventory Tracking** - Ingredient management
- **Notification Service** - Email/SMS/Push alerts
- **User Authentication** - JWT-secured endpoints
- **API Gateway** - Unified service access
- **Event-Driven Architecture** - RabbitMQ integration

## 🏗️ System Architecture

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│   Client    │ ←→ │ API Gateway │ ←→ │  Services   │
└─────────────┘    └─────────────┘    └─────────────┘
                         ↑
                    ┌────┴────┐
                    │ RabbitMQ │
                    └─────────┘
```

### Services Overview

1. **API Gateway** - Route requests to services
2. **Order Service** - Process customer orders
3. **Inventory Service** - Track ingredients
4. **Notification Service** - Send alerts
5. **Auth Service** - Handle authentication

## 🚀 Quick Start

### Prerequisites

- Docker 20.10+
- Docker Compose 2.0+
- Java 17+
- Maven 3.8+

### Installation

```bash
git clone https://github.com/your-repo/bytebites-restaurant.git
cd bytebites-restaurant
docker-compose up -d
```

## 🔧 Service Configuration

### Core Services

| Service          | Port  | Description                     |
|------------------|-------|---------------------------------|
| API Gateway      | 8080  | Entry point for all requests    |
| Order Service    | 8081  | Handles order processing        |
| Inventory Service| 8082  | Manages ingredient inventory    |
| Notification     | 8083  | Sends notifications             |
| Auth Service     | 8084  | Handles authentication          |

### Infrastructure

| Service       | Port    | Description                     |
|--------------|---------|---------------------------------|
| PostgreSQL   | 5432    | Primary database                |
| RabbitMQ     | 5672    | Message broker                  |
| Redis        | 6379    | Cache                           |
| Prometheus   | 9090    | Monitoring                      |
| Grafana      | 3000    | Metrics visualization           |

## 🔐 Authentication Flow

1. Obtain JWT token:
   ```bash
   curl -X POST http://localhost:8080/auth/login \
     -H "Content-Type: application/json" \
     -d '{"username":"admin", "password":"admin123"}'
   ```

2. Use token in requests:
   ```bash
   curl -X GET http://localhost:8080/api/orders \
     -H "Authorization: Bearer <YOUR_TOKEN>"
   ```

## 📊 Monitoring

- **Prometheus**: http://localhost:9090
- **Grafana**: http://localhost:3000 (admin/admin)
- **RabbitMQ Management**: http://localhost:15672 (guest/guest)

## 🧪 Testing

### Postman Collection

1. Import `docs/ByteBites.postman_collection.json`
2. Set environment variables:
   - `base_url`: http://localhost:8080
   - `auth_token`: (from login response)

### Sample Requests

**Create Order**:
```bash
curl -X POST http://localhost:8080/orders \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <TOKEN>" \
  -d '{
    "items": [
      {"menuItemId": 1, "quantity": 2},
      {"menuItemId": 3, "quantity": 1}
    ],
    "customerId": "cust-123"
  }'
```

**Check Inventory**:
```bash
curl -X GET http://localhost:8080/inventory/ingredients \
  -H "Authorization: Bearer <TOKEN>"
```

## 🗺️ Event Flows

### Order Processing Flow
```
1. [Order Service] → Order Created → [RabbitMQ]
2. [Inventory Service] ← Order Event ← [RabbitMQ]
3. [Notification Service] ← Order Update ← [RabbitMQ]
```

### Notification Flow
```
1. [Any Service] → Notification Request → [RabbitMQ]
2. [Notification Service] processes request
3. [Notification Service] → Email/SMS/Push
```

## 🚨 Troubleshooting

**Common Issues**:

1. **Port conflicts**:
   ```bash
   sudo lsof -i :8080 # Check port usage
   ```

2. **Database migrations**:
   ```bash
   mvn flyway:migrate -pl order-service
   ```

3. **RabbitMQ connection**:
   ```bash
   docker-compose restart rabbitmq
   ```

## 📜 License

MIT License - See [LICENSE](LICENSE) for details.
