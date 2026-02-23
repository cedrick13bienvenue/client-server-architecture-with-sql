# Client-Server Architecture with MySQL

## Project Overview

This project demonstrates the implementation of a **Client-Server Architecture** using two remote AWS EC2 instances. One instance is configured as a dedicated **Database Server** running MySQL, and the other acts as a **Database Client**. This setup provides hands-on experience with remote database connectivity, networking security, and firewall configuration.

---

## Phase 1: Provisioning the Compute Instances

To demonstrate remote communication, we require two distinct virtual environments within the same network.

### 1.1 EC2 Instance Setup

* **Launch Instance A (DB Server):** Name this instance `mysql server`.
* **Launch Instance B (Client):** Name this instance `mysql client`.
* **AMI:** Ubuntu 24.04 LTS for both.

> **Metadata:** Take note of the **Private IP Address** of your `mysql client`. You will use this to restrict database access for maximum security.

---

## Phase 2: Database Server Configuration

In this phase, we install the MySQL engine on **Server A** and configure it to accept requests from our client.

### 2.1 Installation

* **Update and Install MySQL Server:**

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install mysql-server -y

```

* **Verify service status:**

```bash
sudo systemctl status mysql

```

> **Expected Output:** The terminal should show the service as **"active (running)"**.

### 2.2 Security Group Configuration (Port 3306)

MySQL listens on **TCP Port 3306** by default. We must "open the door" in the AWS Security Group.

* **Rule Setup:** Add an Inbound Rule to the `mysql server` Security Group.
* **Type:** MySQL/Aurora (Port 3306).
* **Source:** Enter the **Private IP of the `mysql client**` (e.g., `172.31.x.x/32`).

> **Security Note:** By using the specific private IP of the client, we ensure no other machine on the internet can attempt to connect to our database.

### 2.3 Enabling Remote Access (bind-address)

By default, MySQL only listens for local connections (`127.0.0.1`). We must modify the configuration to listen on all network interfaces.

* **Edit the config file:**

```bash
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf

```

* **Modification:** Locate the `bind-address` line and change it to `0.0.0.0`.

```text
bind-address = 0.0.0.0

```

* **Restart MySQL to apply changes:**

```bash
sudo systemctl restart mysql

```

---

## Phase 3: Client Machine Configuration

Now, we prepare **Server B** to act as the client that will send requests to our server.

### 3.1 Installation

* **Install MySQL Client utility:**

```bash
sudo apt update
sudo apt install mysql-client -y

```

> **Note:** The client machine does not need the full `mysql-server` package, only the `mysql-client` tool to communicate with the remote engine.

---