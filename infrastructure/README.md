# Infrastructure Helm Charts

This directory contains Helm charts for the infrastructure components of the voting application using open source images.

## Components

### PostgreSQL (`postgresql/`)
- **Image**: `postgres:15-alpine`
- **Purpose**: Database for storing vote results
- **Default Configuration**:
  - Database: `votes`
  - User: `okteto`
  - Password: `okteto`
  - Port: `5432`

### Zookeeper (`zookeeper/`)
- **Image**: `zookeeper:3.8`
- **Purpose**: Coordination service for Kafka
- **Default Configuration**:
  - Port: `2181`
  - Single node setup

### Kafka (`kafka/`)
- **Image**: `confluentinc/cp-kafka:7.4.0`
- **Purpose**: Message broker for vote events
- **Default Configuration**:
  - Internal port: `9092`
  - External port: `9093`
  - Depends on Zookeeper

## Deployment

### Deploy Individual Services

```bash
# Deploy PostgreSQL
helm install postgresql ./infrastructure/postgresql/

# Deploy Zookeeper
helm install zookeeper ./infrastructure/zookeeper/

# Deploy Kafka (requires Zookeeper to be running)
helm install kafka ./infrastructure/kafka/
```

### Deploy All Services

```bash
# Deploy in order (Zookeeper first, then others)
helm install zookeeper ./infrastructure/zookeeper/
helm install postgresql ./infrastructure/postgresql/
helm install kafka ./infrastructure/kafka/
```

## Configuration

Each service can be customized by modifying the `values.yaml` file or by providing custom values:

```bash
# Example: Deploy PostgreSQL with custom password
helm install postgresql ./infrastructure/postgresql/ --set database.password=mypassword
```

## Persistent Storage

All services are configured with persistent storage by default:
- PostgreSQL: 8Gi
- Kafka: 8Gi  
- Zookeeper: 4Gi (data) + 4Gi (logs)

To disable persistence:

```bash
helm install postgresql ./infrastructure/postgresql/ --set persistence.enabled=false
```

## Upgrading from Bitnami Images

The Docker Compose configuration has been updated to use these open source images:
- `bitnami/postgresql:14` → `postgres:15-alpine`
- `bitnami/kafka:3.1` → `confluentinc/cp-kafka:7.4.0`
- `bitnami/zookeeper:3.8` → `zookeeper:3.8`

Environment variables have been adjusted to match the new images' requirements.