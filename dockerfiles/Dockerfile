# syntax=docker/dockerfile:1.4

FROM python:3
WORKDIR /app

ADD pyproject.toml pyproject.toml
ADD poetry.lock poetry.lock

RUN pip install poetry

ENV POETRY_VIRTUALENVS_IN_PROJECT=true
ENV POETRY_CACHE_DIR=/cache

# Install all dependencies to fill the cache
RUN --mount=type=tmpfs,target=/app/.venv --mount=type=cache,id=poetry-cache,target=/cache poetry install

ADD . .

RUN --network=none --mount=type=cache,id=dev-dependencies,target=/app/.venv --mount=type=cache,id=poetry-cache,target=/cache poetry install
# Run tests
RUN --mount=type=cache,id=dev-dependencies,target=/app/.venv POETRY_VIRTUALENVS_IN_PROJECT=true /app/.venv/bin/pytest /app

RUN --network=none --mount=type=cache,id=poetry-cache,target=/cache poetry install --no-dev
