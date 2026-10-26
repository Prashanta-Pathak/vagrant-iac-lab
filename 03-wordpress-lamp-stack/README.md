# Automated WordPress Stack Provisioning with Vagrant

This repository provides an Infrastructure as Code (IaC) configuration using **Vagrant** to deploy a fully functional, self-hosted **WordPress** stack (LAMP: Linux, Apache, MySQL, PHP) on Ubuntu 26.04.

The provisioning script handles everything automatically—from package installation and MySQL database creation to Apache virtual host configuration and security hardening via WP-CLI.

---

## 🏗️ Environment Specifications

| Component | Specification |
| --- | --- |
| **OS Base Box** | Ubuntu 26.04 ARM64 (`luminositylabsllc/bento-ubuntu-26.04-arm64`) |
| **Provider** | VirtualBox / Provider-compatible (`vb`) |
| **Resources** | 4 vCPUs, 5000 MB (5 GB) RAM |
| **Private IP** | `192.168.56.60` |
| **Public Network** | Enabled (Bridged Mode) |
| **Web Server** | Apache2 with `mod_rewrite` enabled |
| **Database** | MySQL Server |
| **Language Runtime**| PHP with required extensions (`bcmath`, `curl`, `imagick`, `intl`, `mbstring`, `mysql`, `xml`, `zip`) |
| **CLI Tooling** | WP-CLI (Command-line interface for WordPress) |

---

## ⚙️ Prerequisites

Before launching the virtual machine, ensure you have installed:

1. [Vagrant](https://www.vagrantup.com/) (v2.3+)
2. VirtualBox (or your preferred hypervisor matching provider `vb`)

---

## 🚀 Quick Start

### 1. Launch & Provision the VM

Run the following command from the project root:

```bash
vagrant up
```

During provisioning, the automated inline shell script will execute the following steps:
1. Update apt repositories and install Apache2, MySQL Server, PHP, and required modules.
2. Download and extract the latest official WordPress core release into `/srv/www/wordpress`.
3. Set up an Apache VirtualHost pointing to `/srv/www/wordpress` and enable rewrite rules.
4. Create the MySQL `wordpress` database and user credentials (`wordpress` / `devops`).
5. Generate `wp-config.php` dynamically and populate database credentials.
6. Download **WP-CLI**, install it globally to `/usr/local/bin/wp`, and execute `wp config shuffle-salts` for dynamic security salt generation.
7. Set correct directory permissions (`www-data:www-data`) and restart services.

---

### 2. Complete WordPress Setup

Once `vagrant up` completes:

1. Open your web browser and go to:
   ```text
   http://192.168.56.60
   ```
2. Complete the standard 1-minute WordPress web installation wizard (Site Title, Admin Username, Password, Email).

---

## 🛠️ Operational Commands

| Command | Action |
| --- | --- |
| `vagrant up` | Provision and boot the VM environment |
| `vagrant ssh` | Connect directly into the guest VM |
| `vagrant halt` | Gracefully shut down the VM |
| `vagrant reload --provision` | Reboot VM and re-run shell provisioning |
| `vagrant destroy -f` | Terminate and clean up all VM resources |

---

## 🔐 Database & Configuration Summary

* **Database Name:** `wordpress`
* **Database User:** `wordpress`
* **Database Host:** `localhost`
* **Document Root:** `/srv/www/wordpress`
* **Apache Config:** `/etc/apache2/sites-available/wordpress.conf`

---

## 📜 Shell Provisioning Workflow Summary

```bash
# 1. Core package installation
sudo apt update && sudo apt install -y apache2 mysql-server php ...

# 2. WordPress download & directory setup
mkdir -p /srv/www && curl https://wordpress.org/latest.tar.gz | sudo -u www-data tar zx -C /srv/www

# 3. Apache VirtualHost creation & module enablement
a2ensite wordpress && a2enmod rewrite && a2dissite 000-default

# 4. MySQL database & user creation
mysql -u root -e "CREATE DATABASE wordpress; ..."

# 5. Security hardening via WP-CLI
curl -s -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
chmod +x wp-cli.phar && sudo mv wp-cli.phar /usr/local/bin/wp
sudo wp config shuffle-salts --allow-root --path=/srv/www/wordpress
```