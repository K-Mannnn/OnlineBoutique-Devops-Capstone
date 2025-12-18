

# Phase 1 – CI/CD Pipeline for CartService (Azure DevOps)

## Overview

This project represents **Phase 1** of my DevOps learning and portfolio journey. The goal of this phase was to build a **fully working CI/CD pipeline from scratch** for a real-world microservice using **Azure DevOps**, focusing on fundamentals rather than production-scale complexity.

The service deployed is **CartService**, a backend **.NET gRPC microservice**, taken from a larger microservices demo application. In this phase, the emphasis is on **source control, branching strategy, CI pipelines, CD automation, and Azure integration**.

---

## Objectives of Phase 1

* Set up professional **Git branching strategy**
* Build a **CI pipeline from scratch using YAML**
* Understand restore, build, test, and publish workflows for .NET
* Automate deployment using **Azure DevOps CD pipelines**
* Deploy to **Azure App Service**
* Learn real-world platform limitations and trade-offs

---

## Technology Stack

* **Source Control:** Azure Repos (mirrored to GitHub for portfolio)
* **CI/CD:** Azure DevOps Pipelines (YAML)
* **Application:** .NET gRPC CartService
* **Cloud Platform:** Azure App Service
* **Build Agent:** Microsoft-hosted Azure Pipelines agent

---

## Repository Structure

```
CartService/
├── src/
│   └── cartservice/
    └── appsettings.json
├── tests/
├── azure-pipelines.yml
└── README.md
```

---

## Branching Strategy

This project follows a simplified but realistic Git workflow:

* **main** → stable, deployable branch
* **feature/*** → used for development work

Workflow:

1. Develop changes in a feature branch
2. Merge into `main`
3. CI/CD pipeline triggers automatically on `main`

This mirrors real-world DevOps practices used by teams to maintain stability while enabling continuous delivery.

---

## CI Pipeline (Build Stage)

The CI pipeline was written **entirely in YAML**, without using the visual designer.

### CI Steps:

1. **Restore dependencies** (`dotnet restore`)
2. **Build the application** (`dotnet build`)
3. **Run automated tests** (`dotnet test`)
4. **Publish build output** (`dotnet publish`)
5. **Publish build artifacts** for use in CD

Key learning outcome: understanding how Azure DevOps tasks map directly to underlying CLI commands.

---

## CD Pipeline (Deploy Stage)

The CD stage consumes the published artifact and deploys it to **Azure App Service**.

### CD Concepts Covered:

* Artifact download
* Deployment automation
* Environment configuration
* Separation of CI and CD concerns

The pipeline demonstrates how deployment can be fully automated once artifacts are produced.

---

## Health Checks & Smoke Testing

A simple HTTP health endpoint (`/healthz`) was added to the service to support **post-deployment verification**.

> Note: Due to platform limitations of Azure App Service with non-containerized gRPC workloads, full runtime verification is addressed in later phases.

This intentionally reflects **real-world engineering trade-offs**, not a toy example.

---

## Key Learnings from Phase 1

* How CI/CD pipelines are built **from first principles**
* Why pipelines can succeed even when applications have runtime constraints
* The difference between **build success** and **application health**
* Platform limitations of Azure App Service for gRPC workloads
* Why containerization is the industry standard for microservices

---

## Why This Matters

This phase focuses on **DevOps fundamentals**, not just getting an app running:

* Repeatability
* Automation
* Source control discipline
* Cloud deployment pipelines

These skills are foundational and transferable across stacks and platforms.

---

## Next Steps (Phase 2)

Phase 2 will build on this foundation by:

* Dockerizing CartService
* Publishing images to a container registry
* Improving CD reliability
* Preparing for Kubernetes-based deployments

---

## Final Notes

This project intentionally prioritizes **learning depth and realism** over surface-level success. The challenges encountered and design decisions made are documented as part of the learning journey.

---

📌 *This repository represents Phase 1 of an evolving DevOps portfolio.*
