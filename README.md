
***This repository contains the scripts and documentation required to deploy, configure, and manage a MariaDB cluster using a Master–Replica architecture on Linux (Ubuntu 22.04) with Ansible.
It also includes the configuration of two proxy layers, MaxScale and ProxySQL, to manage traffic and distribute load between the database servers.

## System Architecture

* **MariaDB Cluster:** One Master and two Replicas.
* **Ansible:** Used for automating installation, configuration, and management.
* **MaxScale:** Proxy layer for connection management and query routing (running on server `192.168.0.0`).
* **ProxySQL:** Another proxy layer with similar capabilities (running on server `192.168.0.0`).

## Prerequisites

* Linux operating system (Ubuntu 22.04) on all servers.
* SSH access to servers with sudo privileges.
* Ansible installed on the control node (Controller).
* Servers properly defined and reachable via Ansible (correct `inventory.ini` configuration).

## Project Structure

```
## Key Files

* **`inventory.ini`:** Defines servers, groups, and Ansible connection settings.
* **`ansible.cfg`:** Global configuration for Ansible.
* **`setup_ssh_keys.yml`:** Playbook to distribute the SSH public key to target servers.
* **`install_mariadb.yml`:** Installs and performs the initial configuration of MariaDB.
* **`configure_master.yml`:** Configures the Master server.
* **`configure_replicas.yml`:** Configures Replica servers and sets up replication.
* **`install_maxscale.yml`:** Installs and configures MaxScale.
* **`install_proxysql.yml`:** Installs and configures ProxySQL.

```
Set up SSH Keys

First, distribute the SSH key between the control node and the target servers (if you have not already done so):
```
ansible-playbook setup_ssh_keys.yml --ask-pass
```

Install MariaDB
```
ansible-playbook -i inventory.ini install_mariadb.yml
```

 Configure the Master and Replicas:
```
ansible-playbook -i inventory.ini configure_master.yml
ansible-playbook -i inventory.ini configure_replicas.yml
```

Install MaxScale
```
ansible-playbook -i inventory.ini install_maxscale.yml
```

Install ProxySQL
```
ansible-playbook -i inventory.ini install_proxysql.yml
```

***Note: All playbooks run by default for the [mariadb_servers] and [proxy_servers] groups defined in the inventory.ini file. Modify these groups if needed.

