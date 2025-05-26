## Overview

### Apache Kafka

Apache Kafka is a distributed event streaming platform. It is designed to handle large volumes of data in real-time and is widely used for:

* Real-time data pipelines
* Log aggregation
* Stream processing
* Communication between microservices

Kafka works by allowing **producers** to send messages to **topics**, where **consumers** can subscribe and read the messages in a decoupled and fault-tolerant manner.

---

### Apache Zookeeper

Zookeeper is a centralized service used by Kafka for:

* Maintaining metadata (such as topic configuration and broker information)
* Coordinating distributed nodes in the Kafka cluster
* Performing leader election to ensure high availability

Kafka 3.x still uses Zookeeper unless configured to run in KRaft (Kafka Raft) mode.

---

## Installation Steps on a New VM

Here are the steps taken to install and configure Kafka and Zookeeper on a fresh virtual machine using Ansible:

### 1. Install Required System Packages

Install necessary tools such as `wget`, `tar`, and `gnupg` to download and handle external files and repositories.

### 2. Set Up Java (Temurin JDK 21)

Add the GPG key and APT repository for Adoptium. Then install the Temurin JDK 21, which is required to run Kafka.

### 3. Create a Kafka System User

Add a dedicated `kafka` user. This user owns and runs Kafka processes, improving system security by isolating Kafka from root privileges.

### 4. Download and Extract Kafka

Download the official Kafka archive from Apache’s website. Extract it to `/opt`, then create a symlink (`/opt/kafka`) pointing to the versioned directory. This allows for easier upgrades later.

### 5. Set Ownership for Kafka Directory

Change the ownership of the extracted Kafka directory to the `kafka` user so it has the correct permissions to read and write necessary files.

### 6. Configure Kafka and Zookeeper

Deploy configuration files for:

* `zookeeper.properties` – defines how Zookeeper will run
* `server.properties` – defines Kafka broker settings such as ports, broker ID, log directories

These are usually templated to allow easy reuse and customization.

### 7. Create Systemd Service Files

Add systemd unit files to define how Zookeeper and Kafka should be started as services. This ensures that they start in the correct order and can be managed with systemctl.

### 8. Reload Systemd and Start Services

Reload the systemd manager to recognize the new service files. Then enable and start the Zookeeper and Kafka services so that they run immediately and automatically on reboot.

---

Here’s a **“Future Enhancements”** section you can include to outline next steps and improvements in your Kafka setup:

---

## Future Enhancements

### 1. Kafka UI – Kafbat or Other Management Interfaces

Adding a UI like [**Kafbat**](https://github.com/kafbat/kafka-ui):

* **Visual Topic Management**: Easily create, delete, and inspect topics.
* **Consumer Group Monitoring**: Track lag and offsets in real-time.
* **Broker Status Overview**: Check health, configuration, and metadata for each broker.
* **Message Browsing**: View raw messages for debugging or auditing.
* **Operational Efficiency**: Reduces need to rely on CLI commands for every operation.

---

### 2. Kafka Broker Scaling and Multi-Broker Testing

After initial single-node deployment, the next enhancement is to scale Kafka to multiple brokers. This brings:

* **High Availability**: Prevents single point of failure.
* **Load Distribution**: Better throughput and resilience.
* **Partition Replication**: Ensures message durability and fault tolerance.

Steps would include:

* Provisioning and configuring additional Kafka VMs.
* Assigning unique broker IDs.
* Configuring appropriate listeners and advertised hostnames.
* Testing inter-broker communication and replication.

---
