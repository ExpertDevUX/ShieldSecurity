# SecureDeployX Installation Guide
# Please not: Our source just for Education Purpose # Not For Bussiness #
This document provides instructions for installing and configuring SecureDeployX on your VPS or server.

## Installation Options

There are two ways to install and configure SecureDeployX:

1. **Full Installation Script**: Installs everything including dependencies, creates the database, and configures the application.
2. **Auto-Configuration Script**: Only handles the configuration part, assuming you have already installed the necessary dependencies.

## 1. Full Installation Script

### What it does:

- Installs all system dependencies (Node.js, PostgreSQL, NGINX)
- Creates necessary directories, users, and permissions
- Sets up the PostgreSQL database
- Clones the repository and installs dependencies
- Creates environment configuration files
- Sets up systemd service and NGINX configuration
- Builds and starts the application

### Requirements:

- Ubuntu 20.04/22.04 or Debian 11/12
- Root access
- Internet connection

### Usage:

```bash
# Download the installation script
wget https://raw.githubusercontent.com/yourusername/securedeployx/main/install_securedeployx.sh

# Make it executable
chmod +x install_securedeployx.sh

# Run the installer
sudo ./install_securedeployx.sh
```

### Post-Installation:

After installation, you will need to:

1. Edit your NGINX configuration to set your actual domain name
2. Configure SSL with Let's Encrypt if needed
3. Add your API keys to the environment file
4. Access the application using the provided URL

## 2. Auto-Configuration Script

### What it does:

- Creates/updates environment configuration
- Sets up systemd service
- Configures NGINX
- Sets up SSL (optional)
- Configures database (if not already set up)
- Starts services

### Requirements:

- Ubuntu 20.04/22.04 or Debian 11/12
- Root access
- SecureDeployX application files already present
- Core dependencies already installed

### Usage:

```bash
# Download the auto-configuration script
wget https://raw.githubusercontent.com/yourusername/securedeployx/main/autoconfig_securedeployx.sh

# Make it executable
chmod +x autoconfig_securedeployx.sh

# Run with basic options
sudo ./autoconfig_securedeployx.sh

# Or run with custom options
sudo ./autoconfig_securedeployx.sh --domain yourdomain.com --ssl --admin-email admin@example.com
```

### Available Options:

```
--app-dir DIR        Application directory (default: /opt/securedeployx)
--data-dir DIR       Data directory (default: /var/lib/securedeployx)
--log-dir DIR        Log directory (default: /var/log/securedeployx)
--db-name NAME       Database name (default: securedeployx)
--db-user USER       Database user (default: securedeployx)
--port PORT          Server port (default: 5000)
--domain DOMAIN      Domain name for the application
--ssl                Enable SSL configuration with Let's Encrypt
--admin-email EMAIL  Admin email address for Let's Encrypt
--no-auto-start      Don't automatically start services after configuration
--help               Display this help message
```

## System Requirements

- **OS**: Ubuntu 20.04/22.04 or Debian 11/12
- **CPU**: 2+ cores recommended
- **RAM**: 2GB minimum, 4GB+ recommended
- **Storage**: 20GB+ free space
- **Network**: Public IP address or domain name with DNS configured

## Environment Variables

The scripts create an environment file with the following variables:

```
# Database Configuration
DATABASE_URL=postgresql://username:password@localhost:5432/dbname
PGUSER=username
PGPASSWORD=password
PGDATABASE=dbname
PGPORT=5432
PGHOST=localhost

# Application Configuration
NODE_ENV=production
PORT=5000
SESSION_SECRET=generated_secret

# Security Settings
COOKIE_SECRET=generated_secret
JWT_SECRET=generated_secret

# API Keys (you need to fill these in)
# OPENAI_API_KEY=
# GEMINI_API_KEY=
# AWS_ACCESS_KEY_ID=
# AWS_SECRET_ACCESS_KEY=
# RENDER_API_KEY=
```

## Troubleshooting

### Common Issues:

1. **Services not starting**: Check the logs with `journalctl -u securedeployx -f`
2. **Database connection errors**: Verify PostgreSQL is running and credentials are correct
3. **Permission issues**: Check ownership and permissions of application directories
4. **NGINX configuration**: Make sure your domain is correctly set and DNS is pointing to your server

### Log Locations:

- Application logs: `journalctl -u securedeployx`
- NGINX access logs: `/var/log/nginx/securedeployx_access.log`
- NGINX error logs: `/var/log/nginx/securedeployx_error.log`
- PostgreSQL logs: `/var/log/postgresql/postgresql-*.log`

## Updating

To update the application:

1. SSH into your server
2. Navigate to the application directory: `cd /opt/securedeployx`
3. Pull the latest changes: `git pull`
4. Install any new dependencies: `npm install`
5. Build the application: `npm run build`
6. Restart the service: `sudo systemctl restart securedeployx`

## Security Considerations

- Both scripts generate secure random passwords and secrets
- The environment file permissions are set to 600 (owner read/write only)
- SSL can be configured using Let's Encrypt
- The application runs as a dedicated system user with limited permissions

## Support

If you encounter any issues with these scripts or the SecureDeployX application, please:

1. Check the troubleshooting section above
2. Review the application logs for specific error messages
3. Contact support at support@securedeployx.com or open an issue on GitHub

## License

SecureDeployX is licensed under [License Name]. See LICENSE file for details.
