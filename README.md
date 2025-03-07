# DevOps Repository

This repository manages DevOps processes and infrastructure code for the Modgy project. It includes Continuous Integration (CI) and Continuous Deployment (CD) workflows, configuration files, scripts for provisioning and maintaining infrastructure using Ansible, Terraform, and Kubernetes, and settings for deploying test environments using Docker Compose and Nginx.

---
<br/><br/>

# CI/CD Pipelines

## Purpose

- **Centralized management of DevOps pipelines**: Ensures a unified approach to building, testing, and deploying applications.
- **Reusable workflows**: Provides consistency and standardization across multiple repositories.
- **Automation and orchestration**: Simplifies testing, building, and deployment processes, reducing manual effort.

## Features

- **CI/CD pipelines** for frontend, backend, and other services.
- **Reusable GitHub Actions workflows** for common DevOps tasks.
- **Integration** with infrastructure and application repositories.

## Workflows

### Publish Docker Image

- **File**: `publish_docker_image.yml`
- **Purpose**: Automates building and publishing Docker images to a container registry.

### Update and Restart App Services on Server

- **File**: `update_service.yml`
- **Purpose**: Automates updating application services running in Docker containers on a remote server.  
  Connects via SSH, pulls the latest Docker images, and restarts the specified service (or all services if none is specified).

## Usage

Each workflow is designed to be reusable across multiple repositories in the organization. Refer to the individual workflow files for more detailed configuration and usage instructions.

---
<br/><br/>

# Infrastructure

Below are instructions for deploying applications on the server using Docker and Nginx. **All IP addresses have been replaced with placeholders.** No sensitive information should remain in this document.

## Server Access

- **Server Host**: `your.server.ip`  
- **SSH Connection**:  
  ```bash
  ssh root@your.server.ip
  ```
- **SSH Key Setup**:  
  To add your public key to the server, append it to `.ssh/authorized_keys` on the server.

## Docker Compose

- **Start Services**  
  ```bash
  docker-compose up -d
  ```
  This command runs all services in the background. If necessary, it will build or pull the required images.

- **Stop Services**  
  ```bash
  docker-compose down -v
  ```
  This stops and removes all containers, networks, and (by default) anonymous volumes defined in `docker-compose.yml`.

- **Update Specific Service**  
  ```bash
  docker-compose pull <service_name>
  docker-compose up -d --no-deps <service_name>
  ```
  These commands pull the latest image for the service and restart only that service without affecting dependencies.

- **Create an External Volume for Databases**  
  ```bash
  docker volume create --name=modgy_postgres_data
  docker volume create --name=modgy_minio_data
  ```
  This keeps data outside of the container so that it persists through container updates or removals.

## Nginx Configuration

- **Check Nginx Config**  
  ```bash
  docker compose exec nginx nginx -t
  ```
  Use this command regularly to ensure there are no configuration errors.

- **Reload Nginx**  
  ```bash
  docker compose exec nginx nginx -s reload
  ```
  Reload Nginx after making configuration changes.

## Certificates and SSL Setup

Below are example commands for working with certificates. Replace domain names and emails with your own.

```bash
sudo mkdir -p ./ssl/private ./ssl/certs

sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048   -keyout ./ssl/private/selfsigned.key   -out ./ssl/certs/selfsigned.crt   -subj "/CN=server.domain.com"
```

Use **Certbot** for production certificates:
```bash
# Dry-run renewal with a deploy hook that reloads Nginx
certbot renew --dry-run --deploy-hook "docker exec modgy-nginx nginx -s reload"

# Request a certificate (example with staging mode and webroot)
sudo certbot certonly --staging --non-interactive --agree-tos   --email admin@organization.com   --webroot -w /var/www/certbot   -d mydomain.com

# Delete a certificate
sudo certbot delete --cert-name mydomain.com

# View existing certificates
certbot certificates
```

## Building and Pushing Docker Images (Example)

```bash
docker build --build-arg API_URL=https://mydomain.com/api/ -t modgy/modgy-front:latest .
docker push modgy/modgy-front:latest
```

Replace `modgy/modgy-front:latest` and `API_URL=https://mydomain.com/api/` with your own image name and API endpoint as necessary.

---
<br/><br/>

For any questions or issues related to this repository, please contact the DevOps team.
