# 1. Use a slim Python image
FROM ubuntu:latest

# 2. Install necessary build tools and libmysqlclient
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    default-libmysqlclient-dev \
    pkg-config \
    gcc \
    python3-pip \
    python3-venv \
    && rm -rf /var/lib/apt/lists/*

ENV PYTHONUNBUFFERED=1

# 3. Install uv
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

# 4. Set working directory
WORKDIR /app

COPY . .

# 6. Install dependencies using uv
RUN uv sync

EXPOSE 8000

# 8. Run application
CMD ["uv", "run", "manage.py", "runserver", "0.0.0.0:8000"]