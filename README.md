# RHEL-Project-02

This project that involves setting up a centralized logging system using Rsyslog on Red Hat Enterprise Linux (RHEL). Centralized logging is crucial for monitoring and troubleshooting in enterprise environments. We'll configure Rsyslog to collect logs from multiple servers and store them centrally.

### Project: Setting up Centralized Logging with Rsyslog on RHEL

#### Step 1: Install and Configure Rsyslog Server

First, we'll set up a Rsyslog server on a designated RHEL server.

1. **Install Rsyslog:**

   ```bash
   sudo yum install rsyslog
   ```

2. **Configure Rsyslog:**

   Edit the Rsyslog configuration file (`/etc/rsyslog.conf`) to enable receiving logs from remote servers:

   ```bash
   sudo vi /etc/rsyslog.conf
   ```

   Uncomment or add the following lines to allow Rsyslog to listen for incoming UDP and TCP connections:

   ```bash
   # Provides UDP syslog reception
   $ModLoad imudp
   $UDPServerRun 514

   # Provides TCP syslog reception
   $ModLoad imtcp
   $InputTCPServerRun 514
   ```

3. **Restart Rsyslog:**

   After making changes, restart the Rsyslog service:

   ```bash
   sudo systemctl restart rsyslog
   ```

#### Step 2: Configure Rsyslog Clients (Remote Servers)

Now, configure remote RHEL servers to send their logs to the centralized Rsyslog server.

1. **Install Rsyslog:**

   Install Rsyslog on each remote server from which you want to send logs:

   ```bash
   sudo yum install rsyslog
   ```

2. **Configure Rsyslog Client:**

   Edit the Rsyslog configuration file (`/etc/rsyslog.conf`) on each remote server:

   ```bash
   sudo vi /etc/rsyslog.conf
   ```

   Add the following line to forward logs to the centralized Rsyslog server (replace `centralized_rsyslog_server_ip` with the IP address of your Rsyslog server):

   ```bash
   *.* @centralized_rsyslog_server_ip:514
   ```

3. **Restart Rsyslog:**

   After configuring, restart the Rsyslog service on the remote servers:

   ```bash
   sudo systemctl restart rsyslog
   ```

#### Step 3: Verify Centralized Logging

Verify that logs are being received and stored on the centralized Rsyslog server.

1. **Check Rsyslog Logs:**

   Monitor Rsyslog logs on the centralized server to verify incoming logs:

   ```bash
   sudo tail -f /var/log/messages
   ```

2. **Generate Test Logs:**

   On one of the remote servers, generate some test logs (e.g., using `logger` command):

   ```bash
   logger "Testing centralized logging"
   ```

3. **Verify Logs on Central Server:**

   Check if the test logs appear in the `/var/log/messages` file on the centralized Rsyslog server.

#### Step 4: Additional Configuration (Optional)

- **Log Rotation:** Configure log rotation settings (`/etc/logrotate.d/rsyslog`) to manage log file sizes and retention.
  
- **Security:** Implement firewall rules and SELinux policies to secure Rsyslog communications.

### Summary

This project guides you through setting up a centralized logging system using Rsyslog on Red Hat Enterprise Linux. It involves configuring a Rsyslog server to receive logs from remote servers and setting up Rsyslog clients on those servers to forward logs. Centralized logging enhances monitoring, troubleshooting, and compliance efforts in enterprise environments. Adjust configurations and security measures according to your organization's policies and requirements.
