# Microservices Blog Application

A full-stack blog application built with a microservices architecture. This project demonstrates how to build, deploy, and scale a complex application using decoupled services.

## Architecture

The application is composed of several small, independent services that communicate via an Event Bus. This ensures loose coupling and allows each service to handle its own specific domain logic.

### Services Overview

- **Client (`/client`)**: A React application built with Vite that serves as the user interface. It interacts with the backend services to display posts and comments.
- **Posts Service (`/posts`)**: Manages the creation and storage of blog posts.
- **Comments Service (`/comments`)**: Handles the creation and management of comments associated with posts.
- **Query Service (`/query`)**: detailed implementation of the CQRS (Command Query Responsibility Segregation) pattern. It listens for separate `PostCreated` and `CommentCreated` events and aggregates them into a full data structure to serve the frontend efficiently.
- **Moderation Service (`/moderation`)**: Watches for `CommentCreated` events and moderates the content (e.g., flagging inappropriate words like "orange") before approving or rejecting them.
- **Event Bus (`/event-bus`)**: The communication backbone. It receives events (requests) from services and broadcasts them to all other subscribed services.
- **JWT Auth (`/jwt-auth`)**: Manages user authentication and token generation.

### Infrastructure

- **Kubernetes (`/k8s`)**: Contains all the deployment and service configuration files (`.yaml`) required to run the application in a Kubernetes cluster (e.g., Minikube, Docker Desktop).
- **Docker**: Each service has its own `Dockerfile` for containerization.

## Tech Stack

- **Frontend**: React, Vite
- **Backend**: Node.js, Express
- **Infrastructure**: Docker, Kubernetes (K8s)
- **Communication**: REST APIs, Async Events (Event Bus)

## Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) installed
- [Docker](https://www.docker.com/) (for containerization)
- [Kubernetes](https://kubernetes.io/) / Minikube (for orchestration)

### Running Locally (Manual)

To run the services individually without Docker/K8s (useful for debugging a specific service):

1.  Navigate to a service directory (e.g., `cd posts`).
2.  Install dependencies: `npm install`.
3.  Start the service: `npm start`.

*Note: You will need to start the Event Bus and other dependent services for the application to function correctly.*

### Running with Kubernetes

1.  Ensure your Kubernetes cluster is running.
2.  Apply the Kubernetes configuration files:
    ```bash
    kubectl apply -f k8s/
    ```
3.  The application should now be spinning up. Check status with:
    ```bash
    kubectl get pods
    ```

## Development Features

- **Event-Driven**: Uses an event bus to propagate state changes across services.
- **Fault Tolerance**: Services like `Query` allow the frontend to load data even if `Posts` or `Comments` services are temporarily down.
- **Scalability**: Each service can be scaled independently using Kubernetes deployments.
