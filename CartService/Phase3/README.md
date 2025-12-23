# Phase 3 – Docker & Containerization for CartService

## Overview

In Phase 3, the focus is on **containerizing the CartService** microservice and integrating **Redis caching**, then deploying it to **Azure App Service** using Docker and Azure Container Registry (ACR). This phase builds on the previous CI/CD setup but moves towards **cloud-native, containerized deployment**.

---

## Objectives

* Create a **basic Dockerfile** for CartService
* Run the microservice in a **Docker container locally**
* Integrate **Redis caching**
* Use **Docker Compose** for multi-container setup
* Push Docker image to **Azure Container Registry (ACR)**
* Deploy containerized service to **Azure App Service**
* Verify container health and connectivity to Redis

---

## Technology Stack

* **Containerization:** Docker, Docker Compose
* **Application:** .NET gRPC CartService
* **Cache:** Redis
* **CI/CD:** Azure DevOps Pipelines
* **Cloud Platform:** Azure App Service, Azure Container Registry

---

## Local Docker Setup

### Build and Run CartService Container

```bash``
# Build Docker image
docker build -f src/dockerfile.basic -t cartservice:basic .

# Run container locally
docker run -p 7070:7070 -t cartservice:basic


# Verify Service

curl http://localhost:8080/healthz

You should see a health confirmation from the microservice.

# Docker Compose Setup
``Yaml``
version: '3.8'
services:
  cartservice:
    build:
      context: .
      dockerfile: src/dockerfile.basic
    ports:
      - "7070:7070"
    environment:
      - REDIS_ADDR=redis:6379
    depends_on:
      - redis
  redis:
    image: redis:latest
    ports:
      - "6379:6379"

``bash``

docker-compose up

* Runs CartService and Redis together

* Verifies connectivity between microservice and Redis

# Azure Deployment

* Push Docker image to Azure Container Registry (ACR)

* Configure App Service to use the container image from ACR

* Set Redis endpoint as an environment variable in App Service

* Verify container logs in Azure portal to confirm the service is running and connected to Redis

# Key Learning Outcomes

* Building Docker images for .NET microservices

* Running multi-container setups with Docker Compose

* Pushing images to Azure Container Registry

* Deploying containerized applications to Azure App Service

* Integrating external dependencies (Redis) with containers


# Next Steps

* Phase 4: Full CI/CD for containerized microservices

    Automated Docker builds and pushes

    Multi-environment deployments (Dev/Prod)

    Image versioning and rollback strategies

* Phase 5: Kubernetes Deployment

    Orchestrate multiple microservices

    Explore scaling, service discovery, and networking


