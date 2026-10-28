# Refindme
1. Project Title
Refind: A Cloud-Native Automated Screenshot Organization Platform

2. Motivation & Problem Statement
Smartphones and desktops accumulate hundreds of unorganized screenshots—receipts, code snippets, chat logs, tickets, and study notes. Finding a specific image later is painful because standard photo galleries lack semantic text recognition and automatic categorization.

Refind solves this by providing an automated pipeline that ingests screenshots, extracts semantic information via AI vision models, and indexes them into a searchable dashboard.

To ensure production-grade reliability, this project packages Refind as a cloud-native, containerized application managed by Kubernetes and deployed through automated CI/CD workflows.

3. Core System Components
A. Application Layer
Frontend: Responsive web dashboard (React / Vite) for drag-and-drop screenshot uploads, gallery browsing, and tag-based search.

Backend: REST API (FastAPI) handling image ingestion, vision model classification calls, and search queries.

Database: PostgreSQL storing screenshot metadata, tags, and category taxonomies.

Health & Probes: Standard /healthz (liveness) and /readyz (readiness) endpoints for cluster self-healing.

B. Containerization (Docker)
Multi-stage Dockerfile definitions for both backend and frontend to minimize production image footprint and remove build tools from runtime.

Non-root user execution (USER appuser) to adhere to container security best practices.

Local orchestration with docker-compose for rapid local development.

C. Container Orchestration (Kubernetes)
Declarative Manifests: Configured using Kustomize (base and overlays for local development).

Workloads & Networking:

Deployment controllers for Frontend and Backend with rolling update strategies and resource limits (CPU/Memory).

StatefulSet with PersistentVolumeClaim (PVC) for PostgreSQL data persistence.

Internal Service resources (ClusterIP) for secure inter-service communication.

Resilience: Liveness and readiness probes to automatically restart failing containers.

Local Cluster: Deployable directly on a local lightweight cluster using Kind or k3d.

D. CI/CD Automation (GitHub Actions)
Automated Pipeline (.github/workflows/main.yml):

Test & Lint: Runs code formatters, linters, and unit test suites on every pull request.

Image Build & Security: Builds Docker containers and runs vulnerability scans.

Registry Publication: Pushes production-ready images to GitHub Container Registry (GHCR).

Smoke Test: Spins up an ephemeral local Kind cluster in the CI runner to verify all Kubernetes pods reach a Running state without crash looping.
