# Micro Warehouse API

A microservices-based warehouse management system built with Go, featuring modular architecture with separate services for users, products, merchants, transactions, warehouses, and notifications.

## 🏗️ Architecture

The system is built using a microservices architecture with the following services:

### Services

1. **API Gateway** (Port 8080)
   - Central entry point for all API requests
   - Authentication and authorization
   - Rate limiting using Redis
   - Routes requests to appropriate microservices

2. **User Service** (Port 8081)
   - User management and authentication
   - Role-based access control (RBAC)
   - JWT token generation and validation
   - User profile management

3. **Product Service** (Port 8082)
   - Product catalog management
   - Category management
   - Product image uploads (Supabase)
   - Product search and filtering

4. **Merchant Service** (Port 8083)
   - Merchant profile management
   - Merchant product assignments
   - File uploads and management

5. **Warehouse Service** (Port 8084)
   - Warehouse management
   - Warehouse product stock management
   - Inventory tracking

6. **Transaction Service** (Port 8085)
   - Transaction processing
   - Order management
   - Midtrans payment integration
   - Transaction history and reporting

7. **Notification Service** (Port 8086)
   - Email notifications
   - RabbitMQ message consumer
   - Event-driven notifications

## 🛠️ Technology Stack

- **Language**: Go 1.21+
- **Database**: PostgreSQL (multiple instances for each service)
- **Message Queue**: RabbitMQ
- **Caching**: Redis
- **File Storage**: Supabase Storage
- **Payment Gateway**: Midtrans
- **Containerization**: Docker & Docker Compose
- **API Framework**: Fiber/Echo (Go web framework)

## 📋 Prerequisites

- Go 1.21 or higher
- PostgreSQL 13+
- RabbitMQ
- Redis
- Docker & Docker Compose (for containerized deployment)
- Supabase account (for file storage)
- Midtrans account (for payment processing)

## 🚀 Quick Start

### Using Docker Compose

```bash
# Clone the repository
git clone https://github.com/lana-techn/micro-warehouse-api-main.git
cd micro-warehouse-api-main

# Start all services with Docker Compose
docker-compose up -d

# Check logs
docker-compose logs -f
```

### Manual Setup

1. **Setup Environment Variables**

   Create `.env` files in each service directory:
   
   ```bash
   # Example for user-service
   cp user-service/env.example user-service/.env
   ```

   Configure the following for each service:
   - `APP_ENV`: development/production
   - `APP_PORT`: service port
   - `DATABASE_*`: PostgreSQL credentials
   - `RABBITMQ_*`: RabbitMQ credentials
   - `REDIS_*`: Redis credentials
   - `SUPABASE_*`: Supabase credentials

2. **Setup Databases**

   ```bash
   # Create databases for each service
   createdb warehouse_user_db
   createdb warehouse_product_db
   createdb warehouse_merchant_db
   createdb warehouse_transaction_db
   createdb warehouse_warehouse_db
   createdb warehouse_notification_db
   ```

3. **Run Services**

   ```bash
   # Navigate to a service and run
   cd user-service
   go run main.go start
   
   # Repeat for other services in different terminals
   ```

## 📦 Service Dependencies

```
API Gateway (8080)
├── User Service (8081)
├── Product Service (8082)
├── Merchant Service (8083)
├── Warehouse Service (8084)
├── Transaction Service (8085)
└── Notification Service (8086)

External Services:
├── PostgreSQL (multiple instances)
├── RabbitMQ
├── Redis
├── Supabase Storage
└── Midtrans
```

## 🔐 Authentication

All services use JWT (JSON Web Tokens) for authentication. The flow:

1. User logs in via User Service
2. JWT token is generated
3. Token is included in Authorization header for subsequent requests
4. API Gateway validates token and authorizes requests

## 📡 API Endpoints

### User Service
- `POST /api/v1/auth/login` - User login
- `POST /api/v1/auth/register` - User registration
- `GET /api/v1/auth/me` - Get current user
- `GET /api/v1/users` - List users
- `POST /api/v1/users` - Create user
- `PUT /api/v1/users/:id` - Update user
- `DELETE /api/v1/users/:id` - Delete user
- `GET /api/v1/roles` - List roles
- `POST /api/v1/roles` - Create role

### Product Service
- `GET /api/v1/products` - List products
- `POST /api/v1/products` - Create product
- `PUT /api/v1/products/:id` - Update product
- `DELETE /api/v1/products/:id` - Delete product
- `GET /api/v1/categories` - List categories
- `POST /api/v1/categories` - Create category
- `POST /api/v1/upload` - Upload product image

### Merchant Service
- `GET /api/v1/merchants` - List merchants
- `POST /api/v1/merchants` - Create merchant
- `PUT /api/v1/merchants/:id` - Update merchant
- `DELETE /api/v1/merchants/:id` - Delete merchant
- `GET /api/v1/merchant-products/:id` - List merchant products
- `POST /api/v1/merchant-products/:id/assign` - Assign product to merchant

### Warehouse Service
- `GET /api/v1/warehouses` - List warehouses
- `POST /api/v1/warehouses` - Create warehouse
- `PUT /api/v1/warehouses/:id` - Update warehouse
- `DELETE /api/v1/warehouses/:id` - Delete warehouse
- `GET /api/v1/warehouse-products/:id` - List warehouse products
- `POST /api/v1/warehouse-products/:id/assign` - Assign product to warehouse

### Transaction Service
- `GET /api/v1/transactions` - List transactions
- `POST /api/v1/transactions` - Create transaction
- `GET /api/v1/transactions/:id` - Get transaction details
- `PUT /api/v1/transactions/:id/payment` - Process payment

## 🔄 Message Queue (RabbitMQ)

Services communicate asynchronously using RabbitMQ for:
- Email notifications
- Order confirmations
- Inventory updates

## 📊 Database Schema

Each service has its own database with relevant tables:

- **User Service**: users, roles, user_roles
- **Product Service**: products, categories
- **Merchant Service**: merchants, merchant_products
- **Warehouse Service**: warehouses, warehouse_products
- **Transaction Service**: transactions, transaction_products
- **Notification Service**: notifications (logs)

## 🧪 Testing

```bash
# Run tests for a service
cd user-service
go test ./...
```

## 📝 Logging

Logs are written to console and can be configured in each service's config file. For production, consider using centralized logging solutions like ELK Stack or Datadog.

## 🐛 Troubleshooting

### Services not starting
- Check Docker containers: `docker-compose ps`
- View logs: `docker-compose logs <service-name>`
- Ensure all ports are available

### Database connection errors
- Verify PostgreSQL is running: `psql -U postgres`
- Check environment variables in `.env` files
- Ensure databases are created

### RabbitMQ connection errors
- Verify RabbitMQ is running
- Check RabbitMQ credentials in environment variables
- Access RabbitMQ management console: http://localhost:15672

### Redis connection errors
- Verify Redis is running: `redis-cli ping`
- Check Redis credentials

## 📚 Documentation

For detailed documentation on each service, refer to individual service README files:
- [User Service](./user-service/README.md)
- [Product Service](./product-service/README.md)
- [Merchant Service](./merchant-service/README.md)
- [Warehouse Service](./warehouse-service/README.md)
- [Transaction Service](./transaction-service/README.md)
- [Notification Service](./notification-service/README.md)
- [API Gateway](./api-gateway/README.md)

## 🤝 Contributing

1. Create a feature branch
2. Make your changes
3. Test thoroughly
4. Submit a pull request

## 📄 License

This project is proprietary and confidential.

## 👥 Team

- **Owner**: lana-techn
- **Repository**: https://github.com/lana-techn/micro-warehouse-api-main

## 📞 Support

For issues or questions, please create an issue in the GitHub repository.

---

**Last Updated**: November 2025
**Version**: 1.0.0
