# OS2BorgerPC Admin Site Deployment

This is an experimental deployment setup for the OS2BorgerPC Admin Site. It can be used at your own risk.

## Usage

This setup is currently used in Sønderborg Kommune, where it is installed on a server that can only be accessed from the local network. There is no internet access to the server.

## Security Considerations

Security considerations for exposing this installation to the internet have not been made. If you plan to expose this setup to the internet, please ensure that you take appropriate security measures to protect the server and the application.

## Docker Compose Setup

The deployment uses Docker Compose to manage the services. Here is an overview of the services defined in the `docker-compose.yml` file:

### Services

- **os2borgerpc-admin**: This is the main service running the OS2BorgerPC Admin Site. It uses the image `ghcr.io/os2borgerpc/os2borgerpc-admin-site:7.0.0`. The service is configured to use environment variables from the `.env` file and mounts the `settings.py` file as read-only. It exposes ports `9999` and `8080`.

- **db**: This service runs a PostgreSQL database using the `postgres:latest` image. It is configured to always restart and uses environment variables from the `.env` file. The database data is persisted using the `postgres-data` volume.

- **nginx**: This service runs an Nginx proxy using the `nginx:latest` image. It is configured to handle SSL termination and proxy requests to the `os2borgerpc-admin` service. The Nginx configuration file (`nginx.conf`) and SSL certificates (`ssl` directory) are mounted as read-only. The service exposes ports `80` and `443`.

### Volumes

- **admin-media**: This volume is used to store media files for the `os2borgerpc-admin` service.
- **postgres-data**: This volume is used to persist PostgreSQL database data.

### Proxy and SSL Certificates

The Nginx service acts as a reverse proxy for the `os2borgerpc-admin` service. It handles SSL termination and forwards requests to the appropriate backend service. The SSL certificates are stored in the `ssl` directory and are mounted into the Nginx container.

## Environment Variables

The following environment variables are used in this setup:

- `DB_HOST`: Database host
- `DB_NAME`: Name of the database
- `DB_USER`: Username for the database
- `DB_PASSWORD`: Password for the database
- `DB_PORT`: Database port
- `ALLOWED_HOSTS`: Allowed hosts
- `CORE_SCRIPT_VERSION_TAG`: Version of the global scripts
- `CORE_SCRIPT_COMMIT_HASH`: Matching commit hash for the selected tagged version of the global scripts
- `PC_IMAGE_RELEASES_URL`: URL for downloading BorgerPC ISO images
- `KIOSK_IMAGE_RELEASES_URL`: URL for downloading Kiosk ISO images
- `DEBUG`: Enable debug mode
- `SECRET_KEY`: Secret key for Django
- `ADMIN_USERNAME`: Username for admin user
- `ADMIN_EMAIL`: Email for admin user
- `ADMIN_PASSWORD`: Password for admin user
- `TIME_ZONE`: Time zone
- `LANGUAGE_CODE`: Language code
- `INITIALIZE_DATABASE`: Initialize the database
- `LOG_LEVEL`: Log level
- `CICERO_URL`: URL for Cicero server
- `CITIZEN_LOGIN_API_VALIDATOR`: Validator for citizen login API
- `POSTGRES_USER`: Username for PostgreSQL
- `POSTGRES_PASSWORD`: Password for PostgreSQL
- `USE_X_FORWARDED_HOST`: Ensure that Django knows it's behind a proxy
- `SECURE_PROXY_SSL_HEADER`: Secure proxy SSL header

## TODO

The entire Django `settings.py` fro os2borgerpc-admin-site is stored as a copy in this project and overwrites the `settings.py` in the image. This is done only to append two lines at the bottom:

```python
# Ensure that Django knows it's behind a proxy
USE_X_FORWARDED_HOST = True
SECURE_PROXY_SSL_HEADER = ('HTTP_X_FORWARDED_PROTO', 'https')

This is not a proper approach and should be handled differently. For example, by adding the lines to settings.py in the os2borgerpc-admin-site project.