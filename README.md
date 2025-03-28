# MySQL and phpMyAdmin Docker Setup

This repository contains a Docker Compose configuration for quickly setting up a MySQL database with phpMyAdmin for development purposes.

## Prerequisites

Before you can use this setup, you need to install Docker and Docker Compose on your system.

### Installing Docker

#### For Ubuntu/Debian:

```bash
# Update your package index
sudo apt-get update

# Install required packages
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common

# Add Docker's official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Add the Docker repository
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

# Update package index again
sudo apt-get update

# Install Docker CE
sudo apt-get install docker-ce

# Add your user to the docker group to run Docker without sudo
sudo usermod -aG docker $USER

# Apply the new group membership (or you can log out and log back in)
newgrp docker
```

#### For CentOS/RHEL:

```bash
# Install required packages
sudo yum install -y yum-utils device-mapper-persistent-data lvm2

# Add Docker repository
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

# Install Docker CE
sudo yum install docker-ce

# Start and enable Docker
sudo systemctl start docker
sudo systemctl enable docker

# Add your user to the docker group
sudo usermod -aG docker $USER

# Apply the new group membership (or you can log out and log back in)
newgrp docker
```

#### For macOS:

1. Download and install Docker Desktop from [Docker Hub](https://hub.docker.com/editions/community/docker-ce-desktop-mac/)
2. Follow the installation instructions

#### For Windows:

1. Download and install Docker Desktop from [Docker Hub](https://hub.docker.com/editions/community/docker-ce-desktop-windows/)
2. Follow the installation instructions
3. Make sure to enable WSL 2 if prompted

### Installing Docker Compose

Docker Compose is included with Docker Desktop for macOS and Windows. For Linux, you need to install it separately:

```bash
# Download the current stable release of Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/download/v2.24.6/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# Apply executable permissions
sudo chmod +x /usr/local/bin/docker-compose

# Create a symbolic link to make it available in your PATH
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

# Verify the installation
docker-compose --version
```

Note: In newer Docker installations, Docker Compose is available as a Docker plugin and can be used with `docker compose` (with a space) instead of `docker-compose`.

## Using This Docker Setup

### Starting the Services

1. Navigate to the directory containing the `docker-compose.yml` file:
   ```bash
   cd /path/to/mysql/directory
   ```

2. Start the services in detached mode:
   ```bash
   docker compose up -d
   ```

3. Verify that the containers are running:
   ```bash
   docker ps
   ```

### Accessing MySQL

You can connect to the MySQL database using any MySQL client with the following credentials:

- **Host**: localhost
- **Port**: 3306 (default MySQL port)
- **Username**: root or user
- **Password**: password
- **Database**: wpdb

### Accessing phpMyAdmin

1. Open your web browser and navigate to:
   ```
   http://localhost:3333
   ```

2. Log in with either:
   - **Username**: root, **Password**: password
   - **Username**: user, **Password**: password

### Stopping the Services

To stop the services while preserving your data:

```bash
docker compose down
```

To stop the services and remove all data (this will delete your database):

```bash
docker compose down -v
```

## Configuration Details

The `docker-compose.yml` file sets up:

1. **MySQL 8.0 Database**:
   - Uses native password authentication for better compatibility
   - Creates a database named "wpdb"
   - Creates a user with username "user" and password "password"
   - Stores data in a persistent volume at `./mysql-data`

2. **phpMyAdmin**:
   - Provides a web interface for database management
   - Accessible at http://localhost:3333
   - Automatically connects to the MySQL service

## Troubleshooting

### Common Issues

1. **Port conflicts**: If port 3333 is already in use, change it in the `docker-compose.yml` file.

2. **Permission issues**: If you encounter permission problems, make sure your user is in the docker group:
   ```bash
   sudo usermod -aG docker $USER
   newgrp docker
   ```

3. **Connection refused**: Make sure the containers are running:
   ```bash
   docker ps
   ```

4. **Data persistence issues**: Check that the volume is properly mounted:
   ```bash
   docker volume ls
   ```

## Customizing the Configuration

To customize the setup, edit the `docker-compose.yml` file:

- Change database credentials in the `environment` section
- Modify port mappings in the `ports` section
- Add additional services as needed

After making changes, restart the services:

```bash
docker compose down
docker compose up -d
```

## Security Notice

This configuration is designed for development purposes only. For production environments:

- Use strong, unique passwords
- Restrict network access
- Consider using Docker secrets or environment files instead of hardcoded credentials
- Implement proper backup strategies
