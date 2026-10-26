# Automated Apache Web Server Provisioning with Vagrant & UTM

This repository contains Infrastructure as Code (IaC) configuration using **Vagrant** to automatically set up and provision a light web server running an Apache HTTP server with a pre-configured web template (**Mochi Space**).

This configuration is optimized for Apple Silicon (ARM64) Macs using the **UTM** hypervisor provider.

## 🏗️ Environment Specifications

| Component | Detail | 
| ----- | ----- | 
| **OS Base Box** | Ubuntu 26.04 (`luminositylabsllc/bento-ubuntu-26.04-arm64`) | 
| **Provider** | UTM | 
| **Resources Allocated** | 2 vCPUs, 1024 MB (1 GB) RAM | 
| **Private IP** | `192.168.56.30` | 
| **Public Network** | Enabled (Bridged Mode) | 
| **Web Server** | Apache2 (`httpd`) | 
| **Deployed Content** | [Mochi Space HTML Template](https://www.tooplate.com/view/2159-mochi-space) | 

## ⚙️ Prerequisites

Before running this setup, ensure you have installed:

1. [Vagrant](https://www.vagrantup.com/) (v2.3+)

2. [UTM Hypervisor](https://mac.getutm.app/) (for Apple Silicon/macOS)

3. Vagrant UTM Provider Plugin:

   ```
   vagrant plugin install vagrant-utm
   
   ```

## 🚀 Quick Start

### 1. Provision and Start the VM

Run the following command in the root folder of this repository:

```
vagrant up --provider=utm

```

The shell provisioner will automatically:

* Update Ubuntu system packages (`apt-get update`).

* Install Apache2, `wget`, and `unzip`.

* Start and enable the Apache service.

* Download the **Mochi Space** web template and deploy it to `/var/www/html/`.

* Restart Apache to serve the new site.

### 2. Access the Web Application

Once `vagrant up` completes, open your web browser and navigate to:

```
http://192.168.56.30

```

### 3. SSH into the Virtual Machine

To access the guest OS terminal:

```
vagrant ssh

```

### 4. Teardown

To stop the virtual machine:

```
vagrant halt

```

To completely delete the virtual machine and clean up resources:

```
vagrant destroy -f

```

## 📜 Provisioning Logic Breakdown

```
sudo -i 
apt-get update
apt-get install -y apache2 wget unzip
systemctl start apache2
systemctl enable apache2

# Download and extract the template
mkdir -p /tmp/Mochi
cd /tmp/Mochi
wget https://www.tooplate.com/zip-templates/2159_mochi_space.zip
unzip 2159_mochi_space.zip
cp -r 2159_mochi_space/* /var/www/html/

# Apply changes & cleanup
systemctl restart apache2
cd /tmp/
rm -rf Mochi

```

## 