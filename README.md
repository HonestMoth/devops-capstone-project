# DevOps Capstone Project — Customer Accounts Microservice

**Repository owner:** [HonestMoth](https://github.com/HonestMoth)  
**Project:** IBM DevOps and Software Engineering Capstone

A RESTful Customer Accounts microservice built with Flask and PostgreSQL. This repository is maintained under my GitHub account as a hands-on DevOps capstone project, covering API development, automated testing, continuous integration, containerization, Kubernetes, and Tekton-based delivery.

## Project overview

The service exposes REST endpoints to create, retrieve, update, list, and delete customer accounts. It uses SQLAlchemy for database access and includes request validation, error handlers, security headers, and CORS configuration.

## Technology stack

- **Language:** Python 3.9
- **API:** Flask
- **Database:** PostgreSQL with SQLAlchemy
- **Testing:** Nose and coverage
- **Code quality:** Flake8
- **Security:** Flask-Talisman and Flask-CORS
- **Containerization:** Docker
- **Orchestration:** Kubernetes
- **CI/CD:** GitHub Actions and Tekton

## Repository structure

```text
.
├── .github/workflows/ci-build.yaml  # Continuous integration
├── deploy/                          # Kubernetes manifests
├── service/                         # Flask application and API
│   ├── common/                      # CLI, status, logging, errors
│   ├── config.py                    # Application/database configuration
│   ├── models.py                    # Account data model
│   └── routes.py                    # REST API endpoints
├── tekton/                          # Pipeline, tasks, and PVC manifests
├── tests/                           # Automated tests
├── Dockerfile                       # Container image definition
├── Makefile                         # Development and deployment commands
├── requirements.txt                 # Python dependencies
└── setup.cfg                        # Test and lint configuration
```

## REST API

The service runs on port **8080** by default.

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/` | API information |
| GET | `/health` | Health check |
| POST | `/accounts` | Create an account |
| GET | `/accounts` | List all accounts |
| GET | `/accounts/{account_id}` | Retrieve an account |
| PUT | `/accounts/{account_id}` | Update an account |
| DELETE | `/accounts/{account_id}` | Delete an account |

For JSON request bodies, send the header `Content-Type: application/json`.

## Run locally

### 1. Clone the repository

```bash
git clone https://github.com/HonestMoth/devops-capstone-project.git
cd devops-capstone-project
```

### 2. Create and activate a virtual environment

```bash
python3 -m venv venv
source venv/bin/activate
```

On Windows PowerShell, activate it with:

```powershell
.\venv\Scripts\Activate.ps1
```

### 3. Install dependencies

```bash
python -m pip install --upgrade pip wheel
pip install -r requirements.txt
```

### 4. Start PostgreSQL

Docker is required for the database command in the Makefile:

```bash
make db
```

The default local configuration uses database `postgres`, username `postgres`, and password `postgres` on `localhost:5432`. You can override the connection with the `DATABASE_URI` environment variable.

### 5. Run tests and linting

```bash
nosetests -v --with-coverage --cover-package=service
flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics
flake8 . --count --max-complexity=10 --max-line-length=127 --statistics
```

The local test run completed with **34 tests passing and 96% code coverage** in my development environment. Both listed Flake8 checks also returned zero issues. Results may vary if the code or environment changes.

### 6. Build the Docker image

```bash
docker build --rm --pull --tag accounts:1.0 .
```

## CI/CD and deployment

- **GitHub Actions:** [CI Build workflow](https://github.com/HonestMoth/devops-capstone-project/blob/main/.github/workflows/ci-build.yaml) runs linting and unit tests on pushes and pull requests targeting `main`.
- **Kubernetes:** deployment and service manifests are in [`deploy/`](https://github.com/HonestMoth/devops-capstone-project/tree/main/deploy).
- **Tekton:** pipeline resources are in [`tekton/`](https://github.com/HonestMoth/devops-capstone-project/tree/main/tekton).

Deployment requires the relevant Docker, Kubernetes/OpenShift, and Tekton environments to be configured; the presence of manifests alone does not mean the application is deployed.

## Project learning goals

- Build and test a REST API using test-driven development practices.
- Validate API behavior and error handling.
- Automate linting and tests in CI.
- Apply security headers and CORS policies.
- Package the service as a Docker image.
- Explore Kubernetes deployment and Tekton-based continuous delivery.


