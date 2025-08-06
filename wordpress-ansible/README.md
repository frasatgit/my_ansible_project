Sure! Here's a **single, professional `README.md` file** ready for your project — clean, production-ready, and suitable for publishing on GitHub.

---

### ✅ Final `README.md`

```markdown
# 🚀 WordPress Deployment with Ansible

This project automates the deployment of a full WordPress stack using **Ansible**. It installs and configures **NGINX**, **PHP**, **MariaDB**, and **WordPress**, and optionally sets up a domain with HTTPS.

---

## 📦 Features

- ✔️ One-command WordPress installation
- 🌐 NGINX configured for your custom domain
- 🐘 PHP and required extensions
- 🛡️ MariaDB with secure credentials
- 🔥 UFW firewall configuration
- 📝 Structured Ansible roles
- 🔒 Ready for Let's Encrypt SSL (optional)

---

## 📁 Project Structure

```

wordpress-ansible/
├── inventory.ini                  # Ansible inventory with target server(s)
├── wordpress\_setup.yml           # Main playbook
└── roles/
├── common/                   # Basic packages and system setup
├── php/                      # PHP installation and configuration
├── mysql/                    # MariaDB installation and DB setup
├── nginx/                    # NGINX install and domain config
└── wordpress/                # WordPress download and setup

````

---

## 🔧 Prerequisites

- A server with Ubuntu 20.04/22.04 (tested)
- SSH access to the server (`ansible_user` with sudo)
- Domain name pointing to the server's public IP
- Ansible installed on your control machine

---

## ⚙️ Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/wordpress-ansible.git
cd wordpress-ansible
````

---

### 2. Configure Inventory

Edit `inventory.ini` with your server IP and SSH user:

```ini
[web]
192.168.1.14 ansible_user=ubuntu ansible_python_interpreter=/usr/bin/python3
```

---

### 3. Configure Domain in NGINX

Edit the file:

```
roles/nginx/templates/wordpress.conf.j2
```

Replace:

```nginx
server_name example.com www.example.com;
```

With your domain name.

---

### 4. (One-Time) Create MySQL Admin User Manually

SSH into the server:

```bash
ssh ubuntu@192.168.1.14
sudo mysql
```

Run:

```sql
CREATE USER 'admin_user'@'localhost' IDENTIFIED BY 'admin_pass';
GRANT ALL PRIVILEGES ON *.* TO 'admin_user'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;
```

This allows Ansible to manage the database (WordPress DB + user).

---

### 5. Run the Playbook

From your local machine:

```bash
ansible-playbook -i inventory.ini wordpress_setup.yml
```

After a few minutes, WordPress will be installed and accessible via your domain name.

---

## 🔐 Optional: Enable HTTPS with Let's Encrypt

To add SSL support:

1. Update the `nginx` role to install `certbot`:

```yaml
- name: Install Certbot
  apt:
    name: [certbot, python3-certbot-nginx]
    state: present
```

2. Add the SSL task:

```yaml
- name: Obtain SSL certificate
  command: >
    certbot --nginx --non-interactive --agree-tos
    -m your-email@example.com
    -d yourdomain.com -d www.yourdomain.com
```

3. Reload NGINX and access your WordPress site via HTTPS.

---

## 🛡️ Security Notes

* Replace all default credentials:

  * `admin_user` / `admin_pass`
  * `wpuser` / `wp_password`
* Use **Ansible Vault** to store sensitive data.
* Harden SSH and limit sudo access.
* Set up auto-renewal for Let's Encrypt (cron/systemd timer).

---

## 📄 License

MIT License. Free to use, modify, and distribute.

---

## 🙋‍♂️ Support & Contributions

Contributions are welcome! Feel free to fork this repo, submit pull requests, or open issues for improvements or bugs.

---

