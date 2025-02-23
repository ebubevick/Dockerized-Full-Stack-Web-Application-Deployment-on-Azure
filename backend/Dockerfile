# Stage 1: Build the backend service
FROM python:3.10-slim AS build-backend

WORKDIR /app

# Install Poetry
RUN pip install poetry

# Copy project files
COPY pyproject.toml poetry.lock ./
COPY app ./app

# Install dependencies
RUN poetry config virtualenvs.create false && poetry install --only main

# Stage 2: Runtime
FROM python:3.10-slim AS runtime

WORKDIR /app

# Copy the installed packages from the build stage
COPY --from=build-backend /usr/local/lib/python3.10/site-packages /usr/local/lib/python3.10/site-packages
COPY --from=build-backend /usr/local/bin /usr/local/bin

# Copy the application code
COPY --from=build-backend /app /app

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]