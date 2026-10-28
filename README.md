# FestPass — Docker & Container Security Assessment

## Student Details

- **Name:** Pulkit Verma
- **Registration Number:** `202401100200247`
- **Port:** `8047`
- **Docker Hub Username:** `pulkit1245`
- **Docker Hub Repository:** [pulkit1245/fest-backend](https://hub.docker.com/r/pulkit1245/fest-backend)

## Project Overview

FestPass is a three-tier registration application consisting of a frontend, backend API, and PostgreSQL database.

The application is containerized using Docker and Docker Compose. Trivy is used to scan the backend image for vulnerabilities. The fixed image is published to Docker Hub using versioned tags, and Cosign will be used to sign and verify the release.

## Technology Stack

- Docker and Docker Compose
- Python and Flask
- PostgreSQL
- Nginx
- Trivy
- Docker Hub
- Cosign

## Application Architecture

- **Frontend:** Nginx serves the registration page.
- **Backend:** Flask API handles health checks and registrations.
- **Database:** PostgreSQL stores registration records.
- **Networking:** Separate frontend and backend networks restrict direct database access from the frontend.
- **Persistence:** A named Docker volume stores PostgreSQL data.

## Stage 3 — Vulnerability Scanning

Trivy was used to scan the original backend image (`fest-backend:0.1`) and the fixed image (`fest-backend:0.2`).

The fixed image uses a pinned Python base image, runs the application as a non-root user, and installs the required Python dependencies.

### Before-and-After Comparison

Trivy reports separate results for operating-system packages and Python packages.

| Metric | Before (`fest-backend:0.1`) | After (`fest-backend:0.2`) |
|---|---:|---:|
| OS CRITICAL vulnerabilities | 234 | 0 |
| OS HIGH vulnerabilities | 2243 | 44 |
| Python CRITICAL vulnerabilities | 0 | 0 |
| Python HIGH vulnerabilities | 6 | 5 |
| Image content size | 401 MB | 71.9 MB |

The fixed image reduced the reported OS CRITICAL vulnerabilities from 234 to 0 and reduced its image content size from 401 MB to 71.9 MB.

### Security Verification

The following command was used to check for fixable CRITICAL vulnerabilities:

```bash
trivy image --severity CRITICAL --ignore-unfixed --exit-code 1 fest-backend:0.2
