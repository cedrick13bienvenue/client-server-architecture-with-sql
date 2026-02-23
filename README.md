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
