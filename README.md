# S1M Ghost Container

## Introduction

This repository provides the necessary configuration for deploying a Ghost instance on an Oracle Cloud free-tier VPS using Ansible and Podman. The setup includes backup and restore functionality for PostgreSQL databases and supports deployment via a reverse proxy like Traefik or Caddy.

## Getting Started

To use this repository, ensure you have the necessary `.env` files. These files are not included in the repository and must be created and configured manually for each service.

## Prerequisites

Before you begin, ensure the following:

1. You have a functioning Oracle Cloud free-tier VPS with SSH access.
2. A reverse proxy, such as Traefik or Caddy, is installed to provide access to the Ghost instance.
3. Your server is preconfigured with Ansible and Podman.

### Local Dependencies

Install the following on your local machine:

- Python 3.13
- Ansible
- The required Podman Ansible collection:

`ansible-galaxy collection install containers.podman`

## How to run

To deploy the Ghost instance, execute the following command:

`ansible-playbook main.yml`

## Backup

Backup the content of all ghost data volumes (db and content). Otherwise uploaded files cannot be restored.

You can create a MYSQL backup by connecting to the production server via SSH and running:

`podman exec ghost-db /bin/bash -c "mysqldump -u root -p'password' ghost" > $(pwd)/backup.sql`

## Restore

To restore a MYSQL backup, first connect to the production server via SSH, then run the following commands before starting the application for the first time:

`podman exec -i ghost-db mysql -u root -p'password' ghost < $(pwd)/backup.sql`

## TODO

- Automate container deployment and updates using GitHub Actions
- Add another section on how to update the podman containers
