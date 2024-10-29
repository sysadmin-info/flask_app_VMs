---

# Flask Application Deployment with MySQL in Development

This setup uses Ansible to deploy a Flask application with MySQL on a development environment. The application consists of three main roles: `packages`, `mysql_db`, and `flask_web`.

## Prerequisites

1. **Ansible** installed on the control machine.
2. **Python3** installed on the managed host.
3. **Encrypted files**: Inventory and `group_vars` contain encrypted configuration variables managed by Ansible Vault.
4. Ensure the **`ansible-vault`** password is available for decryption when running the playbook.

## Project Structure

```
.
├── playbook.yml                  # Main playbook to execute roles
├── group_vars/
│   └── db_and_web_servers.yml    # Encrypted group variables with database credentials and configurations
├── roles/
│   ├── packages/
│   │   └── tasks/
│   │       └── main.yml          # Installs required packages including Python3
│   ├── mysql_db/
│   │   └── tasks/
│   │       └── main.yml          # Configures and sets up MySQL database and user
│   └── flask_web/
│       └── tasks/
│           └── main.yml          # Deploys the Flask application and ensures it runs on port 5000
└── app.py                        # Flask application source file
```

## File Descriptions

- **playbook.yml**: The main Ansible playbook file that includes roles to set up the environment.
- **group_vars/db_and_web_servers.yml**: Encrypted variables for MySQL credentials.
- **roles/packages/tasks/main.yml**: Installs necessary system packages including Python3.
- **roles/mysql_db/tasks/main.yml**: Configures MySQL, creating a database and user.
- **roles/flask_web/tasks/main.yml**: Deploys the Flask app on port 5000 and sets up dependencies.
- **app.py**: The Flask application file with endpoints configured to interact with the MySQL database.

## Deployment Instructions

1. **Decrypt Inventory and Group Variables** (if needed):
   Ensure you have the Ansible Vault password ready, as `inventory` and `group_vars/db_and_web_servers.yml` are encrypted.

2. **Run the Playbook**:
   Use the following command to run the playbook. The playbook will automatically apply roles to set up the environment.

   ```bash
   ansible-playbook playbook.yml -i inventory --ask-vault-pass
   ```

3. **Access the Application**:
   - After the playbook runs successfully, the Flask application should be accessible on port 5000.
   - Open a browser or use `curl` to check:

     ```bash
     curl http://localhost:5000
     ```

4. **Endpoints**:
   - **`/`** - Basic welcome message.
   - **`/how_are_you`** - Responds with a greeting.
   - **`/read_from_database`** - Fetches data from the MySQL database.

## Notes

- **Sensitive Variables**: The MySQL credentials and other sensitive information are stored in the encrypted `group_vars/db_and_web_servers.yml`.
- **Ansible Vault**: To edit encrypted files, use:

  ```bash
  ansible-vault edit group_vars/db_and_web_servers.yml
  ```
