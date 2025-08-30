# 단순화된 Python RAG Chatbot Dockerfile
FROM python:3.11-slim

# Cache buster to force rebuild
ARG CACHE_BUST=2025-08-30-23-15
RUN echo "Cache bust: $CACHE_BUST"

# Install system dependencies
RUN apt-get update && apt-get install -y \
    curl \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# Create non-root user
RUN useradd --create-home --shell /bin/bash app

# Set working directory
WORKDIR /app

# Copy dependency files
COPY pyproject.toml .
COPY README.md .

# Install Python dependencies directly to system Python
RUN pip install --no-cache-dir fastapi uvicorn python-multipart \
    google-generativeai openai anthropic \
    qdrant-client sentence-transformers \
    langchain langchain-community \
    python-dotenv pyyaml aiofiles \
    slowapi redis pandas numpy

# Copy application code
COPY --chown=app:app . .

# Create necessary directories
RUN mkdir -p logs uploads/temp && \
    chown -R app:app /app

# Switch to non-root user
USER app

# Expose port
EXPOSE 8000

# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
    CMD curl -f http://localhost:${PORT:-8000}/health || exit 1

# Create simple start script
RUN echo '#!/bin/bash' > /app/start.sh && \
    echo 'exec python -m uvicorn main:app --host 0.0.0.0 --port ${PORT:-8000}' >> /app/start.sh && \
    chmod +x /app/start.sh

# Command to run the application
CMD ["/app/start.sh"]