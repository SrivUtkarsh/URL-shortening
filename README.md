# <div align="center">URL Shortener</div>

<div align="center">

<img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python" />
<img src="https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white" alt="FastAPI" />
<img src="https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white" alt="Redis" />
<img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white" alt="PostgreSQL" />
<img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker" />
<img src="https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white" alt="Nginx" />

<br><br>

**A fast, scalable URL shortening service built with modern backend technologies.**

</div>

---

> **Project Status: Work in Progress**
>
> This project is actively under development, with ongoing improvements to authentication, rate limiting, monitoring, performance optimization, and database reliability.

## Overview

A backend URL shortening service that converts long URLs into short, shareable links. It uses Redis caching to reduce repeated database lookups and supports URL redirection, custom aliases, expiration, and click analytics.

The project combines REST API development, database integration, caching, reverse proxying, and containerization.

## Features

- **URL Shortening** — Convert long URLs into short, shareable links.
- **Fast Redirection** — Redirect users to their original URLs using unique short codes.
- **Redis Caching** — Cache frequently accessed URLs to reduce database queries and improve response times.
- **Custom Aliases** — Support personalized short codes for URLs.
- **URL Expiration** — Support expiration for shortened URLs.
- **Click Analytics** — Track the number of times a shortened URL is accessed.
- **RESTful APIs** — Expose URL management functionality through FastAPI.
- **Reverse Proxy** — Use Nginx to route incoming requests to the application.
- **Containerization** — Use Docker to package and run application services.
- **Interactive API Documentation** — Explore and test endpoints using Swagger UI.

## Tech Stack

| Technology | Purpose |
|:---|:---|
| Python | Backend development |
| FastAPI | REST API framework |
| PostgreSQL | Relational database |
| Redis | URL caching |
| Nginx | Reverse proxy and request routing |
| Docker | Containerization |
| SQLAlchemy | Database ORM, if configured |
| Swagger UI | API documentation and testing |


## System Architecture

```text
                    ┌──────────────┐
                    │    Client    │
                    └──────┬───────┘
                           │
                           ▼
                ┌────────────────────┐
                │   Nginx Reverse    │
                │       Proxy        │
                └─────────┬──────────┘
                          │
                          ▼
                ┌────────────────────┐
                │   FastAPI App      │
                │                    │
                │ URL Shortening     │
                │ Redirection        │
                │ Click Analytics    │
                └─────────┬──────────┘
                          │
                 ┌────────┴────────┐
                 │                 │
                 ▼                 ▼
        ┌────────────────┐  ┌────────────────┐
        │  Redis Cache   │  │   PostgreSQL   │
        │                │  │                │
        │ Cached URLs    │  │ Persistent URL │
        │                │  │ Records        │
        └────────────────┘  └────────────────┘
```

### Request Flow

1. The client sends an HTTP request through Nginx.
2. Nginx forwards the request to the FastAPI application.
3. For URL redirection, FastAPI checks Redis for the requested short code.
4. If the URL is not cached, FastAPI retrieves the corresponding record from PostgreSQL.
5. The URL can then be cached in Redis for subsequent requests.
6. FastAPI redirects the client to the original destination.

*The diagram illustrates the intended architecture. The actual request flow depends on the application's current implementation.*
### Request Flow

1. The client sends an HTTP request through Nginx.
2. Nginx forwards the request to the FastAPI application.
3. For a redirection request, the application checks Redis for the short code.
4. If the URL is not cached, the application retrieves the corresponding record from the database.
5. The retrieved URL can be cached in Redis for subsequent requests.
6. The application redirects the client to the original destination.

*The diagram represents the intended cache-aside flow; the actual request flow depends on the current implementation.*

## Getting Started

### Prerequisites

Ensure the following tools are installed and configured as required by your setup:

- Python 3.10+
- Git
- Docker
- Redis
- PostgreSQL, if used as the primary database

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
cd YOUR_REPOSITORY
```

Replace `YOUR_USERNAME` and `YOUR_REPOSITORY` with your GitHub username and repository name.

### 2. Create a Virtual Environment

```bash
python -m venv venv
```

Activate the environment.

**Windows — PowerShell**

```powershell
.\venv\Scripts\Activate.ps1
```

**Linux / macOS**

```bash
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure Environment Variables

Create a `.env` file in the project root and add the connection settings expected by your application.

Example:

```env
DATABASE_URL=your_database_connection_string
REDIS_URL=redis://localhost:6379/0
```

Update the variable names and values to match your application configuration. If services run in Docker containers, use the appropriate service hostnames.

**Security:** Never commit database credentials, API keys, or other secrets to GitHub.

### 5. Start the Application

If the FastAPI instance is named `app` in `main.py`, run:

```bash
uvicorn main:app --reload
```

### 6. Access the API Documentation

When running directly on port `8000`, open:

[http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)

If Nginx exposes the application on port `8080`, open:

[http://localhost:8080/docs](http://localhost:8080/docs)

Use the URL corresponding to your current configuration.

## Running with Docker

If your repository contains a `Dockerfile`, build the image:

```bash
docker build -t url-shortener .
```

Run the application container:

```bash
docker run --name url-shortener -p 8000:8000 url-shortener
```

For a multi-container setup, configure networking between FastAPI, Redis, PostgreSQL, and Nginx. Docker Compose can be used to manage these services together if a Compose configuration is available.

## API Endpoints

The following table describes the core operations. Verify the exact routes and HTTP methods against your implementation.

| Operation | Method | Description |
|:---|:---:|:---|
| Create short URL | `POST` | Generate a short URL from a long URL |
| Redirect to original URL | `GET` | Resolve a short code and redirect the client |
| Retrieve analytics | `GET` | Retrieve click statistics |
| Create custom alias | `POST` | Create a short URL with a custom alias |

### Example Request

A typical URL creation request might use the following JSON body:

```json
{
  "url": "https://example.com/some/long/path"
}
```

The response schema depends on the implementation.

### Example Redirection

```text
http://localhost:8080/{short_code}
```

Replace `{short_code}` with the generated short code.

For the exact request schemas, response formats, and endpoint paths, refer to the interactive API documentation at `/docs`.

## Performance and Scalability

The project incorporates backend techniques intended to improve efficiency and support future scaling.

- **Redis Caching:** Reduces repeated database lookups for frequently accessed URLs.
- **Nginx Reverse Proxy:** Provides a centralized entry point for incoming HTTP requests.
- **Database Persistence:** Stores URL records and associated metadata.
- **Docker Containerization:** Simplifies service configuration and deployment.
- **Performance Evaluation:** Provides a foundation for measuring latency, throughput, and error rates under load.

Actual performance and concurrency limits should be established through benchmarking and load testing.

---

<div align="center">

**Built with FastAPI, Redis, PostgreSQL, Nginx, and Docker.**

</div>
