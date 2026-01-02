# dev

Infrastructure and core services for local development of Go-based microservices.

## Infrastructure services

Infrastructure services provide the core technical capabilities required for running the Core and other Services. They supply data storage, messaging infrastructure, as well as observability and administration tooling.

### Start services

```
docker compose --profile infra up -d
```

| Service | Description |
|---------|-------------|
| postgres | Provides the PostgreSQL database for core services. Exposes port `5432` and uses persistent volume for data. Default user: `postgres`. Default password: `password`. |
| pgadmin | Web-based administration interface for PostgreSQL. Connects to postgres and exposes port `5050`. Default email: `root@microcore.dev`. Default password: `password`. |
| redis | In-memory key-value store for caching and fast data access. Exposes port `6379`. Default password: `password`. |
| tempo | Distributed tracing backend for telemetry and observability. |
| mimir | Long-term metrics storage and query engine for Prometheus metrics. |
| loki | Log aggregation and storage backend, compatible with Grafana. |
| alloy | OpenTelemetry collector and storage server. Exposes ports `12345` (ui) and `4317` (collector). |
| grafana | Visualization and dashboard tool. Connects to all observability backends. Exposes port `3000`. |
| kafka | Distributed message broker for event-driven communication. Exposes port `9092`. |
| kafka-ui | Web-based management interface for Kafka. Connects to `kafka` and exposes port `18080`. |

## Core services

The Core Services are the main application components of the system. Each service runs in its own container and provides a specific domain of functionality:

| Service | Description |
|---------|-------------|
| auth-service-server | Handles authentication, JWT issuance, and user sessions. |
| notifications-service-server | Manages sending notifications, emails, and event-driven messages. |
| users-service-server | Handles user management, including profiles, roles, and OTPs. |
| files-service-server | Provides file storage and access, supporting WebDAV and other storage drivers. |

All core services are connected to the shared infra network, allowing them to communicate with infrastructure services like Postgres, Redis, Kafka, and telemetry services. They are configured to restart automatically and expose HTTP ports for local development.

### Run database migrations for the services

```
docker compose --profile migrations up -d
```

### Start the core services

```
docker compose --profile services up -d
```

### Populate initial data for the core services

```
docker compose --profile seed up -d
```

## Gateway

Start the gateway/proxy (Nginx) to access the core services:

```
docker compose --profile gateway up -d
```

## Notes

- Profiles allow you to start only the containers you need.
- Containers that perform migrations or seeding can be removed after completion since they run only once.
- All services are connected to the shared infra network, so they can communicate using container names.

## License

This project is licensed under the terms of the [MIT License](LICENSE).

Copyright © 2026 Microcore
