# Odoo 18 Development Environment

This repository contains a Docker-based development environment for Odoo 18 with PostgreSQL 16.

## Prerequisites

Before you begin, you need to install the following tools on your system:

### Docker

Docker is required to create and manage containers for Odoo and PostgreSQL.

#### macOS:

1. Download [Docker Desktop for Mac](https://www.docker.com/products/docker-desktop)
2. Drag the Docker app to your Applications folder
3. Open Docker from your Applications folder
4. Verify installation by opening Terminal and running: `docker --version`

#### Linux (Ubuntu 22.04):

```bash
# Update package index
sudo apt update
sudo apt upgrade
sudo apt full-upgrade

# Install prerequisites
sudo apt install apt-transport-https ca-certificates curl software-properties-common gnupg lsb-release

# Add Docker's official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Add Docker repository
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker Engine
sudo apt update
sudo apt install docker-ce=5:20.10.17~3-0~ubuntu-jammy docker-ce-cli=5:20.10.17~3-0~ubuntu-jammy containerd.io

# Check Docker Service
systemctl status docker.service

# Verify installation
docker --version

```

### Docker Compose

Docker Compose is included with Docker Desktop for Windows and macOS. For Linux:

```bash
# Download the current stable release of Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/download/v2.23.3/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# Apply executable permissions
sudo chmod +x /usr/local/bin/docker-compose

# Verify installation
docker-compose --version
```

### Git (Optional)

Git is needed if you want to clone the repository.

#### macOS:

1. Install Git using Homebrew (recommended):

   ```bash
   # Install Homebrew if not already installed
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

   # Install Git
   brew install git
   ```

2. Alternatively, download the [Git installer for macOS](https://git-scm.com/download/mac)
3. Verify installation: `git --version`

#### Linux (Ubuntu/Debian):

```bash
# Update package index
sudo apt update

# Install Git
sudo apt install git

# Verify installation
git --version
```

## Getting Started

### 1. Clone the repository (or download it)

```bash
git clone <repository-url>
cd <repository-directory>
```

### 2. Environment Configuration

Create a `.env` file in the root directory with the following variables:

```bash
PG_DB=odoo
PG_USER=odoo
PG_PASSWORD=odoo
```

You can customize these values as needed for your environment.

### 3. Start the services

Run the following command to start Odoo and PostgreSQL:

```bash
docker-compose up -d
```

This command will:

- Pull the necessary Docker images (Odoo 18 and PostgreSQL 16)
- Create and start the containers
- Initialize the Odoo database

### 4. Access Odoo

Once the containers are running, you can access Odoo through your web browser:

- URL: http://localhost:8090
- Database: The value specified in PG_DB (default: odoo)
- Email: admin
- Password: admin

## Project Structure

- `docker-compose.yml`: Configuration file for Docker services
- `addons/`: Directory for custom Odoo modules
- `.env`: Environment variables (you need to create this)

## Developing Custom Modules

To develop custom Odoo modules:

1. Create your module directory inside the `addons/` folder
2. Develop your module following Odoo's module structure
3. Install your module through Odoo's interface or by adding it to the command in docker-compose.yml

## Stopping the Environment

To stop the running containers:

```bash
docker-compose down
```

To stop the containers and remove the volumes (this will delete all data):

```bash
docker-compose down -v
```

## Data Persistence

Data is persisted in Docker volumes:

- `db-data`: PostgreSQL database data
- `odoo-web-data`: Odoo filestore data

## Troubleshooting

### Database Connection Issues

If Odoo cannot connect to the database:

1. Check if the PostgreSQL container is running:

   ```bash
   docker-compose ps
   ```

2. Verify the environment variables in your `.env` file match the ones in `docker-compose.yml`

3. Inspect the logs:
   ```bash
   docker-compose logs db
   docker-compose logs odoo
   ```

### Port Conflicts

If you have port conflicts (e.g., port 8090 or 5433 already in use), modify the port mappings in `docker-compose.yml`.
