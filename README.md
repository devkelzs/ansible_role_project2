# LAMP Stack with WordPress Deployment Using Ansible

## Prerequisites

Before you begin, make sure you have the following:

1. **Remote Ubuntu Server**  
   - A clean Ubuntu server (18.04, 20.04, or newer) accessible via SSH.  
   - Obtain the server's IP address or domain name.

2. **SSH Access**  
   - SSH access to the server with either a password or preferably an SSH key (`.pem` or private key file).  
   - Ensure your local machine can connect via SSH on port 22.

3. **Ansible Installed Locally**  
   - Ansible installed on your local machine from where you will run the playbook.  
   - Install using package manager or pip:  
     ```bash
     sudo apt update && sudo apt install ansible
     ```  
     or  
     ```bash
     pip install ansible
     ```

4. **Basic Knowledge**  
   - Some familiarity with Linux terminal commands.  
   - Basic understanding of Ansible concepts (inventory, playbooks, roles) is helpful but not required.

5. **Internet Connectivity**  
   - Your server must have internet access to download software packages and WordPress files.


## Project Overview

This Ansible project automates the installation and configuration of a **LAMP stack** (Linux, Apache, MySQL, PHP) along with **WordPress** on your remote Ubuntu server.

By running the playbook, you will get:

- Apache HTTP server installed and configured.
- MySQL database installed with a dedicated WordPress database and user.
- PHP installed with all necessary modules for WordPress.
- Firewall configured to allow HTTP (port 80) and HTTPS (port 443) traffic.
- WordPress downloaded, extracted, and configured to run from your web root `/var/www/html`.


## Project Structure

lamp-ansible/
├── inventory.ini
├── site.yml
└── roles/
├── apache/
│ ├── tasks/
│ │ └── main.yml
│ └── templates/
│ └── wp-config.php.j2
├── mysql/
│ ├── tasks/
│ │ └── main.yml
│ └── vars/
│ └── main.yml
├── php/
│ └── tasks/
│ └── main.yml
└── firewall/
└── tasks/
└── main.yml


- `site.yml`: Main playbook to run all roles.
- `inventory.ini`: Defines target server(s) and connection info.
- Roles: Separate folders for Apache, MySQL, PHP, Firewall, each with tasks and templates.


## Role Summaries

### Apache Role

- Installs Apache web server.
- Ensures Apache service is running and enabled.
- Downloads the latest WordPress package.
- Removes default Apache index page.
- Extracts WordPress files to `/var/www/html` (web root).
- Sets ownership and permissions for WordPress files.
- Deploys a `wp-config.php` file from a template with database settings.

### MySQL Role

- Installs MySQL server.
- Starts and enables MySQL service.
- Installs required Python MySQL bindings for Ansible.
- Creates a WordPress database.
- Creates a MySQL user with privileges for the WordPress database.

### PHP Role

- Installs PHP and required PHP modules for WordPress.

### Firewall Role

- Configures UFW to allow web traffic on ports 80 and 443.


## Setup Instructions

#### Edit your inventory file
Open inventory.ini and specify your server IP and connection info:

[webservers]
```your IP ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/your-key.pem```
Replace IP address and SSH user/key as appropriate.

#### Configure MySQL variables
Edit roles/mysql/vars/main.yml to set WordPress database credentials:

db_name: wordpress_db
db_user: wordpress_user
db_password: secure_password_here
Make sure to choose a strong password.

#### Customize WordPress config template
roles/apache/templates/wp-config.php.j2 uses the above variables to generate WordPress configuration. You can customize this file if needed.

#### Run the Ansible playbook
Run the playbook to deploy your LAMP stack and WordPress:
 
```ansible-playbook -i inventory.ini site.yml```
This will run all roles in order and set up everything automatically.

After Deployment
Open your browser and navigate to your server IP or domain 

