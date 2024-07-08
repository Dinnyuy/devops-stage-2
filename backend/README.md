# Backend - FastAPI with PostgreSQL

This directory contains the backend of the application built with FastAPI and a PostgreSQL database.

## Prerequisites

- Python 3.8 or higher
- Poetry (for dependency management)
- PostgreSQL (ensure the database server is running)

### Installing Poetry

To install Poetry, follow these steps:

```sh
curl -sSL https://install.python-poetry.org | python3 -
```

Add Poetry to your PATH (if not automatically added):

## Setup Instructions

1. **Navigate to the backend directory**:
    ```sh
    cd backend
    ```

2. **Install dependencies using Poetry**:
    ```sh
    poetry install
    ```

3. **Set up the database with the necessary tables**:
    ```sh
    poetry run bash ./prestart.sh
    ```

4. **Run the backend server**:
    ```sh
    poetry run uvicorn app.main:app --reload
    ```

5. **Update configuration**:
   Ensure you update the necessary configurations in the `.env` file, particularly the database configuration.


Set Up Environment Variables:
Create a .env file in the backend directory with the following variables:

bash
Copy code
DATABASE_URL=postgresql://user:password@db:5432/database
Replace user, password, and database with your PostgreSQL credentials.

Build and Run the Containers:

sh
Copy code
docker-compose up -d --build
This command builds and starts the backend and database containers in detached mode.

Accessing API Documentation:

Swagger UI: Navigate to http://your_domain/docs
ReDoc: Navigate to http://your_domain/redoc
Additional Docker Commands
Stop Containers:

sh
Copy code
docker-compose down
View Logs:

sh
Copy code
docker-compose logs -f
Folder Structure
The main folders and files within the backend directory are structured as follows:

app/: Contains the FastAPI application code.
alembic/: Contains database migration scripts.
docker-compose.yaml: Docker Compose configuration file.
Dockerfile: Dockerfile for FastAPI application.
Database Management
Adminer: Access Adminer at http://db.domain:8080 to manage PostgreSQL database.
Deployment
Ensure to configure appropriate environment variables and Docker Compose settings for production deployment. Deploy the containers to your cloud provider or local server.

Troubleshooting
Check Docker logs for backend and database containers for any startup errors.
Ensure PostgreSQL database is accessible and configured correctly in DATABASE_URL.
Dockerize the FastAPI Backend:

Navigate to the FastAPI backend directory.
Create a Dockerfile with the following content:
Dockerfile
FROM python:3.9
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
EXPOSE 8000
Build and run the FastAPI backend container:
bash
Copy code
docker build -t fastapi-backend .
docker run -d -p 8000:8000 fastapi-backend
# Step 3: Configure Traefik or Nginx Proxy Manager
Traefik Configuration:

Create a docker-compose.yml file in the root directory of your project.
Add the following configuration to serve the frontend on / and proxy the backend:

version: '3.7'

services:
  traefik:
    image: traefik:v2.4
    command:
      - "--api.insecure=true"
      - "--providers.docker=true"
      - "--entrypoints.web.address=:80"
    ports:
      - "80:80"
      - "8080:8080"
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock"
    networks:
      - web

  frontend:
    build: ./frontend
    labels:
      - "traefik.http.routers.frontend.rule=PathPrefix(`/`)"
      - "traefik.http.services.frontend.loadbalancer.server.port=80"
    networks:
      - web

  backend:
    build: ./backend
    labels:
      - "traefik.http.routers.backend.rule=PathPrefix(`/api`)"
      - "traefik.http.services.backend.loadbalancer.server.port=8000"
    networks:
      - web

networks:
  web:
    external: true
   
