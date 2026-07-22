# DevOps and Docker Fundamentals - Level 1 & 2

## 1. DevOps Culture

### 1.1 The Concept of DevOps

DevOps emerged around 28 as a cultural and technical bridge to unite Development (Dev) and Operations (Ops) teams. Its core purpose is to break down silos, improve communication, and leverage automation to increase efficiency.

**Organizations Without DevOps (Anti-Patterns)**

- **Strong Segmentation:** A rigid barrier exists between Development (Frontend, Backend, Mobile) and Operations (Infrastructure). Teams operate in isolation without understanding the other's scope.
- **Knowledge Silos:** Information is hoarded within specific teams. Knowledge does not flow across the organization.
- **Lack of Feedback:** There is no systematic way to identify areas for improvement or learn from team mistakes.
- **Lack of Continuous Learning:** Without shared knowledge, there is no stimulation for collective professional growth.
- **Manual Processes:** High dependency on repetitive, manual tasks, which are prone to human error and inefficiency.

**Organizations With DevOps (Desired State)**

- **Integrated Teams:** Collaboration is prioritized to achieve higher agility and results.
- **Continuous Documentation:** A culture of maintaining and updating documentation to ensure system maintainability.
- **Feedback Loops:** Mechanisms for constant improvement.
- **Testability:** Integrating automated testing to accelerate feature delivery.
- **Fail Fast, Fix Fast:** Embracing failures as learning opportunities rather than occasions for blame.

### 1.2 The "Three Ways" of DevOps

These principles serve as a practical guide for implementing DevOps culture:

- **First Way (Flow):** Maximize the flow of work from Development to Operations. Emphasizes visibility, constant optimization, and automation.
- **Second Way (Feedback):** Establish fast and constant feedback loops at every level. Focuses on communication, error detection, and rapid recovery.
- **Third Way (Learning):** Continuous experimentation and organizational learning. Focuses on productivity and applying local learning to global improvements.

### 1.3 CALMS Framework

A framework to diagnose an organization's maturity in DevOps:

- **C (Culture):** Moving from a blame-culture to one of collaboration and "post-mortem" learning.
- **A (Automation):** Automating continuous delivery and infrastructure management (GitOps/IaC).
- **L (Lean):** Iterative, fast, and high-value delivery (MVPs).
- **M (Measurement):** Using technical and business metrics to detect errors before they impact users.
- **S (Sharing):** Decentralizing knowledge to avoid the "Hero Syndrome."

---

## 2. Containers and Docker

### 2.1 Core Concepts

- **Container:** An isolated environment containing all the resources an application needs to run. Containers share the host Kernel and are lightweight/portable compared to Virtual Machines (which have their own OS).
- **Docker:** A popular interface/tool for managing containers, written in Go.
- **LXC (Linux Containers):** The native Linux resource used to run containers.
- **OCI (Open Container Initiative):** A governance structure that standardizes container formats (runtime, image, distro) to promote interoperability and prevent vendor lock-in.

### 2.2 Docker Isolation

Docker utilizes native Linux kernel features:

- **CGroups:** Limits the hardware resources a process can consume to prevent a container from monopolizing host resources.
- **Namespaces:** Provides isolation for resources like networks, file systems, and processes.
- **Unshare:** Creates a new namespace for an existing process without needing a full container engine.

### 2.3 Working with Docker

- **Image:** An immutable, read-only template that defines how a container executes.
- **Container:** A runtime instance of an image. They are ephemeral by default; data is lost when the container is deleted unless persisted.

**Essential Commands**

- `docker build`: Creates an image from a Dockerfile.
- `docker run`: Executes a container.
  - `-d`: Run in background.
  - `-p`: Map host ports to container ports (e.g., `-p 30:30`).
  - `--rm`: Automatically remove the container when it stops.
- `docker ps`: List running containers.
- `docker stop` / `docker start`: Manage container lifecycle.
- `docker logs`: View container output.
- `docker image ls`: List available images.

### 2.4 Dockerfile Best Practices

The Dockerfile is a declarative manifesto for your image.

- **Versioning:** Always use specific tags (e.g., `node:18-slim`) instead of `latest`.
- **Performance:**
  - Create a `.dockerignore` file to exclude unnecessary folders like `node_modules`, `dist`, `.git`, etc.
  - Order instructions to leverage cache layers: Copy package files first, then run install, then copy source code.

### 2.5 Networking

Docker provides an abstraction layer for container communication.

- **Default Networks:**
  - `bridge`: Default network for containers.
  - `none`: Isolates the container.
  - `host`: Containers share the host's network interfaces.
- **Management:**
  - `docker network create`: Create a custom network.
  - `docker network connect`: Link a running container to a network.
  - `docker network inspect`: Inspect network details.

### 2.6 Persistence with Volumes

Because containers are ephemeral, you must use Volumes to persist data.

- **Management:** `docker volume create <name>`
- **Usage:** Associate a volume during the `docker run` command:
  ```bash
  docker run -v volume_name:/path/in/container image_name
  ```
