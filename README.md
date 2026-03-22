# Config-Server

Centralized external configuration server for the ECA microservices ecosystem. It serves configuration properties to all registered services from a single source, enabling environment-specific settings without redeployment.

## About

The Config-Server provides centralized externalized configuration management for the entire ECA microservices ecosystem. It serves configuration properties from a native filesystem backend, enabling environment-specific settings to be managed independently without requiring service redeployment. All microservices fetch their bootstrap configuration from this server at startup.

## Tech Stack

| Technology | Details |
|---|---|
| Java | 25 |
| Spring Boot | 4.0.3 |
| Spring Cloud | 2025.1.0 |
| Spring Cloud Config Server | Native filesystem backend |
| Spring Boot Actuator | Health & management endpoints |

## How It Works

The Config-Server uses a **native filesystem** backend, loading configuration files from the classpath. All microservices bootstrap by importing their configuration from this server before starting up.

### Configuration Layout

```
src/main/resources/configurations/
├── application.yaml          # Shared config for all services (Eureka URL, logging)
├── platform/
│   ├── api-gateway.yaml      # Api-Gateway routes & CORS config
│   └── service-registry.yaml # Eureka server settings
└── services/
    ├── iam-service.yaml      # IAM Service datasource (MySQL)
    ├── product-service.yaml  # Product Service datasource (MongoDB)
    └── order-service.yaml    # Order Service datasource (MySQL)
```

## Service Details

| Property | Value |
|---|---|
| Port | `9000` |
| Artifact ID | `Config-Server` |
| Group ID | `lk.ijse.eca` |

## Getting Started

Follow the lecture guidelines, refer to the lecture video for more information and how to get started correctly.

> **Important:** Config-Server must be started **first** before any other service, as all other services fetch their configuration from this server on startup.

```bash
./mvnw spring-boot:run
```

The server will be available at: `http://localhost:9000`

You can verify a service's configuration is being served correctly by visiting:
```
http://localhost:9000/{service-name}/default
```

## Troubleshoot

| Issue | Solution |
|---|---|
| Config-Server fails to start | Check if port 9000 is available and not blocked by another process |
| Services fail to fetch config | Verify Config-Server is fully started before launching other services |
| Configuration not updating | Confirm file paths in `spring.cloud.config.server.native.search-locations` are correct |
| 404 on config endpoint | Check that YAML files exist in `src/main/resources/configurations/` with correct naming |
| Git backend issues | Currently using native filesystem backend; ensure config files are in classpath |
| Profile-specific config not loading | Verify profile name matches the file suffix (e.g., `application-dev.yaml`) |
