---

# Flask App Deployment with Gunicorn and Nginx

This project sets up a Flask application deployed with Gunicorn and Nginx, secured with MariaDB, and managed by Ansible. Sensitive variables are encrypted using Ansible Vault for secure deployments.

## Project Structure

```
flask_app_VMs_prod/
├── group_vars/
│   └── db_and_web_servers.yml         # Encrypted group variables for production
├── host_vars/                         # Optional: Add host-specific variables here
├── inventory                          # Encrypted inventory file
├── playbook.yml                       # Main Ansible playbook
├── tasks/
│   ├── deploy_db.yml                  # Database deployment tasks
│   ├── deploy_web.yml                 # Web application deployment tasks
│   └── deploy_nginx.yml               # Nginx configuration tasks
└── README.md                          # Project documentation
```

## Prerequisites

- **Ansible**: Ensure Ansible is installed on the control node (e.g., the system running this playbook).
- **Ansible Vault**: Used for securing sensitive information.
- **Target VMs**: Target VMs should be running Debian or Ubuntu and configured in the `inventory` file.

## Initial Setup

1. **Encrypt Sensitive Files**:
   Use Ansible Vault to encrypt the inventory and `group_vars/db_and_web_servers.yml`:
   ```bash
   ansible-vault encrypt inventory
   ansible-vault encrypt group_vars/db_and_web_servers.yml
   ```

2. **Set Up Directory Structure**:
   Ensure the `tasks/` folder contains the specific deployment tasks (`deploy_db.yml`, `deploy_web.yml`, `deploy_nginx.yml`), and `group_vars/db_and_web_servers.yml` contains the necessary variables, such as:
   - `db_name`
   - `db_user`
   - `db_password`
   - Application environment variables

## Configuration Files

### `inventory`
This file defines the target hosts under the `db_and_web_servers` group. Ensure each host is listed with the correct SSH connection details.

### `group_vars/db_and_web_servers.yml`
Contains sensitive group variables for the database and app. Encrypt this file using Ansible Vault.

### `playbook.yml`
This playbook orchestrates the deployment:
- Installs necessary packages (e.g., Python, Gunicorn, Nginx, MariaDB).
- Configures the Flask application to run with Gunicorn.
- Sets up Nginx as a reverse proxy.
- Secures the database and configures users.

### Tasks Files
- **`tasks/deploy_db.yml`**: Manages MariaDB installation, database setup, and user configuration.
- **`tasks/deploy_web.yml`**: Handles Flask application setup and Gunicorn configuration.
- **`tasks/deploy_nginx.yml`**: Configures Nginx as a reverse proxy for Gunicorn.

## Running the Playbook

Run the playbook with Ansible Vault to deploy the Flask application securely:

```bash
ansible-playbook playbook.yml --ask-vault-pass -i inventory
```

### Explanation
- `--ask-vault-pass`: Prompts for the vault password to decrypt sensitive information.
- `-i inventory`: Specifies the inventory file containing the target hosts.

## Example: Using a Vault Password File (Optional)
To avoid entering the password each time, create a vault password file:

1. Store your password in a secure file, e.g., `/path/to/vault_password_file`.
2. Pass the file to Ansible with:
   ```bash
   ansible-playbook playbook.yml --vault-password-file /path/to/vault_password_file -i inventory
   ```

## Security Best Practices

- Use `group_vars` and `host_vars` for managing environment-specific configurations.
- Encrypt sensitive files using Ansible Vault.
- Follow secure coding practices when configuring user permissions and database access.

---
