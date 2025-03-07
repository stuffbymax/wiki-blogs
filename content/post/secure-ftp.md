+++
date = '2025-02-27T09:24:14Z'
draft = false
title = "Secure FTP Server Setup on Arch Linux: A Tutorial"
description = "Secure FTP Server Setup on Arch Linux: A Tutorial"
categories = ["Tutorial"]
series = ["Tech"]
tags = ["FTP", "linux"]
aliases = ["secute-ftp"]
+++


This tutorial will guide you through setting up a secure FTP (FTPS) server on Arch Linux using `vsftpd`.  It uses passive mode for better compatibility with firewalls, and enforces SSL/TLS encryption for secure data transfer.  The tutorial uses `ftpuser` as an example user; remember to change this in production environments.

## Prerequisites

**Arch Linux Installation:**

A working Arch Linux installation.

**Root Access:**

You will need root privileges to run the commands.

**Basic Linux Knowledge:**

Familiarity with command-line operations and text editing.

## Steps

1.  **Update System Packages:**

    Open a terminal and update your system packages to ensure you have the latest versions.

    ```bash
    sudo pacman -Syu
    ```

    This command synchronizes the package database and updates all installed packages.  The `-Syu` flag combines `-Sy` (sync package database) and `-Su` (update system).
2.  **Install `vsftpd`:**

    Install the `vsftpd` package.

    ```bash
    sudo pacman -S vsftpd
    ```

    This command downloads and installs the `vsftpd` package from the Arch Linux repositories.
3.  **Backup Existing Configuration:**

    Before modifying the configuration file, create a backup of the original `vsftpd.conf` file.

    ```bash
    sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.bak
    ```

    This command copies the original configuration file to a backup named `vsftpd.conf.bak`.
4.  **Configure `vsftpd`:**

    Create a new `vsftpd.conf` file with the following configuration settings. This configuration enables secure connections and restricts user access.

    ```bash
    sudo nano /etc/vsftpd.conf
    ```

    Paste the following configuration into the file:

    ```
    # Basic Configurations
    listen=YES
    listen_ipv6=NO
    anonymous_enable=NO
    local_enable=YES
    write_enable=YES
    local_umask=022
    dirmessage_enable=YES
    use_localtime=YES
    xferlog_enable=YES
    connect_from_port_20=YES
    xferlog_file=/var/log/vsftpd.log
    xferlog_std_format=YES
    ftpd_banner=Welcome to Secure FTP Server.
    chroot_local_user=YES
    allow_writeable_chroot=NO
    secure_chroot_dir=/var/run/vsftpd/empty

    # Passive Mode Configuration
    pasv_enable=YES
    pasv_min_port=10000
    pasv_max_port=10100
    pasv_address=<YOUR_PUBLIC_IP_ADDRESS>

    # Logging
    log_ftp_protocol=YES
    vsftpd_log_file=/var/log/vsftpd.log

    # Security Settings
    ssl_enable=YES
    rsa_cert_file=/etc/ssl/certs/vsftpd.pem
    rsa_private_key_file=/etc/ssl/private/vsftpd.key
    ssl_tlsv1=YES
    ssl_sslv2=NO
    ssl_sslv3=NO
    require_ssl_cert=YES
    require_ssl_reuse=NO
    ```

    **Important Notes:**

    *   **`pasv_address`:** Replace `<YOUR_PUBLIC_IP_ADDRESS>` with the *public* IP address of your server.  If you're behind a NAT router, this is crucial for passive mode to work correctly.  You can often find your public IP address by visiting `https://api.ipify.org` from a browser on the server or using `curl https://api.ipify.org`.
    *   **`chroot_local_user=YES`:** This confines users to their home directories.  This is a key security measure.
    *   **`allow_writeable_chroot=NO`:**  This setting *prevents* users from writing to their chroot directory. When `chroot_local_user=YES`, it's very dangerous to allow the chroot directory to be writable. Without this setting, users could potentially escalate privileges.
    *   **`secure_chroot_dir=/var/run/vsftpd/empty`:**  This setting specifies an empty directory that `vsftpd` uses for security.  The directory *must* be owned by root and *must not* be writable by anyone else.
    *   **`ssl_tlsv1=YES`:** Enables TLS v1.0 or higher.
    *   **`ssl_sslv2=NO` and `ssl_sslv3=NO`:** Disables the use of SSLv2 and SSLv3 which are considered insecure protocols.
    *   **`require_ssl_cert=YES`:** Requires client to provide a certificate if `ssl_enable` is activated.
    *   **`require_ssl_reuse=NO`:** Prevents SSL session reuse, mitigating certain security vulnerabilities.

    Save the file and exit the editor. (In `nano`, press `Ctrl+X`, then `Y`, then `Enter`.)
5.  **Generate SSL Certificates:**

    Create a directory to store the private key and generate self-signed SSL certificates.

    ```bash
    sudo mkdir -p /etc/ssl/private
    sudo openssl req -newkey rsa:2048 -nodes -keyout /etc/ssl/private/vsftpd.key -x509 -days 365 -out /etc/ssl/certs/vsftpd.pem -subj "/CN=FTPServer"
    sudo chmod 600 /etc/ssl/private/vsftpd.key
    ```

    This command generates a 2048-bit RSA key and a self-signed certificate valid for 365 days.  The `-subj` flag sets the subject name of the certificate.  The `chmod 600` command restricts access to the private key to the root user only.  Provide appropriate information when prompted.

6.  **Create an FTP User:**

    Create a dedicated user for FTP access.  This example uses `ftpuser`.

    ```bash
    sudo useradd -m ftpuser
    sudo passwd ftpuser
    ```

    This command creates a new user named `ftpuser` with a home directory. You will be prompted to set a password for the user. **Remember this password!**

    To provide the user with an `upload` directory:

    ```bash
    sudo mkdir -p /home/ftpuser/ftp/upload
    sudo chown -R ftpuser:ftpuser /home/ftpuser/ftp
    sudo chmod -R 750 /home/ftpuser/ftp
    ```

    *   `chown -R ftpuser:ftpuser`: Sets the owner and group of the `/home/ftpuser/ftp` directory and its contents to `ftpuser`.
    *   `chmod -R 750`:  Sets permissions:
        *   User (owner): Read, write, and execute permissions (7).
        *   Group: Read and execute permissions (5).
        *   Others: No permissions (0). This prevents other users from accessing the files.
    * The owner of the directory `/home/ftpuser/ftp/upload` is now `ftpuser`, and the group is `ftpuser`. This ensures that only `ftpuser` (or users with the appropriate group membership) can manage the files within that directory. This combination makes sure the `chroot` is secure.

7.  **Enable and Start `vsftpd` Service:**

    Enable and start the `vsftpd` service to make it run on boot.

    ```bash
    sudo systemctl enable vsftpd
    sudo systemctl start vsftpd
    ```

    The `systemctl enable` command creates symbolic links to automatically start the service at boot time.  The `systemctl start` command starts the service immediately.

    You can check the status of the service with:

    ```bash
    sudo systemctl status vsftpd
    ```
8. **Configure the Firewall:**

    Arch Linux commonly uses `iptables` or `ufw`.  This example shows `iptables`. Adjust based on your firewall setup.

    ```bash
    sudo iptables -A INPUT -p tcp --dport 21 -j ACCEPT
    sudo iptables -A INPUT -p tcp --dport 10000:10100 -j ACCEPT
    ```

    These commands allow incoming TCP connections on port 21 (FTP control port) and the passive mode port range (10000-10100).  **Warning:**  Make sure you have SSH access allowed before making firewall changes, or you might lock yourself out of the server!  You might need to adjust these rules depending on the specific needs of your network. Consider adding a rule for the SSH port (usually 22) if needed.
    To save iptables rules so that they persist after reboot, you will need to install and use `iptables-persistent` or similar.

9.  **Test the FTP Server:**

    You can test the FTP server from a local machine or another machine on the network using an FTP client such as FileZilla.

    *   **Hostname/IP:** The IP address of your Arch Linux server.
    *   **Username:** `ftpuser` (or the user you created).
    *   **Password:** The password you set for the user.
    *   **Port:** 21
    *   **Encryption:**  Explicit FTPS.  (This depends on the client; some call it "FTP with explicit TLS/SSL" or similar.)

    **Important:**  Ensure that your FTP client is configured to use explicit FTPS and passive mode.

## Complete Script (for convenience)

Here's the complete script equivalent to the tutorial:

```bash
#!/bin/bash

# Exit immediately if a command exits with a non-zero status
set -e

# Ensure the script is run as root
if [[ $EUID -ne 0 ]]; then
   echo "This script must be run as root"
   exit 1
fi

echo "Updating package database..."
pacman -Syu --noconfirm

echo "Installing vsftpd..."
pacman -S vsftpd --noconfirm

echo "Backing up the original configuration file..."
cp /etc/vsftpd.conf /etc/vsftpd.conf.bak

echo "Writing a new vsftpd configuration file..."
cat <<EOL > /etc/vsftpd.conf
# Basic Configurations
listen=YES
listen_ipv6=NO
anonymous_enable=NO
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
use_localtime=YES
xferlog_enable=YES
connect_from_port_20=YES
xferlog_file=/var/log/vsftpd.log
xferlog_std_format=YES
ftpd_banner=Welcome to Secure FTP Server.
chroot_local_user=YES
allow_writeable_chroot=NO
secure_chroot_dir=/var/run/vsftpd/empty

# Passive Mode Configuration
pasv_enable=YES
pasv_min_port=10000
pasv_max_port=10100
pasv_address=$(ip addr show | grep "inet " | grep -v "127.0.0.1" | awk '{print $2}' | cut -d'/' -f1)

# Logging
log_ftp_protocol=YES
vsftpd_log_file=/var/log/vsftpd.log

# Security Settings
ssl_enable=YES
rsa_cert_file=/etc/ssl/certs/vsftpd.pem
rsa_private_key_file=/etc/ssl/private/vsftpd.key
ssl_tlsv1=YES
ssl_sslv2=NO
ssl_sslv3=NO
require_ssl_cert=YES
require_ssl_reuse=NO
EOL

echo "Generating SSL certificates..."
mkdir -p /etc/ssl/private
openssl req -newkey rsa:2048 -nodes -keyout /etc/ssl/private/vsftpd.key -x509 -days 365 -out /etc/ssl/certs/vsftpd.pem -subj "/CN=FTPServer"
chmod 600 /etc/ssl/private/vsftpd.key

echo "Creating FTP directories for the example user..."
useradd -m ftpuser
echo "Please enter a password for ftpuser:"
passwd ftpuser
mkdir -p /home/ftpuser/ftp/upload
chown -R ftpuser:ftpuser /home/ftpuser/ftp
chmod -R 750 /home/ftpuser/ftp

echo "Enabling and starting the vsftpd service..."
systemctl enable vsftpd
systemctl restart vsftpd

echo "Configuring firewall rules..."
iptables -A INPUT -p tcp --dport 21 -j ACCEPT
iptables -A INPUT -p tcp --dport 10000:10100 -j ACCEPT

echo "Setup complete!"
echo "You can now connect to the FTP server using the following credentials:"
echo "  Username: ftpuser"
echo "  Password: <your_password>"
echo "  Local IP Address: $(hostname -I | awk '{print $1}')"
echo "Make sure to change the password and customize configurations for production use."
