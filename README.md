# WordPress Deployment with GitHub Actions

This repository contains the code and instructions for deploying a WordPress website using GitHub Actions. You can follow these steps to set up the environment locally and automate the deployment process.

## Local Environment Setup

### 1. Install Local Development Tools

Make sure you have the following tools installed on your local machine:

- nginx server (for a local web server)
- mysql server
- php

### 2. Download WordPress

Download the latest version of WordPress from the official website: [WordPress.org](https://wordpress.org/download/).

### 3. Install WordPress

- Extract the WordPress files to your web server's document root directory (e.g., `/var/www/html` for nginx)

- Create a MySQL database and user for your WordPress installation using the MySQL command-line tool.

### 4. Configure WordPress

- Rename or use mv command and move the `wp-config-sample.php` file in the WordPress directory to `wp-config.php`.

- Open `wp-config.php` and update the database details with the information for the MySQL database you created.

### 5. Access the Local WordPress Site

- Start your local web server and MySQL server.

- Open your web browser and navigate to `http://localhost` to access your local WordPress site.

- Follow the WordPress setup wizard to complete the installation, including creating an admin user and configuring your site.

## Deployment with GitHub Actions

### 1. Configure GitHub Secrets

In your GitHub repository, go to "Settings" > "Secrets" and add the following secrets:

##- `SERVER_IP`: The IP address or hostname of your Ubuntu server.
##- `SERVER_USERNAME`: The username to use when SSHing into your server.
- `SSH_PRIVATE_KEY`: The SSH private key that allows authentication to your server.

### 2. Set Up a GitHub Actions Workflow

Create a GitHub Actions workflow file (e.g., `.github/workflows/deploy.yml`) in your repository with the following content:

```yaml
# Project Name
name: Deploy WordPress

on:
  push:
    branches:
      - main  

jobs:
  deploy:
    runs-on: self-hosted

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up SSH
        uses: webfactory/ssh-agent@v0.5.3
        with:
          ssh-private-key: ${{ secrets.SSH_PRIVATE_KEY }}

      - name: Deploy to Server
        run: |
          ssh ubuntu@15.207.115.79 'bash -s' < /home/ubuntu/deploy-script.sh

### 3 Deployment Script (deploy-script.sh)

#!/bin/bash

# Define the target directory where your WordPress files are located
TARGET_DIRECTORY="/var/www/html/wordpress"

# Ensure you are in the correct directory
cd "$TARGET_DIRECTORY"

# Pull the latest code from the GitHub repository
git pull

# Perform any necessary WordPress updates or migrations
# For example, you might run WP-CLI commands here, like:
# wp core update
# wp plugin update --all
# wp theme update --all

# Restart the web server to apply changes (you may need to adapt this based on your web server)
sudo service nginx restart

# Optional: Clear the cache, if needed
# For example, you can use WP-CLI to clear the cache for specific plugins or the entire site.
# wp cache flush

echo "Deployment completed"

