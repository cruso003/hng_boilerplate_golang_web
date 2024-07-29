# Automated Deployment and Configuration with Ansible for Boilerplates

## Overview

This project demonstrates the automated deployment and configuration of a boilerplate application using Infrastructure as Code (IaC) with Ansible. The task involves setting up an environment that includes a PostgreSQL database, and Nginx as a reverse proxy on a remote Ubuntu 22.04 server. The configuration ensures the application runs smoothly, logs errors and outputs correctly, and secures sensitive information.

## Repository Structure

```
boilerplate/
├── ansible/
│   ├── main.yaml
│   ├── inventory.cfg
│   └── ansible.cfg
├── .github/
├── external/
├── internal/
├── pkg/
├── scripts/
├── services/
├── tests/
├── utility/
├── .dockerignore
├── .gitignore
├── app-sample.env
├── Dockerfile
├── ecosystem.config.js
├── go.mod
├── go.sum
├── LICENSE
├── log.json
├── main.go
├── README.md
├── run_deployment_app.sh
├── run_production_app.sh
├── run_staging_app.sh
```

## Ansible Configuration

### main.yaml

This playbook performs the following actions:

1. **System Preparation**:
   - Installs essential packages like Git, curl, and wget.
   - Installs PostgreSQL and sets up the necessary database and user.
   - Saves the PostgreSQL credentials securely.

2. **User and Directory Setup**:
   - Creates a user `hng` with sudo privileges.
   - Clones the boilerplate repository from the devops branch into `/opt/stage_5b`.
   - Sets appropriate permissions for the cloned directory.

3. **Application Setup**:
   - Installs Go and sets up the environment for the `hng` user.
   - Builds the application and ensures it runs on port `3000` (localhost).

4. **Nginx Configuration**:
   - Installs and configures Nginx to reverse proxy requests from port 80 to the application on port 3000.

5. **Logging**:
   - Sets up directories and files for logging stdout and stderr.
   - Ensures logs are owned by the `hng` user.

### ansible.cfg

Configures Ansible to use the appropriate inventory file, remote user, and SSH private key:

```ini
[defaults]
inventory = inventory.cfg
remote_user = ubuntu
private_key_file = /home/truthserum/Devops5/Devops-5b.pem
host_key_checking = False
allow_world_readable_tmpfiles = True

[vars]
ansible_python_interpreter = /usr/bin/python3
```

### inventory.cfg

Defines the hosts for Ansible to manage:

```ini
[hng]
54.158.11.23 ansible_user=ubuntu
```

## Usage Instructions

1. **Ensure Ansible and required dependencies are installed** on your local machine.
2. **Prepare the SSH key** (`Devops-5b.pem`) and ensure it has the correct permissions.
3. **Run the playbook** on a fresh Ubuntu 22.04 server:

   ```bash
   ansible-playbook ansible/main.yaml -b
   ```

4. The playbook will:
   - Clone the repository and set up the environment.
   - Install necessary dependencies and configure the PostgreSQL database.
   - Set up Nginx and the application.
   - Configure logging and ensure the application is running correctly.

## Important Notes

- **Do not push sensitive information** such as the `inventory.cfg` or any private keys to public repositories.
- **Testing and Verification**: Ensure all tasks execute without errors and the application is accessible via the provided Nginx proxy.

## Conclusion

This setup provides a robust and automated deployment process for the golang web boilerplate application, ensuring that all components are correctly configured and operational. The use of Ansible simplifies the process, making it reproducible and easy to manage.