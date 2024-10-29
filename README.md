
# Flask App Deployment with Ansible (Development)

This project deploys a Flask application connected to a MySQL/MariaDB database in a development environment using Ansible. The app is served by Gunicorn, with Nginx as a reverse proxy, ensuring a development-ready setup.

## Project Structure

- **`inventory.txt`**: Inventory file specifying target hosts (IP addresses) for running the Ansible playbook.
- **`playbook.yml`**: The main Ansible playbook orchestrating tasks for deploying the application and configuring the environment.
- **`group_vars/`**: Contains group-specific variables, allowing centralized configuration for `db_and_web_servers`.
- **`tasks/`**: Houses individual task files for different components:
  - `app.py`: The Flask application code.
  - `deploy_db.yml`: Task for setting up the MySQL/MariaDB database.
  - `deploy_web.yml`: Task for setting up the Flask app with Gunicorn.

## File Details

### `playbook.yml`

This is the main Ansible playbook that automates the deployment process by calling individual tasks from `tasks/`, such as:

1. **Database Setup (`tasks/deploy_db.yml`)**:
   - Sets up the MySQL/MariaDB database and inserts sample data if needed.

2. **Web Application Setup (`tasks/deploy_web.yml`)**:
   - Deploys the Flask app using Gunicorn.

### `inventory.txt`

The inventory file defines the remote servers to configure:

```plaintext
[db_and_web_servers]
db_and_web_server1 ansible_host=10.10.0.115 ansible_ssh_pass=Passw0rd
db_and_web_server2 ansible_host=10.10.0.116 ansible_ssh_pass=Passw0rd
```

Each entry includes:
- `ansible_host`: IP address of the server.
- `ansible_ssh_pass`: Password for SSH access.

### `group_vars/db_and_web_servers.yml`

Contains group-specific variables for the `db_and_web_servers` group, centralizing configuration settings used in `playbook.yml`.

## Prerequisites

- Ansible installed on the control machine (your local or another machine).
- SSH access to the target servers specified in `inventory.txt`.

## Running the Playbook

1. **Test Connection**:
   Test the connection to your servers with:

   ```bash
   ansible -i inventory.txt -m ping all
   ```

   This should return a successful `pong` response from each server.

2. **Run the Playbook**:
   Execute the playbook to deploy the app:

   ```bash
   ansible-playbook playbook.yml -i inventory --ask-vault-pass
   ```

   The playbook will:
   - Install and configure all necessary software on each server.
   - Set up the database.
   - Deploy and start the Flask application.

3. **Access the Application**:
   Open a web browser and go to the IP address of any configured server. For example:

   ```
   http://10.10.0.115:5000
   ```

This setup ensures your Flask app runs in a development-grade environment using Ansible.
