# Ansible AWS Web Server Automation

Ansible automation project that configures an Ubuntu AWS EC2 instance as an Nginx web server.

The project demonstrates Ansible inventory management, privilege escalation, package installation, service management, Jinja2 templating, handlers, idempotency, and automated HTTP verification.

## Architecture

```text
Developer
    │
    │ ansible-playbook
    ▼
Ansible Control Node
    │
    │ SSH
    ▼
AWS EC2
(Ubuntu)
    │
    ├── Install Nginx
    ├── Start Nginx
    ├── Enable Nginx
    └── Deploy HTML template
            │
            ▼
        Web Server
```

## Technologies

* Ansible
* AWS EC2
* Ubuntu
* Nginx
* YAML
* Jinja2
* SSH

## Prerequisites

Before running the project, make sure you have:

* An AWS EC2 Ubuntu instance
* Ansible installed on the control machine
* SSH access to the EC2 instance
* The EC2 security group allowing SSH traffic
* The EC2 security group allowing HTTP traffic on port 80

## Configure Inventory

Edit:

```text
inventory/hosts.ini
```

and replace the EC2 placeholder with your instance's public IP:

```ini
[webservers]
web01 ansible_host=<EC2_PUBLIC_IP> ansible_user=ubuntu
```

## Test Connectivity

Run:

```bash
ansible all -m ansible.builtin.ping
```

Expected result:

```text
web01 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

## Run the Playbook

Run:

```bash
ansible-playbook playbooks/webserver.yml --private-key ~/.ssh/my-key.pem
```

Ansible will:

1. Test connectivity.
2. Update the apt package cache.
3. Install Nginx.
4. Start Nginx.
5. Enable Nginx at boot.
6. Deploy the custom HTML page.
7. Verify that Nginx responds successfully.

## Verify the Web Server

After the playbook completes, open:

```text
http://<EC2_PUBLIC_IP>
```

You should see:

```text
DevOps Practice Server

Managed by Ansible

Hostname: <EC2_HOSTNAME>
Operating System: Ubuntu <VERSION>
```

## Idempotency

Run the playbook again:

```bash
ansible-playbook playbooks/webserver.yml --private-key ~/.ssh/my-key.pem
```

After the initial configuration, most tasks should report:

```text
ok
```

instead of:

```text
changed
```

This demonstrates Ansible's idempotent configuration management.

## 👨‍💻 Author

**Pushkar Narayan**
