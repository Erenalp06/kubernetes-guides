
# Harbor Installation Guide

This document provides a step-by-step guide on how to install Harbor, an open-source container registry, on a Linux server. This guide covers the installation process, configuration, and initial setup.

## Prerequisites

Before starting, ensure that you have the following:

- A Linux server with root or sudo access.
- Docker installed on the server.
- Docker Compose installed on the server.

## Step 1: Download and Extract Harbor

1. First, download the Harbor installer from the official Harbor GitHub repository:

    ```bash
    https://github.com/goharbor/harbor/releases/download/v2.10.3/harbor-online-installer-v2.10.3.tgz
    ```

2. Extract the downloaded tarball:

    ```bash
    tar xvf harbor-online-installer-v2.10.3.tgz
    ```

3. Change to the Harbor directory:

    ```bash
    cd harbor
    ```

## Step 2: Configure Harbor

1. Rename the default configuration template file:

    ```bash
    mv harbor.yml.tmpl harbor.yml
    ```

2. Edit the `harbor.yml` file to configure Harbor according to your needs. Key configurations include:

    - **hostname:** The domain name or IP address to access the Harbor UI and registry services.

    - **http:** Configure the HTTP port (default is 80).

    - **https:** Configure the HTTPS port (default is 443) and provide the paths to your SSL certificate and private key files.

    Example configuration for HTTP access:

    ```yaml
    hostname: reg.mydomain.com

    http:
      port: 80

    # https:
    #   port: 443
    #   certificate: /your/certificate/path
    #   private_key: /your/private/key/path
    ```

3. Save the `harbor.yml` file after making the necessary changes.

## Step 3: Install and Start Harbor

1. Run the Harbor installation script:

    ```bash
    sudo ./install.sh
    ```

2. Once the installation is complete, start Harbor using Docker Compose:

    ```bash
    sudo docker-compose up -d
    ```

## Step 4: Access the Harbor UI

1. Open your web browser and navigate to the hostname or IP address you configured in the `harbor.yml` file:

    ```plaintext
    http://reg.mydomain.com
    ```

2. Log in with the default credentials:

    - **Username:** `admin`
    - **Password:** `Harbor12345`

3. After logging in, you can change the default password by going to the user settings.

## Troubleshooting

If you encounter issues during installation or startup, consider the following:

- Ensure Docker and Docker Compose are up to date.
- Check file permissions, especially for the configuration files.
- Review the Docker daemon logs for more detailed error messages:

    ```bash
    sudo journalctl -u docker.service
    ```

- Make sure no other services are conflicting with the ports used by Harbor.

## Conclusion

You have successfully installed and configured Harbor on your Linux server. You can now use Harbor as your container registry for managing and distributing Docker images.
