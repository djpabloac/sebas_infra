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
  - *RabbitMQ 4.3.0*: Message broker queue with admin interface, aligned with staging/prod (image `rabbitmq:4.3.0-management`, verified on Erlang/OTP 27.3.4.11; the tag pins RabbitMQ, not the Erlang patch)

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

### Upgrading RabbitMQ from 4.0

The queue stack now sets a fixed `hostname: rabbitmq`. RabbitMQ stores its data under the node name (`rabbit@<hostname>`), so existing data in `data/rabbitmq` created by the 4.0 container (node `rabbit@<container-id>`) is not picked up and the broker starts empty. Choose one:

- **Local data not needed:** stop the stack, remove `data/rabbitmq` and start it again.
- **Keep queues, exchanges and users:** export the definitions from the 4.0 container before upgrading and import them into 4.3 (queued messages are not kept):

```bash
docker exec sebas_rabbitmq rabbitmqctl export_definitions /tmp/defs.json
docker cp sebas_rabbitmq:/tmp/defs.json ./rabbit-defs.json
# after starting 4.3.0
docker cp ./rabbit-defs.json sebas_rabbitmq:/tmp/defs.json
docker exec sebas_rabbitmq rabbitmqctl import_definitions /tmp/defs.json
```

---

**Tip:** Use the absolute or relative path to refer to any compose file you wish to start. Replace `queue/docker-compose.yml` with any other path, such as `database/docker-compose.yml` or `ci/docker-compose.yml`, to launch different stacks.
