# Full-Stack Application Template

This repository provides a comprehensive, containerized template for full-stack web development. It features a Django backend, a React frontend, and a MySQL database, all orchestrated with Docker Compose.

## Tech Stack

-   **Backend**: Django, Python, `uv`
-   **Frontend**: React, TypeScript, Vite, `pnpm`
-   **Database**: MySQL 8.0
-   **Containerization**: Docker & Docker Compose

## Project Structure

The repository is organized into two main application directories:

-   `src/backend/`: Contains the Django project.
-   `src/frontend/`: Contains the React (Vite + TypeScript) project.

Key configuration files at the root level include:

-   `docker-compose.yml`: Defines and configures the multi-container Docker application (frontend, backend, database).
-   `.env.example`: A template for the required environment variables.

## Getting Started

Follow these instructions to get a local copy up and running.

### Prerequisites

-   [Docker](https://docs.docker.com/get-docker/)
-   [Docker Compose](https://docs.docker.com/compose/install/)

### Configuration

1.  **Clone the repository:**
    ```sh
    git clone https://github.com/sofiaksbarros/fullstack-template.git
    cd fullstack-template
    ```

2.  **Create an environment file:**
    Copy the example environment file to create your own configuration.
    ```sh
    cp .env.example .env
    ```

3.  **Set environment variables:**
    Open the `.env` file and fill in the required values.

    ```dotenv
    # BACKEND SECRET KEY!
    SECRET_KEY=your_django_secret_key_here

    # Database
    MYSQL_ROOT_PASSWORD=your_mysql_root_password
    MYSQL_DATABASE=your_db_name
    MYSQL_USER=your_db_user
    MYSQL_PASSWORD=your_db_user_password
    MYSQL_HOST=mysql_db
    MYSQL_PORT=3306
    ```
    > **Note:** The `SECRET_KEY` is a critical security setting for Django. You can generate a new one easily, for example, using Python: `python -c 'from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())'`.

### Running the Application

Once the `.env` file is configured, you can build and run the services using Docker Compose:

```sh
docker-compose up --build
```

This command will:
1.  Build the Docker images for the `frontend` and `backend` services based on their respective `Dockerfile`s.
2.  Pull the `mysql:8.0` image for the database service.
3.  Create and start the containers for all three services.
4.  Establish a network (`app_network`) for the containers to communicate with each other.

### Accessing the Services

After the containers are running, you can access the different parts of the application:

-   **Frontend**: [http://localhost:8080](http://localhost:8080)
-   **Backend API**: [http://localhost:8280](http://localhost:8280) (e.g., [http://localhost:8280/admin/](http://localhost:8280/admin/))
-   **Database**: The MySQL database is accessible on port `3306` from your host machine.

## Service Details

### Backend (Django)

-   The Django application is defined in the `src/backend/` directory.
-   Dependencies are managed by `uv`, a fast Python package installer and resolver.
-   The `Dockerfile` in this directory sets up an Ubuntu environment, installs `uv` and build essentials, and installs project dependencies from `pyproject.toml`.
-   It connects to the `mysql_db` service for its database, using connection details from the `.env` file.

### Frontend (React)

-   The React application, built with Vite and TypeScript, is located in the `src/frontend/` directory.
-   Node.js dependencies are managed with `pnpm`, as specified in `package.json`.
-   The `Dockerfile` uses a multi-stage approach:
    1.  It installs dependencies and builds the static production assets (`pnpm run build`).
    2.  It then copies the built assets and a lightweight `http` server into a clean final stage to serve the content.
-   The application is served on port 8000 inside the container.

### Database (MySQL)

-   The `mysql_db` service uses the official `mysql:8.0` Docker image.
-   Database credentials and the initial database name are configured through the `.env` file.
-   Data is persisted in a Docker volume named `db_data` to ensure data is not lost when the container is stopped or removed.