# Expense Management System - Shell Scripts

This project contains a set of shell scripts to automate the setup and deployment of a three-tier expense management system. The system consists of a MySQL database, a Node.js backend, and a web-based frontend served by Nginx.

## Project Structure

- `mysql.sh`: Script to install and configure MySQL server.
- `backend.sh`: Script to set up the Node.js backend application.
- `frontend.sh`: Script to set up the Nginx server and deploy the frontend application.
- `expense.conf`: Nginx configuration file for the frontend and API proxy.
- `backend.service`: Systemd service file for the backend application.

## Prerequisites

- A Linux-based operating system (tested on RHEL/CentOS).
- `sudo` or root access to run the scripts.
- Internet connectivity to download application code and dependencies.

## Setup Instructions

The scripts should be executed in the following order:

### 1. MySQL Setup

This script installs and configures the MySQL server.

```bash
sudo ./mysql.sh
```

### 2. Backend Setup

This script sets up the Node.js backend, creates a systemd service, and loads the database schema.

```bash
sudo ./backend.sh
```

### 3. Frontend Setup

This script installs Nginx and deploys the frontend application. It also configures Nginx to act as a reverse proxy for the backend API.

```bash
sudo ./frontend.sh
```

## How it Works

### `mysql.sh`

- Installs `mysql-server`.
- Enables and starts the `mysqld` service.
- Sets the root password for MySQL to `ExpenseApp@1` if it's not already set.

### `backend.sh`

- Disables the default Node.js module and enables Node.js 20.
- Installs Node.js.
- Creates an `expense` application user.
- Downloads the backend code from an S3 bucket.
- Installs npm dependencies.
- Copies the `backend.service` file to systemd.
- Installs the MySQL client, and loads the schema into the database.
- Reloads the systemd daemon, enables and starts the `backend` service.

### `frontend.sh`

- Installs `nginx`.
- Enables and starts the `nginx` service.
- Downloads the frontend code from an S3 bucket.
- Copies the `expense.conf` Nginx configuration file.
- Restarts Nginx to apply the new configuration.

## Configuration

- **`expense.conf`**: This file configures Nginx to serve the frontend application and proxy requests to `/api/` to the backend service.
- **`backend.service`**: This systemd unit file defines the backend service, sets the `DB_HOST` environment variable, and configures the service to run as the `expense` user.

## Logging

Each script creates a log file in the `/var/log/expense-logs` directory. The log file is named after the script and includes a timestamp, for example: `mysql-2025-08-22-21-59-00.log`.
