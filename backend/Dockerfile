# backend/Dockerfile

# Use an official Python runtime as a parent image
FROM python:3.10-slim

# Install PostgreSQL client libraries
RUN apt-get update && apt-get install -y libpq-dev gcc

# Set the working directory in the container to /app
WORKDIR /app

# Copy the requirements.txt file into the container at /app
COPY requirements.txt .

# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Install sentry_sdk
RUN pip install sentry_sdk

# Install additional dependencies
RUN pip install sqlmodel PyJWT passlib pydantic-settings psycopg psycopg2-binary sqlalchemy pydantic

# Copy the contents of the local backend directory into the container at /app
COPY . .

# Expose port 8000 to allow communication to/from server
EXPOSE 8000

# Command to run when container starts
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
