# 🚀 Kubernetes CRUD App

A full-stack CRUD application containerized with Docker and deployed on Kubernetes, featuring a JavaScript frontend, Node.js backend, and MySQL database.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
  - [Running with Docker Compose](#running-with-docker-compose)
  - [Deploying to Kubernetes](#deploying-to-kubernetes)
- [Kubernetes Architecture](#kubernetes-architecture)
- [Environment Variables](#environment-variables)
- [Contributing](#contributing)

---

## Overview

This project demonstrates how to build, containerize, and orchestrate a full-stack CRUD application using **Kubernetes**. It covers:

- Building and running Docker containers for the frontend and backend
- Setting up a MySQL database with Kubernetes ConfigMaps and Secrets
- Deploying the entire stack to a Kubernetes cluster using YAML manifests

---

## Tech Stack

| Layer      | Technology          |
|------------|---------------------|
| Frontend   | HTML, CSS, JavaScript |
| Backend    | Node.js             |
| Database   | MySQL               |
| Container  | Docker              |
| Orchestration | Kubernetes       |

---

## Project Structure

```
kubernetes-crud/
├── client/                  # Frontend application
├── server/                  # Backend API (Node.js)
├── crudapp.yaml             # Kubernetes deployment for the CRUD app
├── mysql.yaml               # Kubernetes deployment for MySQL
├── mysql-config.yaml        # MySQL ConfigMap (non-sensitive config)
├── mysql-secret.yaml        # MySQL Secret (credentials)
├── docker-compose.yml       # Docker Compose for local development
└── README.md
```

---

## Prerequisites

Make sure you have the following installed:

- [Docker](https://www.docker.com/get-started) & [Docker Compose](https://docs.docker.com/compose/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- A running Kubernetes cluster (e.g., [Minikube](https://minikube.sigs.k8s.io/docs/), [Kind](https://kind.sigs.k8s.io/), or a cloud provider)

---

## Getting Started

### Running with Docker Compose

For local development, spin up all services with a single command:

```bash
docker-compose up --build
```

The app will be available at `http://localhost:<port>` (check `docker-compose.yml` for the configured port).

To stop the services:

```bash
docker-compose down
```

---

### Deploying to Kubernetes

Apply the manifests in the following order:

**1. Create MySQL ConfigMap and Secrets**

```bash
kubectl apply -f mysql-config.yaml
kubectl apply -f mysql-secret.yaml
```

**2. Deploy MySQL**

```bash
kubectl apply -f mysql.yaml
```

**3. Deploy the CRUD Application**

```bash
kubectl apply -f crudapp.yaml
```

**4. Verify all pods are running**

```bash
kubectl get pods
kubectl get services
```

---

## Kubernetes Architecture

```
┌─────────────────────────────────────────────┐
│                 Kubernetes Cluster           │
│                                             │
│  ┌──────────────┐      ┌─────────────────┐  │
│  │  CRUD App    │────▶ │   MySQL Pod     │  │
│  │  (client +   │      │                 │  │
│  │   server)    │      └────────┬────────┘  │
│  └──────────────┘               │           │
│                         ┌───────┴──────┐    │
│                         │  ConfigMap & │    │
│                         │   Secret     │    │
│                         └──────────────┘    │
└─────────────────────────────────────────────┘
```

| Manifest              | Purpose                                          |
|-----------------------|--------------------------------------------------|
| `crudapp.yaml`        | Deployment + Service for the frontend & backend  |
| `mysql.yaml`          | Deployment + Service for MySQL                   |
| `mysql-config.yaml`   | ConfigMap with MySQL database name/host config   |
| `mysql-secret.yaml`   | Kubernetes Secret for MySQL credentials          |

---

## Environment Variables

Sensitive configuration is managed via Kubernetes Secrets and ConfigMaps.

| Variable          | Source              | Description              |
|-------------------|---------------------|--------------------------|
| `MYSQL_HOST`      | ConfigMap           | MySQL service hostname   |
| `MYSQL_DATABASE`  | ConfigMap           | Database name            |
| `MYSQL_USER`      | Secret              | MySQL username           |
| `MYSQL_PASSWORD`  | Secret              | MySQL password           |
| `MYSQL_ROOT_PASSWORD` | Secret          | MySQL root password      |

> ⚠️ **Never commit real credentials.** Update `mysql-secret.yaml` with base64-encoded values before deploying.

To encode a value:
```bash
echo -n 'your_password' | base64
```

---

## Contributing

Contributions are welcome! Feel free to open an issue or submit a pull request.

1. Fork the repository
2. Create a new branch (`git checkout -b feature/your-feature`)
3. Commit your changes (`git commit -m 'Add your feature'`)
4. Push to the branch (`git push origin feature/your-feature`)
5. Open a Pull Request

---

> Built with ❤️ using Kubernetes, Docker, Node.js, and MySQL.
