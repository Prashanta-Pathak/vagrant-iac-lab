# 🚀 Infrastructure as Code (IaC) - Vagrant Lab Suite

A collection of automated, local development environments provisioned using **Vagrant**, **VirtualBox**, **UTM**, and **Shell Scripts**. This repository demonstrates various infrastructure patterns ranging from lightweight web servers to multi-node enterprise application stacks and dynamic CMS deployments.

---

## 📁 Projects Overview

```text
.
├── 01-java-multi-node-stack/    # 5-Node Microservices/Application Environment
├── 02-apache-mochi-website/     # Single-Node Apache Web Server with Mochi Template
└── 03-wordpress-lamp-stack/     # Fully Automated LAMP Stack with WordPress & WP-CLI
```

---

## 📌 Project Details

### 1️⃣ Enterprise Java Application Stack (`/01-java-multi-node-stack`)
A multi-machine environment establishing isolated services for a standard 3-tier Java web application.

* **Nodes & Services:**
  * `database`: Fedora 42 with MySQL (`192.168.56.15`)
  * `mamcache`: Fedora 42 with Memcached (`192.168.56.16`)
  * `rabbitmq`: Fedora 42 with RabbitMQ (`192.168.56.17`)
  * `tomcat`: Fedora 42 App Server (`192.168.56.18`)
  * `nginx`: Ubuntu 26.04 Load Balancer / Reverse Proxy (`192.168.56.19`)
* **Key Features:** Private network communication across VMs, host-manager integration for local DNS lookup.

---

### 2️⃣ Lightweight Apache Web Server (`/02-apache-mochi-website`)
An automated single-node web server deployment optimized for Apple Silicon (ARM64) via the **UTM** hypervisor provider.

* **OS / Provider:** Ubuntu 26.04 ARM64 (`luminositylabsllc/bento-ubuntu-26.04-arm64`) on UTM
* **IP Address:** `192.168.56.30` (Private Network) + Bridged Public Network
* **Automated Provisioning:** Downloads, extracts, and serves the *Mochi Space* web template over Apache2.

---

### 3️⃣ Automated WordPress LAMP Stack (`/03-wordpress-lamp-stack`)
A full-stack CMS deployment configured entirely through automated shell provisioning scripts.

* **Specs:** 4 vCPUs, 5 GB RAM (`192.168.56.60`)
* **Stack:** Linux, Apache2, MySQL, PHP (with essential modules: `mbstring`, `imagick`, `curl`, etc.)
* **Automation Highlights:**
  * Automated database & DB user creation (`wordpress` / `devops`)
  * Apache VirtualHost setup with URL rewrite enabled
  * Automated global installation of **WP-CLI**
  * Dynamic security salt rotation via `wp config shuffle-salts`

---

## 🛠️ Prerequisites

Ensure you have the following tools installed on your host machine:

* [Vagrant](https://www.vagrantup.com/downloads) (v2.3+)
* [VirtualBox](https://www.virtualbox.org/) (for projects 1 and 3)
* [UTM](https://mac.getutm.app/) (for macOS ARM64 / project 2, if running UTM provider)
* `vagrant-hostmanager` plugin (optional, for project 1):
  ```bash
  vagrant plugin install vagrant-hostmanager
  ```

---

## 🚀 How to Run a Project

1. **Clone the repository:**
   ```bash
   git clone https://github.com/<your-username>/vagrant-iac-lab.git
   cd vagrant-iac-lab
   ```

2. **Navigate to desired project folder:**
   ```bash
   cd 03-wordpress-lamp-stack
   ```

3. **Spin up the environment:**
   ```bash
   vagrant up
   ```

4. **Access the environment:**
   * **WordPress (Project 3):** Navigate to `http://192.168.56.60` in your browser.
   * **Apache Site (Project 2):** Navigate to `http://192.168.56.30`.
   * **SSH into guest machine:** `vagrant ssh`

5. **Tear down resources when finished:**
   ```bash
   vagrant destroy -f
   ```

---

## 📄 License
This repository is open-source and available under the [MIT License](LICENSE).