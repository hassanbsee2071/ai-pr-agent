# Base stage
FROM python:3.10 as base

WORKDIR /app
COPY pyproject.toml requirements.txt ./
RUN echo "Installing dependencies..." && \
    pip install . && \
    echo "Cleaning up pyproject.toml and requirements.txt..." && \
    rm pyproject.toml requirements.txt
ENV PYTHONPATH=/app

# GitHub App stage
FROM base as github_app
WORKDIR /app
COPY ai_pr_agent ai_pr_agent
RUN echo "Setting up GitHub App..."
CMD ["python", "ai_pr_agent/servers/github_app.py"]

# Bitbucket App stage
FROM base as bitbucket_app
WORKDIR /app
COPY ai_pr_agent ai_pr_agent
RUN echo "Setting up Bitbucket App..."
CMD ["python", "ai_pr_agent/servers/bitbucket_app.py"]

# Bitbucket Server Webhook stage
FROM base as bitbucket_server_webhook
WORKDIR /app
COPY ai_pr_agent ai_pr_agent
RUN echo "Setting up Bitbucket Server Webhook..."
CMD ["python", "ai_pr_agent/servers/bitbucket_server_webhook.py"]

# GitHub Polling stage
FROM base as github_polling
WORKDIR /app
COPY ai_pr_agent ai_pr_agent
RUN echo "Setting up GitHub Polling..."
CMD ["python", "ai_pr_agent/servers/github_polling.py"]

# GitLab Webhook stage
FROM base as gitlab_webhook
WORKDIR /app
COPY ai_pr_agent ai_pr_agent
RUN echo "Setting up GitLab Webhook..."
CMD ["python", "ai_pr_agent/servers/gitlab_webhook.py"]

# Azure DevOps Webhook stage
FROM base as azure_devops_webhook
WORKDIR /app
COPY ai_pr_agent ai_pr_agent
RUN echo "Setting up Azure DevOps Webhook..."
CMD ["python", "ai_pr_agent/servers/azuredevops_server_webhook.py"]

# Test stage
FROM base as test
WORKDIR /app
COPY requirements-dev.txt ./
RUN echo "Installing development dependencies..." && \
    pip install -r requirements-dev.txt && \
    echo "Cleaning up requirements-dev.txt..." && \
    rm requirements-dev.txt
COPY ai_pr_agent ai_pr_agent
COPY tests tests
RUN echo "Test stage setup complete."

# CLI stage
FROM base as cli
WORKDIR /app
COPY ai_pr_agent ai_pr_agent
RUN echo "Setting up CLI..."
ENTRYPOINT ["python", "ai_pr_agent/cli.py"]
