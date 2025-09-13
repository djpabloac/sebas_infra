# Infra

This directory compose holds several docker-compose environments, organized by functionality. Each subfolder contains a `docker-compose.yml` file describing a group of related services. This modular structure helps you run only what you need, when you need it.

## Structure

- **ci/**
  - *Jenkins*: CI/CD automation server
  - *SonarQube*: Static code analysis

- **database/**
  - *Redis*: In-memory key-value store
  - *MariaDB*: Relational database

- **others/**
  - *Weaviate*: Vector search engine for AI/semantic search

- **queue/**
  - *RabbitMQ*: Message broker queue with admin interface

- **search/**
  - *Elasticsearch*: Search and analytics engine
  - *Kibana*: Visualization for Elasticsearch

## Example Usage

To launch a specific stack, use the `-f` flag with docker-compose. For example, to start the queue services (RabbitMQ):

```bash
# Start RabbitMQ stack
docker-compose -f compose/queue/docker-compose.yml up -d
```

You can adjust any variables or ports by editing the respective `docker-compose.yml` file in its folder.

> All persistent data is mapped relative to a root-level `data` directory (`../../data/`).

---

**Tip:** Use the absolute or relative path to refer to any compose file you wish to start. Replace `queue/docker-compose.yml` with any other path, such as `database/docker-compose.yml` or `ci/docker-compose.yml`, to launch different stacks.
