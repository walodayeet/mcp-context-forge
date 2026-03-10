FROM python:3.12-slim

WORKDIR /app

# 1. Install standard Debian build tools (Natively supports x86-64-v2 CPUs)
RUN apt-get update && apt-get install -y \
    gcc \
    git \
    libpq-dev \
    python3-dev \
    && rm -rf /var/lib/apt/lists/*

# 2. Copy the ContextForge source code
COPY . /app

# 3. Create the virtual environment expected by IBM's startup scripts
RUN python3 -m venv /app/.venv && \
    /app/.venv/bin/pip install --no-cache-dir pip setuptools pdm uv && \
    /app/.venv/bin/uv pip install ".[redis,postgres,mysql,alembic,observability,granian]"

# 4. Set permissions for non-root execution
RUN chown -R 1001:0 /app && chmod -R g=u /app

# 5. Runtime Configuration
USER 1001
ENV PATH="/app/.venv/bin:$PATH"
EXPOSE 4444

ENV HTTP_SERVER=gunicorn
CMD ["./docker-entrypoint.sh"]
