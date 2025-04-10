# OS2BorgerPC Hosting and Deployment

This is a deployment tool designed for hosting the OS2BorgerPC Admin Site application using Docker. It provides a Docker-based setup and pre-configured files to simplify the deployment and management of the application.

## Pre-installation Requirements

### Secure Mode Configuration

It is assumed that the server will run in secure mode using HTTPS (port 443). To ensure proper operation, you must have a valid domain name and an SSL certificate.

**Steps to Enable Secure Mode:**

- **Domain Name:** Use a fully qualified domain name (FQDN) that points to your server’s IP address. Update `nginx.conf` with your domain name in two locations.
- **SSL Certificate:** Obtain a valid SSL certificate and the corresponding private key for your domain.  
  - Place the certificate file (`nginx.crt`) and the private key file (`nginx.key`) inside the `ssl` directory.
 
### Network Requirements

The OS2BorgerPC Admin server can operate reliably within a protected internal network, provided that outgoing HTTP and HTTPS traffic is permitted.  
This setup helps improve security while keeping the configuration simple.

Even in this setup, it's recommended to configure the server to run in secure mode using HTTPS.

### Make Your Server Available in the Connect Script

When connecting a newly installed BorgerPC to your OS2BorgerPC Admin server, the connect script displays a list of available servers to choose from.  
To include your server in this list, send an email to the coordination team at [os2borgerpc@os2.eu](mailto:os2borgerpc@os2.eu) with your server's domain name (FQDN).  

Once received, we’ll update the list immediately, and the changes will take effect right away.



## Docker Compose Setup

The deployment uses Docker Compose to manage the services. Here is an overview of the services defined in the `docker-compose.yml` file:

### Services

- **os2borgerpc-admin**: This is the main service running the OS2BorgerPC Admin Site. It uses the image `ghcr.io/os2borgerpc/os2borgerpc-admin-site:7.0.1`. The service is configured to use environment variables from the `.env` file and mounts the `settings.py` file as read-only. It exposes ports `9999` and `8080`.

- **db**: This service runs a PostgreSQL database using the `postgres:latest` image. It is configured to always restart and uses environment variables from the `.env` file. The database data is persisted using the `postgres-data` volume.

- **nginx**: This service runs an Nginx proxy using the `nginx:latest` image. It is configured to handle SSL termination and proxy requests to the `os2borgerpc-admin` service. The Nginx configuration file (`nginx.conf`) and SSL certificates (`ssl` directory) are mounted as read-only. The service exposes ports `80` and `443`.

### Volumes

- **admin-media**: This volume is used to store media files for the `os2borgerpc-admin` service.
- **postgres-data**: This volume is used to persist PostgreSQL database data.

### Proxy and SSL Certificates

The Nginx service acts as a reverse proxy for the `os2borgerpc-admin` service. It handles SSL termination and forwards requests to the appropriate backend service. The SSL certificates are stored in the `ssl` directory and are mounted into the Nginx container.

## Security Considerations

Security considerations for exposing this installation to the internet have not been made. If you plan to expose this setup to the internet, please ensure that you take appropriate security measures to protect the server and the application.
