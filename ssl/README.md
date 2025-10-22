# SSL Certificates Directory

This directory contains SSL/TLS certificates required for HTTPS configuration.

## Required Files

Place the following files in this directory:

- `nginx.crt` - SSL certificate file (public key)
- `nginx.key` - SSL private key file

## File Format Requirements

- Both files must be in PEM format
- Files must be named exactly as specified above
- Ensure proper file permissions (recommended: 644 for .crt, 600 for .key)


## Security Notes

- Keep the private key file (`nginx.key`) secure and never share it
- Ensure certificates are valid and not expired
- Consider setting up automatic renewal for Let's Encrypt certificates

## Installation Requirement

These certificate files must be placed here BEFORE running the installation.
The installation process checks for these files as part of the pre-installation requirements.

