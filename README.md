# CI/CD Repository

This repository is dedicated to managing Continuous Integration and Continuous Deployment (CI/CD) workflows for all projects in the organization.

## Purpose

- Centralized management of CI/CD pipelines.
- Reusable workflows for consistent deployment processes.
- Automation and orchestration of testing, building, and deployment.

## Features

- CI/CD pipelines for frontend, backend, and other services.
- Reusable GitHub Actions workflows.
- Integration with infrastructure and application repositories.

## Workflows

### Publish Docker Image

**Workflow File:** `publish_docker_image.yml`  
**Purpose:** Automates the building and publishing of Docker images to a container registry.

### Update and Restart App Services on Server
**Workflow File:**  `update_service.yml`  
**Purpose:** Automates the process of updating application services running in Docker containers on a remote server. This workflow connects to the server via SSH, pulls the latest Docker images, and restarts the specified service or all services if none are specified.

## Usage

Workflows in this repository are designed to be used across multiple repositories in the organization. Refer to individual workflow files for configuration and usage instructions.

## Contact

For questions or issues related to this repository, please contact the DevOps team.
