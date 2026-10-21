# <div align="center">URL Shortener</div>

 <div align="center">

<img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python" />
<img src="https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white" alt="FastAPI" />
<img src="https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white" alt="Redis" />
<img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white" alt="PostgreSQL" />
<img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker" />
<img src="https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white" alt="Nginx" />

</div>

---

This project is actively under development, with ongoing improvements to authentication, rate limiting, monitoring, performance optimization, and database reliability.

A backend project focused on efficient URL redirection, Redis caching, click analytics, and request handling.

**Current status:** Actively under development, with ongoing work on authentication, rate limiting, monitoring, performance optimization, and database reliability.
A URL shortening service built with FastAPI, Redis, PostgreSQL, Nginx, and Docker. The project focuses on building a backend service with fast URL redirection, caching, click analytics, and efficient request handling.

Project Status: Work in Progress

This project is actively under development, with ongoing improvements to authentication, rate limiting, monitoring, performance, and database reliability.

Features

URL Shortening: Convert long URLs into short, shareable links.

Fast Redirection: Redirect users to their original URLs using unique short codes.

Redis Caching: Cache frequently accessed URLs to reduce database queries and improve response times.

Custom Aliases: Support personalized short codes for URLs.

URL Expiration: Configure expiration for shortened URLs.

Click Analytics: Track the number of times a shortened URL is accessed.

RESTful APIs: Expose URL management functionality through FastAPI.

Nginx Reverse Proxy: Route incoming requests to the FastAPI application.

Docker: Containerize application services for consistent development and deployment.

Interactive API Documentation: Test endpoints using FastAPI's Swagger UI.

Tech Stack

Technology

Purpose

Python

Backend application development

FastAPI

REST API framework

PostgreSQL

Relational database

Redis

Caching frequently accessed URLs

Nginx

Reverse proxy and request routing

Docker

Application containerization

SQLAlchemy

Database ORM, if configured

Swagger UI

API documentation and testing

System Architecture

                  Client
                    |
                    v
              Nginx Reverse Proxy
                    |
                    v
                FastAPI App
                    |
              URL Redirection
                    |
              +-----+-----+
              |           |
              v           v
           Redis       PostgreSQL
           Cache       Database

Request Flow

A client sends a request through Nginx.

Nginx forwards the request to the FastAPI application.

For URL redirection, the application checks Redis for the requested short code.

If the URL is not cached, the application retrieves the corresponding record from the database.

The retrieved URL can be cached in Redis for subsequent requests.

FastAPI redirects the client to the original destination.

The actual request flow depends on the application's implemented caching and database logic.

Getting Started

Prerequisites

Make sure you have the following installed:

Python 3.10 or later

Git

Docker

Redis

PostgreSQL

1. Clone the Repository

git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
cd YOUR_REPOSITORY

Replace YOUR_USERNAME and YOUR_REPOSITORY with your GitHub username and repository name.

2. Create a Virtual Environment

python -m venv venv

Activate the virtual environment.

Windows — PowerShell

.\venv\Scripts\Activate.ps1

Linux / macOS

source venv/bin/activate

3. Install Dependencies

pip install -r requirements.txt

4. Configure Environment Variables

Create a .env file in the project root and configure the connection settings required by your application.

Example:

DATABASE_URL=your_database_connection_string
REDIS_URL=redis://localhost:6379/0

Update the variable names and values to match your application's configuration. If services run in Docker containers, use the appropriate service hostnames and ports.

Security: Never commit database credentials, API keys, or other secrets to GitHub.

5. Start the Application

If your FastAPI instance is named app in main.py, run:

uvicorn main:app --reload

6. Access the API Documentation

If the application is running directly on port 8000, open:

http://127.0.0.1:8000/docs

If Nginx exposes the application on port 8080, open:

http://localhost:8080/docs

Use the address that matches your configuration.

Running with Docker

Build the Docker image if your repository includes a Dockerfile:

docker build -t url-shortener .

Run the application container:

docker run --name url-shortener -p 8000:8000 url-shortener

For containerized deployments, configure networking between FastAPI, Redis, PostgreSQL, and Nginx. Docker Compose can be used to manage multiple services together if a Compose configuration is available.

API Endpoints

The service supports URL creation, redirection, and analytics functionality. The table below describes the intended operations; actual paths and HTTP methods depend on the implemented routes.

Operation

Method

Description

Create short URL

POST

Generate a short URL from a long URL

Redirect to original URL

GET

Resolve a short code and redirect the client

Retrieve analytics

GET

Retrieve click statistics

Create custom alias

POST

Create a short URL with a custom alias

Example: Create a Short URL

A typical request body could look like this:

{
  "url": "https://example.com/some/long/path"
}

The response format depends on the application's implementation.

Example: Access a Short URL

http://localhost:8080/{short_code}

Replace {short_code} with the code returned by the URL creation endpoint.

Refer to /docs for the exact request schemas, response formats, and endpoint paths.

Performance and Scalability

The project incorporates backend techniques that can improve performance and support future scaling.

Redis Caching: Reduces repeated database lookups for frequently accessed URLs.

Nginx Reverse Proxy: Provides a centralized entry point for incoming HTTP requests.

Database Persistence: Stores URL records and associated metadata.

Docker Containerization: Simplifies service configuration and deployment.

Performance Evaluation: Provides a foundation for measuring latency, throughput, and error rates under load.

Actual performance and concurrency limits should be evaluated through benchmarking and load testing.
