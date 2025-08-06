Here's a clean and professional `README.md` file you can include with your Ansible WordPress deployment project. It explains the project, prerequisites, structure, usage, and customization.

---

### ✅ `README.md`

```markdown
# WordPress Deployment with Ansible

This project automates the installation and configuration of a full **WordPress stack** (NGINX + PHP + MariaDB) using **Ansible**, and links a custom **domain name** to your server.

---

## 🚀 Features

- Installs and configures:
  - NGINX web server
  - PHP and necessary extensions
  - MariaDB/MySQL with secure credentials
  - Latest WordPress
- Custom domain support via NGINX
- Firewall rules (UFW) for HTTP/S
- Modular Ansible roles
- Ready for SSL/HTTPS (optional)

---

## 📁 Project Structure

```

wordpress-ansible/
├── inventory.ini
├── wordpress\_setup.yml
├── roles/
│   ├── common/
│   ├── mysql/
│   ├── nginx/
│   ├── php/
│   └── wordpress/

````

---

## 🧰 Prerequisites

- A Linux server (Ubuntu 20.04/22.04 recommended)
- Domain name pointing to your server's public IP (via A record)
- Ansible installed on your control machine
- SSH access to the server (key or password)

---

## ⚙️ Configuration

### 1. Update `inventory.ini`

Replace with your server IP and SSH user:

```ini
[web]
192.168.1.14 ansible_user=ubuntu
````

> You may need to add `ansible_python_interpreter=/usr/bin/python3` depending on your system.

---

### 2. Update Domain Name in NGINX Template

Edit `roles/nginx/templates/wordpress.conf.j2`:

```nginx
server_name yourdomain.com www.yourdomain.com;
```

Replace with your actual domain.

---

### 3. (Optional) Update MySQL Credentials

Default credentials used in `roles/mysql/tasks/main.yml`:

```yaml
admin_user: admin_user
admin_pass: admin_pass
wpuser: wpuser
wp_password: wp_password
```

> In production, secure with **Ansible Vault**.

---

## 📦 How to Deploy

1. SSH into the server once and run:

```bash
sudo mysql
```

Then:

```sql
CREATE USER 'admin_user'@'localhost' IDENTIFIED BY 'admin_pass';
GRANT ALL PRIVILEGES ON *.* TO 'admin_user'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;
```

This avoids Ansible using the restricted `root` MySQL user.

---

2. From your local machine, run:

```bash
ansible-playbook -i inventory.ini wordpress_setup.yml
```

This will:

* Install packages
* Configure services
* Set up WordPress
* Serve your domain via NGINX

---

## 🔒 Optional: Enable HTTPS with Let's Encrypt

To secure your site with SSL:

1. Add these tasks to the NGINX role (or create a new `ssl` role):

```yaml
- name: Install Certbot
  apt:
    name:
      - certbot
      - python3-certbot-nginx
    state: present

- name: Obtain SSL certificate
  command: >
    certbot --nginx --non-interactive --agree-tos
    -m your-email@example.com
    -d yourdomain.com -d www.yourdomain.com
```

2. Reload NGINX and verify HTTPS.

---

## 🧼 Cleanup

* Change all passwords before going live
* Secure access with firewalls and SSH keys
* Remove default NGINX configs

---

---



```




```

