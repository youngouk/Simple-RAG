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
RUN pip install --no-cache-dir \
    # FastAPI and server
    "fastapi>=0.104.1" \
    "uvicorn[standard]>=0.24.0" \
    "python-multipart>=0.0.6" \
    "websockets>=12.0" \
    # HTTP client
    "httpx>=0.25.2" \
    # AI libraries  
    "google-generativeai>=0.3.2" \
    "anthropic>=0.7.8" \
    "openai>=1.3.8" \
    "cohere>=4.37" \
    # Vector database
    "qdrant-client>=1.6.9" \
    # Document processing
    "pypdf>=3.17.0" \
    "python-docx>=1.1.0" \
    "openpyxl>=3.1.2" \
    "pandas>=2.1.4" \
    "beautifulsoup4>=4.12.2" \
    "markdown>=3.5.1" \
    # LangChain
    "langchain>=0.1.9" \
    "langchain-google-genai>=0.0.8" \
    "langchain-openai>=0.0.5" \
    "langchain-qdrant>=0.1.0" \
    # FastEmbed for BM42 sparse vectors
    "fastembed>=0.3.0" \
    # Text processing
    "tiktoken>=0.5.2" \
    # Configuration
    "pyyaml>=6.0.1" \
    "python-dotenv>=1.0.0" \
    "pydantic-settings>=2.1.0" \
    # Logging and monitoring
    "structlog>=23.2.0" \
    # Rate limiting
    "slowapi>=0.1.9" \
    # System monitoring
    "psutil>=5.9.0"

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