# Java Application Infrastructure as Code (IaC) - Vagrant Multi-VM Environment

This repository contains Infrastructure as Code (IaC) using **Vagrant** and **VirtualBox** to automatically provision a multi-node local development and testing environment. The infrastructure simulates a complete multi-tier Java web application stack.

---

## 🏗️ Architecture Overview

The multi-VM environment provisions 5 dedicated Virtual Machines connected via a private network (`192.168.56.0/24` subnet):

| VM Name | Hostname | IP Address | OS / Base Box | Role / Description |
| :--- | :--- | :--- | :--- | :--- |
| **`database`** | `MySql-Database` | `192.168.56.15` | Fedora 42 (`bento/fedora-42`) | MySQL Database Server |
| **`mamcache`** | `Memcache` | `192.168.56.16` | Fedora 42 (`bento/fedora-42`) | Memcached Caching Engine |
| **`rabbitmq`** | `Rabbitmq` | `192.168.56.17` | Fedora 42 (`bento/fedora-42`) | RabbitMQ Message Broker |
| **`tomcat`** | `Tomcat` | `192.168.56.18` | Fedora 42 (`bento/fedora-42`) | Java Application Server |
| **`nginx`** | `Nginx` | `192.168.56.19` | Ubuntu 26.04 (`arm64`) | Web Server & Reverse Proxy |

---

## ⚙️ Prerequisites

Before getting started, make sure you have the following installed on your host machine:

- [VirtualBox](https://www.virtualbox.org/) (Hypervisor)
- [Vagrant](https://www.vagrantup.com/) (v2.0+)
- **Vagrant Hostmanager Plugin** (Required for automatic `/etc/hosts` management):
  ```bash
  vagrant plugin install vagrant-hostmanager
  ```

---

## 🚀 Getting Started

### 1. Clone the Repository
```bash
git clone https://github.com/your-username/Java-App-IAC-Vagrant-File.git
cd Java-App-IAC-Vagrant-File
```

### 2. Provision the Infrastructure
To spin up all 5 virtual machines at once:
```bash
vagrant up
```

Alternatively, you can bring up a specific virtual machine individually:
```bash
vagrant up tomcat
```

### 3. Accessing the Virtual Machines
You can SSH into any machine using its defined name:
```bash
vagrant ssh nginx
vagrant ssh tomcat
vagrant ssh database
```

### 4. Stopping or Destroying the Environment
To suspend all VMs without deleting them:
```bash
vagrant suspend
```

To halt/shutdown all VMs:
```bash
vagrant halt
```

To destroy the entire environment and free up disk space:
```bash
vagrant destroy -f
```

---

## 📝 Configuration Highlights

- **Automatic Host Resolution**: Uses the `vagrant-hostmanager` plugin to automatically update the `/etc/hosts` file on both the host system and guest VMs.
- **Resource Allocation**: Each VM is provisioned with **1024 MB (1GB)** of RAM by default under the VirtualBox provider.
- **Networking**: All VMs communicate securely isolated from external traffic using a private network adapter.

---

## 📄 License

This project is open-source and available under the [MIT License].