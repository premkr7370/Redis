Redis — 

A hands-on learning repository covering **Redis fundamentals and practical Redis use cases** in a Node.js/Express backend. This level builds on the Docker concepts from Level 1 and demonstrates how to run Redis with Docker Compose and integrate it into a backend application.

> **Scope:** This README documents the Level 2 Redis material in the supplied lesson transcript. It is a learning/practice project, not a production-ready application.

## Overview

Redis is an in-memory key-value data store commonly used when an application needs very fast access to frequently requested or temporary data.

The lesson uses a shop-counter analogy to explain caching: instead of repeatedly going to the main database for popular data, the server first checks Redis. If the data is cached, it can be returned quickly. If it is not cached, the server fetches it from the database and stores it in Redis for later requests.

## Topics Covered

### 1. Redis Fundamentals
- What Redis is and why in-memory storage is useful.
- How Redis can work alongside a primary database such as MongoDB.
- Cache hits and cache misses.
- The cache-aside flow:
  1. Client sends a request to the backend.
  2. Backend checks Redis for the requested data.
  3. On a cache hit, the cached data is returned.
  4. On a cache miss, the backend fetches data from MongoDB.
  5. The fetched data is stored in Redis and returned to the client.

### 2. Running Redis with Docker Compose
- Using the Redis Docker image.
- Creating a `docker-compose.yml` file.
- Mapping Redis port `6379` to the local machine.
- Starting the Redis container with Docker Compose.
- Keeping Redis available locally for the backend application.

### 3. Node.js and MongoDB Backend
- Creating a basic Node.js application with Express.
- Using environment variables for configuration.
- Connecting to MongoDB Atlas with Mongoose.
- Creating a simple User schema/model.
- Starting the backend with Nodemon.
- Testing backend API responses with an API client.

### 4. Redis Data Operations
- Connecting the Node.js backend to Redis.
- Setting, getting, and deleting values.
- Understanding that values stored as strings may need JSON serialization before storing and JSON parsing after retrieval.
- Using Redis keys to retrieve data for a particular user or purpose.

### 5. API Caching
- Fetching data from MongoDB when it is not cached.
- Saving the result in Redis.
- Returning cached data on subsequent requests.
- Comparing the example response times shown in the lesson to understand the performance benefit of a cache hit.

### 6. OTP Storage
- Storing an OTP against an identifier such as an email address.
- Building a Redis key using a prefix and the email address.
- Retrieving the OTP for verification.
- Using an expiry time so temporary OTP data does not remain indefinitely.

### 7. Rate Limiting
- Why APIs need request limits.
- Tracking requests with Redis.
- Blocking requests after the configured limit is exceeded during a time window.
- Using key expiration so the request counter resets after the window.

### 8. Background Jobs with BullMQ
- Understanding queues and workers.
- Creating a BullMQ queue backed by Redis.
- Adding a job to a queue from the backend.
- Processing queued jobs in a worker.
- Demonstrating an email-related background job.
- Keeping the main request responsive while longer work runs in the background.

### 9. Redis and AI Memory
- Redis is introduced as a possible place to keep application memory/context for AI-related use cases.
- The transcript mentions this use case; it does not provide a complete production AI-memory architecture.

## Technology Stack

| Technology | Purpose in the lesson |
|---|---|
| Redis | In-memory data store, caching and queue backing store |
| Docker | Running Redis locally in a container |
| Docker Compose | Defining and starting the Redis service |
| Node.js | Backend runtime |
| Express.js | HTTP API |
| MongoDB Atlas | Primary database in the practical backend |
| Mongoose | MongoDB object modeling |
| BullMQ | Background job queue and worker |
| Nodemon | Restarting the development server after code changes |
| Thunder Client | Testing backend endpoints |

## Prerequisites

- Node.js and npm
- Docker Desktop
- A MongoDB Atlas database/connection string
- A code editor such as Visual Studio Code
- An API client for testing endpoints

Basic familiarity with JavaScript, Node.js, Express, MongoDB, and Docker Level 1 concepts is helpful.

## Getting Started

The transcript builds the examples in stages. A typical local setup follows this sequence:

1. Start Docker Desktop.
2. Create a Level 2 working folder.
3. Add a `docker-compose.yml` file to run Redis.
4. Start Redis with Docker Compose.
5. Create the Node.js/Express backend in the practice folder.
6. Configure the MongoDB connection string through an environment variable.
7. Connect the backend to MongoDB and Redis.
8. Run the backend and test the API examples.

### Example Redis Docker Compose Configuration

The lesson maps Redis's default port `6379` to the same local port.

```yaml
services:
  redis:
    image: redis
    ports:
      - "6379:6379"
```

Start the service from the directory containing `docker-compose.yml`:

```bash
docker compose up
```

To run it in the background:

```bash
docker compose up -d
```

To stop the service:

```bash
docker compose down
```

### Example Backend Setup Commands

The lesson uses Express, dotenv, Mongoose, and Nodemon for its Node.js practice backend.

```bash
npm init -y
npm install express dotenv mongoose
npm install --save-dev nodemon
```

The lesson uses an environment variable for the MongoDB connection string. Create a local `.env` file:

```env
PORT=8000
MONGO_DB_URL=your_mongodb_connection_string
```

Add `.env` to `.gitignore`. Do not commit database passwords, API keys, or other secrets.

## Suggested Repository Structure

The transcript creates a Level 2 folder, a Docker Compose file, and a Phase 1 Node.js backend. One clear way to organize the practice files is:

```text
docker-redis-level-2/
├── README.md
├── docker-compose.yml
└── phase-1/
    ├── package.json
    ├── package-lock.json
    ├── .env.example
    ├── .gitignore
    └── src/
        ├── index.js
        ├── config/
        │   └── db.js
        ├── models/
        │   └── user.model.js
        ├── routes/
        ├── middleware/
        ├── workers/
        └── queues/
```

This is a suggested organization for learning; adjust the paths to match the actual code in your repository.

## Important Redis Concepts

| Concept | Meaning |
|---|---|
| In-memory database | Keeps data in memory for fast access |
| Cache hit | Requested data is found in Redis |
| Cache miss | Requested data is not found in Redis |
| TTL / expiry | Time after which a key expires |
| Key-value pair | A key is used to store and retrieve a value |
| Queue | Holds jobs waiting to be processed |
| Worker | Processes jobs from a queue |
| Rate limit | Restricts how many requests can be made in a time window |

## Security and Best Practices

- Keep `.env` files out of version control.
- Use `.env.example` with placeholder values for required variables.
- Do not expose Redis publicly without proper network restrictions and authentication.
- Use suitable expiration times for temporary values such as OTPs and rate-limit counters.
- Avoid caching sensitive data unless the design explicitly protects it.
- Consider cache invalidation when database records are updated or deleted.
- Handle Redis connection failures so the backend can respond predictably.
- For production background jobs, plan retries, failure handling, and monitoring.

## Learning Outcomes

After completing this level, you should be able to:

- Explain why Redis is used alongside a primary database.
- Describe cache hits, cache misses, and a basic cache-aside flow.
- Run Redis locally using Docker Compose.
- Connect a Node.js backend to MongoDB and Redis.
- Perform basic Redis set, get, and delete operations.
- Understand Redis use cases such as API caching, OTP storage, sessions, and rate limiting.
- Explain how BullMQ queues and workers run background jobs.
- Identify AI memory/context storage as another Redis use case introduced in the lesson.

## Further Learning

- [Redis Documentation](https://redis.io/docs/)
- [Redis Docker Image](https://hub.docker.com/_/redis)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [BullMQ Documentation](https://docs.bullmq.io/)
- [Mongoose Documentation](https://mongoosejs.com/docs/)
- [MongoDB Atlas](https://www.mongodb.com/products/platform/atlas)

## Notes

This repository is intended for learning and hands-on practice. The exact files, routes, environment variable names, and implementation details may differ depending on the code you build while following the lesson. The transcript introduces sessions and AI memory as Redis use cases, but the README does not claim a complete implementation of those features.

## License

This repository is for educational purposes. Add a license file if you intend to distribute or reuse the code publicly.
