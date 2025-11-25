# Use a modern Debian (buster is EOL)
FROM python:3.11-slim-bookworm as prod

# Install system dependencies
RUN apt-get update && apt-get install -y \
    default-libmysqlclient-dev \
    gcc \
    g++ \
    pkg-config \
    openjdk-17-jdk \
    build-essential \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Install Poetry
RUN pip install --no-cache-dir poetry==1.4.2

# Configure Poetry
RUN poetry config virtualenvs.create false

# Copy project metadata (improves cache)
COPY pyproject.toml /app/src/
WORKDIR /app/src

# Install production dependencies only
RUN poetry install --only main

# Remove build dependencies
RUN apt-get purge -y \
    gcc \
    g++ \
    pkg-config \
    make \
    && apt-get autoremove -y \
    && rm -rf /var/lib/apt/lists/*

# Copy all application code
COPY . /app/src/

# Install again after copying full project (Poetry optimize)
RUN poetry install --only main

CMD ["python", "-m", "reworkd_platform"]


# Dev image
FROM prod as dev
RUN poetry install
