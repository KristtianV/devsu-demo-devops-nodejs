# Demo DevOps NodeJS

This project is a REST API built in Node.js used as a technical assessment for DevOps practices.

It has been extended to include a full CI/CD pipeline, Dockerization, Kubernetes deployment, and security scanning.

---

## Getting Started

### Prerequisites

- Node.js 18.15.0 or higher
- Docker
- Kubernetes (Minikube or Docker Desktop Kubernetes)
- Git

---

### Installation

Clone this repo:

```bash
git clone https://github.com/<your-repo>/devsu-demo-devops-nodejs.git
```

Install dependencies.

```bash
npm i
```

### Database

The database is a local SQLite file generated automatically on first execution:
```bash
dev.sqlite
```
No external database is required.

## Usage

To run tests you can use this command.

```bash
npm run test
```

To run locally the project you can use this command.

```bash
npm run start
```

Open http://localhost:8000/api/users with your browser to see the result.

### Features

These services can perform,

#### Health Check (ADDED)

A health endpoint was added for monitoring and Kubernetes probes.
```bash
GET /health
```
Response:

```json
{
  "status": "UP"
}
```
Used for:

readiness/liveness probes in Kubernetes
basic service monitoring
load balancer health checks

A health endpoint was added for monitoring and Kubernetes probes.

#### Create User

To create a user, the endpoint **/api/users** must be consumed with the following parameters:

```bash
  Method: POST
```

```json
{
    "dni": "dni",
    "name": "name"
}
```

If the response is successful, the service will return an HTTP Status 200 and a message with the following structure:

```json
{
    "id": 1,
    "dni": "dni",
    "name": "name"
}
```

If the response is unsuccessful, we will receive status 400 and the following message:

```json
{
    "error": "error"
}
```

#### Get Users

To get all users, the endpoint **/api/users** must be consumed with the following parameters:

```bash
  Method: GET
```

If the response is successful, the service will return an HTTP Status 200 and a message with the following structure:

```json
[
    {
        "id": 1,
        "dni": "dni",
        "name": "name"
    }
]
```

#### Get User

To get an user, the endpoint **/api/users/<id>** must be consumed with the following parameters:

```bash
  Method: GET
```

If the response is successful, the service will return an HTTP Status 200 and a message with the following structure:

```json
{
    "id": 1,
    "dni": "dni",
    "name": "name"
}
```

If the user id does not exist, we will receive status 404 and the following message:

```json
{
    "error": "User not found: <id>"
}
```

If the response is unsuccessful, we will receive status 400 and the following message:

```json
{
    "errors": [
        "error"
    ]
}
```

# ------------------------------------------------------------

# DEVOPS EXTENSIONS (ADDED BEYOND ORIGINAL REQUIREMENT)

# ------------------------------------------------------------

## CI/CD Pipeline

A GitHub Actions pipeline was implemented to automate the software delivery lifecycle.

Stages:

* Code Build (npm ci)
* Unit Tests (Jest)
* Code Coverage
* Static Code Analysis (lint placeholder)
* Docker image build
* Docker image push to registry
* Vulnerability scanning (Trivy)
* Kubernetes deployment (manifest execution)

---

## Testing (ADDED)

Additional test cases were implemented to improve coverage and validate business logic.

Added scenarios:

* User not found (404)
* Duplicate user creation (400)
* Successful user creation
* API response validation improvements

Frameworks:

* Jest
* Supertest

Execution:

```bash
npm run test
```

Coverage:

```bash
npx jest --coverage
```

---

## Docker

Application is containerized using Docker.

Image published to Docker Hub:

```
kristtiandocker/devsu-demo
```

---

## Kubernetes Deployment

The application is deployed locally using Minikube / Docker Desktop Kubernetes.

Resources:

* Deployment (2 replicas)
* Service (ClusterIP)
* ConfigMap
* Secret
* Ingress (NGINX)

Access:

```
http://devsu.local
```

Note:
In local environments (Windows + Minikube), external ingress access may require port-forwarding or tunnel configuration.

---

## Security Scanning

A vulnerability scan was integrated using Trivy in the CI/CD pipeline.

It detects known CVEs in dependencies and container layers.

Example finding:

* validator@13.9.0 (CVE-2025-12758)

This is reported as part of the security quality gate.

---

## Deployment Strategy

The deployment stage is defined in the CI/CD pipeline.

Due to local Kubernetes limitations, execution is validated using:

```bash
kubectl apply -f k8s/
```

or simulated in CI environments without cluster access.

---

## Architecture Overview

```
GitHub Repository
        |
        v
GitHub Actions CI/CD
        |
        v
Docker Image (Docker Hub)
        |
        v
Kubernetes Cluster (Minikube)
        |
        +--> Deployment (2 replicas)
        +--> Service
        +--> Ingress
```

---


## License

Copyright © 2023 Devsu. All rights reserved.
